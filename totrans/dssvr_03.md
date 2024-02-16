# 快速入门

> 原文：[https://huggingface.co/docs/datasets-server/quick_start](https://huggingface.co/docs/datasets-server/quick_start)

在这个快速入门中，您将学习如何使用Datasets Server的REST API：

+   检查Hub上的数据集是否可用。

+   返回数据集的配置和拆分。

+   预览数据集的前100行。

+   下载数据集的行片段。

+   在数据集中搜索单词。

+   根据查询字符串过滤行。

+   访问数据集的parquet文件。

+   获取数据集的大小（行数或字节数）。

+   获取有关数据集的统计信息。

## API端点

每个特征都通过下表中总结的端点提供：

| 端点 | 方法 | 描述 | 查询参数 |
| --- | --- | --- | --- |
| [/is-valid](./valid) | GET | 检查特定数据集是否有效。 | `dataset`：数据集的名称 |
| [/splits](./splits) | GET | 获取数据集的配置和拆分列表。 | `dataset`：数据集的名称 |

| [/first-rows](./first_rows) | GET | 获取数据集拆分的前几行。 | - `dataset`：数据集的名称 - `config`：配置的名称

- `split`：拆分的名称|

| [/rows](./rows) | GET | 获取数据集拆分的行片段。 | - `dataset`：数据集的名称 - `config`：配置的名称

- `split`：拆分的名称

- `offset`：片段的偏移量

- `length`：片段的长度（最大100）|

| [/search](./search) | GET | 在数据集拆分中搜索文本。 | - `dataset`：数据集的名称 - `config`：配置的名称

- `split`：拆分的名称

- `query`：要搜索的文本

|

| [/filter](./filter) | GET | 过滤数据集拆分中的行。 | - `dataset`：数据集的名称 - `config`：配置的名称

- `split`：拆分的名称

- `where`：过滤查询

- `offset`：片段的偏移量

- `length`：片段的长度（最大100）|

| [/parquet](./parquet) | GET | 获取数据集的parquet文件列表。 | `dataset`：数据集的名称 |
| --- | --- | --- | --- |
| [/size](./size) | GET | 获取数据集的大小。 | `dataset`：数据集的名称 |

| [/statistics](./statistics) | GET | 获取数据集拆分的统计信息。 | - `dataset`：数据集的名称 - `config`：配置的名称

- `split`：拆分的名称|

使用Datasets Server不需要安装或设置。

如果您还没有[Hugging Face账户](https://huggingface.co/join)，请注册一个！虽然您可以在没有Hugging Face账户的情况下使用Datasets Server，但是如果不提供您在用户设置中找到的[用户令牌](https://huggingface.co/settings/tokens)，则无法访问[受限数据集](https://huggingface.co/docs/hub/datasets-gated)如[CommonVoice](https://huggingface.co/datasets/mozilla-foundation/common_voice_10_0)和[ImageNet](https://huggingface.co/datasets/imagenet-1k)。

请随时在[Postman](https://www.postman.com/huggingface/workspace/hugging-face-apis/documentation/23242779-d068584e-96d1-4d92-a703-7cb12cbd8053)、[ReDoc](https://redocly.github.io/redoc/?url=https://datasets-server.huggingface.co/openapi.json)或[RapidAPI](https://rapidapi.com/hugging-face-hugging-face-default/api/hugging-face-datasets-api/)中尝试API。这个快速入门将向您展示如何以编程方式查询端点。

REST API的基本URL是：

```py
https://datasets-server.huggingface.co
```

## 受限数据集

对于受限数据集，您需要在查询的`headers`中提供您的用户令牌。否则，您将收到错误消息，要求使用身份验证重试。

PythonJavaScriptcURL

```py
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://datasets-server.huggingface.co/is-valid?dataset=mozilla-foundation/common_voice_10_0"
def query():
    response = requests.get(API_URL, headers=headers)
    return response.json()
data = query()
```

如果您尝试访问受限数据集而没有提供用户令牌，您将看到以下错误：

```py
print(data)
{'error': 'The dataset does not exist, or is not accessible without authentication (private or gated). Please check the spelling of the dataset name or retry with authentication.'}
```

## 检查数据集的有效性

要检查特定数据集是否有效，例如[Rotten Tomatoes](https://huggingface.co/datasets/rotten_tomatoes)，请使用`/is-valid`端点：

PythonJavaScriptcURL

```py
import requests
API_URL = "https://datasets-server.huggingface.co/is-valid?dataset=rotten_tomatoes"
def query():
    response = requests.get(API_URL)
    return response.json()
data = query()
```

这将返回数据集是否提供预览（参见/first-rows）、查看器（参见/rows）、搜索（参见/search）和过滤器（参见/filter）：

```py
{ "preview": true, "viewer": true, "search": true, "filter": true }
```

## 列出配置和拆分

`/splits`端点返回数据集中拆分的JSON列表：

PythonJavaScriptcURL

```py
import requests
API_URL = "https://datasets-server.huggingface.co/splits?dataset=rotten_tomatoes"
def query():
    response = requests.get(API_URL)
    return response.json()
data = query()
```

这将返回数据集中可用配置和拆分：

```py
{
  "splits": [
    { "dataset": "rotten_tomatoes", "config": "default", "split": "train" },
    {
      "dataset": "rotten_tomatoes",
      "config": "default",
      "split": "validation"
    },
    { "dataset": "rotten_tomatoes", "config": "default", "split": "test" }
  ],
  "pending": [],
  "failed": []
}
```

## 预览数据集

`/first-rows`端点返回数据集的前100行的JSON列表。它还返回数据特征的类型（“列”数据类型）。您应该指定数据集名称，配置名称（您可以从`/splits`端点找到配置名称），以及您想要预览的数据集的拆分名称：

PythonJavaScriptcURL

```py
import requests
API_URL = "https://datasets-server.huggingface.co/first-rows?dataset=rotten_tomatoes&config=default&split=train"
def query():
    response = requests.get(API_URL)
    return response.json()
data = query()
```

这将返回数据集的前100行：

```py
{
  "dataset": "rotten_tomatoes",
  "config": "default",
  "split": "train",
  "features": [
    {
      "feature_idx": 0,
      "name": "text",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 1,
      "name": "label",
      "type": { "names": ["neg", "pos"], "_type": "ClassLabel" }
    }
  ],
  "rows": [
    {
      "row_idx": 0,
      "row": {
        "text": "the rock is destined to be the 21st century's new \" conan \" and that he's going to make a splash even greater than arnold schwarzenegger , jean-claud van damme or steven segal .",
        "label": 1
      },
      "truncated_cells": []
    },
    {
      "row_idx": 1,
      "row": {
        "text": "the gorgeously elaborate continuation of \" the lord of the rings \" trilogy is so huge that a column of words cannot adequately describe co-writer/director peter jackson's expanded vision of j . r . r . tolkien's middle-earth .",
        "label": 1
      },
      "truncated_cells": []
    },
    ...,
    ...
  ]
}
```

## 下载数据集的片段

`/rows`端点返回数据集在任何给定位置（偏移量）的行片段的JSON列表。它还返回数据特征的类型（“列”数据类型）。您应该指定数据集名称，配置名称（您可以从`/splits`端点找到配置名称），拆分名称以及您想要下载的片段的偏移量和长度：

PythonJavaScriptcURL

```py
import requests
API_URL = "https://datasets-server.huggingface.co/rows?dataset=rotten_tomatoes&config=default&split=train&offset=150&length=10"
def query():
    response = requests.get(API_URL)
    return response.json()
data = query()
```

您可以一次下载最多100行的片段。

响应如下：

```py
{
  "features": [
    {
      "feature_idx": 0,
      "name": "text",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 1,
      "name": "label",
      "type": { "names": ["neg", "pos"], "_type": "ClassLabel" }
    }
  ],
  "rows": [
    {
      "row_idx": 150,
      "row": {
        "text": "enormously likable , partly because it is aware of its own grasp of the absurd .",
        "label": 1
      },
      "truncated_cells": []
    },
    {
      "row_idx": 151,
      "row": {
        "text": "here's a british flick gleefully unconcerned with plausibility , yet just as determined to entertain you .",
        "label": 1
      },
      "truncated_cells": []
    },
    ...,
    ...
  ],
  "num_rows_total": 8530,
  "num_rows_per_page": 100,
  "partial": false
}
```

## 在数据集中搜索文本

`/search`端点返回一个JSON列表，其中包含与文本查询匹配的数据集行片段。即使值嵌套在字典中，文本也会在类型为`string`的列中搜索。它还返回数据特征的类型（“列”数据类型）。响应格式与/rows端点相同。您应该指定数据集名称，配置名称（您可以从`/splits`端点找到配置名称），拆分名称和您想在文本列中查找的搜索查询：

PythonJavaScriptcURL

```py
import requests
API_URL = "https://datasets-server.huggingface.co/search?dataset=rotten_tomatoes&config=default&split=train&query=cat"
def query():
    response = requests.get(API_URL)
    return response.json()
data = query()
```

您可以一次获取最多100行的片段，并且可以使用`offset`和`length`参数请求其他片段，就像对`/rows`端点一样。

响应如下：

```py
{
  "features": [
    {
      "feature_idx": 0,
      "name": "text",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 1,
      "name": "label",
      "type": { "dtype": "int64", "_type": "Value" }
    }
  ],
  "rows": [
    {
      "row_idx": 9,
      "row": {
        "text": "take care of my cat offers a refreshingly different slice of asian cinema .",
        "label": 1
      },
      "truncated_cells": []
    },
    {
      "row_idx": 472,
      "row": {
        "text": "[ \" take care of my cat \" ] is an honestly nice little film that takes us on an examination of young adult life in urban south korea through the hearts and minds of the five principals .",
        "label": 1
      },
      "truncated_cells": []
    },
    ...,
    ...
  ],
  "num_rows_total": 12,
  "num_rows_per_page": 100,
  "partial": false
}
```

## 访问Parquet文件

数据集服务器将Hub上的每个数据集转换为[Parquet](https://parquet.apache.org/)格式。 `/parquet`端点返回一个JSON列表，其中包含数据集的Parquet URL：

PythonJavaScriptcURL

```py
import requests
API_URL = "https://datasets-server.huggingface.co/parquet?dataset=rotten_tomatoes"
def query():
    response = requests.get(API_URL)
    return response.json()
data = query()
```

这将为每个拆分返回一个Parquet文件的URL：

```py
{
  "parquet_files": [
    {
      "dataset": "rotten_tomatoes",
      "config": "default",
      "split": "test",
      "url": "https://huggingface.co/datasets/rotten_tomatoes/resolve/refs%2Fconvert%2Fparquet/default/test/0000.parquet",
      "filename": "0000.parquet",
      "size": 92206
    },
    {
      "dataset": "rotten_tomatoes",
      "config": "default",
      "split": "train",
      "url": "https://huggingface.co/datasets/rotten_tomatoes/resolve/refs%2Fconvert%2Fparquet/default/train/0000.parquet",
      "filename": "0000.parquet",
      "size": 698845
    },
    {
      "dataset": "rotten_tomatoes",
      "config": "default",
      "split": "validation",
      "url": "https://huggingface.co/datasets/rotten_tomatoes/resolve/refs%2Fconvert%2Fparquet/default/validation/0000.parquet",
      "filename": "0000.parquet",
      "size": 90001
    }
  ],
  "pending": [],
  "failed": [],
  "partial": false
}
```

## 获取数据集的大小

`/size`端点返回一个JSON，其中包含数据集的大小（行数和字节大小），以及每个配置和拆分：

PythonJavaScriptcURL

```py
import requests
API_URL = "https://datasets-server.huggingface.co/size?dataset=rotten_tomatoes"
def query():
    response = requests.get(API_URL)
    return response.json()
data = query()
```

这将返回数据集的大小，以及每个配置和拆分：

```py
{
  "size": {
    "dataset": {
      "dataset": "rotten_tomatoes",
      "num_bytes_original_files": 487770,
      "num_bytes_parquet_files": 881052,
      "num_bytes_memory": 1345449,
      "num_rows": 10662
    },
    "configs": [
      {
        "dataset": "rotten_tomatoes",
        "config": "default",
        "num_bytes_original_files": 487770,
        "num_bytes_parquet_files": 881052,
        "num_bytes_memory": 1345449,
        "num_rows": 10662,
        "num_columns": 2
      }
    ],
    "splits": [
      {
        "dataset": "rotten_tomatoes",
        "config": "default",
        "split": "train",
        "num_bytes_parquet_files": 698845,
        "num_bytes_memory": 1074806,
        "num_rows": 8530,
        "num_columns": 2
      },
      {
        "dataset": "rotten_tomatoes",
        "config": "default",
        "split": "validation",
        "num_bytes_parquet_files": 90001,
        "num_bytes_memory": 134675,
        "num_rows": 1066,
        "num_columns": 2
      },
      {
        "dataset": "rotten_tomatoes",
        "config": "default",
        "split": "test",
        "num_bytes_parquet_files": 92206,
        "num_bytes_memory": 135968,
        "num_rows": 1066,
        "num_columns": 2
      }
    ]
  },
  "pending": [],
  "failed": [],
  "partial": false
}
```
