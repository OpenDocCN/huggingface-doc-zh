# 使用AWS Neuron（Inf2/Trn1）进行推理管道

> 原文链接：[https://huggingface.co/docs/optimum-neuron/guides/pipelines](https://huggingface.co/docs/optimum-neuron/guides/pipelines)

`pipeline()`函数使得在各种任务上使用[Model Hub](https://huggingface.co/models)中的模型进行加速推理变得简单，例如文本分类、问答和图像分类。

您还可以使用Transformers中的[pipeline()](https://huggingface.co/docs/transformers/main/en/main_classes/pipelines#pipelines)函数，并提供您的NeurModel模型类。

目前支持的任务有：

+   `feature-extraction`

+   `fill-mask`

+   `text-classification`

+   `token-classification`

+   `question-answering`

+   `zero-shot-classification`

## 最佳管道使用

虽然每个任务都有一个相关的pipeline类，但使用通用的`pipeline()`函数更简单，它将所有特定任务的pipeline封装在一个对象中。`pipeline()`函数会自动加载一个默认模型和能够执行任务推理的分词器/特征提取器。

1.  通过指定推理任务来创建一个pipeline：

```py
>>> from optimum.neuron.pipelines import pipeline

>>> classifier = pipeline(task="text-classification")
```

1.  将输入文本/图像传递给`pipeline()`函数：

```py
>>> classifier("I like you. I love you.")
[{'label': 'POSITIVE', 'score': 0.9998838901519775}]
```

*注意：`pipeline()`函数中使用的默认模型未经过优化或量化，因此与它们的PyTorch对应物相比不会有性能提升。*

### 使用原始Transformers模型并转换为AWS Neuron

`pipeline()`函数接受来自[Hugging Face Hub](https://huggingface.co/models)的任何支持的模型。在Model Hub上有标签，可以用来筛选您想要用于任务的模型。

为了能够使用Neuron Runtime加载模型，需要支持将模型导出到neuron以考虑的架构。

您可以在[这里](../package_reference/configuration#supported-architectures)查看支持的架构列表。

一旦选择了合适的模型，可以通过指定模型存储库来创建`pipeline()`：

```py
>>> from optimum.neuron.pipelines import pipeline

# The model will be loaded to an NeuronModelForQuestionAnswering.
>>> neuron_qa = pipeline("question-answering", model="deepset/roberta-base-squad2", export=True)
>>> question = "What's my name?"
>>> context = "My name is Philipp and I live in Nuremberg."

>>> pred = neuron_qa(question=question, context=context)
```

还可以使用与`NeuronModelForXXX`类相关联的`from_pretrained(model_name_or_path, export=True)`方法加载它。

例如，这里是如何加载用于问答的`~neuron.NeuronModelForQuestionAnswering`类：

```py
>>> from transformers import AutoTokenizer
>>> from optimum.neuron import NeuronModelForQuestionAnswering, pipeline

>>> tokenizer = AutoTokenizer.from_pretrained("deepset/roberta-base-squad2")

>>> # Loading the PyTorch checkpoint and converting to the neuron format by providing export=True
>>> model = NeuronModelForQuestionAnswering.from_pretrained(
...     "deepset/roberta-base-squad2",
...     export=True
... )

>>> neuron_qa = pipeline("question-answering", model=model, tokenizer=tokenizer)
>>> question = "What's my name?"
>>> context = "My name is Philipp and I live in Nuremberg."

>>> pred = neuron_qa(question=question, context=context)
```

### 定义输入形状

NeuronModels目前需要静态的`input_shapes`来运行推理。如果在提供`export=True`参数时没有提供输入形状，则将使用默认输入形状。以下是如何指定序列长度和批量大小的输入形状的示例。

```py
>>> from optimum.neuron.pipelines import pipeline

>>> input_shapes = {"batch_size": 1, "sequence_length": 64} 
>>> clt = pipeline("token-classification", model="dslim/bert-base-NER", export=True,input_shapes=input_shapes)
>>> context = "My name is Philipp and I live in Nuremberg."

>>> pred = clt(context)
```
