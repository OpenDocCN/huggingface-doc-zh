# 欢迎来到 Hugging Face 音频课程！

> 原文：[`huggingface.co/learn/audio-course/zh-CN/chapter0/introduction`](https://huggingface.co/learn/audio-course/zh-CN/chapter0/introduction)

           

屏幕前的同学，

欢迎来到本课！我们将为你介绍 Transformer 模型在音频领域的应用。Transformer 模型已经多次证明了自己是最强大、多功能的深度学习模型之一，并在自然语言处理（Natural Language Processing, NLP）、机器视觉（Computer Vision, CV）、和最近的音频处理等等任务上取得了最佳成绩。

在本课中，我们会学习如何将 Trasnfromer 结构应用到音频数据上。你会学到如何使用 Transformer 解决各种各样与音频有关的任务。无论你是对自动语音识别（Automatic Speech Recognition, ASR）、音频分类（Audio CLassification）、或是由文字生成语音（Text-to-Speech synthesis， TTS）感兴趣，本课和 Transformer 都会成为你的得力助手。

让我们直观地感受一下这些模型的威力吧！在下面的 demo 里随便说些什么，然后看看我们的模型是如何实时地将语音记录成文字的（仅支持英语）：

[`openai-whisper.hf.space`](https://openai-whisper.hf.space)

在本课中，你会学习到针对音频数据的特殊处理方法、各种不同的 Transformer 架构、以及使用强大的预训练模型来训练你自己的音频 Transformer。

本课是为有一定深度学习基础，并对 Transformer 有一些初步了解的同学而设计。你不需要对音频数据处理有任何经验。如果你需要加强自己对 Transformer 结构的基础了解，请参考我们的[自然语言处理课程](https://huggingface.co/learn/nlp-course/zh-CN/chapter1/1)。这门课程包含了 Transformer 结构的基础知识和更多细节。

## 关于作者

**Sanchit Gandhi, Hugging Face 机器学习科研工程师**

嗨! 我是 Sanchit，来自 Hugging Face🤗开源组的的音频机器学习研发工程师。我的主要方向是自动语音识别和（automatic speech recognition,ASR）和自动语音翻译。我现在的工作方向是构建更快、更轻、更易用的语音模型。

**Matthijs Hollemans, Hugging Face 机器学习工程师**

我是 Matthijs, Hugging Face 开源组的音频机器学习工程师。我也是《how to write sound synthesizers》（如何编写音频合成器）一书的作者。我喜欢在业余时间编写音频插件。

**Maria Khalusova, Hugging Face 文档和课程经理**

我是 Maria，我的主要工作是制作教学内容和文档，使我们的 Transformers 库和其他开源工具更加易用。我会解构复杂的技术概念，并且帮助大家从零开始理解最前沿的技术。

**Vaibhav Srivastav, Hugging Face 机器学习开发者大使**

我是 Vaibhav (VB)，Hugging Face 开源组的开发者大使（Developer Advocate）。我的研究方向是低资源 TTS 以及尖端语音研究的大众化推进。

## 课程结构

本课程被分为多个单元，涵盖了众多方向的深度议题：

*   第 1 单元：学习音频数据的特殊性，包括音频信号处理技巧和数据预处理。
*   第 2 单元：了解音频机器学习的应用，使用🤗Transformers pipelines 实现不同任务，包括音频分类（Audio classification）和语音识别（speech recognition）。
*   第 3 单元：探索各种音频 Transformer 架构，了解他们的区别和各自的优势任务。
*   第 4 单元：学习如何构建你自己的音乐风格分类器（music genre classifier）
*   第 5 单元：深入学习语音识别（speech recognition），搭建一个会议转录模型。
*   第 6 单元：学习如何从文字生成语音。
*   第 7 单元：学习利用 Transformer 进行音频到音频转换（audio to audio）。

每个单元都包含了理论部分，对模型背后的概念和技巧做了深入的极少。本课程的各个章节都包含了课后习题，用来测试和强化你的学习成果。部分单元还包含了实战练习。

完成本课程的学习后，你会在 Transformer 和音频数据上拥有一个扎实的基础，并且会学习到针对多种音频相关任务的工具，帮助你快速开发应用。

本课程的各个单元会按照如下的时间表发布：（译注：此表为英文版课程发布时间）

| 单元 | 发布日期 |
| --- | --- |
| 第 0、1、2 单元 | 2023 年 6 月 14 日 |
| 第 3、4 单元 | 2023 年 6 月 21 日 |
| 第 5 单元 | 2023 年 6 月 28 日 |
| 第 6 单元 | 2023 年 7 月 5 日 |
| 第 7、8 单元 | 2023 年 7 月 12 日 |

## 学习方法和证书

你可以用任何方式来利用这门课程。本课程的所有内容均为 100%免费、公开且开源。你可以按照自己喜欢的节奏来学习本课程，不过我们建议按照顺序来阅读各个单元。

如果你想要获取结课证明，我们提供两种不同的选项：

| 证书类别 | 要求 |
| --- | --- |
| 结课证书 | 在 2023 年 7 月底前，根据指引完成 80%的实战练习内容。 |
| 荣誉证书 | 在 2023 年 7 月底前，根据指引完成 100%的实战练习内容。 |

每个单元的实战练习内容均有各自的完成标准。当你完成了任意类别的证书要求后，请按照最后一个单元的指引来获取你的结课证明。祝你好运！

## 注册本课程

本课程的不同单元会在数周内逐渐更新。我们建议你注册课程更新通知，以防错过新内容的更新。注册课程的同学还能第一时间了解到我们举办的特殊活动。

[注册链接（仅英文）](http://eepurl.com/insvcI)

祝你学习愉快！