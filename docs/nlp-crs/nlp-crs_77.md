# Gradio 简介

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter9/1?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter9/1?fw=pt)

           

在本章中，我们将学习如何为您的机器学习构建**交互式演示**模型。

为什么首先要为您的机器学习模型构建演示或 GUI？演示可以带来：

*   **机器学习开发人员**可以轻松地向包括非技术团队或客户在内的广大受众展示他们的工作
*   **研究人员**更轻松地重现机器学习模型和行为
*   **质量测试人员**或**最终用户**更容易识别和调试模型的故障点
*   **不同的用户**发现模型中的算法偏差

我们将使用 Gradio 库为我们的模型构建演示。 Gradio 允许您完全使用 Python 为任何机器学习模型构建、自定义和共享基于 Web 的演示。

以下是一些使用 Gradio 构建的机器学习演示示例：

*   一个**草图识别**模型，它接收草图并输出它认为正在绘制的标签：

[`course-demos-draw2.hf.space`](https://course-demos-draw2.hf.space)

*   一个抽取式**问题回答**模型，它接受上下文段落和一个任务并输出一个结果和一个概率分数（我们在第七章中讨论了这种模型）：

[`course-demos-question-answering-simple.hf.space`](https://course-demos-question-answering-simple.hf.space)

*   一个**背景去除**模型，它接收图像并输出去除背景的图像：

[`course-demos-remove-bg-original.hf.space`](https://course-demos-remove-bg-original.hf.space)

本章分为两个部分，包括*概念*和*应用程序*。在您了解每个部分的概念后，您将应用它来构建特定类型的演示，范围从图像分类到语音识别。当你读完本章时，你将能够用几行 Python 代码构建这些演示（以及更多！）。

👀 点击 [Hugging Face Spaces](https://huggingface.co/spaces) 以查看机器学习社区构建的许多机器学习演示的最新示例！