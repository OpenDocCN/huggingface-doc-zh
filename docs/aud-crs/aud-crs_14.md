# 第 2 单元：音频应用的入门介绍

> 原文：[`huggingface.co/learn/audio-course/zh-CN/chapter2/introduction`](https://huggingface.co/learn/audio-course/zh-CN/chapter2/introduction)

           

欢迎来到 Hugging Face 音频课程的第二单元！我们已经掌握了音频数据的基础，以及使用🤗 Datasets 和 🤗 Transformers 加载音频数据的方法。我们已经了解了许多音频相关的概念，例如采样率、位深度、波形、时频谱等等。我们也已经了解到如何对数据进行预处理，并为模型的预训练准备数据。

现在，你也许会想了解🤗 Transformers 能处理怎样的任务，并且你已经掌握了所有相关的基础知识！我们来看看这些令人震惊的音频应用吧：

*   **声音分类**（Audio classification）：将音频片段分入不同的类别。我们可以认出该音频是汪汪的狗叫声、喵喵的猫叫声，或者某个特定的音乐风格。
*   **自动语音识别**（Automatic speech recognition, ASR）：将语音片段自动转录成文字。你可以将一段对话自动转化为文字稿，例如一段“你今天怎么样？”的录音。对于做笔记会很有用！
*   **说话人识别**（Speaker diarization）：想知道录音里在说话的人是谁吗？我们可以利用🤗 Transformers 来识别出一段录音中正在说话的人的身份，例如在一段对话的录音中识别出 Alice 和 Bob 各自说话的部分。
*   **由文字生成语音**（Text to speech, TTS）：由一段文字合成其对应的语音，例如生成一本有声书，语音助手，或者给游戏中的 NPC 配音。利用🤗 Transformers，你可以轻松地做到！

在本单元中，你会学到如何使用🤗 Transformers 的`pipeline()`函数加载预训练模型来完成上述的一些任务。我们将重点介绍如何使用预训练模型完成声音分类和自动语音识别任务。让我们开始吧！