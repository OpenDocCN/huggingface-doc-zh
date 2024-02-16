# 本章简介

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter2/1?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter2/1?fw=pt)

              ![Ask a Question](https://discuss.huggingface.co/t/chapter-2-questions)

正如你在 Chapter 1,中看到的那样，Transformers 模型通常非常大。对于数以百万计到数千万计数十亿的参数，训练和部署这些模型是一项复杂的任务。此外，由于几乎每天都在发布新模型，而且每种模型都有自己的实现，因此尝试它们绝非易事。

创建🤗 Transformers 库就是为了解决这个问题。它的目标是提供一个 API，通过它可以加载、训练和保存任何 Transformer 模型。这个库的主要特点是：

*   **易于使用**：下载、加载和使用最先进的 NLP 模型进行推理只需两行代码即可完成。
*   **灵活**：所有型号的核心都是简单的 PyTorch **nn.Module** 或者 TensorFlow **tf.kears.Model**，可以像它们各自的机器学习（ML）框架中的任何其他模型一样进行处理。
*   **简单**：当前位置整个库几乎没有任何摘要。“都在一个文件中”是一个核心概念：模型的正向传递完全定义在一个文件中，因此代码本身是可以理解的，并且是可以破解的。

最后一个特性使🤗 Transformers 与其他 ML 库截然不同。这些模型不是基于通过文件共享的模块构建的；相反，每一个模型都有自己的菜单。除了使模型更加容易接受和更容易理解，这还允许你轻松地在一个模型上实验，而且不影响其他模型。

本章将从一个端到端的示例开始，在该示例中，我们一起使用模型和 tokenizer 分词器来复制 Chapter 1 中引入的函数 pipeline(). 接下来，我们将讨论模型 API：我们将深入研究模型和配置类，并向您展示如何加载模型以及如何将数值输入处理为输出预测。

然后我们来看看标记器 API，它是 pipeline()函数的另一个主要组件。它是作用分词器负责第一个和最后一个处理步骤，处理从文本到神经网络数字输入的转换，以及在需要时转换回文本。最后，我们将向您展示如何处理在一个准备好的批处理中通过一个模型发送多个句子的问题，然后详细介绍 pipeline()函数。

⚠️ 为了从模型集线器和🤗Transformers 的所有可用功能中获益，我们建议[creating an account](https://huggingface.co/join).