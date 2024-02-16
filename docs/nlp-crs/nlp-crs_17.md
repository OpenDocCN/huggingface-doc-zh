# 模型

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter2/3?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter2/3?fw=pt)

                Pytorch TensorFlow![Ask a Question](https://discuss.huggingface.co/t/chapter-2-questions) ![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/zh-CN/chapter2/section3_pt.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/zh-CN/chapter2/section3_pt.ipynb)

[`www.youtube-nocookie.com/embed/AhChOFRegn4`](https://www.youtube-nocookie.com/embed/AhChOFRegn4)

在本节中，我们将更详细地了解如何创建和使用模型。我们将使用 AutoModel 类，当您希望从检查点实例化任何模型时，这非常方便。

这个 AutoModel 类及其所有相关项实际上是对库中各种可用模型的简单包装。它是一个聪明的包装器，因为它可以自动猜测检查点的适当模型体系结构，然后用该体系结构实例化模型。

但是，如果您知道要使用的模型类型，则可以使用直接定义其体系结构的类。让我们看看这是如何与 BERT 模型一起工作的。

## 创建转换器

初始化 BERT 模型需要做的第一件事是加载配置对象：

```py
from transformers import BertConfig, BertModel

# Building the config
config = BertConfig()

# Building the model from the config
model = BertModel(config)
```

配置包含许多用于构建模型的属性：

```py
print(config)
```

```py
BertConfig {
  [...]
  "hidden_size": 768,
  "intermediate_size": 3072,
  "max_position_embeddings": 512,
  "num_attention_heads": 12,
  "num_hidden_layers": 12,
  [...]
}
```

虽然您还没有看到所有这些属性都做了什么，但您应该认识到其中的一些属性：hidden*size 属性定义了 hidden*状态向量的大小，num_hidden_layers 定义了 Transformer 模型的层数。

### 不同的加载方式

从默认配置创建模型会使用随机值对其进行初始化：

```py
from transformers import BertConfig, BertModel

config = BertConfig()
model = BertModel(config)

# Model is randomly initialized!
```

该模型可以在这种状态下使用，但会输出胡言乱语；首先需要对其进行训练。我们可以根据手头的任务从头开始训练模型，但正如您在 Chapter 1 ，这将需要很长的时间和大量的数据，并将产生不可忽视的环境影响。为了避免不必要的重复工作，必须能够共享和重用已经训练过的模型。

加载已经训练过的 Transformers 模型很简单-我们可以使用 from_pretrained() 方法：

```py
from transformers import BertModel

model = BertModel.from_pretrained("bert-base-cased")
```

正如您之前看到的，我们可以用等效的 AutoModel 类替换 Bert 模型。从现在开始，我们将这样做，因为这会产生检查点不可知的代码；如果您的代码适用于一个检查点，那么它应该与另一个检查点无缝地工作。即使体系结构不同，这也适用，只要检查点是针对类似任务（例如，情绪分析任务）训练的。

在上面的代码示例中，我们没有使用 BertConfig

，而是通过 Bert base cased 标识符加载了一个预训练模型。这是一个模型检查点，由 BERT 的作者自己训练；您可以在 [model card](https://huggingface.co/bert-base-cased)中找到更多细节.

该模型现在使用检查点的所有权重进行初始化。它可以直接用于对训练过的任务进行推理，也可以对新任务进行微调。通过预先训练重量而不是从头开始的训练，我们可以很快取得好的效果。

权重已下载并缓存在缓存文件夹中（因此将来对 from_pretrained()方法的调用将不会重新下载它们）默认为 ~/.cache/huggingface/transformers . 您可以通过设置 HF_HOME 环境变量来自定义缓存文件夹。

用于加载模型的标识符可以是模型中心 Hub 上任何模型的标识符，只要它与 BERT 体系结构兼容。可以找到可用的 BERT 检查点的完整列表 [here](https://huggingface.co/models?filter=bert) .

### 保存模型

保存模型和加载模型一样简单—我们使用 save_pretrained() 方法，类似于 from_pretrained() 方法：

```py
model.save_pretrained("directory_on_my_computer")
```

这会将两个文件保存到磁盘：

```py
ls directory_on_my_computer 
config.json pytorch_model.bin
```

如果你看一下 config.json 文件，您将识别构建模型体系结构所需的属性。该文件还包含一些元数据，例如检查点的来源以及上次保存检查点时使用的🤗 Transformers 版本。

这个 *pytorch_model.bin* 文件就是众所周知的*state dictionary*; 它包含模型的所有权重。这两个文件齐头并进；配置是了解模型体系结构所必需的，而模型权重是模型的参数。

### 使用 Transformers 模型进行推理

既然您知道了如何加载和保存模型，那么让我们尝试使用它进行一些预测。Transformer 模型只能处理数字——分词器生成的数字。但在我们讨论标记化器之前，让我们先探讨模型接受哪些输入。

标记化器可以将输入转换为适当的框架张量，但为了帮助您了解发生了什么，我们将快速了解在将输入发送到模型之前必须做什么。

假设我们有几个序列：

```py
sequences = ["Hello!", "Cool.", "Nice!"]
```

分词器将这些转换为词汇表索引，通常称为 input IDs . 每个序列现在都是一个数字列表！结果是：

```py
encoded_sequences = [
    [101, 7592, 999, 102],
    [101, 4658, 1012, 102],
    [101, 3835, 999, 102],
]
```

这是一个编码序列列表：一个列表列表。张量只接受矩形（想想矩阵）。此“数组”已为矩形，因此将其转换为张量很容易：

```py
import torch

model_inputs = torch.tensor(encoded_sequences)
```

### 使用张量作为模型的输入

在模型中使用张量非常简单-我们只需将输入称为模型：

```py
output = model(model_inputs)
```

虽然模型接受许多不同的参数，但只需要 input IDs。我们稍后将解释其他参数的作用以及何时需要它们，但首先我们需要更仔细地了解 Transformer 模型可以理解的输入的标记