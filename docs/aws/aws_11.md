# 在 AWS Trainium 上对 Llama 2 7B 进行微调和测试

> 原始文本：[`huggingface.co/docs/optimum-neuron/tutorials/fine_tune_llama_7b`](https://huggingface.co/docs/optimum-neuron/tutorials/fine_tune_llama_7b)

*这里有一个关于该教程的笔记本版本[链接](https://github.com/huggingface/optimum-neuron/blob/main/notebooks/text-generation/llama2-7b-fine-tuning.ipynb)*。

本教程将教您如何在 AWS Trainium 上微调开放的 LLM（如[Llama 2](https://huggingface.co/meta-llama/Llama-2-7b-hf)）。在我们的示例中，我们将利用 Hugging Face [`huggingface.co/docs/optimum-neuron/index`](https://huggingface.co/docs/optimum-neuron/index)、Transformers 和[`huggingface.co/docs/datasets/index`](https://huggingface.co/docs/datasets/index)。

您将学习如何：

1.  设置 AWS 环境

1.  加载和处理数据集

1.  在 AWS Trainium 上使用`NeuronTrainer`对 Llama 进行微调

1.  评估和测试微调的 Llama 模型

## 快速介绍：AWS Trainium

[AWS Trainium (Trn1)](https://aws.amazon.com/de/ec2/instance-types/trn1/)是专为深度学习（DL）训练工作负载而设计的 EC2。Trainium 是[AWS Inferentia](https://aws.amazon.com/ec2/instance-types/inf1/?nc1=h_ls)的后继产品，专注于高性能训练工作负载。Trainium 已经针对训练自然语言处理、计算机视觉和推荐模型进行了优化。该加速器支持多种数据类型，包括 FP32、TF32、BF16、FP16、UINT8 和可配置的 FP8。

最大的 Trainium 实例，`trn1.32xlarge`配备了超过 500GB 的内存，使得在单个实例上对约 10B 参数模型进行微调变得容易。下面是可用实例类型的概述。更多细节[在这里](https://aws.amazon.com/de/ec2/instance-types/trn1/#Product_details)：

| 实例大小 | 加速器 | 加速器内存 | vCPU | CPU 内存 | 每小时价格 |
| --- | --- | --- | --- | --- | --- |
| trn1.2xlarge | 1 | 32 | 8 | 32 | \$1.34 |
| trn1.32xlarge | 16 | 512 | 128 | 512 | \$21.50 |
| trn1n.32xlarge (2 倍带宽) | 16 | 512 | 128 | 512 | \$24.78 |

*注意：本教程是在 trn1.32xlarge AWS EC2 实例上创建的。*

## 1. 设置 AWS 环境

在本示例中，我们将在 AWS 上使用`trn1.32xlarge`实例，其中包括 16 个加速器，包括 32 个神经元核心和[Hugging Face Neuron Deep Learning AMI](https://aws.amazon.com/marketplace/pp/prodview-gr3e6yiscria2)。Hugging Face AMI 预先安装了所有重要的库，如 Transformers、Datasets、Optimum 和 Neuron 包。这使得入门变得非常容易，因为无需进行环境管理。

本教程不详细介绍如何创建实例。您可以查看关于“为 Hugging Face Transformers 设置 AWS Trainium”的专门教程，其中包括有关设置环境的逐步指南。

一旦实例启动并运行，我们可以 ssh 进入它。但是，我们不想在终端内开发，而是想使用`Jupyter`环境，我们可以用来准备我们的数据集和启动训练。为此，我们需要在`ssh`命令中添加一个用于转发的端口，这将把我们的本地主机流量隧道传输到 Trainium 实例。

```py
PUBLIC_DNS="" # IP address, e.g. ec2-3-80-....
KEY_PATH="" # local path to key, e.g. ssh/trn.pem

ssh -L 8080:localhost:8080 -i ${KEY_NAME}.pem ubuntu@$PUBLIC_DNS
```

现在让我们拉取带有[示例笔记本和脚本](https://github.com/huggingface/optimum-neuron/tree/main/notebooks/text-generation)的最佳存储库。

```py
git clone https://github.com/huggingface/optimum-neuron.git
```

接下来，我们可以将目录更改为`notbooks/text-generation`并启动`jupyter`环境。

```py
# change directory
cd optimum-neuron/notebooks/text-generation
# launch jupyter
python -m notebook --allow-root --port=8080
```

您应该看到一个熟悉的**`jupyter`**输出，其中包含指向笔记本的 URL。

**`http://localhost:8080/?token=8c1739aff1755bd7958c4cfccc8d08cb5da5234f61f129a9`**

我们可以单击它，在本地浏览器中打开一个**`jupyter`**环境。打开笔记本**`llama2-7b-fine-tuning.ipynb`**，然后开始吧。

*注意：我们将仅在 Jupyter 环境中准备数据集，然后使用`torchrun`启动我们的分布式训练脚本。*

如果您要使用官方的 Llama 2 检查点，您需要登录到我们的 Hugging Face 帐户，该帐户可以访问模型，以使用您的令牌访问门控存储库。我们可以通过运行以下命令来实现这一点：

*注意：我们还提供了一个非门控检查点。*

```py
!huggingface-cli login --token YOUR_TOKEN
```

## 2\. 加载和准备数据集

我们将使用[Dolly](https://huggingface.co/datasets/databricks/databricks-dolly-15k)，这是一个开源数据集，记录了在[InstructGPT paper](https://arxiv.org/abs/2203.02155)中概述的类别上的指令跟随记录，包括头脑风暴、分类、封闭 QA、生成、信息提取、开放 QA 和摘要。

```py
{
  "instruction": "What is world of warcraft",
  "context": "",
  "response": "World of warcraft is a massive online multi player role playing game. It was released in 2004 by bizarre entertainment"
}
```

加载`dolly`数据集，我们使用🤗数据集库中的`load_dataset()`方法。

```py
from datasets import load_dataset
from random import randrange

# Load dataset from the hub
dataset = load_dataset("databricks/databricks-dolly-15k", split="train")

print(f"dataset size: {len(dataset)}")
print(dataset[randrange(len(dataset))])
# dataset size: 15011

```

为了指导调整我们的模型，我们需要将我们的结构化示例转换为通过指令描述的任务集合。我们定义一个`formatting_function`，它接受一个样本并返回一个带有我们格式指令的字符串。

```py
def format_dolly(sample):
    instruction = f"### Instruction\n{sample['instruction']}"
    context = f"### Context\n{sample['context']}" if len(sample["context"]) > 0 else None
    response = f"### Answer\n{sample['response']}"
    # join all the parts together
    prompt = "\n\n".join([i for i in [instruction, context, response] if i is not None])
    return prompt

```

让我们在一个随机示例上测试我们的格式化函数。

```py
from random import randrange

print(format_dolly(dataset[randrange(len(dataset))]))
```

除了格式化我们的样本，我们还希望将多个样本打包到一个序列中，以实现更高效的训练。这意味着我们将多个样本堆叠到一个序列中，并用 EOS 标记分割它们。这使得训练更加高效。打包/堆叠样本可以在训练期间或之前完成。我们将在训练之前执行此操作以节省时间。我们创建了一个实用方法[pack_dataset](https://github.com/huggingface/optimum-neuron/tree/main/notebooks/text-generation/scripts/utils/pack_dataset.py)，它接受一个数据集和一个打包函数，并返回一个打包后的数据集。

```py
from transformers import AutoTokenizer

# Hugging Face model id
model_id = "philschmid/Llama-2-7b-hf" # ungated
# model_id = "meta-llama/Llama-2-7b-hf" # gated

tokenizer = AutoTokenizer.from_pretrained(model_id)
```

为了打包/堆叠我们的数据集，我们需要首先对其进行标记化，然后我们可以使用`pack_dataset`方法对其进行打包。为了准备我们的数据集，我们现在将：

1.  使用模板方法格式化我们的样本，并在每个样本末尾添加一个 EOS 标记

1.  将我们的数据集标记化，将其从文本转换为令牌

1.  将我们的数据集打包到 2048 个令牌

```py
from random import randint
# add utils method to path for loading dataset
import sys
sys.path.append("./scripts/utils") # make sure you change this to the correct path
from pack_dataset import pack_dataset

# template dataset to add prompt to each sample
def template_dataset(sample):
    sample["text"] = f"{format_dolly(sample)}{tokenizer.eos_token}"
    return sample

# apply prompt template per sample
dataset = dataset.map(template_dataset, remove_columns=list(dataset.features))
# print random sample
print(dataset[randint(0, len(dataset))]["text"])

# tokenize dataset
dataset = dataset.map(
    lambda sample: tokenizer(sample["text"]), batched=True, remove_columns=list(dataset.features)
)

# chunk dataset
lm_dataset = pack_dataset(dataset, chunk_length=2048) # We use 2048 as the maximum length for packing
```

处理完数据集后，我们将其保存到磁盘。您也可以将其保存到 S3 或 Hugging Face Hub 以供以后使用。

*注意：打包和预处理数据集可以在 Trainium 实例之外运行。*

```py
# save train_dataset to disk
dataset_path = "tokenized_dolly"
lm_dataset.save_to_disk(dataset_path)
```

## 3\. 在 AWS Trainium 上使用 NeuronTrainer 对 Llama 进行微调

通常情况下，您会使用**[Trainer](https://huggingface.co/docs/transformers/main/en/main_classes/trainer#transformers.Trainer)**和**[TrainingArguments](https://huggingface.co/docs/transformers/main/en/main_classes/trainer#transformers.TrainingArguments)**来微调基于 PyTorch 的变压器模型。

但是与 AWS 一起，我们开发了一个`NeuronTrainer`来提高在 Trainium 实例上训练时的性能、稳健性和安全性。`NeuronTrainer`是`optimum-neuron`库的一部分，可以作为`Trainer`的 1 对 1 替代品使用。

在 AWS Trainium 上进行分布式训练时，有一些事项需要注意。由于 Llama 是一个庞大的模型，可能无法适应单个加速器，因此我们为`NeuronTrainer`添加了对不同分布式训练策略的支持，包括：

+   [ZeRO-1](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/frameworks/torch/torch-neuronx/tutorials/training/zero1_gpt2.html)：将优化器状态分片到多个设备上。

+   [Tensor Parallelism](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/libraries/neuronx-distributed/tensor_parallelism_overview.html)：将模型参数沿着给定维度分片到多个设备上，使用`tensor_parallel_size`定义

+   [序列并行](https://arxiv.org/pdf/2205.05198.pdf) 在张量并行区域之外的序列轴上分片激活。这很有用，因为它通过分片激活来节省内存。

+   [管道并行](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/libraries/neuronx-distributed/pipeline_parallelism_overview.html)：*即将推出*

我们准备了一个[run_clm.py](https://github.com/huggingface/optimum-neuron/blob/main/notebooks/text-generation/scripts/run_clm.py)，它已经为您实现了这些分布式训练策略。如果您想了解更多细节，可以查看[文档](https://huggingface.co/docs/optimum-neuron/guides/distributed_training)。在 AWS 加速器上训练模型时，我们首先需要使用训练参数编译我们的模型。

为了克服这个问题，我们添加了一个[model cache](https://huggingface.co/docs/optimum-neuron/guides/cache_system)，它允许我们使用 Hugging Face Hub 中预编译的模型和配置来跳过编译步骤。但是，配置的每次更改都将导致新的编译，这可能会导致一些缓存未命中。

*注意：如果您的配置没有缓存，请在[Github](https://github.com/huggingface/optimum-neuron/issues)上提出问题，我们很乐意包含它。*

我们已经为我们的训练预编译了配置，这意味着您可以跳过下面的单元格，或者重新运行它只需要几分钟，因为它重用了缓存的配置。

```py
# precompilation command
!MALLOC_ARENA_MAX=64 neuron_parallel_compile torchrun --nproc_per_node=32 scripts/run_clm.py \
 --model_id {model_id} \
 --dataset_path {dataset_path} \
 --bf16 True \
 --learning_rate 5e-5 \
 --output_dir dolly_llama \
 --overwrite_output_dir True \
 --per_device_train_batch_size 1 \
 --gradient_checkpointing True \
 --tensor_parallel_size 8 \
 --max_steps 10 \
 --logging_steps 10 \
 --gradient_accumulation_steps 16
```

*注意：没有缓存的编译可能需要约 40 分钟。它还会在编译过程中在`dolly_llama_sharded`中创建虚拟文件，之后我们必须将它们删除。我们还需要添加`MALLOC_ARENA_MAX=64`来限制 CPU 分配，以避免潜在的崩溃，现在不要删除它。*

```py
# remove dummy artifacts which are created by the precompilation command
!rm -rf dolly_llama
```

编译完成后，我们可以使用类似的命令开始我们的训练，只需要删除`neuron_parallel_compile`。我们将使用`torchrun`来启动我们的训练脚本。`torchrun`是一个工具，可以自动将 PyTorch 模型分布到多个加速器上。我们可以通过`nproc_per_node`参数传递加速器的数量以及我们的超参数。与编译命令的区别在于，我们从`max_steps=10`更改为`num_train_epochs=3`。

使用以下命令启动训练。

```py
!MALLOC_ARENA_MAX=64 torchrun --nproc_per_node=32 scripts/run_clm.py \
 --model_id {model_id} \
 --dataset_path {dataset_path} \
 --bf16 True \
 --learning_rate 5e-5 \
 --output_dir dolly_llama \
 --overwrite_output_dir True \
 --skip_cache_push True \
 --per_device_train_batch_size 1 \
 --gradient_checkpointing True \
 --tensor_parallel_size 8 \
 --num_train_epochs 3 \
 --logging_steps 10 \
 --gradient_accumulation_steps 16
```

这就是了，我们成功地在 AWS Trainium 上训练了 Llama 7B。在 dolly 上进行了 3 个 epochs 的训练（15k 个样本），花费了 43:24 分钟，而原始训练时间仅为 31:46 分钟。这导致在 trn1.32xlarge 实例上进行端到端训练的成本约为 15.5 美元。不错！

但在我们分享和测试模型之前，我们需要合并模型。由于我们在训练过程中使用了 Tensor Parallelism，所以我们需要在使用之前合并模型权重。Tensor Parallelism 将模型权重分片到不同的工作节点，只有在训练期间才会保存分片检查点。

Optimum CLI 通过`optimum neuron consolidate`命令提供了一种非常容易实现这一点的方式：

```py
!optimum-cli neuron consolidate dolly_llama/tensor_parallel_shards dolly_llama
```

让我们删除我们的“sharded”检查点，因为我们已经将它们合并到 safetensors 中。

```py
!rm -rf dolly_llama/tensor_parallel_shards
```

## 4. 评估和测试微调的 Llama 模型

与训练类似，为了能够在 AWS Trainium 或 AWS Inferentia2 上运行推断，我们需要为正确的使用编译我们的模型。我们将使用我们的 Trainium 实例进行推断测试，但我们建议客户切换到 Inferentia2 进行推断。

Optimum Neuron 实现了类似于 Transformers AutoModel 类的功能，用于简化推断使用。我们将使用`NeuronModelForCausalLM`类来加载我们的 vanilla transformers 检查点并将其转换为 neuron。

```py
from optimum.neuron import NeuronModelForCausalLM
from transformers import AutoTokenizer

compiler_args = {"num_cores": 2, "auto_cast_type": 'fp16'}
input_shapes = {"batch_size": 1, "sequence_length": 2048}

tokenizer = AutoTokenizer.from_pretrained("dolly_llama")
model = NeuronModelForCausalLM.from_pretrained(
        "dolly_llama",
        export=True,
        **compiler_args,
        **input_shapes)

```

*注意：推断编译可能需要约 25 分钟。幸运的是，您只需要运行一次。因为您之后可以保存模型。如果您要在 Inferentia2 上运行，您需要重新编译。编译是参数和硬件特定的。*

```py
# COMMENT IN if you want to save the compiled model
# model.save_pretrained("compiled_dolly_llama")
```

现在我们可以测试推理，但必须确保我们将输入格式化为我们用于微调的提示格式。因此，我们创建了一个辅助方法，它接受一个带有我们的“指令”和可选“上下文”的`dict`。

```py
def format_dolly_inference(sample):
    instruction = f"### Instruction\n{sample['instruction']}"
    context = f"### Context\n{sample['context']}" if "context" in sample else None
    response = f"### Answer\n"
    # join all the parts together
    prompt = "\n\n".join([i for i in [instruction, context, response] if i is not None])
    return prompt

def generate(sample):
    prompt = format_dolly_inference(sample)
    inputs = tokenizer(prompt, return_tensors="pt")
    outputs = model.generate(**inputs,
                         max_new_tokens=512,
                         do_sample=True,
                         temperature=0.9,
                         top_k=50,
                         top_p=0.9)
    return tokenizer.decode(outputs[0], skip_special_tokens=False)[len(prompt):]
```

让我们测试推理。首先我们测试没有上下文的情况。

*注意：在 AWS Trainium 上使用 2 个核心进行推理不会非常快。对于推理，我们建议使用 Inferentia2。*

```py
prompt = {
  "instruction": "Can you tell me something about AWS?"
}
res = generate(prompt)

print(res)
```

> AWS 代表亚马逊网络服务。AWS 是亚马逊提供的一套远程计算服务，其中最广泛使用的包括 Amazon Elastic Compute Cloud（Amazon EC2），它在云中提供可调整大小的计算能力；Amazon Simple Storage Service（Amazon S3），它是一种对象存储服务；以及 Amazon Elastic Block Store（Amazon EBS），它旨在为与 AWS 实例一起使用的高性能、耐用的块存储卷提供支持。AWS 还提供其他服务，例如 AWS 身份和访问管理（IAM），这是一项使组织能够控制对其 AWS 资源访问的服务，以及 AWS 密钥管理服务（AWS KMS），它帮助客户创建和控制加密密钥的使用。

看起来正确。现在，让我们添加一些上下文，例如您在 RAG 应用程序中所做的那样

```py
prompt = {
  "instruction": "How can I train models on AWS Trainium?",
  "context": "🤗 Optimum Neuron is the interface between the 🤗 Transformers library and AWS Accelerators including [AWS Trainium](https://aws.amazon.com/machine-learning/trainium/?nc1=h_ls) and [AWS Inferentia](https://aws.amazon.com/machine-learning/inferentia/?nc1=h_ls). It provides a set of tools enabling easy model loading, training and inference on single- and multi-Accelerator settings for different downstream tasks."
}
res = generate(prompt)

print(res)
```

> 您可以使用 Optimum Neuron 界面在 AWS Trainium 上训练模型。

太棒了，我们的模型还正确使用了提供的上下文。我们完成了。祝贺您在 AWS Trainium 上对 Llama 进行了精细调整。
