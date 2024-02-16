# 概述

> 原文：[`huggingface.co/docs/api-inference/quicktour`](https://huggingface.co/docs/api-inference/quicktour)

让我们快速看一下🤗托管推理 API。

## 主要特点：

+   利用**150,000 多个 Transformer、Diffusers 或 Timm 模型**（T5、Blenderbot、Bart、GPT-2、Pegasus...）

+   上传、管理和私密地提供您的**自有模型**

+   运行分类、NER、对话、摘要、翻译、问答、嵌入提取任务

+   获得**高达 10 倍的推理加速**以减少用户延迟

+   在 CPU 上加速推理的多个支持模型

+   运行**难以部署的大型模型**

+   通过内置的**自动扩展**扩展到每秒 1,000 个请求

+   **更快地推出新的 NLP、CV、音频或 RL 功能**，因为新模型变得可用

+   在由 ML 中的参考开源项目驱动的平台上构建您的业务

## 获取您的 API 令牌

要开始使用，您需要：

+   [注册](https://huggingface.co/join)或[登录](https://huggingface.co/login)。

+   获取用户访问或 API 令牌[在您的 Hugging Face 个人资料设置中](https://huggingface.co/settings/tokens)。

您应该看到一个令牌`hf_xxxxx`（旧令牌为`api_XXXXXXXX`或`api_org_XXXXXXX`）。

如果在向 API 发送请求时未提交 API 令牌，则将无法在您的私有模型上运行推理。

## 使用 API 请求运行推理

第一步是选择要运行的模型。转到[模型中心](https://huggingface.co/models)并选择要使用的模型。如果您不确定从哪里开始，请确保检查每个 ML 任务的[推荐模型](https://api-inference.huggingface.co/docs/python/html/detailed_parameters.html#detailed-parameters)，或者[Tasks](https://huggingface.co/tasks)概述。

```py
ENDPOINT = https://api-inference.huggingface.co/models/<MODEL_ID>
```

让我们以[gpt2](https://huggingface.co/gpt2)为例。要运行推理，只需使用此代码：

PythonJavaScriptcURL

```py
import requests
API_URL = "https://api-inference.huggingface.co/models/gpt2"
headers = {"Authorization": f"Bearer {API_TOKEN}"}
def query(payload):
    response = requests.post(API_URL, headers=headers, json=payload)
    return response.json()
data = query("Can you please let us know more details about your ")
```

## API 选项和参数

根据模型配置的任务（即管道），请求将接受特定参数。在发送运行任何模型的请求时，API 选项允许您指定缓存和模型加载行为。所有 API 选项和参数在此处详细说明`详细参数`。

## 使用 CPU 加速推理

作为 API 客户，如果支持模型类型，您的 API 令牌将自动在请求中启用 CPU 加速推理。例如，如果通过我们的 API 比较 gpt2 模型推理与 CPU 加速，与在本地设置中直接运行模型推理相比，您应该测量到**~10 倍的加速**。具体的性能提升取决于模型和输入负载（以及您的本地硬件）。

要验证您是否使用了模型的 CPU 加速版本，您可以检查请求的 x-compute-type 标头，该标头应为 cpu+optimized。如果您没有看到它，这仅意味着并非所有优化都已打开。这可能是由于各种因素造成的；模型可能是最近添加到 transformers 中的，或者模型可以以多种不同的方式进行优化，最佳方式取决于您的用例。

如果您联系我们 api-enterprise@huggingface.co，我们将能够根据您的实际用例提高推理速度。

## 模型加载和延迟

托管推理 API 可以从 Hugging Face Hub 上部署的超过 100,000 个模型中动态加载共享基础设施上的预测。如果请求的模型未加载到内存中，托管推理 API 将首先将模型加载到内存中并返回 503 响应，然后才能响应预测。

如果您的使用情况需要大量数据或可预测的延迟，您可以使用我们的付费解决方案[推理端点](https://huggingface.co/inference-endpoints)来轻松部署您的模型在专用、完全托管的基础设施上。使用推理端点，您可以快速在云端、区域、CPU 或 GPU 计算实例上创建端点。
