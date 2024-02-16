# 获取数据集信息

> 原文链接：[`huggingface.co/docs/datasets-server/info`](https://huggingface.co/docs/datasets-server/info)

Datasets Server 提供了一个`/info`端点，用于探索有关数据集的一般信息，包括描述、引用、主页、许可证和特性等字段。

`/info`端点接受两个查询参数：

+   `dataset`：数据集名称

+   `config`：配置名称

PythonJavaScriptcURL

```py
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://datasets-server.huggingface.co/info?dataset=ibm/duorc&config=SelfRC"
def query():
    response = requests.get(API_URL, headers=headers)
    return response.json()
data = query()
```

端点响应是一个带有`dataset_info`键的 JSON。其结构和内容对应于`datasets`库的[DatasetInfo](https://huggingface.co/docs/datasets/package_reference/main_classes#datasets.DatasetInfo)对象。

```py
{
   "dataset_info":{
      "description":"",
      "citation":"",
      "homepage":"",
      "license":"",
      "features":{
         "plot_id":{
            "dtype":"string",
            "_type":"Value"
         },
         "plot":{
            "dtype":"string",
            "_type":"Value"
         },
         "title":{
            "dtype":"string",
            "_type":"Value"
         },
         "question_id":{
            "dtype":"string",
            "_type":"Value"
         },
         "question":{
            "dtype":"string",
            "_type":"Value"
         },
         "answers":{
            "feature":{
               "dtype":"string",
               "_type":"Value"
            },
            "_type":"Sequence"
         },
         "no_answer":{
            "dtype":"bool",
            "_type":"Value"
         }
      },
      "builder_name":"parquet",
      "dataset_name":"duorc",
      "config_name":"SelfRC",
      "version":{
         "version_str":"0.0.0",
         "major":0,
         "minor":0,
         "patch":0
      },
      "splits":{
         "train":{
            "name":"train",
            "num_bytes":248966361,
            "num_examples":60721,
            "dataset_name":null
         },
         "validation":{
            "name":"validation",
            "num_bytes":56359392,
            "num_examples":12961,
            "dataset_name":null
         },
         "test":{
            "name":"test",
            "num_bytes":51022318,
            "num_examples":12559,
            "dataset_name":null
         }
      },
      "download_size":21001846,
      "dataset_size":356348071
   },
   "partial":false
}
```
