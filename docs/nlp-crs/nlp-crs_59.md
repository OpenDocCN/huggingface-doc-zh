# 章节简介

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter7/1?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter7/1?fw=pt)

             Pytorch TensorFlow

在第三章，您了解了如何微调文本分类的模型。在本章中，我们将处理以下常见的 NLP 任务：

*   词元（token）分类
*   掩码语言建模（如 BERT）
*   文本摘要
*   翻译
*   因果语言建模预训练（如 GPT-2）
*   问答

为此，您需要利用第三章中学到的 `Trainer` API 和 🤗 Accelerate 库、第五章中的 🤗 Datasets 库以及第六章中的 🤗 Tokenizers 库的所有知识。我们同样会将结果上传到模型中心，就像我们在第四章中所做的那样，所以这确实是融会贯通的一章！

每个部分都可以独立阅读，并将向您展示如何使用 `Trainer` API 或按照您自己的训练循环训练模型，并采用 🤗 Accelerate 加速。你可以随意跳过任何一部分，专注于您最感兴趣的部分：`Trainer` API 非常适用于微调（fine-tuning）或训练您的模型，且无需担心幕后发生的事情；而采用 `Accelerate` 的训练循环可以让您更轻松地自定义所需的任何结构。

如果您按顺序阅读这些部分，您会注意到它们有很多共同的代码和陈述。 重复是有意为之的，让您可以深入（或稍后返回）任何您感兴趣的任务并找到一个完整的工作示例。