# 本章简介

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter6/1?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter6/1?fw=pt)

             ![Ask a Question](https://discuss.huggingface.co/t/chapter-6-questions)

在 [第三章] (/course/chapter3) 中，我们研究了如何在给定任务上微调模型。 当我们这样做时，我们需要使用与模型预训练相同的标记器——但是当我们想从头开始训练模型时该怎么办？ 不过，使用在来自其他领域或语言的语料库上预训练的标记器通常不是最理想的。 例如，在英语语料库上训练的标记器在日语文本语料库上表现不佳，因为两种语言中空格和标点符号的使用非常不同。

在本章中，您将学习如何在文本语料库上训练一个全新的标记器，然后将其用于预训练语言模型。 这一切都将在 [🤗 Tokenizers](https://github.com/huggingface/tokenizers) 库的帮助下完成，该库在 🤗 Transformers /huggingface/transformers) 库之内。 我们将仔细研究这个库提供的功能，并探讨快速标记器与“慢”版本的区别。

我们将涵盖的主题包括：

*   如何训练一个新的标记器，类似于给定检查点在新的文本语料库上使用的标记器
*   快速标记器的特殊功能
*   目前 NLP 中使用的三种主要子词标记化算法之间的差异
*   如何使用🤗 Tokenizers 库从头开始构建标记器并在一些数据上对其进行训练

本章介绍的技术将使您为 第七章 中的部分做好准备，在那部分中，我们着眼于为 Python 源代码创建语言模型。 让我们首先看一下什么是“训练”标记器？