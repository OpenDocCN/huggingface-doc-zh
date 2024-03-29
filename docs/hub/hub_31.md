# 数据集查看器

> 原文链接：[`huggingface.co/docs/hub/datasets-viewer`](https://huggingface.co/docs/hub/datasets-viewer)

数据集页面包括一个按 100 行一页排列的数据集内容表格。您可以使用表格底部的按钮在页面之间导航。

![](img/d53c788abb454a20e42b4dc7ca22f918.png) ![](img/0f394b98ae342347cd077eb027335a02.png)

## 检查数据分布

在列的顶部，您可以看到代表数据分布的图表。这让您快速了解您的类别平衡情况，数值数据的范围和分布以及文本长度，以及列数据中缺失的部分。

## 按值过滤

如果您点击数值列的直方图条，数据集查看器将过滤数据，并仅显示数值落在所选范围内的行。同样，如果您从分类列中选择一个类别，它将仅显示所选类别的行。

## 在数据集中搜索一个单词

您可以通过在表格顶部的搜索栏中输入单词来在数据集中搜索单词。搜索是不区分大小写的，并且将匹配包含该单词的任何行。即使值嵌套在字典或列表中，文本也会在`string`列中搜索。

## 分享特定行

您可以通过点击特定行来分享特定行，并复制浏览器地址栏中的 URL。例如[`huggingface.co/datasets/glue/viewer/mrpc/test?p=2&row=241`](https://huggingface.co/datasets/glue/viewer/mrpc/test?p=2&row=241)将在 MRPC 数据集上打开数据集查看器，在测试分割上，并显示第 241 行。

## 大规模数据集

数据集查看器支持大规模数据集，但根据数据格式，它可能只显示数据集的前 5GB：

+   对于 Parquet 数据集：数据集查看器显示完整的数据集，但仅在前 5GB 上启用过滤和搜索。

+   对于其他格式的大于 5GB 的数据集（例如[WebDataset](https://github.com/webdataset/webdataset)或 JSON Lines）：数据集查看器仅显示前 5GB，并且在这前 5GB 上启用过滤和搜索。

在这种情况下，一个信息性的消息会告诉您查看器是部分的。这应该是一个足够大的样本，能够准确代表整个数据集，如果您需要更大的样本，请告诉我们。

## 访问 parquet 文件

为了支持数据集查看器，每个数据集的前 5GB 都会自动转换为 Parquet 格式（除非它已经是一个 Parquet 数据集）。在数据集查看器中（例如，查看[`datasets/glue`](https://huggingface.co/datasets/glue)），您可以点击[*“自动转换为 Parquet”*](https://huggingface.co/datasets/glue/tree/refs%2Fconvert%2Fparquet/cola)来访问 Parquet 文件。请参考数据集服务器文档以了解如何使用 Polars、Pandas 或 DuckDB 等库查询数据集 parquet 文件。

您也可以通过 Hub API 来以编程方式访问 Parquet 文件列表；例如，端点[`https://huggingface.co/api/datasets/glue/parquet`](https://huggingface.co/api/datasets/glue/parquet)列出了 glue 数据集的 parquet 文件。

## 数据集预览

对于最大的数据集，页面将显示前 100 行的预览，而不是完整的查看器。这个限制仅适用于超过 5GB 的数据集，这些数据集不是原生的 Parquet 格式，或者没有自动转换为 Parquet。

![](img/6bf89d4a26b1d9f9337fa16b0841f4f5.png) ![](img/1fa1f5e1a33de88d31062c497989dd40.png)

## 配置数据集查看器

为了使您的数据集拥有一个正常工作的数据集查看器，请确保您的数据集是以支持的格式和结构存在的。还有一个选项可以使用 YAML 配置您的数据集。

对于**私有**数据集，数据集查看器仅对[PRO 用户](https://huggingface.co/pricing)和[企业中心组织](https://huggingface.co/enterprise)启用。

有关更多信息，请参阅我们的指南：如何配置数据集查看器。
