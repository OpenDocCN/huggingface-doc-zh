# DreamBooth

> 原文链接：[https://huggingface.co/docs/autotrain/dreambooth](https://huggingface.co/docs/autotrain/dreambooth)

DreamBooth是一种个性化文本到图像模型的方法，类似于Stable Diffusion，只需提供主题的几张（3-5张）图像。它允许模型生成主题在不同场景、姿势和视角下的上下文化图像。

## 数据准备

DreamBooth训练的数据格式很简单。您只需要有一个概念（例如一个人）的图像和一个概念标记。

训练DreamBooth模型，请从hub中选择一个合适的模型。在选择hub中的模型时，请确保选择与模型兼容的正确图像大小。

在参数部分，您的概念标记是`prompt`。
