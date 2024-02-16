# 元数据解析

> 原文链接: [https://huggingface.co/docs/safetensors/metadata_parsing](https://huggingface.co/docs/safetensors/metadata_parsing)

考虑到这种格式的简单性，非常简单和高效地获取和解析有关Safetensors权重的元数据 - 即张量列表、它们的类型和它们的形状或参数数量 - 使用小的[(范围)HTTP请求](https://developer.mozilla.org/en-US/docs/Web/HTTP/Range_requests)。

这种解析已经在[`huggingface.js`](https://huggingface.co/docs/huggingface.js/main/en/hub/modules#parsesafetensorsmetadata)中用JS实现（下面是示例代码），但在任何语言中都会类似。

## 示例用例

可能有许多潜在的用例。例如，我们在HuggingFace Hub上使用它来显示具有safetensors权重的模型的信息：

![](../Images/555be3a5ebb40fdd1a0fbd5277d15b1a.png) ![](../Images/1feaa6e3290eb5ae97b1e2e5279e0de1.png)![](../Images/213783eaa480d7a50c99889522778fdd.png) ![](../Images/b63c0bdd195755851d17295e7a367d1b.png)

## 用法

httpjavascriptpython

从[🤗 Hub](hf.co/models)上，您可以通过[HTTP范围请求](https://developer.mozilla.org/en-US/docs/Web/HTTP/Range_requests)获取模型的元数据，而不是下载带有所有权重的完整safetensors文件。在下面的这个示例python脚本中（您可以使用任何支持HTTP请求的语言），我们正在解析[gpt2](https://huggingface.co/gpt2/blob/main/model.safetensors)的元数据。

```py
import requests # pip install requests
import struct

def parse_single_file(url):
    # Fetch the first 8 bytes of the file
    headers = {'Range': 'bytes=0-7'}
    response = requests.get(url, headers=headers)
    # Interpret the bytes as a little-endian unsigned 64-bit integer
    length_of_header = struct.unpack('<Q', response.content)[0]
    # Fetch length_of_header bytes starting from the 9th byte
    headers = {'Range': f'bytes=8-{7 + length_of_header}'}
    response = requests.get(url, headers=headers)
    # Interpret the response as a JSON object
    header = response.json()
    return header

url = "https://huggingface.co/gpt2/resolve/main/model.safetensors"
header = parse_single_file(url)

print(header)
# {
#   "__metadata__": { "format": "pt" },
#   "h.10.ln_1.weight": {
#     "dtype": "F32",
#     "shape": [768],
#     "data_offsets": [223154176, 223157248]
#   },
#   ...
# }
```

## 示例输出

例如，这里是HuggingFace Hub上几个模型每种数据类型的参数数量。还可以查看[此问题](https://github.com/huggingface/safetensors/issues/44)以获取更多用法示例。

| 模型 | safetensors | 参数 |
| --- | --- | --- |
| [gpt2](https://huggingface.co/gpt2?show_tensors=true) | 单文件 | { ‘F32’ => 137022720 } |
| [roberta-base](https://huggingface.co/roberta-base?show_tensors=true) | 单文件 | { ‘F32’ => 124697433, ‘I64’ => 514 } |
| [Jean-Baptiste/camembert-ner](https://huggingface.co/Jean-Baptiste/camembert-ner?show_tensors=true) | 单文件 | { ‘F32’ => 110035205, ‘I64’ => 514 } |
| [roberta-large](https://huggingface.co/roberta-large?show_tensors=true) | 单文件 | { ‘F32’ => 355412057, ‘I64’ => 514 } |
| [distilbert-base-german-cased](https://huggingface.co/distilbert-base-german-cased?show_tensors=true) | 单文件 | { ‘F32’ => 67431550 } |
| [EleutherAI/gpt-neox-20b](https://huggingface.co/EleutherAI/gpt-neox-20b?show_tensors=true) | 分片 | { ‘F16’ => 20554568208, ‘U8’ => 184549376 } |
| [bigscience/bloom-560m](https://huggingface.co/bigscience/bloom-560m?show_tensors=true) | 单文件 | { ‘F16’ => 559214592 } |
| [bigscience/bloom](https://huggingface.co/bigscience/bloom?show_tensors=true) | 分片 | { ‘BF16’ => 176247271424 } |
| [bigscience/bloom-3b](https://huggingface.co/bigscience/bloom-3b?show_tensors=true) | 单文件 | { ‘F16’ => 3002557440 } |
