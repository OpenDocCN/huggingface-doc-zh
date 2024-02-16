# 总结

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter1/9?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter1/9?fw=pt)

             ![Ask a Question](https://discuss.huggingface.co/t/chapter-1-questions)

在本章中，您了解了如何使用来自🤗Transformers 的函数 pipeline()处理不同的 NLP 任务。您还了解了如何在模型中心（hub）中搜索和使用模型，以及如何使用推理 API 直接在浏览器中测试模型。

我们讨论了 Transformer 模型如何在应用层上工作，并讨论了迁移学习和微调的重要性。您可以使用完整的体系结构，也可以仅使用编码器或解码器，具体取决于您要解决的任务类型。下表总结了这一点：

| 模型 | 示例 | 任务 |
| --- | --- | --- |
| 编码器 | ALBERT, BERT, DistilBERT, ELECTRA, RoBERTa | 句子分类、命名实体识别、从文本中提取答案 |
| 解码器 | CTRL, GPT, GPT-2, Transformer XL | 文本生成 |
| 编码器-解码器 | BART, T5, Marian, mBART | 文本摘要、翻译、生成问题的回答 |