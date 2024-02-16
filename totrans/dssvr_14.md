# 获取行数和字节大小

> 原始文本：[https://huggingface.co/docs/datasets-server/size](https://huggingface.co/docs/datasets-server/size)

本指南向您展示如何使用数据集服务器的 `/size` 终端以编程方式检索数据集的大小。也可以尝试使用 [ReDoc](https://redocly.github.io/redoc/?url=https://datasets-server.huggingface.co/openapi.json#operation/getSize)。

`/size` 终端接受数据集名称作为其查询参数：

PythonJavaScriptcURL

```py
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://datasets-server.huggingface.co/size?dataset=ibm/duorc"
def query():
    response = requests.get(API_URL, headers=headers)
    return response.json()
data = query()
```

终端响应是一个 JSON，包含数据集的大小，以及每个配置和拆分。它提供了行数、列数（如果适用）以及数据的不同形式的大小：原始文件、内存中的大小（RAM）和自动转换的 parquet 文件。例如，[ibm/duorc](https://huggingface.co/datasets/ibm/duorc) 数据集在所有配置和拆分中共有 187,213 行，总共 97MB。

```py
{
   "size":{
      "dataset":{
         "dataset":"ibm/duorc",
         "num_bytes_original_files":58710973,
         "num_bytes_parquet_files":58710973,
         "num_bytes_memory":1060742354,
         "num_rows":187213
      },
      "configs":[
         {
            "dataset":"ibm/duorc",
            "config":"ParaphraseRC",
            "num_bytes_original_files":37709127,
            "num_bytes_parquet_files":37709127,
            "num_bytes_memory":704394283,
            "num_rows":100972,
            "num_columns":7
         },
         {
            "dataset":"ibm/duorc",
            "config":"SelfRC",
            "num_bytes_original_files":21001846,
            "num_bytes_parquet_files":21001846,
            "num_bytes_memory":356348071,
            "num_rows":86241,
            "num_columns":7
         }
      ],
      "splits":[
         {
            "dataset":"ibm/duorc",
            "config":"ParaphraseRC",
            "split":"train",
            "num_bytes_parquet_files":26005668,
            "num_bytes_memory":494389683,
            "num_rows":69524,
            "num_columns":7
         },
         {
            "dataset":"ibm/duorc",
            "config":"ParaphraseRC",
            "split":"validation",
            "num_bytes_parquet_files":5566868,
            "num_bytes_memory":106733319,
            "num_rows":15591,
            "num_columns":7
         },
         {
            "dataset":"ibm/duorc",
            "config":"ParaphraseRC",
            "split":"test",
            "num_bytes_parquet_files":6136591,
            "num_bytes_memory":103271281,
            "num_rows":15857,
            "num_columns":7
         },
         {
            "dataset":"ibm/duorc",
            "config":"SelfRC",
            "split":"train",
            "num_bytes_parquet_files":14851720,
            "num_bytes_memory":248966361,
            "num_rows":60721,
            "num_columns":7
         },
         {
            "dataset":"ibm/duorc",
            "config":"SelfRC",
            "split":"validation",
            "num_bytes_parquet_files":3114390,
            "num_bytes_memory":56359392,
            "num_rows":12961,
            "num_columns":7
         },
         {
            "dataset":"ibm/duorc",
            "config":"SelfRC",
            "split":"test",
            "num_bytes_parquet_files":3035736,
            "num_bytes_memory":51022318,
            "num_rows":12559,
            "num_columns":7
         }
      ]
   },
   "pending":[

   ],
   "failed":[

   ],
   "partial":false
}
```

如果大小为 `partial: true`，这意味着无法确定数据集的实际大小，因为它太大了。

在这种情况下，行数和字节数可能低于实际数字。
