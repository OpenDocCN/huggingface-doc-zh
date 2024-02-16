# 🤗 Datasets，回顾！

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter5/7?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter5/7?fw=pt)

             ![Ask a Question](https://discuss.huggingface.co/t/chapter-5-questions)

这是对 🤗 Datasets 库的一次完整游览——祝贺你走到这一步！凭借从本章中获得的知识，您应该能够：

*   从任何地方加载数据集，无论是 Hugging Face Hub、您的笔记本电脑还是您公司的远程服务器。
*   混合使用 Dataset.map()和 Dataset.filter()函数来整理数据。
*   使用`Dataset.set_format()`在 Pandas 和 NumPy 等数据格式之间快速切换.
*   创建您自己的数据集并将其推送到 Hugging Face Hub。.
*   使用 Transformer 模型为您的文档创建词嵌入，并使用 FAISS 构建语义搜索引擎。.

在第七章，当我们深入研究 Transformer 模型非常适合的核心 NLP 任务时，我们将充分利用所有这些。