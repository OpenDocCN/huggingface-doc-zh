# 本章简介

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter3/1?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter3/1?fw=pt)

              Pytorch TensorFlow![Ask a Question](https://discuss.huggingface.co/t/chapter-3-questions)

在 第二章 我们探索了如何使用标记器(Tokenizer)和预训练模型进行预测。但是，如果您想为自己的数据集微调预训练模型，该怎么做呢？这就是本章的主题！你将学到:

*   如何从模型中心(hub)准备大型数据集
*   如何使用高级`训练`API 微调一个模型
*   如何使用自定义训练过程
*   如何利用🤗 Accelerate 库在任何分布式设备上轻松运行自定义训练过程

为了将经过训练的参数上传到 Hugging Face Hub，您需要一个 huggingface.co 帐户: [创建一个账户](https://huggingface.co/join)