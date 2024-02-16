# 数据集卡片

> 原文链接：[https://huggingface.co/docs/hub/datasets-cards](https://huggingface.co/docs/hub/datasets-cards)

## 什么是数据集卡片？

每个数据集可能在存储库的`README.md`文件中有文档。这个文件被称为**数据集卡片**，Hugging Face Hub将在数据集的主页上呈现其内容。为了告知用户如何负责地使用数据，最好包含关于数据集内潜在偏见的信息。通常，数据集卡片帮助用户了解数据集的内容，并为数据集的使用提供背景信息。

您还可以将数据集元数据添加到您的卡片中。元数据描述了关于数据集的重要信息，如许可证、语言和大小。它还包含标签，以帮助用户在Hub上发现数据集，并提供[数据文件配置](./datasets-manual-configuration.md)选项。标签在`README.md`文件顶部的YAML元数据部分中定义。

## 数据集卡片元数据

数据集存储库将其README.md呈现为数据集卡片。为了控制Hub如何显示卡片，您应该在README文件中创建一个YAML部分来定义一些元数据。首先在顶部添加三个---，然后包含所有相关的元数据，并用另一组---关闭该部分，就像下面的示例一样：

```py
language: 
- "List of ISO 639-1 code for your language"
- lang1
- lang2
pretty_name: "Pretty Name of the Dataset"
tags:
- tag1
- tag2
license: "any valid license identifier"
task_categories:
- task1
- task2
```

您添加到数据集卡片的元数据使Hub上的某些交互变得可能。例如：

+   允许用户在[https://huggingface.co/datasets](https://huggingface.co/datasets)上过滤和发现数据集。

+   如果您选择使用右列中列出的关键字在[此表格](./repositories-licenses)上选择许可证，许可证将显示在数据集页面上。

在Hub上的数据集存储库中创建README.md文件时，请使用元数据UI填写主要元数据：

![](../Images/a54b445422105bed0a3b922874bd4dad.png) ![](../Images/65f3997b3c9a8e8172611352127b40e5.png)

要查看元数据字段，请查看详细的[数据集卡片规范](https://github.com/huggingface/hub-docs/blob/main/datasetcard.md?plain=1)。

### 数据集卡片创建指南

有关创建数据集卡片的逐步指南，请查看[创建数据集卡片](https://huggingface.co/docs/datasets/dataset_card)指南。

阅读现有数据集卡片，如[ELI5数据集卡片](https://huggingface.co/datasets/eli5/blob/main/README.md)，是熟悉常见惯例的好方法。

### 链接一篇论文

如果数据集卡片包含指向arXiv上一篇论文的链接，Hub将提取arXiv ID，并以`arxiv:<PAPER ID>`的格式将其包含在数据集标签中。点击标签将让您：

+   访问论文页面

+   在Hub上过滤引用相同论文的其他模型。

![](../Images/d08832e6e5583492a53128e9e41f261a.png) ![](../Images/bdd54f51b78b528831546d01c0a15908.png)

阅读更多关于论文页面的信息[这里](./paper-pages)。
