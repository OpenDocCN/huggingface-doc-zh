# 神经元模型缓存

> 原文：[https://huggingface.co/docs/optimum-neuron/guides/cache_system](https://huggingface.co/docs/optimum-neuron/guides/cache_system)

神经元模型缓存是`neff`格式编译的神经元模型的远程缓存。它集成到`NeuronTrainer`和`NeuronModelForCausalLM`类中，以便从缓存中加载预训练模型，而不是在本地编译它们。

**注意：对于使用其他NeuronModelXX类导出的模型，该类使用不同的导出机制，因此不可用。**

神经元模型缓存托管在[Hugging Face Hub](https://huggingface.co/aws-neuron/optimum-neuron-cache)，包括所有流行和支持的`optimum-neuron`预训练模型的编译文件。

在在Neuron平台上训练Transformers或Diffusion模型或加载NeuronModelForCausalLM之前，需要使用[`torch-neuronx`](https://github.com/aws-neuron/aws-neuron-samples/tree/master/torch-neuronx)将其导出为神经元格式。

在导出模型时，[`torch-neuronx`](https://github.com/aws-neuron/aws-neuron-samples/tree/master/torch-neuronx)将：

+   将其转换为一组[XLA](https://github.com/pytorch/xla/)子图，

+   使用neuronx编译器将每个子图编译成神经元可执行文件格式（NEFF）二进制文件。

第一步相对较快，但编译需要很长时间。为了避免每次在NeuronX主机上加载模型时重新编译所有NEFF文件，[`torch-neuronx`](https://github.com/aws-neuron/aws-neuron-samples/tree/master/torch-neuronx)将NEFF文件存储在本地目录中，通常是`/var/tmp/neuron-compile-cache`。

然而，这个本地缓存在平台之间不共享，这意味着每次在新主机上训练或导出模型时，都需要重新编译。

我们创建了神经元模型缓存来解决这个限制，通过提供一个预编译模型图的公共存储库。

注意：我们还支持创建私有、安全的远程模型缓存。

## 如何使用神经元模型缓存

当您使用`NeuronTrainer`或`NeuronModelForCausalLM`类时，将使用公共模型缓存。不需要额外的更改。

在将模型导出为神经元格式时，`optimum-neuron`将在编译模型子图期间在hub存储库中查找缓存的NEFF文件。

如果NEFF文件被缓存，它们将从hub中获取并直接加载，而不是重新编译。

## 缓存是如何工作的

最佳神经元缓存建立在[NeuronX编译器缓存](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/general/arch/neuron-features/neuron-caching.html)之上。

重要的是要理解缓存操作的是NEFF二进制文件，而不是模型本身。

正如之前解释的，使用`NeuronTrainer`或`NeuronModelForCausalLM`导出到Neuron的每个模型都由[XLA](https://github.com/pytorch/xla/)子图组成。

每个子图都是独一无二的，是由以下组合而成的：

+   `transformers`或`transformers_neuronx` python建模代码，

+   `transformers`模型配置，

+   在导出期间选择的`input_shapes`，

+   模型的精度，全精度、fp16或bf16。

在将子图编译为NEFF文件时，其他参数会影响结果：

+   神经元X编译器的版本，

+   使用的神经元核心数量，

+   编译参数（如优化级别）。

所有这些参数结合在一起，创建一个唯一的哈希，用于标识一个NEFF文件。

这有两个非常重要的后果：

+   只有在实际导出模型时，才能识别相关的NEFF文件，

+   即使模型配置发生微小变化，也会导致不同的NEFF文件集。

因此很难事先知道与特定模型配置相关联的NEFF是否被缓存。

## 神经元模型缓存查找（仅限inferentia）

神经元缓存查找是一个功能，允许用户在导出模型进行推理之前查找兼容的缓存模型配置。

它基于一个由存储的缓存配置组成的专用注册表。

缓存的模型配置存储为神经元模型缓存中特定子文件夹下的条目：

```py
neuronxcc-2.12.54.0+f631c2365
├── 0_REGISTRY
│   └── 0.0.18
│       └── llama │           └── meta-llama │               └── Llama-2-7b-chat-hf │                   └── 54c1f6689cd88f246fce.json
```

每个条目对应于模型配置及其导出参数的组合：这是我们能够唯一标识导出模型的最接近方式。

您可以使用 `optimum-cli` 通过传递一个 hub model_id 或包含模型 `config.json` 的文件路径来查找兼容的缓存条目。

```py
$ optimum-cli neuron cache lookup meta-llama/Llama-2-7b-chat-hf

*** 1 entrie(s) found in cache for meta-llama/Llama-2-7b-chat-hf ***

task: text-generation
batch_size: 1
num_cores: 24
auto_cast_type: fp16
sequence_length: 2048
compiler_type: neuronx-cc
compiler_version: 2.12.54.0+f631c2365
checkpoint_id: meta-llama/Llama-2-7b-chat-hf
checkpoint_revision: c1b0db933684edbfe29a06fa47eb19cc48025e93
```

**请注意，即使存在兼容的缓存条目，这并不总是保证在导出时模型不会重新编译，如果您修改了编译参数或更新了 neuronx 包。**

## 高级用法（仅限 trainium）

### 如何使用私有神经元模型缓存（仅限 trainium）

公共缓存的存储库是 `aws-neuron/optimum-neuron-cache`。该存储库包含所有常用模型的预编译文件，因此它是公开可用且免费供所有人使用。但有两个限制：

1.  您将无法将自己编译的文件推送到此存储库

1.  它是公开的，您可能希望为私有模型使用私有存储库

为了缓解这一点，您可以使用 `optimum-cli` 创建自己的私有缓存存储库，或设置环境变量 `CUSTOM_CACHE_REPO`。

#### 使用 Optimum CLI

Optimum CLI 提供了用于缓存创建和设置的 2 个子命令：

+   `create`：创建一个新的缓存存储库，您可以将其用作私有神经元模型缓存。

+   `set`：要在本地设置神经元缓存存储库的名称，存储库需要存在，并且将默认由 `optimum-neuron` 使用。

创建一个新的神经元缓存存储库：

```py
optimum-cli neuron cache create --help

usage: optimum-cli neuron cache create [-h] [-n NAME] [--public]

optional arguments:
  -h, --help            show this help message and exit
  -n NAME, --name NAME  The name of the repo that will be used as a remote cache for the compilation files.
  --public              If set, the created repo will be public. By default the cache repo is private.

```

`-n` / `--name` 选项允许您为神经元缓存存储库指定一个名称，如果未设置，则将使用默认名称。`--public` 标志允许您将您的神经元缓存设为公共，因为默认情况下它将被创建为私有存储库。

示例：

```py
optimum-cli neuron cache create

Neuron cache created on the Hugging Face Hub: michaelbenayoun/optimum-neuron-cache [private].
Neuron cache name set locally to michaelbenayoun/optimum-neuron-cache in /home/michael/.cache/huggingface/optimum_neuron_custom_cache.
```

设置不同的 Trainiun 缓存存储库：

```py
usage: optimum-cli neuron cache set [-h] name

positional arguments:
  name        The name of the repo to use as remote cache.

optional arguments:
  -h, --help  show this help message and exit
```

示例：

```py
optimum-cli neuron cache set michaelbenayoun/optimum-neuron-cache

Neuron cache name set locally to michaelbenayoun/optimum-neuron-cache in /home/michael/.cache/huggingface/optimum_neuron_custom_cache
```

在新实例上使用 `optimum-cli neuron cache set` 命令很有用，以使用您自己的缓存。

#### 使用环境变量 CUSTOM_CACHE_REPO

使用 CLI 并不总是可行的，对于小型测试也不太实用。在这种情况下，您可以简单地设置环境变量 `CUSTOM_CACHE_REPO`。

例如，如果您的缓存存储库名为 `michaelbenayoun/my_custom_cache_repo`，您只需执行：

```py
CUSTOM_CACHE_REPO="michaelbenayoun/my_custom_cache_repo" torchrun ...
```

或：

```py
export CUSTOM_CACHE_REPO="michaelbenayoun/my_custom_cache_repo"
torchrun ...
```

您必须[登录到 Hugging Face Hub](https://huggingface.co/docs/huggingface_hub/quick-start#login)才能从您的私有缓存存储库推送和拉取文件。

### 缓存系统流程

![缓存系统流程](../Images/f18b7021150569c164d86f67298a14e8.png)

*缓存系统流程*

在每个训练步骤开始时，[NeuronTrainer](/docs/optimum.neuron/main/en/package_reference/trainer#optimum.neuron.NeuronTrainer) 计算一个 `NeuronHash` 并检查 Hugging Face Hub 上的缓存存储库（官方和自定义）是否有与此哈希相关联的编译文件。如果是这样，文件将直接下载到本地缓存目录，无需进行编译。否则将执行编译。

与下载编译文件一样，[NeuronTrainer](/docs/optimum.neuron/main/en/package_reference/trainer#optimum.neuron.NeuronTrainer) 将在每个训练步骤中跟踪新创建的编译文件，并在保存时或训练结束时将它们上传到 Hugging Face Hub。这假定您对缓存存储库具有写入访问权限，否则将不会推送任何内容。

## Optimum CLI

Optimum CLI 可用于执行各种与缓存相关的任务，如 `optimum-cli neuron cache` 命令使用消息所述：

```py
usage: optimum-cli neuron cache [-h] {create,set,add,list} ...

positional arguments:
  {create,set,add,list,synchronize,lookup}
    create              Create a model repo on the Hugging Face Hub to store Neuron X compilation files.
    set                 Set the name of the Neuron cache repo to use locally (trainium only).
    add                 Add a model to the cache of your choice (trainium only).
    list                List models in a cache repo (trainium only).
    synchronize         Synchronize local compiler cache with the hub cache (inferentia only).
    lookup              Lookup the neuronx compiler hub cache for the specified model id (inferentia only).

optional arguments:
  -h, --help            show this help message and exit
```

### 将模型添加到缓存（仅限 trainium）

可以通过 `optimum-cli neuron cache add` 命令将模型编译文件添加到缓存存储库中：

```py
usage: optimum-cli neuron cache add [-h] -m MODEL --task TASK --train_batch_size TRAIN_BATCH_SIZE [--eval_batch_size EVAL_BATCH_SIZE] [--sequence_length SEQUENCE_LENGTH]
                                    [--encoder_sequence_length ENCODER_SEQUENCE_LENGTH] [--decoder_sequence_length DECODER_SEQUENCE_LENGTH]
                                    [--gradient_accumulation_steps GRADIENT_ACCUMULATION_STEPS] --precision {fp,bf16} --num_cores
                                    {1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32} [--max_steps MAX_STEPS]
```

运行此命令时，将运行一个小训练会话，并推送生成的编译文件。

确保要使用的 Neuron 缓存存储库已在本地设置，可以通过运行 `optimum-cli neuron cache set` 命令来完成。您还需要确保已登录到 Hugging Face Hub，并且对指定的缓存存储库具有写入权限，可以通过 `huggingface-cli login` 命令来完成。

如果这些要求中至少有一个未满足，命令将失败。

示例：

```py
optimum-cli neuron cache add \
  --model prajjwal1/bert-tiny \
  --task text-classification \
  --train_batch_size 16 \
  --eval_batch_size 16 \
  --sequence_length 128 \
  --gradient_accumulation_steps 32 \
  --num_cores 32 \
  --precision bf16
```

这将在为指定参数设置的 Neuron 缓存存储库上推送 `prajjwal1/bert-tiny` 模型的编译文件。

### 列出缓存存储库

请求缓存存储库以了解可用的编译文件也很方便。可以通过 `optimum-cli neuron cache list` 命令来完成：

```py
usage: optimum-cli neuron cache list [-h] [-m MODEL] [-v VERSION] [name]

positional arguments:
  name                  The name of the repo to list. Will use the locally saved cache repo if left unspecified.

optional arguments:
  -h, --help            show this help message and exit
  -m MODEL, --model MODEL
                        The model name or path of the model to consider. If left unspecified, will list all available models.
  -v VERSION, --version VERSION
                        The version of the Neuron X Compiler to consider. Will list all available versions if left unspecified.
```

正如您所看到的，可以：

+   列出所有编译器版本可用的所有模型。

+   通过指定 `-v / --version` 参数列出给定编译器版本的所有可用模型。

+   列出给定模型的所有编译文件，可以根据不同的输入形状等指定 `-m / --model` 参数。

示例：

```py
optimum-cli neuron cache list aws-neuron/optimum-neuron-cache
```
