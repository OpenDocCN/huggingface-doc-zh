# Transformers 能做什么？

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter1/3?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter1/3?fw=pt)

               ![Ask a Question](https://discuss.huggingface.co/t/chapter-1-questions) ![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/zh-CN/chapter1/section3.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/zh-CN/chapter1/section3.ipynb)

在本节中，我们将看看 Transformer 模型可以做什么，并使用 🤗 Transformers 库中的第一个工具：pipeline() 函数。

👀 看到那个右上角的 *在 Colab 中打开*的按钮了吗? 单击它就可以打开一个包含本节所有代码示例的 Google Colab 笔记本。 每一个有实例代码的小节都会有它。

如果您想在本地运行示例，我们建议您查看准备.

## Transformer 被应用于各个方面！

Transformer 模型用于解决各种 NLP 任务，就像上一节中提到的那样。以下是一些使用 Hugging Face 和 Transformer 模型的公司和组织，他们也通过分享他们的模型回馈社区：

![使用 Hugging Face 的公司](img/861461d0a1d6503840cfb7a4140cac07.png) [🤗 Transformers 库](https://github.com/huggingface/transformers)提供了创建和使用这些共享模型的功能。[模型中心（hub）](https://huggingface.co/models)包含数千个任何人都可以下载和使用的预训练模型。您还可以将自己的模型上传到 Hub！

⚠️ Hugging Face Hub 不限于 Transformer 模型。任何人都可以分享他们想要的任何类型的模型或数据集！创建一个 Huggingface.co 帐户(https://huggingface.co/join)以使用所有可用功能！

在深入研究 Transformer 模型的底层工作原理之前，让我们先看几个示例，看看它们如何用于解决一些有趣的 NLP 问题。

## 使用 pipelines

[`www.youtube.com/embed/tiZFewofSLM`](https://www.youtube.com/embed/tiZFewofSLM)

(这里有一个视频，但是国内可能打不开,译者注)

🤗 Transformers 库中最基本的对象是 **pipeline()** 函数。它将模型与其必要的预处理和后处理步骤连接起来，使我们能够通过直接输入任何文本并获得最终的答案：

```py
from transformers import pipeline

classifier = pipeline("sentiment-analysis")
classifier("I've been waiting for a HuggingFace course my whole life.")
```

```py
[{'label': 'POSITIVE', 'score': 0.9598047137260437}]
```

我们也可以多传几句！

```py
classifier(
    ["I've been waiting for a HuggingFace course my whole life.", "I hate this so much!"]
)
```

```py
[{'label': 'POSITIVE', 'score': 0.9598047137260437},
 {'label': 'NEGATIVE', 'score': 0.9994558095932007}]
```

默认情况下，此 pipeline 选择一个特定的预训练模型，该模型已针对英语情感分析进行了微调。创建**分类器**对象时，将下载并缓存模型。如果您重新运行该命令，则将使用缓存的模型，无需再次下载模型。

将一些文本传递到 pipeline 时涉及三个主要步骤：

1.  文本被预处理为模型可以理解的格式。
2.  预处理的输入被传递给模型。
3.  模型处理后输出最终人类可以理解的结果。

目前[可用的一些 pipeline](https://huggingface.co/transformers/main_classes/pipelines.html)是：

*   **特征提取**（获取文本的向量表示）
*   **填充空缺**
*   **ner**（命名实体识别）
*   **问答**
*   **情感分析**
*   **文本摘要**
*   **文本生成**
*   **翻译**
*   **零样本分类**

让我们来看看其中的一些吧！

## 零样本分类

我们将首先处理一项非常具挑战性的任务，我们需要对尚未标记的文本进行分类。这是实际项目中的常见场景，因为注释文本通常很耗时并且需要领域专业知识。对于这项任务**zero-shot-classification**pipeline 非常强大：它允许您直接指定用于分类的标签，因此您不必依赖预训练模型的标签。下面的模型展示了如何使用这两个标签将句子分类为正面或负面——但也可以使用您喜欢的任何其他标签集对文本进行分类。

```py
from transformers import pipeline

classifier = pipeline("zero-shot-classification")
classifier(
    "This is a course about the Transformers library",
    candidate_labels=["education", "politics", "business"],
)
```

```py
{'sequence': 'This is a course about the Transformers library',
 'labels': ['education', 'business', 'politics'],
 'scores': [0.8445963859558105, 0.111976258456707, 0.043427448719739914]}
```

此 pipeline 称为 zero-shot，因为您不需要对数据上的模型进行微调即可使用它。它可以直接返回您想要的任何标签列表的概率分数！

✏️**快来试试吧！**使用您自己的序列和标签，看看模型的行为。

## 文本生成

现在让我们看看如何使用 pipeline 来生成一些文本。这里的主要使用方法是您提供一个提示，模型将通过生成剩余的文本来自动完成整段话。这类似于许多手机上的预测文本功能。文本生成涉及随机性，因此如果您没有得到相同的如下所示的结果，这是正常的。

```py
from transformers import pipeline

generator = pipeline("text-generation")
generator("In this course, we will teach you how to")
```

```py
[{'generated_text': 'In this course, we will teach you how to understand and use '
                    'data flow and data interchange when handling user data. We '
                    'will be working with one or more of the most commonly used '
                    'data flows — data flows of various types, as seen by the '
                    'HTTP'}]
```

您可以使用参数 **num_return_sequences** 控制生成多少个不同的序列，并使用参数 **max_length** 控制输出文本的总长度。

✏️**快来试试吧！**使用 num_return_sequences 和 max_length 参数生成两个句子，每个句子 15 个单词。

## 在 pipeline 中使用 Hub 中的其他模型

前面的示例使用了默认模型，但您也可以从 Hub 中选择特定模型以在特定任务的 pipeline 中使用 - 例如，文本生成。转到[模型中心（hub）](https://huggingface.co/models)并单击左侧的相应标签将会只显示该任务支持的模型。[例如这样](https://huggingface.co/models?pipeline_tag=text-generation)。

让我们试试 [**distilgpt2**](https://huggingface.co/distilgpt2) 模型吧！以下是如何在与以前相同的 pipeline 中加载它：

```py
from transformers import pipeline

generator = pipeline("text-generation", model="distilgpt2")
generator(
    "In this course, we will teach you how to",
    max_length=30,
    num_return_sequences=2,
)
```

```py
[{'generated_text': 'In this course, we will teach you how to manipulate the world and '
                    'move your mental and physical capabilities to your advantage.'},
 {'generated_text': 'In this course, we will teach you how to become an expert and '
                    'practice realtime, and with a hands on experience on both real '
                    'time and real'}]
```

您可以通过单击语言标签来筛选搜索结果，然后选择另一种文本生成模型的模型。模型中心（hub）甚至包含支持多种语言的多语言模型。

通过单击选择模型后，您会看到有一个小组件，可让您直接在线试用。通过这种方式，您可以在下载之前快速测试模型的功能。

✏️**快来试试吧！**使用标签筛选查找另一种语言的文本生成模型。使用小组件测试并在 pipeline 中使用它！

## 推理 API

所有模型都可以使用 Inference API 直接通过浏览器进行测试，该 API 可在 [Hugging Face 网站](https://huggingface.co/)上找到。通过输入自定义文本并观察模型的输出，您可以直接在此页面上使用模型。

小组件形式的推理 API 也可作为付费产品使用，如果您的工作流程需要它，它会派上用场。有关更多详细信息，请参阅[定价页面](https://huggingface.co/pricing)。

## Mask filling

您将尝试的下一个 pipeline 是 **fill-mask**。此任务的想法是填充给定文本中的空白：

```py
from transformers import pipeline

unmasker = pipeline("fill-mask")
unmasker("This course will teach you all about <mask> models.", top_k=2)
```

```py
[{'sequence': 'This course will teach you all about mathematical models.',
  'score': 0.19619831442832947,
  'token': 30412,
  'token_str': ' mathematical'},
 {'sequence': 'This course will teach you all about computational models.',
  'score': 0.04052725434303284,
  'token': 38163,
  'token_str': ' computational'}]
```

**top_k** 参数控制要显示的结果有多少种。请注意，这里模型填充了特殊的< **mask** >词，它通常被称为掩码标记。其他掩码填充模型可能有不同的掩码标记，因此在探索其他模型时要验证正确的掩码字是什么。检查它的一种方法是查看小组件中使用的掩码。

✏️**快来试试吧！**在 Hub 上搜索基于 bert 的模型并在推理 API 小组件中找到它的掩码。这个模型对上面 pipeline 示例中的句子预测了什么？

## 命名实体识别

命名实体识别 (NER) 是一项任务，其中模型必须找到输入文本的哪些部分对应于诸如人员、位置或组织之类的实体。让我们看一个例子：

```py
from transformers import pipeline

ner = pipeline("ner", grouped_entities=True)
ner("My name is Sylvain and I work at Hugging Face in Brooklyn.")
```

```py
[{'entity_group': 'PER', 'score': 0.99816, 'word': 'Sylvain', 'start': 11, 'end': 18}, 
 {'entity_group': 'ORG', 'score': 0.97960, 'word': 'Hugging Face', 'start': 33, 'end': 45}, 
 {'entity_group': 'LOC', 'score': 0.99321, 'word': 'Brooklyn', 'start': 49, 'end': 57}
]
```

在这里，模型正确地识别出 Sylvain 是一个人 (PER)，Hugging Face 是一个组织 (ORG)，而布鲁克林是一个位置 (LOC)。

我们在 pipeline 创建函数中传递选项 **grouped_entities=True** 以告诉 pipeline 将对应于同一实体的句子部分重新组合在一起：这里模型正确地将“Hugging”和“Face”分组为一个组织，即使名称由多个词组成。事实上，正如我们即将在下一章看到的，预处理甚至会将一些单词分成更小的部分。例如，**Sylvain** 分割为了四部分：**S、##yl、##va** 和 **##in**。在后处理步骤中，pipeline 成功地重新组合了这些部分。

✏️**快来试试吧！**在模型中心（hub）搜索能够用英语进行词性标注（通常缩写为 POS）的模型。这个模型对上面例子中的句子预测了什么？

## 问答系统

问答 pipeline 使用来自给定上下文的信息回答问题：

```py
from transformers import pipeline

question_answerer = pipeline("question-answering")
question_answerer(
    question="Where do I work?",
    context="My name is Sylvain and I work at Hugging Face in Brooklyn",
)
```

```py
{'score': 0.6385916471481323, 'start': 33, 'end': 45, 'answer': 'Hugging Face'}
klyn",
) 
```

请注意，此 pipeline 通过从提供的上下文中提取信息来工作；它不会凭空生成答案。

## 文本摘要

文本摘要是将文本缩减为较短文本的任务，同时保留文本中的主要（重要）信息。下面是一个例子：

```py
from transformers import pipeline

summarizer = pipeline("summarization")
summarizer(
    """
    America has changed dramatically during recent years. Not only has the number of 
    graduates in traditional engineering disciplines such as mechanical, civil, 
    electrical, chemical, and aeronautical engineering declined, but in most of 
    the premier American universities engineering curricula now concentrate on 
    and encourage largely the study of engineering science. As a result, there 
    are declining offerings in engineering subjects dealing with infrastructure, 
    the environment, and related issues, and greater concentration on high 
    technology subjects, largely supporting increasingly complex scientific 
    developments. While the latter is important, it should not be at the expense 
    of more traditional engineering.

    Rapidly developing economies such as China and India, as well as other 
    industrial countries in Europe and Asia, continue to encourage and advance 
    the teaching of engineering. Both China and India, respectively, graduate 
    six and eight times as many traditional engineers as does the United States. 
    Other industrial countries at minimum maintain their output, while America 
    suffers an increasingly serious decline in the number of engineering graduates 
    and a lack of well-educated engineers.
"""
)
```

```py
[{'summary_text': ' America has changed dramatically during recent years . The '
                  'number of engineering graduates in the U.S. has declined in '
                  'traditional engineering disciplines such as mechanical, civil '
                  ', electrical, chemical, and aeronautical engineering . Rapidly '
                  'developing economies such as China and India, as well as other '
                  'industrial countries in Europe and Asia, continue to encourage '
                  'and advance engineering .'}]
```

与文本生成一样，您指定结果的 **max_length** 或 **min_length**。

## 翻译

对于翻译，如果您在任务名称中提供语言对（例如“**translation_en_to_fr**”），则可以使用默认模型，但最简单的方法是在[模型中心（hub）](https://huggingface.co/models)选择要使用的模型。在这里，我们将尝试从法语翻译成英语：

```py
from transformers import pipeline

translator = pipeline("translation", model="Helsinki-NLP/opus-mt-fr-en")
translator("Ce cours est produit par Hugging Face.")
```

```py
[{'translation_text': 'This course is produced by Hugging Face.'}]

```

与文本生成和摘要一样，您可以指定结果的 **max_length** 或 **min_length**。

✏️**快来试试吧！**搜索其他语言的翻译模型，尝试将前一句翻译成几种不同的语言。

到目前为止显示的 pipeline 主要用于演示目的。它们是为特定任务而编程的，不能对他们进行自定义的修改。在下一章中，您将了解 **pipeline()** 函数内部的内容以及如何进行自定义的修改。