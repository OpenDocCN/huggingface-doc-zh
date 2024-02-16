# 列出 Parquet 文件

> 原始文本：[`huggingface.co/docs/datasets-server/parquet`](https://huggingface.co/docs/datasets-server/parquet)

数据集可以以任何格式（CSV，JSONL，图像目录等）发布到 Hub，并且可以使用🤗 [数据集](https://huggingface.co/docs/datasets/)库轻松访问。为了获得更高性能的体验（特别是对于大型数据集），数据集服务器会自动将每个数据集转换为[Parquet](https://parquet.apache.org/)格式。

## 什么是 Parquet？

Parquet 是一种针对查询和处理大型数据集进行优化的列存储格式。Parquet 是处理大数据和分析的热门选择，广泛用于数据处理和机器学习。

在 Parquet 中，数据被分成称为“行组”的块，每个行组中的数据以列而不是行的形式存储。每个行组列使用最佳的压缩算法单独压缩，包含有关其包含的数据的元数据和统计信息（最小/最大值，NULL 值的数量）。

这种结构允许高效地读取和查询数据：

+   只需从磁盘读取必要的列（投影下推）；无需读取整个文件。这减少了处理 Parquet 数据所需的内存。

+   如果其元数据中存储的统计数据与感兴趣的数据不匹配，则将跳过整个行组（自动过滤）

+   数据被压缩，从而减少了需要存储和传输的数据量。

Parquet 文件包含一个单独的表。如果数据集有多个表（例如多个拆分或配置），则每个表存储在单独的 Parquet 文件中。

## 转换为 Parquet

Parquet 文件发布到 Hub 的特定`refs/convert/parquet`分支（例如`amazon_polarity` [分支](https://huggingface.co/datasets/amazon_polarity/tree/refs%2Fconvert%2Fparquet)）与`main`分支平行。

为了使数据集服务器生成数据集的 Parquet 版本，数据集必须是*公共的*，或者由[PRO 用户](https://huggingface.co/pricing)或[企业 Hub 组织](https://huggingface.co/enterprise)拥有。

## 使用数据集服务器 API

本指南向您展示如何使用数据集服务器的`/parquet`端点检索转换为 Parquet 的数据集文件列表。也可以尝试使用[Postman](https://www.postman.com/huggingface/workspace/hugging-face-apis/request/23242779-f0cde3b9-c2ee-4062-aaca-65c4cfdd96f8)，[RapidAPI](https://rapidapi.com/hugging-face-hugging-face-default/api/hugging-face-datasets-api)或[ReDoc](https://redocly.github.io/redoc/?url=https://datasets-server.huggingface.co/openapi.json#operation/listSplits)。

`/parquet`端点接受数据集名称作为其查询参数：

PythonJavaScriptcURL

```py
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://datasets-server.huggingface.co/parquet?dataset=ibm/duorc"
def query():
    response = requests.get(API_URL, headers=headers)
    return response.json()
data = query()
```

端点响应是一个 JSON，包含数据集文件的 Parquet 格式列表。例如，[`ibm/duorc`](https://huggingface.co/datasets/ibm/duorc)数据集有六个 Parquet 文件，对应于其两个配置`ParaphraseRC`和`SelfRC`的`test`，`train`和`validation`拆分（有关拆分和配置的更多详细信息，请参阅列出拆分和配置指南）。

端点还提供每个文件的文件名和大小：

```py
{
   "parquet_files":[
      {
         "dataset":"ibm/duorc",
         "config":"ParaphraseRC",
         "split":"test",
         "url":"https://huggingface.co/datasets/ibm/duorc/resolve/refs%2Fconvert%2Fparquet/ParaphraseRC/test/0000.parquet",
         "filename":"0000.parquet",
         "size":6136591
      },
      {
         "dataset":"ibm/duorc",
         "config":"ParaphraseRC",
         "split":"train",
         "url":"https://huggingface.co/datasets/ibm/duorc/resolve/refs%2Fconvert%2Fparquet/ParaphraseRC/train/0000.parquet",
         "filename":"0000.parquet",
         "size":26005668
      },
      {
         "dataset":"ibm/duorc",
         "config":"ParaphraseRC",
         "split":"validation",
         "url":"https://huggingface.co/datasets/ibm/duorc/resolve/refs%2Fconvert%2Fparquet/ParaphraseRC/validation/0000.parquet",
         "filename":"0000.parquet",
         "size":5566868
      },
      {
         "dataset":"ibm/duorc",
         "config":"SelfRC",
         "split":"test",
         "url":"https://huggingface.co/datasets/ibm/duorc/resolve/refs%2Fconvert%2Fparquet/SelfRC/test/0000.parquet",
         "filename":"0000.parquet",
         "size":3035736
      },
      {
         "dataset":"ibm/duorc",
         "config":"SelfRC",
         "split":"train",
         "url":"https://huggingface.co/datasets/ibm/duorc/resolve/refs%2Fconvert%2Fparquet/SelfRC/train/0000.parquet",
         "filename":"0000.parquet",
         "size":14851720
      },
      {
         "dataset":"ibm/duorc",
         "config":"SelfRC",
         "split":"validation",
         "url":"https://huggingface.co/datasets/ibm/duorc/resolve/refs%2Fconvert%2Fparquet/SelfRC/validation/0000.parquet",
         "filename":"0000.parquet",
         "size":3114390
      }
   ],
   "pending":[

   ],
   "failed":[

   ],
   "partial":false
}
```

## 分片的 Parquet 文件

大型数据集被分成大约 500MB 的 Parquet 文件（分片）。文件名包含数据集的名称，拆分，分片索引和总分片数（`dataset-name-train-0000-of-0004.parquet`）。对于给定的拆分，列表中的元素按其分片索引按升序排序。例如，[`amazon_polarity`](https://datasets-server.huggingface.co/parquet?dataset=amazon_polarity)数据集的`train`拆分被分成 4 个分片：

```py
{
   "parquet_files":[
      {
         "dataset":"amazon_polarity",
         "config":"amazon_polarity",
         "split":"test",
         "url":"https://huggingface.co/datasets/amazon_polarity/resolve/refs%2Fconvert%2Fparquet/amazon_polarity/test/0000.parquet",
         "filename":"0000.parquet",
         "size":117422360
      },
      {
         "dataset":"amazon_polarity",
         "config":"amazon_polarity",
         "split":"train",
         "url":"https://huggingface.co/datasets/amazon_polarity/resolve/refs%2Fconvert%2Fparquet/amazon_polarity/train/0000.parquet",
         "filename":"0000.parquet",
         "size":259761770
      },
      {
         "dataset":"amazon_polarity",
         "config":"amazon_polarity",
         "split":"train",
         "url":"https://huggingface.co/datasets/amazon_polarity/resolve/refs%2Fconvert%2Fparquet/amazon_polarity/train/0001.parquet",
         "filename":"0001.parquet",
         "size":258363554
      },
      {
         "dataset":"amazon_polarity",
         "config":"amazon_polarity",
         "split":"train",
         "url":"https://huggingface.co/datasets/amazon_polarity/resolve/refs%2Fconvert%2Fparquet/amazon_polarity/train/0002.parquet",
         "filename":"0002.parquet",
         "size":255471883
      },
      {
         "dataset":"amazon_polarity",
         "config":"amazon_polarity",
         "split":"train",
         "url":"https://huggingface.co/datasets/amazon_polarity/resolve/refs%2Fconvert%2Fparquet/amazon_polarity/train/0003.parquet",
         "filename":"0003.parquet",
         "size":254410930
      }
   ],
   "pending":[

   ],
   "failed":[

   ],
   "partial":false
}
```

要读取和查询 Parquet 文件，请查看从数据集服务器查询数据集指南。

## 部分转换的数据集

如果数据集尚未以 Parquet 格式存在，或者数据集大于 5GB，则 Parquet 版本可能是部分的。

在这种情况下，Parquet 文件生成的大小为 5GB，并放置在以“partial”为前缀的分割目录中，例如“partial-train”而不是“train”。

## Parquet 本地数据集

当数据集已经以 Parquet 格式存在时，数据不会被转换，`refs/convert/parquet`中的文件是指向原始文件的链接。这个规则有一个例外，以确保数据集服务器 API 保持快速：如果原始 Parquet 文件的[row group](https://parquet.apache.org/docs/concepts/)大小太大，将生成新的 Parquet 文件。

## 使用 Hugging Face Hub API

为了方便起见，您可以直接使用 Hugging Face Hub 的`/api/parquet`端点，该端点返回 Parquet URL 列表：

PythonJavaScriptcURL

```py
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://huggingface.co/api/datasets/ibm/duorc/parquet"
def query():
    response = requests.get(API_URL, headers=headers)
    return response.json()
urls = query()
```

端点响应是一个 JSON，包含数据集文件 URL 列表，以 Parquet 格式提供每个分割和配置。例如，[`ibm/duorc`](https://huggingface.co/datasets/ibm/duorc) 数据集对于“ParaphraseRC”配置的训练分割有一个 Parquet 文件（有关分割和配置的更多详细信息，请参阅列出分割和配置指南）。

```py
{
   "ParaphraseRC":{
      "test":[
         "https://huggingface.co/api/datasets/ibm/duorc/parquet/ParaphraseRC/test/0.parquet"
      ],
      "train":[
         "https://huggingface.co/api/datasets/ibm/duorc/parquet/ParaphraseRC/train/0.parquet"
      ],
      "validation":[
         "https://huggingface.co/api/datasets/ibm/duorc/parquet/ParaphraseRC/validation/0.parquet"
      ]
   },
   "SelfRC":{
      "test":[
         "https://huggingface.co/api/datasets/ibm/duorc/parquet/SelfRC/test/0.parquet"
      ],
      "train":[
         "https://huggingface.co/api/datasets/ibm/duorc/parquet/SelfRC/train/0.parquet"
      ],
      "validation":[
         "https://huggingface.co/api/datasets/ibm/duorc/parquet/SelfRC/validation/0.parquet"
      ]
   }
}
```

可选地，您可以指定要返回的配置名称，以及要返回的分割：

PythonJavaScriptcURL

```py
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://huggingface.co/api/datasets/ibm/duorc/parquet/ParaphraseRC/train"
def query():
    response = requests.get(API_URL, headers=headers)
    return response.json()
urls = query()
```

```py
[
  "https://huggingface.co/api/datasets/ibm/duorc/parquet/ParaphraseRC/train/0.parquet"
]
```

每个 Parquet 文件也可以通过其分片索引进行访问：`https://huggingface.co/api/datasets/ibm/duorc/parquet/ParaphraseRC/train/0.parquet` 重定向到 `https://huggingface.co/datasets/ibm/duorc/resolve/refs%2Fconvert%2Fparquet/ParaphraseRC/train/0000.parquet` 例如。
