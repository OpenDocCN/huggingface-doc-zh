# 在拆分数据上探索统计数据

> 原始文本：[https://huggingface.co/docs/datasets-server/statistics](https://huggingface.co/docs/datasets-server/statistics)

Datasets Server提供了一个`/statistics`端点，用于获取请求数据集的一些基本预先计算的统计信息。这将让您快速了解数据的分布情况。

目前，仅为[具有Parquet导出的数据集](./parquet)计算统计数据。

`/statistics`端点需要三个查询参数：

+   `dataset`：数据集名称，例如`glue`

+   `config`：配置名称，例如`cola`

+   `split`：拆分名称，例如`train`

让我们为`glue`数据集、`cola`配置、`train`拆分获取一些统计信息：

PythonJavaScriptcURL

```py
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://datasets-server.huggingface.co/statistics?dataset=glue&config=cola&split=train"
def query():
    response = requests.get(API_URL, headers=headers)
    return response.json()
data = query()
```

响应JSON包含两个键：

+   `num_examples` - 拆分中的样本数量

+   `statistics` - 每列的统计信息字典列表，每个字典有三个键：`column_name`、`column_type`和`column_statistics`。`column_statistics`的内容取决于列类型，请参阅[数据类型的响应结构](./statistics#response-structure-by-data-type)以获取更多详细信息

```py
{
  "num_examples": 8551,
  "statistics": [
    {
      "column_name": "idx",
      "column_type": "int",
      "column_statistics": {
        "nan_count": 0,
        "nan_proportion": 0,
        "min": 0,
        "max": 8550,
        "mean": 4275,
        "median": 4275,
        "std": 2468.60541,
        "histogram": {
          "hist": [
            856,
            856,
            856,
            856,
            856,
            856,
            856,
            856,
            856,
            847
          ],
          "bin_edges": [
            0,
            856,
            1712,
            2568,
            3424,
            4280,
            5136,
            5992,
            6848,
            7704,
            8550
          ]
        }
      }
    },
    {
      "column_name": "label",
      "column_type": "class_label",
      "column_statistics": {
        "nan_count": 0,
        "nan_proportion": 0,
        "no_label_count": 0,
        "no_label_proportion": 0,
        "n_unique": 2,
        "frequencies": {
          "unacceptable": 2528,
          "acceptable": 6023
        }
      }
    },
    {
      "column_name": "sentence",
      "column_type": "string_text",
      "column_statistics": {
        "nan_count": 0,
        "nan_proportion": 0,
        "min": 6,
        "max": 231,
        "mean": 40.70074,
        "median": 37,
        "std": 19.14431,
        "histogram": {
          "hist": [
            2260,
            4512,
            1262,
            380,
            102,
            26,
            6,
            1,
            1,
            1
          ],
          "bin_edges": [
            6,
            29,
            52,
            75,
            98,
            121,
            144,
            167,
            190,
            213,
            231
          ]
        }
      }
    }
  ]
}
```

## 数据类型的响应结构

目前，仅支持字符串、浮点数和整数数字的统计数据，以及[`datasets.ClassLabel`](https://huggingface.co/docs/datasets/package_reference/main_classes#datasets.ClassLabel)特征类型的特殊数据集库。

响应中的`column_type`可以是以下值之一：

+   `class_label` - 用于[`datasets.ClassLabel`](https://huggingface.co/docs/datasets/package_reference/main_classes#datasets.ClassLabel)特征

+   `float` - 用于浮点数据类型

+   `int` - 用于整数数据类型

+   `bool` - 用于布尔数据类型

+   `string_label` - 用于字符串数据类型，如果在给定拆分中的字符串列中有小于或等于30个唯一值

+   `string_text` - 用于字符串数据类型，如果在给定拆分中的字符串列中有超过30个唯一值

### class_label

此类型表示编码为[`ClassLabel`](https://huggingface.co/docs/datasets/package_reference/main_classes#datasets.ClassLabel)特征的分类数据。计算以下度量值：

+   `null`值的数量和比例

+   没有标签的值的数量和比例

+   唯一值的数量（不包括`null`和`no label`）

+   每个标签的计数（不包括`null`和`no label`）

<details><summary data-svelte-h="svelte-vpylii">示例</summary>

```py
{
  "column_name": "label",
  "column_type": "class_label",
  "column_statistics": {
    "nan_count": 0,
    "nan_proportion": 0,
    "no_label_count": 0,
    "no_label_proportion": 0,
    "n_unique": 2,
    "frequencies": {
      "unacceptable": 2528,
      "acceptable": 6023
    }
  }
}
```</details>

### 浮点数

以下度量值适用于浮点数据类型：

+   最小值、最大值、均值和标准差值

+   `null`值的数量和比例

+   具有10个bin的直方图

<details><summary data-svelte-h="svelte-vpylii">示例</summary>

```py
{
  "column_name": "clarity",
  "column_type": "float",
  "column_statistics": {
    "nan_count": 0,
    "nan_proportion": 0,
    "min": 0,
    "max": 2,
    "mean": 1.67206,
    "median": 1.8,
    "std": 0.38714,
    "histogram": {
      "hist": [
        17,
        12,
        48,
        52,
        135,
        188,
        814,
        15,
        1628,
        2048
      ],
      "bin_edges": [
        0,
        0.2,
        0.4,
        0.6,
        0.8,
        1,
        1.2,
        1.4,
        1.6,
        1.8,
        2
      ]
    }
  }
}
```</details>

### int

以下度量值适用于整数数据类型：

+   最小值、最大值、均值和标准差值

+   `null`值的数量和比例

+   具有小于或等于10个bin的直方图

<details><summary data-svelte-h="svelte-vpylii">示例</summary>

```py
{
    "column_name": "direction",
    "column_type": "int",
    "column_statistics": {
        "nan_count": 0,
        "nan_proportion": 0.0,
        "min": 0,
        "max": 1,
        "mean": 0.49925,
        "median": 0.0,
        "std": 0.5,
        "histogram": {
            "hist": [
                50075,
                49925
            ],
            "bin_edges": [
                0,
                1,
                1
            ]
        }
    }
}
```</details>

### 布尔

以下度量值适用于布尔数据类型：

+   `null`值的数量和比例

+   值为'True'和'False'的计数

<details><summary data-svelte-h="svelte-vpylii">示例</summary>

```py
{
  "column_name": "penalty",
  "column_type": "bool",
  "column_statistics":
    {
        "nan_count": 3,
        "nan_proportion": 0.15,
        "frequencies": {
            "False": 7,
            "True": 10
        }
    }
}
```</details>

### string_label

如果请求的拆分中的字符串列具有小于或等于30个唯一值，则被视为类别。返回以下度量值：

+   `null`值的数量和比例

+   唯一值的数量（不包括`null`）

+   每个标签的计数（不包括`null`）

<details><summary data-svelte-h="svelte-vpylii">示例</summary>

```py
{
  "column_name": "answerKey",
  "column_type": "string_label",
  "column_statistics": {
    "nan_count": 0,
    "nan_proportion": 0,
    "n_unique": 4,
    "frequencies": {
      "D": 1221,
      "C": 1146,
      "A": 1378,
      "B": 1212
    }
  }
}

```</details>

### string_text

如果请求的拆分中的字符串列具有超过30个唯一值，则被视为包含文本的列，并且响应包含文本长度的统计信息。计算以下度量值：

+   文本长度的最小值、最大值、均值和标准差

+   `null`值的数量和比例

+   具有10个bin的文本长度直方图

<details><summary data-svelte-h="svelte-vpylii">示例</summary>

```py
{
  "column_name": "sentence",
  "column_type": "string_text",
  "column_statistics": {
    "nan_count": 0,
    "nan_proportion": 0,
    "min": 6,
    "max": 231,
    "mean": 40.70074,
    "median": 37,
    "std": 19.14431,
    "histogram": {
      "hist": [
        2260,
        4512,
        1262,
        380,
        102,
        26,
        6,
        1,
        1,
        1
      ],
      "bin_edges": [
        6,
        29,
        52,
        75,
        98,
        121,
        144,
        167,
        190,
        213,
        231
      ]
    }
  }
}
```</details>
