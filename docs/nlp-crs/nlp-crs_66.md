# 精通自然语言处理

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter7/8?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter7/8?fw=pt)

            

如果你在课程中做到了这一步，恭喜你——你现在拥有了用 🤗 Transformers 和 Hugging Face 生态系统解决（几乎）任何 NLP 任务所需的所有知识和工具！

我们见过很多不同的数据整理器，所以我们制作了这个小视频来帮助您找到每个任务使用哪一个：

[`www.youtube-nocookie.com/embed/-RPeakdlHYo`](https://www.youtube-nocookie.com/embed/-RPeakdlHYo)

在完成核心 NLP 任务的快速入门后，您应该：

*   了解哪种架构（编码器、解码器或编码器-解码器）最适合每个任务
*   了解预训练和微调语言模型之间的区别
*   了解如何使用 `Trainer` API 和 🤗 Accelerate 或 TensorFlow 和 Keras 的分布式训练功能来训练 Transformer 模型，具体选择那一种方法取决于您所需要完成的任务。
*   了解 ROUGE 和 BLEU 等指标在文本生成任务中的意义和局限性
*   知道如何在 Hub 上和使用 🤗 Transformers 中的“管道”与您的微调模型进行交互

尽管掌握了所有这些知识，但总有一天你会遇到代码中的困难错误，或者对如何解决特定的 NLP 问题有疑问。幸运的是，Hugging Face 社区随时为您提供帮助！在这部分课程的最后一章中，我们将探讨如何调试 Transformer 模型并有效地寻求帮助。