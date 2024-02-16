# 在AWS Trainium上为文本分类微调BERT

> 原始文本：[https://huggingface.co/docs/optimum-neuron/tutorials/fine_tune_bert](https://huggingface.co/docs/optimum-neuron/tutorials/fine_tune_bert)

*这个教程有一个笔记本版本[在这里](https://github.com/huggingface/optimum-neuron/blob/main/notebooks/text-classification/notebook.ipynb)*。

这个教程将帮助您开始使用[AWS Trainium](https://aws.amazon.com/machine-learning/trainium/?nc1=h_ls)和Hugging Face Transformers。它将涵盖如何在AWS上设置Trainium实例，加载和微调一个用于文本分类的transformers模型

您将学习如何：

1.  [设置AWS环境](#1-setup-aws-environment)

1.  [加载和处理数据集](#2-load-and-process-the-dataset)

1.  [使用Hugging Face Transformers和Optimum Neuron微调BERT](#3-fine-tune-bert-using-hugging-face-transformers)

在开始之前，请确保您有一个[Hugging Face账户](https://huggingface.co/join)以保存工件和实验。

## 快速介绍：AWS Trainium

[AWS Trainium (Trn1)](https://aws.amazon.com/de/ec2/instance-types/trn1/)是专为深度学习（DL）训练工作负载而构建的EC2。Trainium是[AWS Inferentia](https://aws.amazon.com/ec2/instance-types/inf1/?nc1=h_ls)的继任者，专注于高性能训练工作负载，声称与可比较的基于GPU的实例相比，训练成本节省高达50%。

Trainium已经针对训练自然语言处理、计算机视觉和推荐模型进行了优化。该加速器支持广泛的数据类型，包括FP32、TF32、BF16、FP16、UINT8和可配置的FP8。

最大的Trainium实例，`trn1.32xlarge`拥有超过500GB的内存，使得在单个实例上轻松微调约10B参数模型变得容易。下面是可用实例类型的概述。更多细节[在这里](https://aws.amazon.com/de/ec2/instance-types/trn1/#Product_details)：

| 实例大小 | 加速器 | 加速器内存 | vCPU | CPU内存 | 每小时价格 |
| --- | --- | --- | --- | --- | --- |
| trn1.2xlarge | 1 | 32 | 8 | 32 | $1.34 |
| trn1.32xlarge | 16 | 512 | 128 | 512 | $21.50 |
| trn1n.32xlarge (2x bandwidth) | 16 | 512 | 128 | 512 | $24.78 |

* * *

现在我们知道Trainium提供了什么，让我们开始吧。🚀

*注意：这个教程是在一个trn1.2xlarge的AWS EC2实例上创建的。*

## 1\. 设置AWS环境

在这个例子中，我们将在AWS上使用`trn1.2xlarge`实例，包括1个加速器，包括两个Neuron Cores和[Hugging Face Neuron Deep Learning AMI](https://aws.amazon.com/marketplace/pp/prodview-gr3e6yiscria2)。

这篇博文不详细介绍如何创建实例。您可以查看我的之前关于[“为Hugging Face Transformers设置AWS Trainium”](https://www.philschmid.de/setup-aws-trainium)的博客，其中包括关于设置环境的逐步指南。

一旦实例启动运行，我们可以ssh进入它。但是，我们不想在终端内开发，而是想使用一个`Jupyter`环境，我们可以用来准备数据集和启动训练。为此，我们需要在`ssh`命令中添加一个用于转发的端口，这将把我们的本地主机流量隧道到Trainium实例。

```py
PUBLIC_DNS="" # IP address, e.g. ec2-3-80-....
KEY_PATH="" # local path to key, e.g. ssh/trn.pem

ssh -L 8080:localhost:8080 -i ${KEY_NAME}.pem ubuntu@$PUBLIC_DNS
```

我们现在可以启动我们的**`jupyter`**服务器。

```py
python -m notebook --allow-root --port=8080
```

您应该看到一个熟悉的**`jupyter`**输出，其中包含一个指向笔记本的URL。

**`http://localhost:8080/?token=8c1739aff1755bd7958c4cfccc8d08cb5da5234f61f129a9`**

我们可以点击它，在我们的本地浏览器中打开一个**`jupyter`**环境。

![jupyter.webp](../Images/f3e7326719a8cc7f67122b89fb3e1dc1.png)

我们将仅使用Jupyter环境准备数据集，然后使用`torchrun`在两个Neuron Cores上启动我们的训练脚本进行分布式训练。让我们创建一个新的笔记本并开始吧。

## 2\. 加载和处理数据集

我们正在对[情感](https://huggingface.co/datasets/philschmid/emotion)数据集上的文本分类模型进行训练，以保持示例简单。`emotion`是一个包含六种基本情绪（愤怒、恐惧、喜悦、爱、悲伤和惊讶）的英文Twitter消息数据集。

我们将使用[🤗 Datasets](https://huggingface.co/docs/datasets/index)库中的`load_dataset()`方法加载`emotion`。

```py
from datasets import load_dataset

# Dataset id from huggingface.co/dataset
dataset_id = "philschmid/emotion"

# Load raw dataset
raw_dataset = load_dataset(dataset_id)

print(f"Train dataset size: {len(raw_dataset['train'])}")
print(f"Test dataset size: {len(raw_dataset['test'])}")

# Train dataset size: 16000
# Test dataset size: 2000
```

让我们看一个数据集的示例。

```py
from random import randrange

random_id = randrange(len(raw_dataset['train']))
raw_dataset['train'][random_id]
# {'text': 'i feel isolated and alone in my trade', 'label': 0}
```

我们必须将我们的“自然语言”转换为标记ID以训练我们的模型。这是通过一个分词器完成的，它对输入进行分词（包括将标记转换为预训练词汇表中对应的ID）。如果您想了解更多信息，请查看[Hugging Face课程](https://huggingface.co/course/chapter1/1)中的[第6章](https://huggingface.co/course/chapter6/1?fw=pt)。

我们的Neuron加速器期望输入具有固定的形状。我们需要将所有样本截断或填充到相同的长度。

```py
from transformers import AutoTokenizer
import os
# Model id to load the tokenizer
model_id = "bert-base-uncased"
save_dataset_path = "lm_dataset"
# Load Tokenizer
tokenizer = AutoTokenizer.from_pretrained(model_id)

# Tokenize helper function
def tokenize(batch):
    return tokenizer(batch['text'], padding='max_length', truncation=True,return_tensors="pt")

# Tokenize dataset
raw_dataset =  raw_dataset.rename_column("label", "labels") # to match Trainer
tokenized_dataset = raw_dataset.map(tokenize, batched=True, remove_columns=["text"])
tokenized_dataset = tokenized_dataset.with_format("torch")

# save dataset to disk
tokenized_dataset["train"].save_to_disk(os.path.join(save_dataset_path,"train"))
tokenized_dataset["test"].save_to_disk(os.path.join(save_dataset_path,"eval"))
```

## 3. 使用Hugging Face Transformers微调BERT

通常情况下，您会使用[Trainer](https://huggingface.co/docs/transformers/v4.19.4/en/main_classes/trainer#transformers.Trainer)和[TrainingArguments](https://huggingface.co/docs/transformers/v4.19.4/en/main_classes/trainer#transformers.TrainingArguments)来微调基于PyTorch的transformer模型。

但是，与AWS一起，我们开发了一个[NeuronTrainer](https://huggingface.co/docs/optimum-neuron/package_reference/trainer)，以提高在Trainium或Inferentia2实例上训练时的性能、稳健性和安全性。`NeuronTrainer`还配备了一个[模型缓存](https://www.notion.so/Getting-started-with-AWS-Trainium-and-Hugging-Face-Transformers-8428c72556194aed9c393de101229dcf)，允许我们使用Hugging Face Hub中的预编译模型和配置，跳过训练开始时需要的编译步骤。这可以将训练时间缩短约3倍。

`NeuronTrainer`是`optimum-neuron`库的一部分，可以作为`Trainer`的一对一替代品使用。您只需调整训练脚本中的导入即可。

```py
- from transformers import Trainer, TrainingArguments
+ from optimum.neuron import NeuronTrainer as Trainer
+ from optimum.neuron import NeuronTrainingArguments as TrainingArguments
```

我们准备了一个简单的[train.py](https://github.com/huggingface/optimum-neuron/blob/main/notebooks/text-classification/scripts/train.py)训练脚本，基于[“Getting started with Pytorch 2.0 and Hugging Face Transformers”](https://www.philschmid.de/getting-started-pytorch-2-0-transformers#3-fine-tune--evaluate-bert-model-with-the-hugging-face-trainer)博客文章，使用`NeuronTrainer`。以下是摘录

```py
from transformers import TrainingArguments
from optimum.neuron import NeuronTrainer as Trainer

def parse_args():
	...

def training_function(args):

    # load dataset from disk and tokenizer
    train_dataset = load_from_disk(os.path.join(args.dataset_path, "train"))
		...

    # Download the model from huggingface.co/models
    model = AutoModelForSequenceClassification.from_pretrained(
        args.model_id, num_labels=num_labels, label2id=label2id, id2label=id2label
    )

    training_args = TrainingArguments(
			...
    )

    # Create Trainer instance
    trainer = Trainer(
        model=model,
        args=training_args,
        train_dataset=train_dataset,
        eval_dataset=eval_dataset,
        compute_metrics=compute_metrics,
    )

    # Start training
    trainer.train()
```

我们可以使用`wget`命令将训练脚本加载到我们的环境中，也可以从[这里](https://github.com/huggingface/optimum-neuron/blob/notebooks/text-classification/scripts/train.py)手动复制到笔记本中。

```py
!wget https://raw.githubusercontent.com/huggingface/optimum-neuron/main/notebooks/text-classification/scripts/train.py
```

我们将使用`torchrun`在两个神经元核心上启动我们的训练脚本进行分布式训练。`torchrun`是一个工具，可以自动将PyTorch模型分布到多个加速器上。我们可以在超参数旁边传递加速器数量作为`nproc_per_node`参数。

我们将使用以下命令启动训练：

```py
!torchrun --nproc_per_node=2 train.py \
 --model_id bert-base-uncased \
 --dataset_path lm_dataset \
 --lr 5e-5 \
 --per_device_train_batch_size 16 \
 --bf16 True \
 --epochs 3
```

***注意**：如果您看到糟糕的准确率，您可能希望暂时停用`bf16`。*

经过9分钟的训练，获得了出色的`0.914`的f1分数。

```py
***** train metrics *****
  epoch                    =        3.0
  train_runtime            =    0:08:30
  train_samples            =      16000
  train_samples_per_second =     96.337

***** eval metrics *****
  eval_f1                  =      0.914
  eval_runtime             =    0:00:08
```

最后，终止EC2实例以避免不必要的费用。从性价比来看，我们的训练只花费了**`20ct`**（**`1.34$/h * 0.15h = 0.20$`**）
