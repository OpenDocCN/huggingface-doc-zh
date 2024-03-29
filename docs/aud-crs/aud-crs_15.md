# 利用 pipeline 进行音频分类

> 原文：[`huggingface.co/learn/audio-course/zh-CN/chapter2/audio_classification_pipeline`](https://huggingface.co/learn/audio-course/zh-CN/chapter2/audio_classification_pipeline)

            

音频分类任务需要根据音频的内容对其进行单个和多个标签的识别。这些标签可以代表不同的声音类别，如音乐、语音或噪音，也可以代表更细分的分类，如鸟叫声和引擎声等。

在深入了解音频 Transformer 的细节和微调（fine-tuning）之前，我们先来看看如何使用🤗 Transformers 和现成的预训练模型，在几行代码中构建一个音频分类器。

我们使用和前几个单元相同的[MINDS-14](https://huggingface.co/datasets/PolyAI/minds14) 数据集。回忆一下，MINDS-14 数据集的内容是许多不同语言和方言的使用者向一个网银系统提问的录音。其标签包含了`intent_class`，记录了该次对话的目的。dataset that you have explored

和之前一样，我们先加载`en-AU`子集来测试我们的 pipeline，将其上采样至音频模型常见的 16 千赫兹采样率。

```py
from datasets import load_dataset
from datasets import Audio

minds = load_dataset("PolyAI/minds14", name="en-AU", split="train")
minds = minds.cast_column("audio", Audio(sampling_rate=16_000))
```

我们可以使用🤗 Transformers 中的`audio-classification` pipeline 来构建一个将音频分入一个类别集的任务。在我们的设定下，我们需要一个已经针对音频的目的分类在 MINDS-14 数据集上微调过的模型。运气好的是，在🤗 Hub 中已经有了一个这样的模型！让我们使用`pipeline()`函数来加载该模型：

```py
from transformers import pipeline

classifier = pipeline(
    "audio-classification",
    model="anton-l/xtreme_s_xlsr_300m_minds14",
)
```

这个 pipeline 需要以 NumPy 数组形式输入的音频数据。对于原始音频数据的预处理操作则已经包含在了该 pipeline 中。让我们拿一个样本来测试一下：

```py
example = minds[0]
```

回忆一下数据集的结构，我们的原始音频数据是以 NumPy 数组的形式存储在`["audio"]["array"]`中的。我们将其直接传递给`classifier`:

```py
classifier(example["audio"]["array"])
```

**输出：**

```py
[
    {"score": 0.9631525278091431, "label": "pay_bill"},
    {"score": 0.02819698303937912, "label": "freeze"},
    {"score": 0.0032787492964416742, "label": "card_issues"},
    {"score": 0.0019414445850998163, "label": "abroad"},
    {"score": 0.0008378693601116538, "label": "high_value_payment"},
]
```

模型非常确信，在这个样本中，顾客的目的是了解关于账单的信息。让我们来看看该样本的真实标签：

```py
id2label = minds.features["intent_class"].int2str
id2label(example["intent_class"])
```

**输出**

```py
"pay_bill"
```

好耶！模型预测正确！我们很幸运地找到了一个预测标签和我们需要的标签完全相同的预训练模型。很多时候，我们所找的预训练模型可以预测的标签集和我们所需要的标签集不会完全相同。这种情况下，我们需要对该预训练模型进行微调（fine-tuning），来将其“校准”到我们需要的标签集上。我们在接下来的单元中会学到微调相关的知识。现在，我们来看看另一个语音处理中常见的任务，*自动语音识别*。