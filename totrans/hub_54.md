# 搜索

> 原文链接：[`huggingface.co/docs/hub/search`](https://huggingface.co/docs/hub/search)

现在您可以使用**全文搜索**轻松搜索 Hub 上的任何内容。我们索引模型卡片、数据集卡片和 Spaces app.py 文件。

直接访问[`huggingface.co/search`](https://huggingface.co/search)，或者在[`huggingface.co`](https://huggingface.co)顶部的搜索栏中，您可以选择“尝试全文搜索”来帮助找到您在 Hub 上寻找的模型、数据集和空间：

![](img/aac1b03c195d669127558c36bf7d543d.png) ![](img/38caa9f4ffca419fafb99f4a9ecb358d.png)![](img/5b6d891e8e729bd07fcd5ab333a4dd43.png) ![](img/61b5e370fddc9018702781aa1a4b8898.png)

## 轻松筛选

默认情况下，当用户输入查询时，会搜索模型、数据集和空间。如果用户愿意，可以筛选只搜索模型、数据集或空间。

![](img/d4be64082717c690364636b310cacb70.png) ![](img/5592da2da537f2ba9c9092121f0aad49.png)

此外，用户可以复制并分享浏览器地址栏中的 URL，其中应包含作为 URL 查询的筛选信息。例如，当用户搜索查询词`llama`并筛选仅显示`Spaces`时，会得到 URL [`huggingface.co/search/full-text?q=llama&type=space`](https://huggingface.co/search/full-text?q=llama&type=space)
