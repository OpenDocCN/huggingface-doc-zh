# 数据类型

> 原始文本：[https://huggingface.co/docs/datasets-server/data_types](https://huggingface.co/docs/datasets-server/data_types)

Datasets Server支持的数据集具有表格格式，意味着数据点以行表示，其特征包含在列中。使用`/first-rows`端点可以预览数据集的前100行以及每个特征的信息。在`features`键中，您会注意到它返回一个`_type`字段。这个值描述了列的数据类型，也被称为数据集的[`Features`](https://huggingface.co/docs/datasets/about_dataset_features)。

有几种不同的数据`Features`用于表示不同的数据格式，比如[`Audio`](https://huggingface.co/docs/datasets/v2.5.2/en/package_reference/main_classes#datasets.Audio)和[`Image`](https://huggingface.co/docs/datasets/v2.5.2/en/package_reference/main_classes#datasets.Image)分别用于语音和图像数据。了解数据集特征可以让你更好地理解你正在处理的数据类型，以及如何预处理它。

例如，对于[Rotten Tomatoes](https://huggingface.co/datasets/rotten_tomatoes)数据集的`/first-rows`端点返回以下内容：

```py
{"dataset": "rotten_tomatoes",
 "config": "default",
 "split": "train",
 "features": [{"feature_idx": 0,
   "name": "text",
   "type": {"dtype": "string", 
   "id": null,
   "_type": "Value"}},
  {"feature_idx": 1,
   "name": "label",
   "type": {"num_classes": 2,
    "names": ["neg", "pos"],
    "id": null,
    "_type": "ClassLabel"}}],
  ...
 }
```

这个数据集有两列，`text`和`label`：

+   `text`列的类型是`Value`。[`Value`](https://huggingface.co/docs/datasets/v2.5.2/en/package_reference/main_classes#datasets.Value)类型非常灵活，表示标量值，如字符串、整数、日期，甚至时间戳值。

+   `label`列的类型是`ClassLabel`。[`ClassLabel`](https://huggingface.co/docs/datasets/v2.5.2/en/package_reference/main_classes#datasets.ClassLabel)类型表示数据集中的类别数量和它们的标签名称。这意味着在分类数据集中经常会看到`ClassLabel`被使用。

要查看可用数据类型的完整列表，请查看[`Features`](https://huggingface.co/docs/datasets/v2.5.2/en/package_reference/main_classes#datasets.Features)文档。
