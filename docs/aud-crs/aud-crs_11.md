# Check your understanding of the course material

> 原文：[`huggingface.co/learn/audio-course/zh-CN/chapter1/quiz`](https://huggingface.co/learn/audio-course/zh-CN/chapter1/quiz)

             

### 1\. 采样率使用的单位是？

### 2\. 使用流式加载模式加载大规模数据集时，何时才能开始使用该数据集？

### 3\. 什么是时频谱？

### 4\. 下列方法中，最简单的可以将原始音频转化为 Whisper 模型接受的对数梅尔谱的方法是？

A.

```py
librosa.feature.melspectrogram(audio["array"])
```

B.

```py
feature_extractor = WhisperFeatureExtractor.from_pretrained("openai/whisper-small")
feature_extractor(audio["array"])
```

C.

```py
dataset.feature(audio["array"], model="whisper")
```

### 5\. 如何从🤗 Hub 加载数据集？

A.

```py
from datasets import load_dataset

dataset = load_dataset(DATASET_NAME_ON_HUB)
```

B.

```py
import librosa

dataset = librosa.load(PATH_TO_DATASET)
```

C.

```py
from transformers import load_dataset

dataset = load_dataset(DATASET_NAME_ON_HUB)
```

### 6\. 你的自定义数据集包含了 32 千赫兹采样率的高清音频。你现在想要训练一个使用 16 千赫兹采样率的语音识别模型。你应该怎么做？

### 7\. 如何将机器学习模型生成的时频谱转化为波形？