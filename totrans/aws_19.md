# 使用optimum-neuron进行分布式训练

> 原文链接：[https://huggingface.co/docs/optimum-neuron/guides/distributed_training](https://huggingface.co/docs/optimum-neuron/guides/distributed_training)

[AWS Trainium实例](https://aws.amazon.com/machine-learning/trainium/)非常适合训练模型。它们可以包含多达16个Neuron设备，每个设备包含2个Neuron核，内存为32GB（每个核为16GB）。例如，`trn1.32xlarge`实例具有32 x 16 = 512GB的内存。

但有一个警告：每个Neuron核默认是一个独立的数据并行工作者。这意味着模型、梯度状态和优化器状态，大约是模型大小的4倍，必须适合每个Neuron核（16GB）才能进行训练。如果是这种情况，则激活也必须适合剩余内存。

为了缓解这一点，`optimum-neuron`支持并行特性，使您能够充分利用您的Trainium实例的全部功能：

1.  [ZeRO-1](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/frameworks/torch/torch-neuronx/tutorials/training/zero1_gpt2.html)：这是数据并行优化，它通过在数据并行等级上分片优化器状态（通常代表设备上所需内存的一半）来实现。

1.  [张量并行性](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/libraries/neuronx-distributed/tensor_parallelism_overview.html)：这是一种技术，它通过在多个设备上沿着给定维度分片每个模型参数来实现。要在参数上分片的设备数量称为`tensor_parallel_size`。

1.  [流水线并行性](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/libraries/neuronx-distributed/pipeline_parallelism_overview.html)：**即将推出！**

好消息是可以结合这些技术，而且`optimum-neuron`使这变得非常容易！

optimum-neuron存储库中提供的所有示例脚本都通过`NeuronTrainer`实现了这些功能。

## 如何启用ZeRO-1？

无论您使用[NeuronTrainer](/docs/optimum.neuron/main/en/package_reference/trainer#optimum.neuron.NeuronTrainer)还是决定使用自己的训练脚本来使用`NeuronAccelerator`，都可以很容易地启用ZeRO-1优化。

### 通过NeuronTrainer

```py
from optimum.neuron import NeuronTrainingArguments, NeuronTrainer

# To enable ZeRO-1, set the `zero_1` argument to `True` in the training arguments.
training_args = NeuronTrainingArguments(
    ...,
    zero_1=True,
)

trainer = NeuronTrainer(
    model,
    args=training_args,
    train_dataset=train_dataset,
    eval_dataset=eval_dataset,
)

trainer.train()
```

由于示例脚本使用`NeuronTrainer`，因此在使用它们时，可以通过在命令行中添加`--zero_1`标志来启用ZeRO-1。

例如：

```py
torchrun --nproc_per_node=2 examples/language-modeling/run_clm.py \
    --model_name_or_path TinyLlama/TinyLlama-1.1B-Chat-v0.6 \
    --dataset_name wikitext \
    --dataset_config_name wikitext-2-raw-v1 \
    --do_train \
    --per_device_train_batch_size 1 \
    --block_size 1024 \
    --bf16 \
    --zero_1 \
    --output_dir my_training/

```

### 通过NeuronAccelerator

当不使用`NeuronTrainer`时，需要做一些额外的工作：

1.  （可选）包装优化器类以使其惰性化。当启用ZeRO-1时，原始优化器将被覆盖以使用其分片版本。因此，可以使原始优化器惰性加载，以便优化器状态在实际分片之前不会被实现。

```py
from torch.optim import AdamW
from optimum.neuron.distributed import make_optimizer_constructor_lazy

lazy_adamw = make_optimizer_constructor_lazy(AdamW)
```

1.  在实例化`NeuronAccelerator`时，将`zero_1`参数设置为`True`。

```py
accelerator = NeuronAccelerator(
    ...
    zero_1=True,
)

model = ...
lazy_optimizer = lazy_adamw(...) # Actually instantiate the optimizer.

model, optimizer = accelerator.prepare(model, lazy_optimizer)
```

## 如何启用张量并行性？

就像对于ZeRO-1一样，可以使用[NeuronTrainer](/docs/optimum.neuron/main/en/package_reference/trainer#optimum.neuron.NeuronTrainer)或`NeuronAccelerator`来应用张量并行性。

在进行张量并行性时，有不同的设置：

1.  `tensor_parallel_size`。理想情况下，应该是模型适合的最小值。

1.  是否应该启用序列并行性。[序列并行性](https://arxiv.org/pdf/2205.05198.pdf)在张量并行区域之外的序列轴上分片激活。这很有用，因为它通过分片激活来节省内存。

1.  是否应该对嵌入层进行并行化。默认情况下会进行，因为它提供了多种好处：

+   并行化嵌入层可以节省内存，从而可以使批量大小和/或序列长度更大。

+   对于语言模型，其中嵌入层权重和语言建模头权重通常是绑定的，语言建模头最终是并行的，不需要`all-gather`其输出，因为它被馈送到与并行兼容的交叉熵损失，节省了昂贵的通信。

此外，非常重要的是确保原始模型以高效的方式加载：训练脚本将由`torchrun`调用，它将将其分派给工作人员，每个核心一个工作人员。如果每个工作人员（在`trn1.32xlarge`实例中有32个工作人员）加载完整的模型权重，可能会花费很长时间，并且很快会耗尽内存。

`optimum-neuron`提供了一个上下文管理器[distributed.lazy_load_for_parallelism()](/docs/optimum.neuron/main/en/package_reference/distributed#optimum.neuron.distributed.lazy_load_for_parallelism)，它懒加载模型以防止，每个工作人员中只会实现相应模型分片的参数。

### 通过NeuronTrainer

```py
from optimum.neuron import NeuronTrainingArguments, NeuronTrainer
from optimum.neuron.distributed import lazy_load_for_parallelism

# Specify the `tensor_parallel_size` in the training arguments.
training_args = NeuronTrainingArguments(
    ...,
    tensor_parallel_size=8,
    disable_embedding_parallelization=False, # It is `False` by default.
    disable_sequence_parallel=False, # It is `False` by default.
)

with lazy_load_for_parallelism(tensor_parallel_size=training_args.tensor_parallel_size):
    model = ...

trainer = NeuronTrainer(
    model,
    args=training_args,
    train_dataset=train_dataset,
    eval_dataset=eval_dataset,
)

trainer.train()
```

由于示例脚本使用`NeuronTrainer`，您可以通过在命令行中指定`--tensor_parallel_size`参数以及可选的`disable_embedding_parallelization`和`disable_sequence_parallel`标志来在使用它们时启用Tensor Parallelism。

例如：

```py
torchrun --nproc_per_node=2 examples/language-modeling/run_clm.py \
    --model_name_or_path TinyLlama/TinyLlama-1.1B-Chat-v0.6 \
    --dataset_name wikitext \
    --dataset_config_name wikitext-2-raw-v1 \
    --do_train \
    --per_device_train_batch_size 1 \
    --block_size 1024 \
    --bf16 \
    --tensor_parallel_size 2 \
    --output_dir my_training/
```

### 通过NeuronAccelerator

就像对于ZeRO-1一样，可以包装优化器类使其变为懒加载。由于模型参数将被分片，因此在模型并行化之前不需要实现优化器状态：包装器确保其保持未实现状态。

```py
from torch.optim import AdamW
from optimum.neuron import NeuronAccelerator
from optimum.neuron.accelerate.utils import ModelParallelismPlugin
from optimum.neuron.distributed import lazy_load_for_parallelism

tensor_parallel_size = 8
mp_plugin = ModelParallelismPlugin(
    tensor_parallel_size,
    parallelize_embeddings=True,
    sequence_parallel_enabled=True,
    checkpoint_dir=None, # Can be specified when resuming from checkpoint.
)

accelerator = NeuronAccelerator(
    ...
    mp_plugin=mp_plugin,
)

with lazy_load_for_parallelism(tensor_parallel_size=tensor_parallel_size):
    model = ...

lazy_adamw = make_optimizer_constructor_lazy(AdamW)
lazy_optimizer = lazy_adamw(...) # Actually instantiate the optimizer.

model, optimizer = accelerator.prepare(model, lazy_optimizer)
```

### 检查点整合

由于Tensor Parallelism包括将模型权重分片到不同的工作人员中，因此在训练期间只会保存分片检查点。需要整合分片检查点以便能够在特定训练配置之外共享和使用它们。

Optimum CLI通过`optimum neuron consolidate`命令提供了一种非常简单的方法来实现这一点：

```py
optimum-cli neuron consolidate --help

usage: optimum-cli neuron consolidate [-h] [-f {pytorch,safetensors}] checkpoint_dir output_dir

positional arguments:
  checkpoint_dir        The path to the directory containing the checkpoints.
  output_dir            The path to the output directory containing the consolidated checkpoint.

optional arguments:
  -h, --help            show this help message and exit
  -f {pytorch,safetensors}, --format {pytorch,safetensors}
                        The format used to save the consolidated checkpoint.

```

您只需要指定分片检查点目录和包含整合检查点的输出目录，命令会处理其余部分。还可以指定整合检查点的输出格式，默认情况下将其导出为`safetensors`格式，这是推荐使用的格式。

例如：

使用Tensor Parallelism进行训练刚刚完成，输出目录称为`my_training`。该目录看起来如下：

```py
my_training/
├── README.md
├── all_results.json 
├── checkpoint-10 
│   ├── config.json
│   ├── scheduler.pt
│   ├── special_tokens_map.json
│   ├── tensor_parallel_shards
│   ├── tokenizer.json
│   ├── tokenizer.model
│   ├── tokenizer_config.json
│   ├── trainer_state.json
│   └── training_args.bin
├── config.json
├── special_tokens_map.json
├── tensor_parallel_shards
│   ├── tp_rank_00_pp_rank_00
│   ├── tp_rank_01_pp_rank_00
│   ├── tp_rank_02_pp_rank_00
│   ├── tp_rank_03_pp_rank_00
│   ├── tp_rank_04_pp_rank_00
│   ├── tp_rank_05_pp_rank_00
│   ├── tp_rank_06_pp_rank_00
│   └── tp_rank_07_pp_rank_00
├── tokenizer.json
├── tokenizer.model
├── tokenizer_config.json
├── train_results.json
├── trainer_state.json
└── training_args.bin
```

在`my_training/tensor_parallel_shards`中可以 consolide 分片检查点，这些检查点对应于训练结束时保存的分片检查点，通过运行以下命令：

```py
optimum-cli neuron consolidate my_training my_training_consolidated_checkpoint
```

分片检查点保存在一个名为`tensor_parallel_shards`的目录下。`optimum-cli neuron consolidate`命令接受一个包含`tensor_parallel_shards`目录的目录，或者`tensor_parallel_shards`目录本身作为输入。
