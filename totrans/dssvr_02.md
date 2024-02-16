# 🤗 数据集服务器

> 原始文本：[https://huggingface.co/docs/datasets-server/index](https://huggingface.co/docs/datasets-server/index)

数据集服务器是一个轻量级的Web API，用于可视化和探索存储在Hugging Face Hub上的各种类型的数据集 - 计算机视觉、语音、文本和表格。

数据集服务器的主要功能是自动将所有Hub数据集转换为Parquet。在[Parquet部分](./parquet)中了解更多信息。

随着数据集的增大和数据类型的丰富，预处理这些数据集的成本（存储和计算）可能具有挑战性且耗时。为了帮助用户访问这些现代数据集，数据集服务器在幕后运行一个服务器，提前生成API响应并将其存储在数据库中，因此当您通过API进行查询时，它们会立即返回。

让数据集服务器处理繁重的工作，这样您就可以在Hugging Face上的30,000多个数据集中使用简单的REST API：

+   列出数据集的拆分、列名和数据类型

+   获取数据集大小（行数或字节）

+   下载并查看数据集中任意索引处的行

+   在数据集中搜索一个单词

+   根据查询字符串过滤行

+   获取有关数据的深刻统计信息

+   访问数据集作为Parquet文件，以在您喜欢的处理或分析框架中使用

![](../Images/14fe7f6fbf289784fc357e7928a0488c.png) ![](../Images/c8c5a03c962dc340de67029f3b1741b0.png)

[OpenBookQA数据集](https://huggingface.co/datasets/openbookqa)的数据集查看器

今天加入不断增长的社区，在[论坛](https://discuss.huggingface.co/)或[Discord](https://discord.com/invite/JfAtkvEtRb)上，如果您对最新更新感兴趣，请给[数据集服务器存储库](https://github.com/huggingface/datasets-server)点赞！
