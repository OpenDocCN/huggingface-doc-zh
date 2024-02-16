# 在服务器上运行推理

> 原始文本：[https://huggingface.co/docs/huggingface_hub/guides/inference](https://huggingface.co/docs/huggingface_hub/guides/inference)

推理是使用训练好的模型对新数据进行预测的过程。由于这个过程可能需要大量计算资源，运行在专用服务器上可能是一个有趣的选择。`huggingface_hub`库提供了一种简单的方式来调用一个运行托管模型推理的服务。您可以连接到几种服务：

+   [Inference API](https://huggingface.co/docs/api-inference/index)：一个服务，允许您在Hugging Face的基础设施上免费运行加速推理。这项服务是快速开始、测试不同模型和原型化AI产品的快速方式。

+   [Inference Endpoints](https://huggingface.co/docs/inference-endpoints/index)：一个产品，可以轻松部署模型到生产环境。推理由Hugging Face在您选择的云提供商上的专用完全托管基础设施上运行。

这些服务可以通过[InferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient)对象调用。它作为传统[InferenceApi](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceApi)客户端的替代品，为任务添加了特定支持，并处理了[Inference API](https://huggingface.co/docs/api-inference/index)和[Inference Endpoints](https://huggingface.co/docs/inference-endpoints/index)上的推理。在[Legacy InferenceAPI client](#legacy-inferenceapi-client)部分了解如何迁移到新客户端。

[InferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient)是一个Python客户端，通过HTTP调用我们的API。如果您想使用您喜欢的工具（curl、postman等）直接进行HTTP调用，请参考[Inference API](https://huggingface.co/docs/api-inference/index)或[Inference Endpoints](https://huggingface.co/docs/inference-endpoints/index)文档页面。

对于Web开发，已发布了[JS客户端](https://huggingface.co/docs/huggingface.js/inference/README)。如果您对游戏开发感兴趣，可以查看我们的[C#项目](https://github.com/huggingface/unity-api)。

## 入门

让我们开始一个文本到图像的任务：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()

>>> image = client.text_to_image("An astronaut riding a horse on the moon.")
>>> image.save("astronaut.png")
```

我们使用默认参数初始化了一个[InferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient)。您唯一需要知道的是您想要执行的[任务](#supported-tasks)。默认情况下，客户端将连接到推理API并选择一个模型来完成任务。在我们的示例中，我们从文本提示生成了一张图片。返回的值是一个`PIL.Image`对象，可以保存到文件中。

API设计简单。并非所有参数和选项都适用或描述给最终用户。如果您想了解每个任务可用的所有参数，请查看[此页面](https://huggingface.co/docs/api-inference/detailed_parameters)。

### 使用特定模型

如果您想使用特定模型怎么办？您可以将其指定为参数或直接在实例级别指定：

```py
>>> from huggingface_hub import InferenceClient
# Initialize client for a specific model
>>> client = InferenceClient(model="prompthero/openjourney-v4")
>>> client.text_to_image(...)
# Or use a generic client but pass your model as an argument
>>> client = InferenceClient()
>>> client.text_to_image(..., model="prompthero/openjourney-v4")
```

在Hugging Face Hub上有超过200k个模型！[InferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient)中的每个任务都附带一个推荐模型。请注意，HF的推荐可能会随时间而变化，没有事先通知。因此，最好在决定后明确设置一个模型。此外，在大多数情况下，您可能会对找到一个符合*您*需求的模型感兴趣。访问Hub上的[Models](https://huggingface.co/models)页面来探索您的可能性。

### 使用特定URL

我们上面看到的示例使用了免费托管的推理API。这对于快速原型设计和测试非常有用。一旦您准备将模型部署到生产环境中，您将需要使用专用基础设施。这就是[推理端点](https://huggingface.co/docs/inference-endpoints/index)的作用所在。它允许您部署任何模型并将其公开为私有API。一旦部署，您将获得一个URL，您可以使用与之前完全相同的代码连接到该URL，只需更改`model`参数即可：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient(model="https://uu149rez6gw9ehej.eu-west-1.aws.endpoints.huggingface.cloud/deepfloyd-if")
# or
>>> client = InferenceClient()
>>> client.text_to_image(..., model="https://uu149rez6gw9ehej.eu-west-1.aws.endpoints.huggingface.cloud/deepfloyd-if")
```

### 身份验证

使用[InferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient)进行的调用可以使用[用户访问令牌](https://huggingface.co/docs/hub/security-tokens)进行身份验证。默认情况下，如果您已登录，则它将使用保存在您的计算机上的令牌（查看[如何进行身份验证](https://huggingface.co/docs/huggingface_hub/quick-start#authentication)）。如果您未登录，您可以将您的令牌作为实例参数传递：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient(token="hf_***")
```

在使用推理API时，身份验证并不是强制性的。但是，经过身份验证的用户可以获得更高的免费使用额度。如果您想在私有模型或私有端点上运行推理，则令牌也是必需的。

## 支持的任务

[InferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient)的目标是提供在Hugging Face模型上运行推理的最简单接口。它具有一个简单的API，支持最常见的任务。以下是当前支持的任务列表：

| 领域 | 任务 | 支持 | 文档 |
| --- | --- | --- | --- |
| 音频 | [音频分类](https://huggingface.co/tasks/audio-classification) | ✅ | [audio_classification()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.audio_classification) |
|  | [自动语音识别](https://huggingface.co/tasks/automatic-speech-recognition) | ✅ | [automatic_speech_recognition()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.automatic_speech_recognition) |
|  | [文本转语音](https://huggingface.co/tasks/text-to-speech) | ✅ | [text_to_speech()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.text_to_speech) |
| 计算机视觉 | [图像分类](https://huggingface.co/tasks/image-classification) | ✅ | [image_classification()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.image_classification) |
|  | [图像分割](https://huggingface.co/tasks/image-segmentation) | ✅ | [image_segmentation()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.image_segmentation) |
|  | [图像到图像](https://huggingface.co/tasks/image-to-image) | ✅ | [image_to_image()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.image_to_image) |
|  | [图像到文本](https://huggingface.co/tasks/image-to-text) | ✅ | [image_to_text()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.image_to_text) |
|  | [目标检测](https://huggingface.co/tasks/object-detection) | ✅ | [object_detection()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.object_detection) |
|  | [文本到图像](https://huggingface.co/tasks/text-to-image) | ✅ | [text_to_image()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.text_to_image) |
|  | [零样本图像分类](https://huggingface.co/tasks/zero-shot-image-classification) | ✅ | [zero_shot_image_classification()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.zero_shot_image_classification) |
| 多模态 | [文档问答](https://huggingface.co/tasks/document-question-answering) | ✅ | [document_question_answering()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.document_question_answering) |
|  | [视觉问答](https://huggingface.co/tasks/visual-question-answering) | ✅ | [visual_question_answering()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.visual_question_answering) |
| NLP | [对话](https://huggingface.co/tasks/conversational) | ✅ | [conversational()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.conversational) |
|  | [特征提取](https://huggingface.co/tasks/feature-extraction) | ✅ | [feature_extraction()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.feature_extraction) |
|  | [填充掩码](https://huggingface.co/tasks/fill-mask) | ✅ | [fill_mask()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.fill_mask) |
|  | [问答](https://huggingface.co/tasks/question-answering) | ✅ | [question_answering()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.question_answering) |
|  | [句子相似度](https://huggingface.co/tasks/sentence-similarity) | ✅ | [sentence_similarity()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.sentence_similarity) |
|  | [摘要](https://huggingface.co/tasks/summarization) | ✅ | [summarization()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.summarization) |
|  | [表格问答](https://huggingface.co/tasks/table-question-answering) | ✅ | [table_question_answering()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.table_question_answering) |
|  | [文本分类](https://huggingface.co/tasks/text-classification) | ✅ | [text_classification()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.text_classification) |
|  | [文本生成](https://huggingface.co/tasks/text-generation) | ✅ | [text_generation()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.text_generation) |
|  | [标记分类](https://huggingface.co/tasks/token-classification) | ✅ | [token_classification()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.token_classification) |
|  | [翻译](https://huggingface.co/tasks/translation) | ✅ | [translation()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.translation) |
|  | [零样本分类](https://huggingface.co/tasks/zero-shot-classification) | ✅ | [zero_shot_classification()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.zero_shot_classification) |
| 表格 | [表格分类](https://huggingface.co/tasks/tabular-classification) | ✅ | [tabular_classification()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.tabular_classification) |
|  | [表格回归](https://huggingface.co/tasks/tabular-regression) | ✅ | [tabular_regression()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.tabular_regression) |

查看[Tasks](https://huggingface.co/tasks)页面，了解更多关于每个任务的信息，如何使用它们以及每个任务的最受欢迎的模型。

## 自定义请求

然而，并非总是可能覆盖所有用例。对于自定义请求，[InferenceClient.post()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.post)方法为您提供了灵活性，以向推断API发送任何请求。例如，您可以指定如何解析输入和输出。在下面的示例中，生成的图像以原始字节返回，而不是将其解析为`PIL Image`。如果您的设置中没有安装`Pillow`，并且只关心图像的二进制内容，则这可能会有所帮助。[InferenceClient.post()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient.post)还可用于处理尚未正式支持的任务。

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> response = client.post(json={"inputs": "An astronaut riding a horse on the moon."}, model="stabilityai/stable-diffusion-2-1")
>>> response.content # raw bytes
b'...'
```

## 异步客户端

还提供了一个基于`asyncio`和`aiohttp`的异步版本的客户端。您可以直接安装`aiohttp`，也可以使用`[inference]`额外功能：

```py
pip install --upgrade huggingface_hub[inference]
# or
# pip install aiohttp
```

安装后，所有异步API端点都可以通过[AsyncInferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.AsyncInferenceClient)访问。它的初始化和API与仅同步版本严格相同。

```py
# Code must be run in a asyncio concurrent context.
# $ python -m asyncio
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()

>>> image = await client.text_to_image("An astronaut riding a horse on the moon.")
>>> image.save("astronaut.png")

>>> async for token in await client.text_generation("The Huggingface Hub is", stream=True):
...     print(token, end="")
 a platform for sharing and discussing ML-related content.
```

有关`asyncio`模块的更多信息，请参考[官方文档](https://docs.python.org/3/library/asyncio.html)。

## 高级技巧

在上面的部分中，我们看到了[InferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient)的主要方面。让我们深入一些更高级的技巧。

### 超时

在进行推断时，有两个主要原因会导致超时：

+   推断过程需要很长时间才能完成。

+   例如，当推断API首次加载模型时，模型不可用。

[InferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient)有一个全局的`timeout`参数来处理这两个方面。默认情况下，它设置为`None`，这意味着客户端将无限期地等待推断完成。如果您希望在工作流程中有更多控制，可以将其设置为特定的秒数值。如果超时延迟到期，将引发一个[InferenceTimeoutError](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceTimeoutError)。您可以捕获它并在您的代码中处理它：

```py
>>> from huggingface_hub import InferenceClient, InferenceTimeoutError
>>> client = InferenceClient(timeout=30)
>>> try:
...     client.text_to_image(...)
... except InferenceTimeoutError:
...     print("Inference timed out after 30s.")
```

### 二进制输入

有些任务需要二进制输入，例如处理图像或音频文件时。在这种情况下，[InferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient)尝试尽可能宽松，并接受不同类型：

+   原始的`bytes`

+   一个类似文件的对象，以二进制形式打开（`with open("audio.flac", "rb") as f: ...`）

+   一个指向本地文件的路径（`str`或`Path`）

+   一个指向远程文件的URL（例如`https://...`）。在这种情况下，文件将在发送到推断API之前被下载到本地。

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.image_classification("https://upload.wikimedia.org/wikipedia/commons/thumb/4/43/Cute_dog.jpg/320px-Cute_dog.jpg")
[{'score': 0.9779096841812134, 'label': 'Blenheim spaniel'}, ...]
```

## 传统的InferenceAPI客户端

[InferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient)充当传统[InferenceApi](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceApi)客户端的替代品。它为任务添加了特定支持，并处理了[Inference API](https://huggingface.co/docs/api-inference/index)和[Inference Endpoints](https://huggingface.co/docs/inference-endpoints/index)上的推断。

以下是一个简短的指南，帮助您从[InferenceApi](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceApi)迁移到[InferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient)。

### 初始化

更改自

```py
>>> from huggingface_hub import InferenceApi
>>> inference = InferenceApi(repo_id="bert-base-uncased", token=API_TOKEN)
```

至

```py
>>> from huggingface_hub import InferenceClient
>>> inference = InferenceClient(model="bert-base-uncased", token=API_TOKEN)
```

### 在特定任务上运行

更改自

```py
>>> from huggingface_hub import InferenceApi
>>> inference = InferenceApi(repo_id="paraphrase-xlm-r-multilingual-v1", task="feature-extraction")
>>> inference(...)
```

至

```py
>>> from huggingface_hub import InferenceClient
>>> inference = InferenceClient()
>>> inference.feature_extraction(..., model="paraphrase-xlm-r-multilingual-v1")
```

这是将您的代码适应[InferenceClient](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_client#huggingface_hub.InferenceClient)的推荐方式。它让您可以从诸如`feature_extraction`之类的特定任务方法中受益。

### 运行自定义请求

从

```py
>>> from huggingface_hub import InferenceApi
>>> inference = InferenceApi(repo_id="bert-base-uncased")
>>> inference(inputs="The goal of life is [MASK].")
[{'sequence': 'the goal of life is life.', 'score': 0.10933292657136917, 'token': 2166, 'token_str': 'life'}]
```

到

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> response = client.post(json={"inputs": "The goal of life is [MASK]."}, model="bert-base-uncased")
>>> response.json()
[{'sequence': 'the goal of life is life.', 'score': 0.10933292657136917, 'token': 2166, 'token_str': 'life'}]
```

### 带参数运行

从

```py
>>> from huggingface_hub import InferenceApi
>>> inference = InferenceApi(repo_id="typeform/distilbert-base-uncased-mnli")
>>> inputs = "Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!"
>>> params = {"candidate_labels":["refund", "legal", "faq"]}
>>> inference(inputs, params)
{'sequence': 'Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!', 'labels': ['refund', 'faq', 'legal'], 'scores': [0.9378499388694763, 0.04914155602455139, 0.013008488342165947]}
```

到

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> inputs = "Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!"
>>> params = {"candidate_labels":["refund", "legal", "faq"]}
>>> response = client.post(json={"inputs": inputs, "parameters": params}, model="typeform/distilbert-base-uncased-mnli")
>>> response.json()
{'sequence': 'Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!', 'labels': ['refund', 'faq', 'legal'], 'scores': [0.9378499388694763, 0.04914155602455139, 0.013008488342165947]}
```
