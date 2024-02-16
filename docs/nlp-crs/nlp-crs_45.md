# 章末小测试

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter5/8?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter5/8?fw=pt)

               ![Ask a Question](https://discuss.huggingface.co/t/chapter-5-questions)

本章涵盖了很多方面! 如果你没有掌握所有细节, 不用担心; 在下一章将帮助你了解内部的事情是如何工作的。

不过, 在继续下一章之前, 让我们测试一下你在本章学到的内容。

### 1.🤗 Datasets 中的 load_dataset () 函数允许你从下列哪个位置加载数据集？

### 2.假设您加载了 GLUE 任务，如下所示:

```py
from datasets import load_dataset

dataset = load_dataset("glue", "mrpc", split="train")
```

以下哪个命令将从 `dataset` 中生成 50 个元素的随机样本?

### 3.假设你有一个叫做宠物数据集的家庭宠物数据集，它有一个名字列表示每个宠物的名字。下列哪种方法可以让你过滤所有名字以字母”L”开头的宠物的数据？

### 4.什么是内存映射？

### 5.下列哪一项是内存映射的主要好处？

### 6.为什么下面的代码是错误的？

```py
from datasets import load_dataset

dataset = load_dataset("allocine", streaming=True, split="train")
dataset[0]
```

### 7.创建数据集卡的主要好处是什么？

### 8.什么是语义搜索？

### 9.对于非对称语义搜索，通常有:

### 10.我可以使用数据集加载数据用于其他领域，如语音处理？