# 微调，检查!

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter3/5?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter3/5?fw=pt)

              Pytorch TensorFlow![Ask a Question](https://discuss.huggingface.co/t/chapter-3-questions)

这是非常令人高兴的！ 在前两章中，您了解了模型和标记器(tokenizer)，现在您知道如何针对您自己的数据对它们进行微调。回顾一下，在本章中，您：

*   了解了[Hub](https://huggingface.co/datasets)中的数据集
*   学习了如何加载和预处理数据集，包括使用动态填充和整理器
*   实现您自己的模型微调和评估
*   实施了一个较为底层的训练循环
*   使用 🤗 Accelerate 轻松调整您的训练循环，使其适用于多个 GPU 或 TPU