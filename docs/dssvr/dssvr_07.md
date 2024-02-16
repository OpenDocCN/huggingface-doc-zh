# 列出拆分和配置

> 原始文本：[`huggingface.co/docs/datasets-server/splits`](https://huggingface.co/docs/datasets-server/splits)

数据集通常具有拆分，也可能具有配置。*拆分*是数据集的子集，如`train`和`test`，在训练和评估模型的不同阶段中使用。*配置*是包含在较大数据集中的子数据集。配置在多语言语音数据集中特别常见，每种语言可能有不同的配置。如果您想了解更多关于拆分和配置的信息，请查看“拆分和配置”概念指南！

![split-configs-server](img/688577f4daf8a4f28f06e2fe8b399ab5.png)

本指南向您展示如何使用 Datasets Server 的`/splits`端点以编程方式检索数据集的拆分和配置。也可以尝试使用[Postman](https://www.postman.com/huggingface/workspace/hugging-face-apis/request/23242779-f0cde3b9-c2ee-4062-aaca-65c4cfdd96f8)、[RapidAPI](https://rapidapi.com/hugging-face-hugging-face-default/api/hugging-face-datasets-api)或[ReDoc](https://redocly.github.io/redoc/?url=https://datasets-server.huggingface.co/openapi.json#operation/listSplits)

`/splits`端点接受数据集名称作为其查询参数：

PythonJavaScriptcURL

```py
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://datasets-server.huggingface.co/splits?dataset=ibm/duorc"
def query():
    response = requests.get(API_URL, headers=headers)
    return response.json()
data = query()
```

端点响应是一个 JSON，包含数据集的拆分和配置列表。例如，[ibm/duorc](https://huggingface.co/datasets/ibm/duorc)数据集有六个拆分和两个配置：

```py
{
  "splits": [
    { "dataset": "ibm/duorc", "config": "ParaphraseRC", "split": "train" },
    { "dataset": "ibm/duorc", "config": "ParaphraseRC", "split": "validation" },
    { "dataset": "ibm/duorc", "config": "ParaphraseRC", "split": "test" },
    { "dataset": "ibm/duorc", "config": "SelfRC", "split": "train" },
    { "dataset": "ibm/duorc", "config": "SelfRC", "split": "validation" },
    { "dataset": "ibm/duorc", "config": "SelfRC", "split": "test" }
  ],
  "pending": [],
  "failed": []
}
```
