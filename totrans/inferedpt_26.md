# 请求的序列化和反序列化

> 原始文本: [`huggingface.co/docs/inference-endpoints/others/serialization`](https://huggingface.co/docs/inference-endpoints/others/serialization)

Hugging Face 推理端点使用默认的服务容器，用于所有支持的 Transformers 和 Sentence-Transformers 任务以及自定义推理处理程序。服务容器负责根据请求的 `content-type` 和 `accept` 头部对请求和响应负载进行序列化和反序列化。这意味着当您发送一个带有 JSON 主体和 `content-type: application/json` 头部的请求时，服务容器将将 JSON 负载反序列化为 Python 字典并传递给推理处理程序，如果您发送一个带有 `accept: image/png` 头部的请求，服务容器将把任务/自定义处理程序的响应序列化为图像。

以下是支持的 `content-types` 列表以及传递给推理处理程序的反序列化负载。

| Content-Type | 负载 |
| --- | --- |
| application/json | `字典` |
| text/csv | `原始` |
| text/plain | `原始` |
| image/png | `二进制` |
| image/jpeg | `二进制` |
| image/jpg | `二进制` |
| image/tiff | `二进制` |
| image/bmp | `二进制` |
| image/gif | `二进制` |
| image/webp | `二进制` |
| image/x-image | `二进制` |
| audio/x-flac | `{"inputs": bytes(body)}` |
| audio/flac | `{"inputs": bytes(body)}` |
| audio/mpeg | `{"inputs": bytes(body)}` |
| audio/x-mpeg-3 | `{"inputs": bytes(body)}` |
| audio/wave | `{"inputs": bytes(body)}` |
| audio/wav | `{"inputs": bytes(body)}` |
| audio/x-wav | `{"inputs": bytes(body)}` |
| audio/ogg | `{"inputs": bytes(body)}` |
| audio/x-audio | `{"inputs": bytes(body)}` |
| audio/webm | `{"inputs": bytes(body)}` |
| audio/webm;codecs=opus | `{"inputs": bytes(body)}` |
| audio/AMR | `{"inputs": bytes(body)}` |
| audio/amr | `{"inputs": bytes(body)}` |
| audio/AMR-WB | `{"inputs": bytes(body)}` |
| audio/AMR-WB+ | `{"inputs": bytes(body)}` |
| audio/m4a | `{"inputs": bytes(body)}` |
| audio/x-m4a | `{"inputs": bytes(body)}` |

以下是支持的 `accept` 头部列表以及返回的序列化负载。 

| Accept | 负载 |
| --- | --- |
| application/json | `JSON` |
| text/csv | `原始` |
| text/plain | `原始` |
| image/png | `二进制` |
| image/jpeg | `二进制` |
| image/jpg | `二进制` |
| image/tiff | `二进制` |
| image/bmp | `二进制` |
| image/gif | `二进制` |
| image/webp | `二进制` |
| image/x-image | `二进制` |
