# 实战练习

> 原文：[`huggingface.co/learn/audio-course/zh-CN/chapter2/hands_on`](https://huggingface.co/learn/audio-course/zh-CN/chapter2/hands_on)

           

本节的练习不计入课程成绩，旨在帮助你熟悉本课程中将要使用的工具和库。如果你已经熟悉使用 Google Colab、🤗 Datasets、librosa 和 🤗 Transformers，你可以选择跳过本节练习。

1.  创建一个 [Google Colab](https://colab.research.google.com) 笔记本。
2.  使用 🤗 Datasets 加载 [`facebook/voxpopuli` 数据集](https://huggingface.co/datasets/facebook/voxpopuli) 的训练集，语言任选，使用流式加载模式。
3.  获取数据集`train`部分的第三个样本并探索它。根据这个样本所拥有的特征，你可以用这个数据集做哪些音频任务？
4.  绘制这个样本的波形图和频谱图。
5.  前往 [🤗 Hub](https://huggingface.co/models)，探索预训练模型并找到一个可以用于你之前选择的语言的自动语音识别的模型。使用你找到的模型实例化一个对应的 pipeline，并转录这个样本。
6.  将你从 pipeline 得到的转录与样本的真实转录文字进行比较。

如果你在这个练习中遇到了问题，可以参考[示例解决方案](https://colab.research.google.com/drive/1NGyo5wFpRj8TMfZOIuPaJHqyyXCITftc?usp=sharing)。你发现了什么有趣的东西？找到了一个很酷的模型？得到了一个漂亮的频谱图？欢迎在 Twitter 上分享你的工作和发现！

在接下来的章节中，你会学习到更多关于各种音频 Transformer 架构的知识，并训练你自己的模型！