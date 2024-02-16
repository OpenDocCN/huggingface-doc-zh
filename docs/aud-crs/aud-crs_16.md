# 利用 pipeline 进行自动语音识别

> 原文：[`huggingface.co/learn/audio-course/zh-CN/chapter2/asr_pipeline`](https://huggingface.co/learn/audio-course/zh-CN/chapter2/asr_pipeline)

            

自动语音识别（Automatic Speech Recognition, ASR）是一个将录音转录为文字的任务。该任务有许多非常实用的应用，包括生成视频字幕、语音助手（Siri、Alexa 等）的语音指令功能等等。

本小节中，我们将使用`automatic-speech-recognition` pipeline 来转录 MINDS-14 数据集中一段用户询问账单信息的录音。

首先，我们根据利用 pipeline 进行音频分类一节的方法加载数据并上采样至 16 千赫兹。

接下来，我们利用🤗 Transformers 中的`automatic-speech-recognition` pipeline 来构建自动语音识别的任务：

```py
from transformers import pipeline

asr = pipeline("automatic-speech-recognition")
```

下面，我们从数据集中选取一个样本，并将该样本的原始音频数据传递给 pipeline：

```py
example = minds[0]
asr(example["audio"]["array"])
```

**输出**

```py
{"text": "I WOULD LIKE TO PAY MY ELECTRICITY BILL USING MY COD CAN YOU PLEASE ASSIST"}
```

我们可以将模型转录的结果和该样本标签中真实的转录文字进行对比：

```py
example["english_transcription"]
```

**输出：**

```py
"I would like to pay my electricity bill using my card can you please assist"
```

看起来模型在文字转录上的效果还不错！和原始转录相比，仅有一个词（card）转录错误。考虑到用户使用的澳大利亚方言中“r”通常不发音，这个结果已经很不错了。 不过我并不推荐用一条鱼来支付你的下一张电费账单就是了！（译注：cod 为鳕鱼）

在默认条件下，该 pipeline 使用的是一个针对英语语言的自动语音识别模型，这在本例中是可以的。如果你想尝试转录 MINDS-14 数据集中其他语言的子集，你可以在[🤗 Hub](https://huggingface.co/models?pipeline_tag=automatic-speech-recognition&language=fr&sort=downloads)上找到一个预训练的 ASR 模型。你可以首先按任务过滤模型列表，然后再按语言过滤。一旦你找到了喜欢的模型，将其名称作为`model`参数传递给 pipeline 即可。

让我们尝试一下 MINDS-14 数据集中德语子集的转录。首先加载`de-DE`子集：

```py
from datasets import load_dataset
from datasets import Audio

minds = load_dataset("PolyAI/minds14", name="de-DE", split="train")
minds = minds.cast_column("audio", Audio(sampling_rate=16_000))
```

接下来，我们获取一个样本并查看其转录：

```py
example = minds[0]
example["transcription"]
```

**输出**

```py
"ich möchte gerne Geld auf mein Konto einzahlen"
```

接下来，我们在🤗 Hub 上找到一个针对德语语言的预训练 ASR 模型，然后将其名称传递给 pipeline，并对样本进行转录：

```py
from transformers import pipeline

asr = pipeline("automatic-speech-recognition", model="maxidl/wav2vec2-large-xlsr-german")
asr(example["audio"]["array"])
```

**输出**

```py
{"text": "ich möchte gerne geld auf mein konto einzallen"}
```

德语也没问题！

当你在解决自己的任务时，从本单元中展示的简单 pipeline 开始是一个非常有价值的工具，它有以下几个好处：

*   预训练模型可能已经能够很好地解决你的任务，从而节省了大量时间
*   `pipeline()`会为你进行所有的预处理和后处理操作，因此你不必再花时间将数据转换为模型正确的格式
*   如果结果不理想，pipeline 仍然可以为你提供一个快速实现的基线，以便在未来对其进行微调
*   如果你在自己的数据集上微调了一个模型并将其分享到 Hub 上，整个社区都可以通过`pipeline()`方法快速轻松地使用它，从而使 AI 更加易于使用。