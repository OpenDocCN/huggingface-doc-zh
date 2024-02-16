# 章末小测试

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter1/10?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter1/10?fw=pt)

               ![Ask a Question](https://discuss.huggingface.co/t/chapter-1-questions)

这一章涵盖了很多内容！ 如果有一些不太明白的地方，请不要担心； 下一章将帮助你了解这些模块在底层是如何工作的。

让我们来测试一下你在这一章学到了什么！

### 1\. 探索 Hub 并寻找 roberta-large-mnli checkpoint。 它可以完成什么类型的任务？

### 2\. 下面的代码将会返回什么结果？

```py
from transformers import pipeline

ner = pipeline("ner", grouped_entities=True)
ner("My name is Sylvain and I work at Hugging Face in Brooklyn.")
```

### 3\. 在此代码示例中…的地方应该填写什么？

```py
from transformers import pipeline

filler = pipeline("fill-mask", model="bert-base-cased")
result = filler("...")
```

### 4\. 为什么这段代码会无法运行？

```py
from transformers import pipeline

classifier = pipeline("zero-shot-classification")
result = classifier("This is a course about the Transformers library")
```

### 5\. “迁移学习”是什么意思？

### 6\. 语言模型在预训练时通常不需要标签，这样的说法是否正确。

### 7\. 选择最能描述“模型(model)”、“架构(architecture)”和“权重(weights)”的句子。

### 8\. 你将使用以下哪种类型的模型来根据输入的提示生成文本？

### 9\. 你会使用哪些类型的模型来生成文本的摘要？

### 10\. 你会使用哪一种类型的模型来根据特定的标签对文本输入进行分类？

### 11\. 模型中观察到的偏见有哪些可能的来源？