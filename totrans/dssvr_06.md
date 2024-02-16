# 检查数据集的有效性

> 原始文本：[https://huggingface.co/docs/datasets-server/valid](https://huggingface.co/docs/datasets-server/valid)

在从Hub下载数据集之前，了解您感兴趣的特定数据集是否可用是有帮助的。数据集服务器提供了`/is-valid`端点，用于检查特定数据集是否可以正常工作而无任何错误。

API端点将为无法使用[🤗数据集](https://github.com/huggingface/datasets)库加载的数据集返回错误，例如，因为数据尚未上传或格式不受支持。

最大的数据集部分受到数据集服务器的支持。如果它们是[可流式传输的](https://huggingface.co/docs/datasets/stream)，数据集服务器可以提取前100行而无需下载整个数据集。这对于预览大型数据集特别有用，其中下载整个数据集可能需要几个小时！查看`/is-valid`的响应中的`preview`字段，以检查数据集是否部分受支持。

本指南向您展示了如何以编程方式检查数据集的有效性，但您可以尝试使用[Postman](https://www.postman.com/huggingface/workspace/hugging-face-apis/request/23242779-17b761d0-b2b8-4638-a4f7-73be9049c324)、[RapidAPI](https://rapidapi.com/hugging-face-hugging-face-default/api/hugging-face-datasets-api)或[ReDoc](https://redocly.github.io/redoc/?url=https://datasets-server.huggingface.co/openapi.json#operation/isValidDataset)进行尝试。

## 检查数据集是否有效

`/is-valid`检查特定数据集是否可以正常加载。此端点的查询参数要求您指定数据集的名称：

PythonJavaScriptcURL

```py
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://datasets-server.huggingface.co/is-valid?dataset=rotten_tomatoes"
def query():
    response = requests.get(API_URL, headers=headers)
    return response.json()
data = query()
```

如果数据集有效，则响应如下：

```py
{
  "viewer": true,
  "preview": true,
  "search": true,
  "filter": true,
}
```

如果数据集有效但/search不可用，则响应如下：

```py
{
  "viewer": true,
  "preview": true,
  "search": false,
  "filter": true,
}
```

如果数据集有效但/filter不可用，则响应如下：

```py
{
  "viewer": true,
  "preview": true,
  "search": true,
  "filter": false,
}
```

如果只有数据集的前几行可用，则响应如下：

```py
{
  "viewer": false,
  "preview": true,
  "search": true,
  "filter": true,
}
```

最后，如果数据集根本无效，则响应是：

```py
{
  "viewer": false,
  "preview": false,
  "search": false,
  "filter": false,
}
```

数据集无效的一些情况包括：

+   数据集查看器已禁用

+   数据集被封锁，但未授权访问：未传递令牌或传递的令牌未经授权

+   数据集是私有的，但所有者不是PRO用户或企业Hub组织。

+   数据集不包含数据或数据格式不受支持

请记住，如果数据集是[封锁的](./quick_start#gated-datasets)，您需要提供用户令牌才能提交成功的查询！
