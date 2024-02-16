# 推断

> 原始文本：[`huggingface.co/docs/huggingface_hub/package_reference/inference_client`](https://huggingface.co/docs/huggingface_hub/package_reference/inference_client)

推断是使用训练好的模型对新数据进行预测的过程。由于这个过程可能需要大量计算资源，运行在专用服务器上可能是一个有趣的选择。`huggingface_hub`库提供了一种简单的方法来调用一个运行托管模型推断的服务。您可以连接到几种服务：

+   [推断 API](https://huggingface.co/docs/api-inference/index)：一个允许您在 Hugging Face 基础设施上加速推断的服务，免费使用。这项服务是一个快速开始、测试不同模型和原型化 AI 产品的方式。

+   [推断端点](https://huggingface.co/inference-endpoints)：一个产品，可以轻松部署模型到生产环境。推断由 Hugging Face 在您选择的云提供商上的专用、完全托管的基础设施上运行。

这些服务可以通过 InferenceClient 对象调用。请参考此指南获取更多关于如何使用它的信息。

## 推断客户端

### `class huggingface_hub.InferenceClient`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L105)

```py
( model: Optional = None token: Union = None timeout: Optional = None headers: Optional = None cookies: Optional = None )
```

参数

+   `model` (`str`, `可选`) — 用于进行推断的模型。可以是托管在 Hugging Face Hub 上的模型 ID，例如`bigcode/starcoder`，也可以是指向部署的推断端点的 URL。默认为 None，此时将自动选择适合任务的推荐模型。

+   `token` (`str`, *可选*) — Hugging Face token。默认为本地保存的 token。如果不想将您的 token 发送到服务器，请传递`token=False`。

+   `timeout` (`float`, `可选`) — 等待服务器响应的最大秒数。在推断 API 中加载新模型可能需要几分钟。默认为 None，意味着会循环直到服务器可用。

+   `headers` (`Dict[str, str]`, `可选`) — 要发送到服务器的额外标头。默认只发送授权和用户代理标头。此字典中的值将覆盖默认值。

+   `cookies` (`Dict[str, str]`, `可选`) — 要发送到服务器的额外 cookie。

初始化一个新的推断客户端。

InferenceClient 旨在提供一个统一的体验来执行推断。该客户端可以无缝地与（免费的）推断 API 或自托管的推断端点一起使用。

#### `audio_classification`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L264)

```py
( audio: Union model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `audio` (Union[str, Path, bytes, BinaryIO]) — 要分类的音频内容。可以是原始音频字节、本地音频文件或指向音频文件的 URL。

+   `model` (`str`, *可选*) — 用于音频分类的模型。可以是托管在 Hugging Face Hub 上的模型 ID 或指向部署的推断端点的 URL。如果未提供，默认将使用音频分类的推荐模型。

返回

`List[Dict]`

包含预测标签及其置信度的分类输出。

引发

InferenceTimeoutError 或`HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

对提供的音频内容进行音频分类。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.audio_classification("audio.flac")
[{'score': 0.4976358711719513, 'label': 'hap'}, {'score': 0.3677836060523987, 'label': 'neu'},...]
```

自动语音识别

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L302)

```py
( audio: Union model: Optional = None ) → export const metadata = 'undefined';str
```

参数

+   `audio`（Unionstr, Path, bytes, BinaryIO）—要转录的内容。可以是原始音频字节、本地音频文件或音频文件的 URL。

+   `model`（`str`，*可选*）—用于 ASR 的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的 ASR 模型将被使用。

返回

str

转录的文本。

引发

[InferenceTimeoutError 或`HTTPError`

+   InferenceTimeoutError —如果模型不可用或请求超时。

+   `HTTPError` —如果请求失败，并且 HTTP 状态代码不是 HTTP 503。

对给定音频内容执行自动语音识别（ASR 或音频转文本）。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.automatic_speech_recognition("hello_world.flac")
"hello world"
```

#### `对话`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L338)

```py
( text: str generated_responses: Optional = None past_user_inputs: Optional = None parameters: Optional = None model: Optional = None ) → export const metadata = 'undefined';Dict
```

参数

+   `text`（`str`）—对话中用户的最后输入。

+   `generated_responses`（`List[str]`，*可选*）—与模型先前回复对应的字符串列表。默认为 None。

+   `past_user_inputs`（`List[str]`，*可选*）—与用户先前回复对应的字符串列表。应与`generated_responses`的长度相同。默认为 None。

+   `parameters`（`Dict[str, Any]`，*可选*）—对话任务的附加参数。默认为 None。有关可用参数的更多详细信息，请参考[此页面](https://huggingface.co/docs/api-inference/detailed_parameters#conversational-task)

+   `model`（`str`，*可选*）—用于对话任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的对话模型将被使用。默认为 None。

返回

`Dict`

生成的对话输出。

引发

InferenceTimeoutError 或`HTTPError`

+   InferenceTimeoutError —如果模型不可用或请求超时。

+   `HTTPError` —如果请求失败，并且 HTTP 状态代码不是 HTTP 503。

根据给定的输入文本生成对话响应（即与 API 聊天）。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> output = client.conversational("Hi, who are you?")
>>> output
{'generated_text': 'I am the one who knocks.', 'conversation': {'generated_responses': ['I am the one who knocks.'], 'past_user_inputs': ['Hi, who are you?']}, 'warnings': ['Setting `pad_token_id` to `eos_token_id`:50256 for open-end generation.']}
>>> client.conversational(
...     "Wow, that's scary!",
...     generated_responses=output["conversation"]["generated_responses"],
...     past_user_inputs=output["conversation"]["past_user_inputs"],
... )
```

#### `文档问答`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L443)

```py
( image: Union question: str model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `image`（`Union[str, Path, bytes, BinaryIO]`）—上下文的输入图像。可以是原始字节、图像文件或在线图像的 URL。

+   `question`（`str`）—要回答的问题。

+   `model`（`str`，*可选*）—用于文档问答任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的文档问答模型将被使用。默认为 None。

返回

`List[Dict]`

包含预测标签、相关概率、单词 ID 和页码的字典列表。

引发

InferenceTimeoutError 或`HTTPError`

+   InferenceTimeoutError —如果模型不可用或请求超时。

+   `HTTPError` —如果请求失败，并且 HTTP 状态代码不是 HTTP 503。

回答文档图像上的问题。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.document_question_answering(image="https://huggingface.co/spaces/impira/docquery/resolve/2359223c1837a7587402bda0f2643382a6eefeab/invoice.png", question="What is the invoice number?")
[{'score': 0.42515629529953003, 'answer': 'us-001', 'start': 16, 'end': 16}]
```

#### `特征提取`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L484)

```py
( text: str model: Optional = None ) → export const metadata = 'undefined';np.ndarray
```

参数

+   `text` (`str`) — 要嵌入的文本。

+   `model` (`str`，*可选*) — 用于对话任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的对话模型将被使用。默认为 None。

返回

`np.ndarray`

将输入文本表示为 float32 numpy 数组的嵌入。

Raises

InferenceTimeoutError 或`HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，状态码不是 HTTP 503。

为给定文本生成嵌入。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.feature_extraction("Hi, who are you?")
array([[ 2.424802  ,  2.93384   ,  1.1750331 , ...,  1.240499, -0.13776633, -0.7889173 ],
[-0.42943227, -0.6364878 , -1.693462  , ...,  0.41978157, -2.4336355 ,  0.6162071 ],
...,
[ 0.28552425, -0.928395  , -1.2077185 , ...,  0.76810825, -2.1069427 ,  0.6236161 ]], dtype=float32)
```

#### `fill_mask`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L520)

```py
( text: str model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `text` (`str`) — 要填充的字符串，必须包含[MASK]标记（检查模型卡片以获取掩码的确切名称）。

+   `model` (`str`，*可选*) — 用于填充掩码任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的填充掩码模型将被使用。默认为 None。

返回

`List[Dict]`

包含预测标签、相关概率、标记引用和完成文本的填充掩码输出字典列表。

Raises

InferenceTimeoutError 或`HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，状态码不是 HTTP 503。

用缺失的单词（确切地说是标记）填充空白。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.fill_mask("The goal of life is <mask>.")
[{'score': 0.06897063553333282,
'token': 11098,
'token_str': ' happiness',
'sequence': 'The goal of life is happiness.'},
{'score': 0.06554922461509705,
'token': 45075,
'token_str': ' immortality',
'sequence': 'The goal of life is immortality.'}]
```

#### `get_model_status`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1941)

```py
( model: Optional = None ) → export const metadata = 'undefined';ModelStatus
```

参数

+   `model` (`str`，*可选*) — 要检查状态的模型标识符。如果未提供模型，则将使用与此 InferenceClient 实例关联的模型。只能检查 InferenceAPI 服务，因此标识符不能是 URL。

返回

`ModelStatus`

包含有关模型状态的 ModelStatus 数据类的实例：加载、状态、计算类型和框架的信息。

获取托管在推理 API 上的模型的状态。

当您已经知道要使用哪个模型并想要检查其可用性时，此端点非常有用。如果要发现已部署的模型，您应该使用 list_deployed_models()。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.get_model_status("bigcode/starcoder")
ModelStatus(loaded=True, state='Loaded', compute_type='gpu', framework='text-generation-inference')
```

#### `get_recommended_model`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1917)

```py
( task: str ) → export const metadata = 'undefined';str
```

参数

+   `task` (`str`) — 要获取 Hugging Face 推荐的模型的任务。所有可用任务可以在[这里](https://huggingface.co/tasks)找到。

返回

`str`

推荐的输入任务模型的名称。

Raises

`ValueError`

+   `ValueError` — 如果 Hugging Face 没有对输入任务的推荐。

获取 Hugging Face 推荐的输入任务模型。

#### `image_classification`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L560)

```py
( image: Union model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `image` (`Union[str, Path, bytes, BinaryIO]`) — 要分类的图像。可以是原始字节、图像文件或在线图像的 URL。

+   `model` (`str`, *optional*) — 用于图像分类的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，则将使用默认推荐的图像分类模型。

返回

`List[Dict]`

包含预测标签和相关概率的字典列表。

引发

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

使用指定模型对给定图像进行图像分类。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.image_classification("https://upload.wikimedia.org/wikipedia/commons/thumb/4/43/Cute_dog.jpg/320px-Cute_dog.jpg")
[{'score': 0.9779096841812134, 'label': 'Blenheim spaniel'}, ...]
```

#### `image_segmentation`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L596)

```py
( image: Union model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `image` (`Union[str, Path, bytes, BinaryIO]`) — 要分割的图像。可以是原始字节、图像文件或在线图像的 URL。

+   `model` (`str`, *optional*) — 用于图像分割的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，则将使用默认推荐的图像分割模型。

返回

`List[Dict]`

包含分割掩模和相关属性的字典列表。

引发

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

使用指定模型对给定图像进行图像分割。

如果要处理图像，必须安装 `PIL` (`pip install Pillow`)。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.image_segmentation("cat.jpg"):
[{'score': 0.989008, 'label': 'LABEL_184', 'mask': <PIL.PngImagePlugin.PngImageFile image mode=L size=400x300 at 0x7FDD2B129CC0>}, ...]
```

#### `image_to_image`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L647)

```py
( image: Union prompt: Optional = None negative_prompt: Optional = None height: Optional = None width: Optional = None num_inference_steps: Optional = None guidance_scale: Optional = None model: Optional = None **kwargs ) → export const metadata = 'undefined';Image
```

参数

+   `image` (`Union[str, Path, bytes, BinaryIO]`) — 用于翻译的输入图像。可以是原始字节、图像文件或在线图像的 URL。

+   `prompt` (`str`, *optional*) — 用于指导图像生成的文本提示。

+   `negative_prompt` (`str`, *optional*) — 用于指导翻译过程的负面提示。

+   `height` (`int`, *optional*) — 生成图像的像素高度。

+   `width` (`int`, *optional*) — 生成图像的像素宽度。

+   `num_inference_steps` (`int`, *optional*) — 降噪步骤的数量。更多的降噪步骤通常会导致更高质量的图像，但会降低推理速度。

+   `guidance_scale` (`float`, *optional*) — 更高的引导比例鼓励生成与文本 `prompt` 密切相关的图像，通常以降低图像质量为代价。

+   `model` (`str`, *optional*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数会覆盖实例级别定义的模型。默认为 None。

返回

`Image`

翻译后的图像。

引发

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

使用指定模型进行图像到图像的翻译。

如果要使用图像进行工作，则必须安装`PIL`（`pip install Pillow`）。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> image = client.image_to_image("cat.jpg", prompt="turn the cat into a tiger")
>>> image.save("tiger.jpg")
```

#### `image_to_text`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L731)

```py
( image: Union model: Optional = None ) → export const metadata = 'undefined';str
```

参数

+   `image`（`Union[str，Path，bytes，BinaryIO]`）—要添加标题的输入图像。它可以是原始字节，图像文件或在线图像的 URL。

+   `model`（`str`，*可选*）—用于推断的模型。可以是托管在 Hugging Face Hub 上的模型 ID 或部署的推断端点的 URL。此参数将覆盖实例级别定义的模型。默认为 None。

返回

`str`

生成的文本。

Raises

InferenceTimeoutError 或`HTTPError`

+   InferenceTimeoutError —如果模型不可用或请求超时。

+   `HTTPError`—如果请求失败，并且 HTTP 错误状态代码不是 HTTP 503。

接受输入图像并返回文本。

根据您的用例（图像字幕，光学字符识别（OCR），Pix2Struct 等），模型的输出可能会有很大的不同。请查看模型卡片以了解有关模型特定性的更多信息。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.image_to_text("cat.jpg")
'a cat standing in a grassy field '
>>> client.image_to_text("https://upload.wikimedia.org/wikipedia/commons/thumb/4/43/Cute_dog.jpg/320px-Cute_dog.jpg")
'a dog laying on the grass next to a flower pot '
```

#### `list_deployed_models`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L767)

```py
( frameworks: Union = None ) → export const metadata = 'undefined';Dict[str, List[str]]
```

参数

+   `frameworks`（`Literal["all"]`或`List[str]`或`str`，*可选*）—要过滤的框架。默认情况下，仅测试可用框架的子集。如果设置为“all”，将测试所有可用的框架。还可以提供单个框架或一组自定义框架以进行检查。

返回

`Dict[str，List[str]]`

将任务名称映射到模型 ID 的排序列表的字典。

列出当前部署在推断 API 服务上的模型。

此助手按框架检查部署的模型。默认情况下，它将检查支持的四个主要框架，并占托管模型的 95％。但是，如果您想要完整的模型列表，可以指定`frameworks="all"`作为输入。或者，如果您事先知道您感兴趣的框架，也可以将搜索限制为该框架（例如`frameworks="text-generation-inference"`）。检查的框架越多，花费的时间就越多。

此端点主要用于可发现性。如果您已经知道要使用哪个模型并想要检查其可用性，则可以直接使用 get_model_status()。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()

# Discover zero-shot-classification models currently deployed
>>> models = client.list_deployed_models()
>>> models["zero-shot-classification"]
['Narsil/deberta-large-mnli-zero-cls', 'facebook/bart-large-mnli', ...]

# List from only 1 framework
>>> client.list_deployed_models("text-generation-inference")
{'text-generation': ['bigcode/starcoder', 'meta-llama/Llama-2-70b-chat-hf', ...], ...}
```

#### `object_detection`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L842)

```py
( image: Union model: Optional = None ) → export const metadata = 'undefined';List[ObjectDetectionOutput]
```

参数

+   `image`（`Union[str，Path，bytes，BinaryIO]`）—要在其上检测对象的图像。它可以是原始字节，图像文件或在线图像的 URL。

+   `model`（`str`，*可选*）—用于对象检测的模型。可以是托管在 Hugging Face Hub 上的模型 ID 或部署的推断端点的 URL。如果未提供，默认推荐的对象检测模型（DETR）将被使用。

返回

`List[ObjectDetectionOutput]`

包含边界框和相关属性的字典列表。

Raises

InferenceTimeoutError 或`HTTPError`或`ValueError`

+   InferenceTimeoutError —如果模型不可用或请求超时。

+   `HTTPError`—如果请求失败，并且 HTTP 错误状态代码不是 HTTP 503。

+   `ValueError`—如果请求输出不是列表。

使用指定模型对给定图像执行对象检测。

如果要处理图像，必须安装`PIL`（`pip install Pillow`）。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.object_detection("people.jpg"):
[{"score":0.9486683011054993,"label":"person","box":{"xmin":59,"ymin":39,"xmax":420,"ymax":510}}, ... ]
```

#### `post`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L172)

```py
( json: Union = None data: Union = None model: Optional = None task: Optional = None stream: bool = False ) → export const metadata = 'undefined';bytes
```

Parameters

+   `json` (`Union[str, Dict, List]`, *optional*) — 要发送到请求体中的 JSON 数据，针对每个任务特定。默认为 None。

+   `data` (`Union[str, Path, bytes, BinaryIO]`, *optional*) — 要发送到请求体中的内容，针对每个任务特定。可以是原始字节、指向已打开文件的指针、本地文件路径或在线资源的 URL（图像、音频文件等）。如果同时传递了`json`和`data`，则`data`将优先。必须提供至少`json`或`data`。默认为 None。

+   `model` (`str`, *optional*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是指向部署的推理端点的 URL。将覆盖实例级别定义的模型。默认为 None。

+   `task` (`str`, *optional*) — 在推理上执行的任务。所有可用任务可以在[这里](https://huggingface.co/tasks)找到。仅在未提供`model`时才用于默认推荐模型。必须提供至少`model`或`task`。默认为 None。

+   `stream` (`bool`, *optional*) — 是否迭代流式 API。

Returns

bytes

服务器返回的原始字节。

Raises

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，并且 HTTP 状态代码不是 HTTP 503。

向推理服务器发出 POST 请求。

#### `question_answering`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L890)

```py
( question: str context: str model: Optional = None ) → export const metadata = 'undefined';Dict
```

Parameters

+   `question` (`str`) — 待回答的问题。

+   `context` (`str`) — 问题的上下文。

+   `model` (`str`) — 用于问答任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是指向部署的推理端点的 URL。

Returns

`Dict`

包含得分、起始索引、结束索引和答案的问答输出的字典。

Raises

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，并且 HTTP 状态代码不是 HTTP 503。

从给定文本中检索问题的答案。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.question_answering(question="What's my name?", context="My name is Clara and I live in Berkeley.")
{'score': 0.9326562285423279, 'start': 11, 'end': 16, 'answer': 'Clara'}
```

#### `sentence_similarity`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L931)

```py
( sentence: str other_sentences: List model: Optional = None ) → export const metadata = 'undefined';List[float]
```

Parameters

+   `sentence` (`str`) — 用于与其他句子进行比较的主要句子。

+   `other_sentences` (`List[str]`) — 要进行比较的句子列表。

+   `model` (`str`, *optional*) — 用于对话任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是指向部署的推理端点的 URL。如果未提供，默认将使用推荐的对话模型。默认为 None。

Returns

`List[float]`

表示输入文本的嵌入。

Raises

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，并且 HTTP 状态代码不是 HTTP 503。

通过比较它们的嵌入来计算句子与其他句子列表之间的语义相似性。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.sentence_similarity(
...     "Machine learning is so easy.",
...     other_sentences=[
...         "Deep learning is so straightforward.",
...         "This is so difficult, like rocket science.",
...         "I can't believe how much I struggled with this.",
...     ],
... )
[0.7785726189613342, 0.45876261591911316, 0.2906220555305481]
```

#### `summarization`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L978)

```py
( text: str parameters: Optional = None model: Optional = None ) → export const metadata = 'undefined';str
```

参数

+   `text` (`str`) — 要总结的输入文本。

+   `parameters` (`Dict[str, Any]`, *optional*) — 用于总结的额外参数。查看此 [页面](https://huggingface.co/docs/api-inference/detailed_parameters#summarization-task) 获取更多详细信息。

+   `model` (`str`, *optional*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数会覆盖实例级别定义的模型。默认为 None。

返回

`str`

生成的摘要文本。

Raises

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

使用指定的模型生成给定文本的摘要。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.summarization("The Eiffel tower...")
'The Eiffel tower is one of the most famous landmarks in the world....'
```

#### `table_question_answering`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1021)

```py
( table: Dict query: str model: Optional = None ) → export const metadata = 'undefined';Dict
```

参数

+   `table` (`str`) — 以字典列表表示的数据表，其中条目是标题，列表是所有值，所有列表必须具有相同的大小。

+   `query` (`str`) — 您想要询问表格的纯文本查询。

+   `model` (`str`) — 用于表格问题回答任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。

返回

`Dict`

包含答案、坐标、单元格和使用的聚合器的表格问题回答输出的字典。

Raises

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

从表中给定的信息中检索问题的答案。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> query = "How many stars does the transformers repository have?"
>>> table = {"Repository": ["Transformers", "Datasets", "Tokenizers"], "Stars": ["36542", "4512", "3934"]}
>>> client.table_question_answering(table, query, model="google/tapas-base-finetuned-wtq")
{'answer': 'AVERAGE > 36542', 'coordinates': [[0, 1]], 'cells': ['36542'], 'aggregator': 'AVERAGE'}
```

#### `tabular_classification`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1066)

```py
( table: Dict model: str ) → export const metadata = 'undefined';List
```

参数

+   `table` (`Dict[str, Any]`) — 用于分类的属性集。

+   `model` (`str`) — 用于表格分类任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。

返回

`List`

一个标签列表，每行一个标签在初始表中。

Raises

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

基于一组属性对目标类别（一组）进行分类。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> table = {
...     "fixed_acidity": ["7.4", "7.8", "10.3"],
...     "volatile_acidity": ["0.7", "0.88", "0.32"],
...     "citric_acid": ["0", "0", "0.45"],
...     "residual_sugar": ["1.9", "2.6", "6.4"],
...     "chlorides": ["0.076", "0.098", "0.073"],
...     "free_sulfur_dioxide": ["11", "25", "5"],
...     "total_sulfur_dioxide": ["34", "67", "13"],
...     "density": ["0.9978", "0.9968", "0.9976"],
...     "pH": ["3.51", "3.2", "3.23"],
...     "sulphates": ["0.56", "0.68", "0.82"],
...     "alcohol": ["9.4", "9.8", "12.6"],
... }
>>> client.tabular_classification(table=table, model="julien-c/wine-quality")
["5", "5", "5"]
```

#### `tabular_regression`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1110)

```py
( table: Dict model: str ) → export const metadata = 'undefined';List
```

参数

+   `table` (`Dict[str, Any]`) — 存储在表中的属性集。用于预测目标的属性可以是数值和分类的。

+   `model` (`str`) — 用于表格回归任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。

返回

`List`

一个预测的数值目标值列表。

Raises

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，并返回除 HTTP 503 以外的 HTTP 错误状态码。

在表中给定一组属性/特征，预测数值目标值。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> table = {
...     "Height": ["11.52", "12.48", "12.3778"],
...     "Length1": ["23.2", "24", "23.9"],
...     "Length2": ["25.4", "26.3", "26.5"],
...     "Length3": ["30", "31.2", "31.1"],
...     "Species": ["Bream", "Bream", "Bream"],
...     "Width": ["4.02", "4.3056", "4.6961"],
... }
>>> client.tabular_regression(table, model="scikit-learn/Fish-Weight")
[110, 120, 130]
```

#### `text_classification`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1149)

```py
( text: str model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `text` (`str`) — 待分类的字符串。

+   `model` (`str`, *可选*) — 用于文本分类任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，则将使用默认推荐的文本分类模型。默认为 None。

返回

`List[Dict]`

包含预测标签和相关概率的字典列表。

Raises

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，并返回除 HTTP 503 以外的 HTTP 错误状态码。

对给定文本执行文本分类（例如情感分析）。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.text_classification("I like you")
[{'label': 'POSITIVE', 'score': 0.9998695850372314}, {'label': 'NEGATIVE', 'score': 0.0001304351753788069}]
```

#### `text_generation`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1277)

```py
( prompt: str details: bool = False stream: bool = False model: Optional = None do_sample: bool = False max_new_tokens: int = 20 best_of: Optional = None repetition_penalty: Optional = None return_full_text: bool = False seed: Optional = None stop_sequences: Optional = None temperature: Optional = None top_k: Optional = None top_p: Optional = None truncate: Optional = None typical_p: Optional = None watermark: bool = False decoder_input_details: bool = False ) → export const metadata = 'undefined';Union[str, TextGenerationResponse, Iterable[str], Iterable[TextGenerationStreamResponse]]
```

参数

+   `prompt` (`str`) — 输入文本。

+   `details` (`bool`, *可选*) — 默认情况下，text_generation 返回一个字符串。如果要获取详细输出（token、概率、种子、完成原因等），请传递`details=True`。仅适用于在`text-generation-inference`后端上运行的模型。

+   `stream` (`bool`, *可选*) — 默认情况下，text_generation 返回完整生成的文本。如果要返回 token 流，请传递`stream=True`。仅适用于在`text-generation-inference`后端上运行的模型。

+   `model` (`str`, *可选*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数会覆盖实例级别定义的模型。默认为 None。

+   `do_sample` (`bool`) — 激活 logits 采样

+   `max_new_tokens` (`int`) — 生成的 token 的最大数量

+   `best_of` (`int`) — 生成 best_of 序列并返回具有最高 token 对数概率的一个

+   `repetition_penalty` (`float`) — 重复惩罚的参数。1.0 表示没有惩罚。有关更多详细信息，请参阅[此论文](https://arxiv.org/pdf/1909.05858.pdf)。

+   `return_full_text` (`bool`) — 是否将提示添加到生成的文本之前

+   `seed` (`int`) — 随机采样种子

+   `stop_sequences` (`List[str]`) — 如果生成了`stop_sequences`中的成员，则停止生成 token

+   `temperature` (`float`) — 用于调节 logits 分布的值。

+   `top_k` (`int`) — 保留最高概率词汇 token 的数量以进行 top-k 过滤。

+   `top_p` (`float`) — 如果设置为 < 1，则仅保留概率相加达到 `top_p` 或更高的最可能 token 集合用于生成。

+   `truncate` (`int`) — 将输入 token 截断为给定大小

+   `typical_p` (`float`) — 典型解码质量。有关更多信息，请参阅[Typical Decoding for Natural Language Generation](https://arxiv.org/abs/2202.00666)。

+   `watermark` (`bool`) — 使用[A Watermark for Large Language Models](https://arxiv.org/abs/2301.10226)进行水印处理

+   `decoder_input_details` (`bool`) — 返回解码器输入 token 的对数概率和 ID。您必须同时设置`details=True`才能考虑。默认为`False`。

返回

`Union[str, TextGenerationResponse, Iterable[str], Iterable[TextGenerationStreamResponse]]`

服务器返回的生成文本：

+   如果`stream=False`和`details=False`，生成的文本将作为`str`返回（默认）

+   如果`stream=True`和`details=False`，生成的文本将逐个标记返回，作为`Iterable[str]`。

+   如果`stream=False`和`details=True`，生成的文本将以更多细节的形式返回，作为 TextGenerationResponse

+   如果`details=True`和`stream=True`，生成的文本将逐个标记返回，作为 TextGenerationStreamResponse 的可迭代对象

引发

`ValidationError` 或 InferenceTimeoutError 或 `HTTPError`

+   `ValidationError` — 如果输入值无效。不会向服务器发出 HTTP 调用。

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

给定提示，生成以下文本。

建议安装 Pydantic 以验证输入。这样做更好，因为它可以提前失败。

API 端点应该使用`text-generation-inference`后端（TGI）运行。这个后端是运行大型语言模型的首选解决方案。但是，对于一些较小的模型（例如“gpt2”），仍在使用默认的`transformers` + `api-inference`解决方案。这两种方法具有非常相似的 API，但并非完全相同。此方法与两种方法兼容，但某些参数仅适用于`text-generation-inference`。如果忽略了某些参数，将触发警告消息，但进程将继续正确进行。

要了解有关 TGI 项目的更多信息，请参阅[`github.com/huggingface/text-generation-inference`](https://github.com/huggingface/text-generation-inference)。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()

# Case 1: generate text
>>> client.text_generation("The huggingface_hub library is ", max_new_tokens=12)
'100% open source and built to be easy to use.'

# Case 2: iterate over the generated tokens. Useful for large generation.
>>> for token in client.text_generation("The huggingface_hub library is ", max_new_tokens=12, stream=True):
...     print(token)
100
%
open
source
and
built
to
be
easy
to
use
.

# Case 3: get more details about the generation process.
>>> client.text_generation("The huggingface_hub library is ", max_new_tokens=12, details=True)
TextGenerationResponse(
    generated_text='100% open source and built to be easy to use.',
    details=Details(
        finish_reason=<FinishReason.Length: 'length'>,
        generated_tokens=12,
        seed=None,
        prefill=[
            InputToken(id=487, text='The', logprob=None),
            InputToken(id=53789, text=' hugging', logprob=-13.171875),
            (...)
            InputToken(id=204, text=' ', logprob=-7.0390625)
        ],
        tokens=[
            Token(id=1425, text='100', logprob=-1.0175781, special=False),
            Token(id=16, text='%', logprob=-0.0463562, special=False),
            (...)
            Token(id=25, text='.', logprob=-0.5703125, special=False)
        ],
        best_of_sequences=None
    )
)

# Case 4: iterate over the generated tokens with more details.
# Last object is more complete, containing the full generated text and the finish reason.
>>> for details in client.text_generation("The huggingface_hub library is ", max_new_tokens=12, details=True, stream=True):
...     print(details)
...
TextGenerationStreamResponse(token=Token(id=1425, text='100', logprob=-1.0175781, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=16, text='%', logprob=-0.0463562, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=1314, text=' open', logprob=-1.3359375, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=3178, text=' source', logprob=-0.28100586, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=273, text=' and', logprob=-0.5961914, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=3426, text=' built', logprob=-1.9423828, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=271, text=' to', logprob=-1.4121094, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=314, text=' be', logprob=-1.5224609, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=1833, text=' easy', logprob=-2.1132812, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=271, text=' to', logprob=-0.08520508, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=745, text=' use', logprob=-0.39453125, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(
    id=25,
    text='.',
    logprob=-0.5703125,
    special=False),
    generated_text='100% open source and built to be easy to use.',
    details=StreamDetails(finish_reason=<FinishReason.Length: 'length'>, generated_tokens=12, seed=None)
)
```

#### `text_to_image`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1544)

```py
( prompt: str negative_prompt: Optional = None height: Optional = None width: Optional = None num_inference_steps: Optional = None guidance_scale: Optional = None model: Optional = None **kwargs ) → export const metadata = 'undefined';Image
```

参数

+   `prompt` (`str`) — 生成图像的提示。

+   `negative_prompt` (`str`, *可选*) — 用于图像生成的可选负面提示。

+   `height` (`float`, *可选*) — 要生成的图像的像素高度。

+   `width` (`float`, *可选*) — 要生成的图像的像素宽度。

+   `num_inference_steps` (`int`, *可选*) — 去噪步骤的数量。更多的去噪步骤通常会导致更高质量的图像，但会降低推理速度。

+   `guidance_scale` (`float`, *可选*) — 更高的引导比例鼓励生成与文本`prompt`密切相关的图像，通常以降低图像质量为代价。

+   `model` (`str`, *可选*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数将覆盖实例级别定义的模型。默认为 None。

返回

`Image`

生成的图像。

引发

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

根据给定文本使用指定模型生成图像。

如果要处理图像，必须安装`PIL`（`pip install Pillow`）。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()

>>> image = client.text_to_image("An astronaut riding a horse on the moon.")
>>> image.save("astronaut.png")

>>> image = client.text_to_image(
...     "An astronaut riding a horse on the moon.",
...     negative_prompt="low resolution, blurry",
...     model="stabilityai/stable-diffusion-2-1",
... )
>>> image.save("better_astronaut.png")
```

#### `text_to_speech`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1624)

```py
( text: str model: Optional = None ) → export const metadata = 'undefined';bytes
```

Parameters

+   `text` (`str`) — 要合成的文本。

+   `model` (`str`, *optional*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数将覆盖实例级别定义的模型。默认为 None。

Returns

`bytes`

生成的音频。

Raises

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，并且 HTTP 错误状态码不是 HTTP 503。

合成一个发音给定文本的声音的音频。

示例：

```py
>>> from pathlib import Path
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()

>>> audio = client.text_to_speech("Hello world")
>>> Path("hello_world.flac").write_bytes(audio)
```

#### `token_classification`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1656)

```py
( text: str model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

Parameters

+   `text` (`str`) — 要分类的字符串。

+   `model` (`str`, *optional*) — 用于标记分类任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，则将使用默认推荐的标记分类模型。默认为 None。

Returns

`List[Dict]`

包含实体组、置信度分数、单词、起始和结束索引的标记分类输出列表。

Raises

InferenceTimeoutError 或 `HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，并且 HTTP 错误状态码不是 HTTP 503。

对给定文本执行标记分类。通常用于句子解析，无论是语法还是命名实体识别（NER），以理解文本中包含的关键字。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.token_classification("My name is Sarah Jessica Parker but you can call me Jessica")
[{'entity_group': 'PER',
'score': 0.9971321225166321,
'word': 'Sarah Jessica Parker',
'start': 11,
'end': 31},
{'entity_group': 'PER',
'score': 0.9773476123809814,
'word': 'Jessica',
'start': 52,
'end': 59}]
```

#### `translation`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1703)

```py
( text: str model: Optional = None src_lang: Optional = None tgt_lang: Optional = None ) → export const metadata = 'undefined';str
```

Parameters

+   `text` (`str`) — 要翻译的字符串。

+   `model` (`str`, *optional*) — 用于翻译任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，则将使用默认推荐的翻译模型。默认为 None。

+   `src_lang` (`str`, *optional*) — 翻译任务的源语言，即输入语言。不能在没有`tgt_lang`的情况下传递。

+   `tgt_lang` (`str`, *optional*) — 翻译任务的目标语言，即输出语言。不能在没有`src_lang`的情况下传递。

Returns

`str`

生成的翻译文本。

Raises

InferenceTimeoutError 或 `HTTPError` 或 `ValueError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，并且 HTTP 错误状态码不是 HTTP 503。

+   `ValueError` — 如果只提供了`src_lang`和`tgt_lang`参数中的一个。

将文本从一种语言转换为另一种语言。

查看[`huggingface.co/tasks/translation`](https://huggingface.co/tasks/translation)以获取有关如何为特定用例选择最佳模型的更多信息。源语言和目标语言通常取决于模型。但是，可以为某些模型指定源语言和目标语言。如果您正在使用这些模型之一，可以使用`src_lang`和`tgt_lang`参数传递相关信息。您可以在模型卡中找到这些信息。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.translation("My name is Wolfgang and I live in Berlin")
'Mein Name ist Wolfgang und ich lebe in Berlin.'
>>> client.translation("My name is Wolfgang and I live in Berlin", model="Helsinki-NLP/opus-mt-en-fr")
"Je m'appelle Wolfgang et je vis à Berlin."
```

指定语言：

```py
>>> client.translation("My name is Sarah Jessica Parker but you can call me Jessica", model="facebook/mbart-large-50-many-to-many-mmt", src_lang="en_XX", tgt_lang="fr_XX")
"Mon nom est Sarah Jessica Parker mais vous pouvez m'appeler Jessica"
```

#### `visual_question_answering`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L399)

```py
( image: Union question: str model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `image` (`Union[str, Path, bytes, BinaryIO]`) — 上下文的输入图像。可以是原始字节、图像文件或在线图像的 URL。

+   `question` (`str`) — 要回答的问题。

+   `model` (`str`, *可选*) — 用于视觉问答任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的视觉问答模型将被使用。默认为 None。

返回

`List[Dict]`

包含预测标签及其相关概率的字典列表。

Raises

`InferenceTimeoutError`或`HTTPError`

+   `InferenceTimeoutError` — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，并且 HTTP 状态代码不是 HTTP 503。

基于图像回答开放式问题。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> client.visual_question_answering(
...     image="https://huggingface.co/datasets/mishig/sample_images/resolve/main/tiger.jpg",
...     question="What is the animal doing?"
... )
[{'score': 0.778609573841095, 'answer': 'laying down'},{'score': 0.6957435607910156, 'answer': 'sitting'}, ...]
```

#### `zero_shot_classification`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1768)

```py
( text: str labels: List multi_label: bool = False model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `text` (`str`) — 要分类的输入文本。

+   `labels` (`List[str]`) — 字符串可能标签的列表。必须至少有 2 个标签。

+   `multi_label` (`bool`) — 如果类别可以重叠，则设置为 True。

+   `model` (`str`, *可选*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数将覆盖实例级别定义的模型。默认为 None。

返回

`List[Dict]`

包含预测标签及其置信度的分类输出列表。

Raises

InferenceTimeoutError 或`HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，并且 HTTP 状态代码不是 HTTP 503。

提供一个文本和一组候选标签作为输入，对输入文本进行分类。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()
>>> text = (
...     "A new model offers an explanation for how the Galilean satellites formed around the solar system's"
...     "largest world. Konstantin Batygin did not set out to solve one of the solar system's most puzzling"
...     " mysteries when he went for a run up a hill in Nice, France."
... )
>>> labels = ["space & cosmos", "scientific discovery", "microbiology", "robots", "archeology"]
>>> client.zero_shot_classification(text, labels)
[
    {"label": "scientific discovery", "score": 0.7961668968200684},
    {"label": "space & cosmos", "score": 0.18570658564567566},
    {"label": "microbiology", "score": 0.00730885099619627},
    {"label": "archeology", "score": 0.006258360575884581},
    {"label": "robots", "score": 0.004559356719255447},
]
>>> client.zero_shot_classification(text, labels, multi_label=True)
[
    {"label": "scientific discovery", "score": 0.9829297661781311},
    {"label": "space & cosmos", "score": 0.755190908908844},
    {"label": "microbiology", "score": 0.0005462635890580714},
    {"label": "archeology", "score": 0.00047131875180639327},
    {"label": "robots", "score": 0.00030448526376858354},
]
```

#### `zero_shot_image_classification`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_client.py#L1840)

```py
( image: Union labels: List model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `image` (`Union[str, Path, bytes, BinaryIO]`) — 输入图像以进行描述。可以是原始字节、图像文件或在线图像的 URL。

+   `labels` (`List[str]`) — 字符串可能标签的列表。必须至少有 2 个标签。

+   `model` (`str`, *可选*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数将覆盖实例级别定义的模型。默认为 None。

返回

`List[Dict]`

包含预测标签及其置信度的分类输出列表。

Raises

InferenceTimeoutError 或`HTTPError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `HTTPError` — 如果请求失败，并且 HTTP 状态代码不是 HTTP 503。

提供输入图像和文本标签以预测图像的文本标签。

示例：

```py
>>> from huggingface_hub import InferenceClient
>>> client = InferenceClient()

>>> client.zero_shot_image_classification(
...     "https://upload.wikimedia.org/wikipedia/commons/thumb/4/43/Cute_dog.jpg/320px-Cute_dog.jpg",
...     labels=["dog", "cat", "horse"],
... )
[{"label": "dog", "score": 0.956}, ...]
```

## 异步推理客户端

还提供了一个基于`asyncio`和`aiohttp`的异步版本的客户端。要使用它，可以直接安装`aiohttp`，或者使用`[inference]`额外选项：

```py
pip install --upgrade huggingface_hub[inference]
# or
# pip install aiohttp
```

### `class huggingface_hub.AsyncInferenceClient`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L89)

```py
( model: Optional = None token: Union = None timeout: Optional = None headers: Optional = None cookies: Optional = None )
```

参数

+   `model` (`str`, `optional`) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，例如`bigcode/starcoder`，也可以是部署的推理端点的 URL。默认为 None，此时将自动选择任务的推荐模型。

+   `token` (`str`, *optional*) — Hugging Face 令牌。将默认使用本地保存的令牌。如果不想将令牌发送到服务器，请传递`token=False`。

+   `timeout` (`float`, `optional`) — 等待服务器响应的最大秒数。在推理 API 中加载新模型可能需要几分钟。默认为 None，表示会循环直到服务器可用。

+   `headers` (`Dict[str, str]`, `optional`) — 要发送到服务器的额外标头。默认情况下，只发送授权和用户代理标头。此字典中的值将覆盖默认值。

+   `cookies` (`Dict[str, str]`, `optional`) — 要发送到服务器的额外 cookie。

初始化一个新的推理客户端。

InferenceClient 旨在提供执行推理的统一体验。该客户端可以无缝地与（免费的）推理 API 或自托管的推理端点一起使用。

#### `audio_classification`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L259)

```py
( audio: Union model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `audio` (Union[str, Path, bytes, BinaryIO]) — 要分类的音频内容。可以是原始音频字节、本地音频文件或指向音频文件的 URL。

+   `model` (`str`, *optional*) — 用于音频分类的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，则将使用默认推荐的音频分类模型。

返回

`List[Dict]`

包含预测标签及其置信度的分类输出。

Raises

InferenceTimeoutError 或`aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，并且 HTTP 错误状态代码不是 HTTP 503。

对提供的音频内容执行音频分类。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.audio_classification("audio.flac")
[{'score': 0.4976358711719513, 'label': 'hap'}, {'score': 0.3677836060523987, 'label': 'neu'},...]
```

#### `automatic_speech_recognition`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L298)

```py
( audio: Union model: Optional = None ) → export const metadata = 'undefined';str
```

参数

+   `audio` (Union[str, Path, bytes, BinaryIO]) — 要转录的内容。可以是原始音频字节、本地音频文件或音频文件的 URL。

+   `model` (`str`, *optional*) — 用于 ASR 的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，则将使用默认推荐的 ASR 模型。

返回

str

转录的文本。

Raises

InferenceTimeoutError 或`aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

对给定音频内容执行自动语音识别（ASR 或音频转文本）。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.automatic_speech_recognition("hello_world.flac")
"hello world"
```

#### `conversational`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L335)

```py
( text: str generated_responses: Optional = None past_user_inputs: Optional = None parameters: Optional = None model: Optional = None ) → export const metadata = 'undefined';Dict
```

参数

+   `text` (`str`) — 用户在对话中的最后输入。

+   `generated_responses` (`List[str]`, *可选*) — 与模型先前回复对应的字符串列表。默认为 None。

+   `past_user_inputs` (`List[str]`, *可选*) — 与用户先前回复对应的字符串列表。应与`generated_responses`长度相同。默认为 None。

+   `parameters` (`Dict[str, Any]`, *可选*) — 对话任务的其他参数。默认为 None。有关可用参数的更多详细信息，请参考[此页面](https://huggingface.co/docs/api-inference/detailed_parameters#conversational-task)

+   `model` (`str`, *可选*) — 用于对话任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的对话模型将被使用。默认为 None。

返回

`Dict`

生成的对话输出。

引发

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

根据给定的输入文本生成对话响应（即与 API 聊天）。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> output = await client.conversational("Hi, who are you?")
>>> output
{'generated_text': 'I am the one who knocks.', 'conversation': {'generated_responses': ['I am the one who knocks.'], 'past_user_inputs': ['Hi, who are you?']}, 'warnings': ['Setting `pad_token_id` to `eos_token_id`:50256 async for open-end generation.']}
>>> await client.conversational(
...     "Wow, that's scary!",
...     generated_responses=output["conversation"]["generated_responses"],
...     past_user_inputs=output["conversation"]["past_user_inputs"],
... )
```

#### `document_question_answering`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L442)

```py
( image: Union question: str model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `image` (`Union[str, Path, bytes, BinaryIO]`) — 上下文的输入图像。可以是原始字节、图像文件或在线图像的 URL。

+   `question` (`str`) — 要回答的问题。

+   `model` (`str`, *可选*) — 用于文档问答任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的文档问答模型将被使用。默认为 None。

返回

`List[Dict]`

包含预测标签、相关概率、单词 ID 和页码的字典列表。

引发

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

回答文档图像上的问题。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.document_question_answering(image="https://huggingface.co/spaces/impira/docquery/resolve/2359223c1837a7587402bda0f2643382a6eefeab/invoice.png", question="What is the invoice number?")
[{'score': 0.42515629529953003, 'answer': 'us-001', 'start': 16, 'end': 16}]
```

#### `feature_extraction`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L484)

```py
( text: str model: Optional = None ) → export const metadata = 'undefined';np.ndarray
```

参数

+   `text` (`str`) — 要嵌入的文本。

+   `model` (`str`, *可选*) — 用于对话任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的对话模型将被使用。默认为 None。

返回

`np.ndarray`

将输入文本表示为 float32 numpy 数组的嵌入。

引发

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 错误状态码不是 HTTP 503。

为给定文本生成嵌入。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.feature_extraction("Hi, who are you?")
array([[ 2.424802  ,  2.93384   ,  1.1750331 , ...,  1.240499, -0.13776633, -0.7889173 ],
[-0.42943227, -0.6364878 , -1.693462  , ...,  0.41978157, -2.4336355 ,  0.6162071 ],
...,
[ 0.28552425, -0.928395  , -1.2077185 , ...,  0.76810825, -2.1069427 ,  0.6236161 ]], dtype=float32)
```

#### `fill_mask`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L521)

```py
( text: str model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `text` (`str`) — 要填充的字符串，必须包含[MASK]标记（检查模型卡片以获取掩码的确切名称）。

+   `model` (`str`, *可选*) — 用于填充掩码任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的填充掩码模型将被使用。默认为 None。

返回

`List[Dict]`

一个包含填充掩码输出字典的列表，包含预测标签、相关概率、标记引用和完成的文本。

抛出异常

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 错误状态码不是 HTTP 503。

用缺失的单词填充空格（确切地说是标记）。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.fill_mask("The goal of life is <mask>.")
[{'score': 0.06897063553333282,
'token': 11098,
'token_str': ' happiness',
'sequence': 'The goal of life is happiness.'},
{'score': 0.06554922461509705,
'token': 45075,
'token_str': ' immortality',
'sequence': 'The goal of life is immortality.'}]
```

#### `get_model_status`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1969)

```py
( model: Optional = None ) → export const metadata = 'undefined';ModelStatus
```

参数

+   `model` (`str`, *可选*) — 要检查状态的模型标识符。如果未提供模型，则将使用与此 InferenceClient 实例关联的模型。只能检查 InferenceAPI 服务，因此标识符不能是 URL。

返回

`ModelStatus`

包含有关模型状态的 ModelStatus 数据类的实例：加载、状态、计算类型和框架。

获取托管在推理 API 上的模型的状态。

当您已经知道要使用哪个模型并想要检查其可用性时，此端点非常有用。如果要发现已部署的模型，您应该使用 list_deployed_models()。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.get_model_status("bigcode/starcoder")
ModelStatus(loaded=True, state='Loaded', compute_type='gpu', framework='text-generation-inference')
```

#### `get_recommended_model`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1945)

```py
( task: str ) → export const metadata = 'undefined';str
```

参数

+   `task` (`str`) — Hugging Face 推荐的任务。所有可用任务可以在[这里](https://huggingface.co/tasks)找到。

返回

`str`

推荐用于输入任务的模型名称。

抛出异常

`ValueError`

+   `ValueError` — 如果 Hugging Face 没有对输入任务的推荐。

获取 Hugging Face 推荐的用于输入任务的模型。

#### `image_classification`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L562)

```py
( image: Union model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `image` (`Union[str, Path, bytes, BinaryIO]`) — 要分类的图像。可以是原始字节、图像文件或在线图像的 URL。

+   `model` (`str`, *可选*) — 用于图像分类的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的图像分类模型将被使用。

返回

`List[Dict]`

一个包含预测标签和相关概率的字典列表。

抛出异常

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 状态码不是 HTTP 503。

使用指定模型对给定图像执行图像分类。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.image_classification("https://upload.wikimedia.org/wikipedia/commons/thumb/4/43/Cute_dog.jpg/320px-Cute_dog.jpg")
[{'score': 0.9779096841812134, 'label': 'Blenheim spaniel'}, ...]
```

#### `image_segmentation`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L599)

```py
( image: Union model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `image` (`Union[str, Path, bytes, BinaryIO]`) — 要分割的图像。可以是原始字节、图像文件或在线图像的 URL。

+   `model` (`str`, *optional*) — 用于图像分割的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的图像分割模型将被使用。

返回

`List[Dict]`

包含分割掩模和相关属性的字典列表。

引发

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 状态码不是 HTTP 503。

使用指定模型对给定图像执行图像分割。

如果要处理图像，必须安装 `PIL` (`pip install Pillow`)。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.image_segmentation("cat.jpg"):
[{'score': 0.989008, 'label': 'LABEL_184', 'mask': <PIL.PngImagePlugin.PngImageFile image mode=L size=400x300 at 0x7FDD2B129CC0>}, ...]
```

#### `image_to_image`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L651)

```py
( image: Union prompt: Optional = None negative_prompt: Optional = None height: Optional = None width: Optional = None num_inference_steps: Optional = None guidance_scale: Optional = None model: Optional = None **kwargs ) → export const metadata = 'undefined';Image
```

参数

+   `image` (`Union[str, Path, bytes, BinaryIO]`) — 用于翻译的输入图像。可以是原始字节、图像文件或在线图像的 URL。

+   `prompt` (`str`, *optional*) — 用于引导图像生成的文本提示。

+   `negative_prompt` (`str`, *optional*) — 用于引导翻译过程的负面提示。

+   `height` (`int`, *optional*) — 生成图像的像素高度。

+   `width` (`int`, *optional*) — 生成图像的像素宽度。

+   `num_inference_steps` (`int`, *optional*) — 降噪步骤的数量。更多的降噪步骤通常会导致更高质量的图像，但会降低推理速度。

+   `guidance_scale` (`float`, *optional*) — 更高的引导比例鼓励生成与文本提示紧密相关的图像，通常会降低图像质量。

+   `model` (`str`, *optional*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数会覆盖实例级别定义的模型。默认为 None。

返回

`Image`

翻译后的图像。

引发

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 状态码不是 HTTP 503。

使用指定模型执行图像到图像的翻译。

如果要处理图像，必须安装 `PIL` (`pip install Pillow`)。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> image = await client.image_to_image("cat.jpg", prompt="turn the cat into a tiger")
>>> image.save("tiger.jpg")
```

#### `image_to_text`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L736)

```py
( image: Union model: Optional = None ) → export const metadata = 'undefined';str
```

参数

+   `image`（`Union[str，Path，bytes，BinaryIO]`） - 要标注的输入图像。它可以是原始字节，图像文件或在线图像的 URL。

+   `model`（`str`，*可选*） - 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID 或部署的推理端点的 URL。此参数将覆盖实例级别定义的模型。默认为 None。

返回

`str`

生成的文本。

引发

推理超时错误 - 如果模型不可用或请求超时。或`aiohttp.ClientResponseError`

+   推理超时错误 - 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` - 如果请求失败，并且 HTTP 错误状态代码不是 HTTP 503。

接受输入图像并返回文本。

根据您的用例（图像字幕，光学字符识别（OCR），Pix2Struct 等），模型的输出可能会有很大不同。请查看模型卡以了解有关模型特定性的更多信息。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.image_to_text("cat.jpg")
'a cat standing in a grassy field '
>>> await client.image_to_text("https://upload.wikimedia.org/wikipedia/commons/thumb/4/43/Cute_dog.jpg/320px-Cute_dog.jpg")
'a dog laying on the grass next to a flower pot '
```

#### `list_deployed_models`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L773)

```py
( frameworks: Union = None ) → export const metadata = 'undefined';Dict[str, List[str]]
```

参数

+   `frameworks`（`Literal["all"]`或`List[str]`或`str`，*可选*） - 要过滤的框架。默认情况下，仅测试可用框架的子集。如果设置为“all”，将测试所有可用框架。还可以提供单个框架或一组自定义框架以进行检查。

返回

`Dict[str，List[str]]`

将任务名称映射到模型 ID 的排序列表的字典。

列出当前部署在推理 API 服务上的模型。

此助手逐个框架检查部署的模型。默认情况下，它将检查支持的四个主要框架，并占托管模型的 95％。但是，如果您想要完整的模型列表，可以指定`frameworks="all"`作为输入。或者，如果您事先知道您感兴趣的框架，也可以将搜索限制为此框架（例如，`frameworks="text-generation-inference"`）。检查的框架越多，所需的时间就越长。

此端点主要用于可发现性。如果您已经知道要使用哪个模型并且要检查其可用性，可以直接使用 get_model_status()。

示例：

```py
# Must be run in an async contextthon
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()

# Discover zero-shot-classification models currently deployed
>>> models = await client.list_deployed_models()
>>> models["zero-shot-classification"]
['Narsil/deberta-large-mnli-zero-cls', 'facebook/bart-large-mnli', ...]

# List from only 1 framework
>>> await client.list_deployed_models("text-generation-inference")
{'text-generation': ['bigcode/starcoder', 'meta-llama/Llama-2-70b-chat-hf', ...], ...}
```

#### `object_detection`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L854)

```py
( image: Union model: Optional = None ) → export const metadata = 'undefined';List[ObjectDetectionOutput]
```

参数

+   `image`（`Union[str，Path，bytes，BinaryIO]`） - 要在其上检测对象的图像。它可以是原始字节，图像文件或在线图像的 URL。

+   `model`（`str`，*可选*） - 用于对象检测的模型。可以是托管在 Hugging Face Hub 上的模型 ID 或部署的推理端点的 URL。如果未提供，默认推荐的对象检测模型（DETR）将被使用。

返回

`List[ObjectDetectionOutput]`

包含边界框和相关属性的字典列表。

引发

推理超时错误 - 如果模型不可用或请求超时。

+   推理超时错误 - 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` - 如果请求失败，并且 HTTP 错误状态代码不是 HTTP 503。

+   `ValueError` - 如果请求输出不是列表。

使用指定的模型在给定图像上执行对象检测。

如果要处理图像，则必须安装`PIL`（`pip install Pillow`）。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.object_detection("people.jpg"):
[{"score":0.9486683011054993,"label":"person","box":{"xmin":59,"ymin":39,"xmax":420,"ymax":510}}, ... ]
```

#### `post`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L156)

```py
( json: Union = None data: Union = None model: Optional = None task: Optional = None stream: bool = False ) → export const metadata = 'undefined';bytes
```

参数

+   `json` (`Union[str, Dict, List]`, *可选*) — 要发送到请求体中的 JSON 数据，针对每个任务而不同。默认为 None。

+   `data` (`Union[str, Path, bytes, BinaryIO]`, *可选*) — 发送到请求体中的内容，针对每个任务而不同。可以是原始字节、指向已打开文件的指针、本地文件路径或在线资源的 URL（图像、音频文件等）。如果同时传递了 `json` 和 `data`，则 `data` 优先。必须提供至少 `json` 或 `data`。默认为 None。

+   `model` (`str`, *可选*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。将覆盖实例级别定义的模型。默认为 None。

+   `task` (`str`, *可选*) — 推理中要执行的任务。所有可用任务可以在[这里](https://huggingface.co/tasks)找到。仅在未提供 `model` 时才用于默认推荐模型。必须提供至少 `model` 或 `task`。默认为 None。

+   `stream` (`bool`, *可选*) — 是否迭代流式 API。

返回

字节

服务器返回的原始字节。

引发

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

向推理服务器发出 POST 请求。

#### `question_answering`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L903)

```py
( question: str context: str model: Optional = None ) → export const metadata = 'undefined';Dict
```

参数

+   `question` (`str`) — 要回答的问题。

+   `context` (`str`) — 问题的上下文。

+   `model` (`str`) — 用于问答任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。

返回

`Dict`

包含得分、起始索引、结束索引和答案的问答输出字典。

引发

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

从给定文本中检索问题的答案。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.question_answering(question="What's my name?", context="My name is Clara and I live in Berkeley.")
{'score': 0.9326562285423279, 'start': 11, 'end': 16, 'answer': 'Clara'}
```

#### `sentence_similarity`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L945)

```py
( sentence: str other_sentences: List model: Optional = None ) → export const metadata = 'undefined';List[float]
```

参数

+   `sentence` (`str`) — 要与其他句子进行比较的主要句子。

+   `other_sentences` (`List[str]`) — 要与之比较的句子列表。

+   `model` (`str`, *可选*) — 用于对话任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，则将使用默认推荐的对话模型。默认为 None。

返回

`List[float]`

表示输入文本的嵌入。

引发

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

通过比较它们的嵌入来计算句子与其他句子列表之间的语义相似性。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.sentence_similarity(
...     "Machine learning is so easy.",
...     other_sentences=[
...         "Deep learning is so straightforward.",
...         "This is so difficult, like rocket science.",
...         "I can't believe how much I struggled with this.",
...     ],
... )
[0.7785726189613342, 0.45876261591911316, 0.2906220555305481]
```

#### `summarization`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L993)

```py
( text: str parameters: Optional = None model: Optional = None ) → export const metadata = 'undefined';str
```

参数

+   `text` (`str`) — 要总结的输入文本。

+   `parameters` (`Dict[str, Any]`, *可选*) — 用于摘要的附加参数。查看此[页面](https://huggingface.co/docs/api-inference/detailed_parameters#summarization-task)获取更多详细信息。

+   `model` (`str`, *可选*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数覆盖了实例级别定义的模型。默认为 None。

返回

`str`

生成的摘要文本。

引发

InferenceTimeoutError 或`aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

使用指定的模型生成给定文本的摘要。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.summarization("The Eiffel tower...")
'The Eiffel tower is one of the most famous landmarks in the world....'
```

#### `table_question_answering`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1037)

```py
( table: Dict query: str model: Optional = None ) → export const metadata = 'undefined';Dict
```

参数

+   `table` (`str`) — 以列表字典表示的数据表，其中条目是标题，列表是所有值，所有列表必须具有相同的大小。

+   `query` (`str`) — 您想要询问表格的纯文本查询。

+   `model` (`str`) — 用于表-问题回答任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。

返回

`Dict`

一个包含答案、坐标、单元格和使用的聚合器的表格问答输出的字典。

引发

InferenceTimeoutError 或`aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

从表中给出的信息中检索问题的答案。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> query = "How many stars does the transformers repository have?"
>>> table = {"Repository": ["Transformers", "Datasets", "Tokenizers"], "Stars": ["36542", "4512", "3934"]}
>>> await client.table_question_answering(table, query, model="google/tapas-base-finetuned-wtq")
{'answer': 'AVERAGE > 36542', 'coordinates': [[0, 1]], 'cells': ['36542'], 'aggregator': 'AVERAGE'}
```

#### `tabular_classification`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1083)

```py
( table: Dict model: str ) → export const metadata = 'undefined';List
```

参数

+   `table` (`Dict[str, Any]`) — 用于分类的属性集。

+   `model` (`str`) — 用于表格分类任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。

返回

`List`

一个标签列表，每行对应初始表中的一行。

引发

InferenceTimeoutError 或`aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

基于一组属性对目标类别（一组）进行分类。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> table = {
...     "fixed_acidity": ["7.4", "7.8", "10.3"],
...     "volatile_acidity": ["0.7", "0.88", "0.32"],
...     "citric_acid": ["0", "0", "0.45"],
...     "residual_sugar": ["1.9", "2.6", "6.4"],
...     "chlorides": ["0.076", "0.098", "0.073"],
...     "free_sulfur_dioxide": ["11", "25", "5"],
...     "total_sulfur_dioxide": ["34", "67", "13"],
...     "density": ["0.9978", "0.9968", "0.9976"],
...     "pH": ["3.51", "3.2", "3.23"],
...     "sulphates": ["0.56", "0.68", "0.82"],
...     "alcohol": ["9.4", "9.8", "12.6"],
... }
>>> await client.tabular_classification(table=table, model="julien-c/wine-quality")
["5", "5", "5"]
```

#### `tabular_regression`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1128)

```py
( table: Dict model: str ) → export const metadata = 'undefined';List
```

参数

+   `table` (`Dict[str, Any]`) — 存储在表中的属性集。用于预测目标的属性可以是数值和分类的。

+   `model` (`str`) — 用于表格回归任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。

返回

`List`

一组预测的数值目标值。

引发

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 错误状态码不是 HTTP 503。

在表中给定一组属性/特征时预测数值目标值。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> table = {
...     "Height": ["11.52", "12.48", "12.3778"],
...     "Length1": ["23.2", "24", "23.9"],
...     "Length2": ["25.4", "26.3", "26.5"],
...     "Length3": ["30", "31.2", "31.1"],
...     "Species": ["Bream", "Bream", "Bream"],
...     "Width": ["4.02", "4.3056", "4.6961"],
... }
>>> await client.tabular_regression(table, model="scikit-learn/Fish-Weight")
[110, 120, 130]
```

#### `text_classification`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1168)

```py
( text: str model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `text` (`str`) — 要分类的字符串。

+   `model` (`str`, *可选*) — 用于文本分类任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，则将使用默认推荐的文本分类模型。默认为 None。

返回

`List[Dict]`

包含预测标签和相关概率的字典列表。

引发

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 错误状态码不是 HTTP 503。

对给定文本执行文本分类（例如情感分析）。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.text_classification("I like you")
[{'label': 'POSITIVE', 'score': 0.9998695850372314}, {'label': 'NEGATIVE', 'score': 0.0001304351753788069}]
```

#### `text_generation`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1297)

```py
( prompt: str details: bool = False stream: bool = False model: Optional = None do_sample: bool = False max_new_tokens: int = 20 best_of: Optional = None repetition_penalty: Optional = None return_full_text: bool = False seed: Optional = None stop_sequences: Optional = None temperature: Optional = None top_k: Optional = None top_p: Optional = None truncate: Optional = None typical_p: Optional = None watermark: bool = False decoder_input_details: bool = False ) → export const metadata = 'undefined';Union[str, TextGenerationResponse, Iterable[str], Iterable[TextGenerationStreamResponse]]
```

参数

+   `prompt` (`str`) — 输入文本。

+   `details` (`bool`, *可选*) — 默认情况下，text_generation 返回一个字符串。如果要获得详细输出（令牌、概率、种子、完成原因等），请传递`details=True`。仅适用于在`text-generation-inference`后端上运行的模型。

+   `stream` (`bool`, *可选*) — 默认情况下，text_generation 返回完整生成的文本。如果要返回令牌流，请传递`stream=True`。仅适用于在`text-generation-inference`后端上运行的模型。

+   `model` (`str`, *可选*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数将覆盖实例级别定义的模型。默认为 None。

+   `do_sample` (`bool`) — 激活 logits 抽样

+   `max_new_tokens` (`int`) — 生成的令牌的最大数量

+   `best_of` (`int`) — 生成 best_of 序列并返回具有最高令牌对数概率的序列

+   `repetition_penalty` (`float`) — 重复惩罚的参数。1.0 表示没有惩罚。有关更多详细信息，请参阅[此论文](https://arxiv.org/pdf/1909.05858.pdf)。

+   `return_full_text` (`bool`) — 是否将提示添加到生成的文本之前

+   `seed` (`int`) — 随机抽样种子

+   `stop_sequences` (`List[str]`) — 如果生成了`stop_sequences`的成员，则停止生成令牌

+   `temperature` (`float`) — 用于调节 logits 分布的值。

+   `top_k` (`int`) — 保留最高概率词汇标记的数量以进行 top-k 过滤。

+   `top_p` (`float`) — 如果设置为<1，则仅保留概率相加达到`top_p`或更高的最小一组最有可能的令牌进行生成。

+   `truncate` (`int`) — 将输入令牌截断到给定大小

+   `typical_p` (`float`) — 典型的解码质量 参见[自然语言生成的典型解码](https://arxiv.org/abs/2202.00666) 获取更多信息

+   `watermark` (`bool`) — 使用[A Watermark for Large Language Models](https://arxiv.org/abs/2301.10226)进行水印处理。

+   `decoder_input_details` (`bool`) — 返回解码器输入令牌的对数概率和 id。您必须同时设置`details=True`才能考虑。默认为`False`。

返回

`Union[str, TextGenerationResponse, Iterable[str], Iterable[TextGenerationStreamResponse]]`

从服务器返回的生成文本：

+   如果`stream=False`和`details=False`，生成的文本将作为`str`返回（默认）

+   如果`stream=True`和`details=False`，生成的文本将逐令牌返回，作为`Iterable[str]`

+   如果`stream=False`和`details=True`，生成的文本将以更多细节的形式返回，作为 TextGenerationResponse

+   如果`details=True`和`stream=True`，生成的文本将逐令牌返回，作为 TextGenerationStreamResponse 的可迭代对象

Raises

`ValidationError`或 InferenceTimeoutError 或`aiohttp.ClientResponseError`

+   `ValidationError` — 如果输入值无效。不会向服务器发出 HTTP 调用。

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，返回的 HTTP 错误状态码不是 HTTP 503。

给定提示，生成以下文本。

建议安装 Pydantic 以验证输入。这是首选的，因为它可以提前失败。

API 端点应该使用`text-generation-inference`后端（TGI）。这个后端是运行大型语言模型的首选解决方案。然而，对于一些较小的模型（例如“gpt2”），仍在使用默认的`transformers` + `api-inference`解决方案。这两种方法具有非常相似的 API，但并非完全相同。这种方法与两种方法兼容，但某些参数仅适用于`text-generation-inference`。如果某些参数被忽略，将触发警告消息，但流程将继续正确进行。

要了解有关 TGI 项目的更多信息，请参阅[`github.com/huggingface/text-generation-inference`](https://github.com/huggingface/text-generation-inference)。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()

# Case 1: generate text
>>> await client.text_generation("The huggingface_hub library is ", max_new_tokens=12)
'100% open source and built to be easy to use.'

# Case 2: iterate over the generated tokens. Useful async for large generation.
>>> async for token in await client.text_generation("The huggingface_hub library is ", max_new_tokens=12, stream=True):
...     print(token)
100
%
open
source
and
built
to
be
easy
to
use
.

# Case 3: get more details about the generation process.
>>> await client.text_generation("The huggingface_hub library is ", max_new_tokens=12, details=True)
TextGenerationResponse(
    generated_text='100% open source and built to be easy to use.',
    details=Details(
        finish_reason=<FinishReason.Length: 'length'>,
        generated_tokens=12,
        seed=None,
        prefill=[
            InputToken(id=487, text='The', logprob=None),
            InputToken(id=53789, text=' hugging', logprob=-13.171875),
            (...)
            InputToken(id=204, text=' ', logprob=-7.0390625)
        ],
        tokens=[
            Token(id=1425, text='100', logprob=-1.0175781, special=False),
            Token(id=16, text='%', logprob=-0.0463562, special=False),
            (...)
            Token(id=25, text='.', logprob=-0.5703125, special=False)
        ],
        best_of_sequences=None
    )
)

# Case 4: iterate over the generated tokens with more details.
# Last object is more complete, containing the full generated text and the finish reason.
>>> async for details in await client.text_generation("The huggingface_hub library is ", max_new_tokens=12, details=True, stream=True):
...     print(details)
...
TextGenerationStreamResponse(token=Token(id=1425, text='100', logprob=-1.0175781, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=16, text='%', logprob=-0.0463562, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=1314, text=' open', logprob=-1.3359375, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=3178, text=' source', logprob=-0.28100586, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=273, text=' and', logprob=-0.5961914, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=3426, text=' built', logprob=-1.9423828, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=271, text=' to', logprob=-1.4121094, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=314, text=' be', logprob=-1.5224609, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=1833, text=' easy', logprob=-2.1132812, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=271, text=' to', logprob=-0.08520508, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(id=745, text=' use', logprob=-0.39453125, special=False), generated_text=None, details=None)
TextGenerationStreamResponse(token=Token(
    id=25,
    text='.',
    logprob=-0.5703125,
    special=False),
    generated_text='100% open source and built to be easy to use.',
    details=StreamDetails(finish_reason=<FinishReason.Length: 'length'>, generated_tokens=12, seed=None)
)
```

#### `text_to_image`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1566)

```py
( prompt: str negative_prompt: Optional = None height: Optional = None width: Optional = None num_inference_steps: Optional = None guidance_scale: Optional = None model: Optional = None **kwargs ) → export const metadata = 'undefined';Image
```

参数

+   `prompt` (`str`) — 生成图像的提示。

+   `negative_prompt` (`str`, *可选*) — 图像生成的可选负面提示。

+   `height` (`float`, *可选*) — 生成图像的像素高度。

+   `width` (`float`, *可选*) — 生成图像的像素宽度。

+   `num_inference_steps` (`int`, *可选*) — 降噪步骤的数量。更多的降噪步骤通常会导致更高质量的图像，但会降低推断速度。

+   `guidance_scale` (`float`, *可选*) — 更高的指导比例鼓励生成与文本`prompt`密切相关的图像，通常以降低图像质量为代价。

+   `model` (`str`, *可选*) — 用于推断的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推断端点的 URL。此参数将覆盖实例级别定义的模型。默认为 None。

返回

`Image`

生成的图像。

Raises

推理超时错误 或 `aiohttp.ClientResponseError`

+   推理超时错误 — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 错误状态码不是 HTTP 503。

基于给定文本使用指定模型生成图像。

如果要处理图像，必须安装 `PIL`（`pip install Pillow`）。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()

>>> image = await client.text_to_image("An astronaut riding a horse on the moon.")
>>> image.save("astronaut.png")

>>> image = await client.text_to_image(
...     "An astronaut riding a horse on the moon.",
...     negative_prompt="low resolution, blurry",
...     model="stabilityai/stable-diffusion-2-1",
... )
>>> image.save("better_astronaut.png")
```

#### `文本到语音`

[< 源 >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1647)

```py
( text: str model: Optional = None ) → export const metadata = 'undefined';bytes
```

参数

+   `文本` (`str`) — 要合成的文本。

+   `model` (`str`, *可选*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数将覆盖实例级别定义的模型。默认为 None。

返回

`字节`

生成的音频。

引发

推理超时错误 或 `aiohttp.ClientResponseError`

+   推理超时错误 — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 错误状态码不是 HTTP 503。

合成一个朗读给定文本的声音的音频。

示例：

```py
# Must be run in an async context
>>> from pathlib import Path
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()

>>> audio = await client.text_to_speech("Hello world")
>>> Path("hello_world.flac").write_bytes(audio)
```

#### `标记分类`

[< 源 >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1680)

```py
( text: str model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `文本` (`str`) — 要分类的字符串。

+   `model` (`str`, *可选*) — 用于标记分类任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的标记分类模型将被使用。默认为 None。

返回

`List[Dict]`

包含实体组、置信度分数、单词、起始和结束索引的标记分类输出列表。

引发

推理超时错误 或 `aiohttp.ClientResponseError`

+   推理超时错误 — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 错误状态码不是 HTTP 503。

对给定文本执行标记分类。通常用于句子解析，无论是语法还是命名实体识别（NER），以理解文本中包含的关键字。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.token_classification("My name is Sarah Jessica Parker but you can call me Jessica")
[{'entity_group': 'PER',
'score': 0.9971321225166321,
'word': 'Sarah Jessica Parker',
'start': 11,
'end': 31},
{'entity_group': 'PER',
'score': 0.9773476123809814,
'word': 'Jessica',
'start': 52,
'end': 59}]
```

#### `翻译`

[< 源 >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1728)

```py
( text: str model: Optional = None src_lang: Optional = None tgt_lang: Optional = None ) → export const metadata = 'undefined';str
```

参数

+   `文本` (`str`) — 要翻译的字符串。

+   `model` (`str`, *可选*) — 用于翻译任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的翻译模型将被使用。默认为 None。

+   `src_lang` (`str`, *可选*) — 翻译任务的源语言，即输入语言。不能在没有 `tgt_lang` 的情况下传递。

+   `tgt_lang` (`str`, *可选*) — 翻译任务的目标语言，即输出语言。不能在没有 `src_lang` 的情况下传递。

返回

`str`

生成的翻译文本。

引发

推理超时错误 或 `aiohttp.ClientResponseError` 或 `ValueError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 错误状态码不是 HTTP 503。

+   `ValueError` — 如果只提供了`src_lang`和`tgt_lang`参数中的一个。

将文本从一种语言转换为另一种语言。

查看[`huggingface.co/tasks/translation`](https://huggingface.co/tasks/translation)以获取有关如何为特定用例选择最佳模型的更多信息。源语言和目标语言通常取决于模型。但是，对于某些模型，可以指定源语言和目标语言。如果您正在使用这些模型之一，可以使用`src_lang`和`tgt_lang`参数传递相关信息。您可以在模型卡中找到这些信息。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.translation("My name is Wolfgang and I live in Berlin")
'Mein Name ist Wolfgang und ich lebe in Berlin.'
>>> await client.translation("My name is Wolfgang and I live in Berlin", model="Helsinki-NLP/opus-mt-en-fr")
"Je m'appelle Wolfgang et je vis à Berlin."
```

指定语言：

```py
>>> client.translation("My name is Sarah Jessica Parker but you can call me Jessica", model="facebook/mbart-large-50-many-to-many-mmt", src_lang="en_XX", tgt_lang="fr_XX")
"Mon nom est Sarah Jessica Parker mais vous pouvez m'appeler Jessica"
```

#### `visual_question_answering`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L397)

```py
( image: Union question: str model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `image` (`Union[str, Path, bytes, BinaryIO]`) — 上下文的输入图像。可以是原始字节、图像文件或在线图像的 URL。

+   `question` (`str`) — 要回答的问题。

+   `model` (`str`, *optional*) — 用于视觉问答任务的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。如果未提供，默认推荐的视觉问答模型将被使用。默认为 None。

返回

`List[Dict]`

包含预测标签和相关概率的字典列表。

引发

`InferenceTimeoutError`或`aiohttp.ClientResponseError`

+   `InferenceTimeoutError` — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 错误状态码不是 HTTP 503。

基于图像回答开放性问题。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> await client.visual_question_answering(
...     image="https://huggingface.co/datasets/mishig/sample_images/resolve/main/tiger.jpg",
...     question="What is the animal doing?"
... )
[{'score': 0.778609573841095, 'answer': 'laying down'},{'score': 0.6957435607910156, 'answer': 'sitting'}, ...]
```

#### `zero_shot_classification`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1794)

```py
( text: str labels: List multi_label: bool = False model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `text` (`str`) — 输入要分类的文本。

+   `labels` (`List[str]`) — 字符串可能标签的列表。必须至少有 2 个标签。

+   `multi_label` (`bool`) — 如果类别可以重叠，则设置为 True。

+   `model` (`str`, *optional*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数将覆盖实例级别定义的模型。默认为 None。

返回

`List[Dict]`

包含预测标签和置信度的分类输出列表。

引发

InferenceTimeoutError 或`aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，HTTP 错误状态码不是 HTTP 503。

将文本和一组候选标签作为输入，对输入文本进行分类。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()
>>> text = (
...     "A new model offers an explanation async for how the Galilean satellites formed around the solar system's"
...     "largest world. Konstantin Batygin did not set out to solve one of the solar system's most puzzling"
...     " mysteries when he went async for a run up a hill in Nice, France."
... )
>>> labels = ["space & cosmos", "scientific discovery", "microbiology", "robots", "archeology"]
>>> await client.zero_shot_classification(text, labels)
[
    {"label": "scientific discovery", "score": 0.7961668968200684},
    {"label": "space & cosmos", "score": 0.18570658564567566},
    {"label": "microbiology", "score": 0.00730885099619627},
    {"label": "archeology", "score": 0.006258360575884581},
    {"label": "robots", "score": 0.004559356719255447},
]
>>> await client.zero_shot_classification(text, labels, multi_label=True)
[
    {"label": "scientific discovery", "score": 0.9829297661781311},
    {"label": "space & cosmos", "score": 0.755190908908844},
    {"label": "microbiology", "score": 0.0005462635890580714},
    {"label": "archeology", "score": 0.00047131875180639327},
    {"label": "robots", "score": 0.00030448526376858354},
]
```

#### `zero_shot_image_classification`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_generated/_async_client.py#L1867)

```py
( image: Union labels: List model: Optional = None ) → export const metadata = 'undefined';List[Dict]
```

参数

+   `image` (`Union[str, Path, bytes, BinaryIO]`) — 要加注的输入图像。可以是原始字节、图像文件或在线图像的 URL。

+   `labels` (`List[str]`) — 字符串可能标签的列表。必须至少有 2 个标签。

+   `model` (`str`, *optional*) — 用于推理的模型。可以是托管在 Hugging Face Hub 上的模型 ID，也可以是部署的推理端点的 URL。此参数将覆盖实例级别定义的模型。默认为 None。

返回

`List[Dict]`

包含预测标签及其置信度的分类输出列表。

Raises

InferenceTimeoutError 或 `aiohttp.ClientResponseError`

+   InferenceTimeoutError — 如果模型不可用或请求超时。

+   `aiohttp.ClientResponseError` — 如果请求失败，返回的 HTTP 状态码不是 HTTP 503。

提供输入图像和文本标签以预测图像的文本标签。

示例：

```py
# Must be run in an async context
>>> from huggingface_hub import AsyncInferenceClient
>>> client = AsyncInferenceClient()

>>> await client.zero_shot_image_classification(
...     "https://upload.wikimedia.org/wikipedia/commons/thumb/4/43/Cute_dog.jpg/320px-Cute_dog.jpg",
...     labels=["dog", "cat", "horse"],
... )
[{"label": "dog", "score": 0.956}, ...]
```

## InferenceTimeoutError

### `class huggingface_hub.InferenceTimeoutError`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_common.py#L100)

```py
( *args **kwargs )
```

当模型不可用或请求超时时引发错误。

## Return types

对于大多数任务，返回值具有内置类型（字符串、列表、图像...）。以下是更复杂类型的列表。

### ClassificationOutput

### `class huggingface_hub.inference._types.ClassificationOutput`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_types.py#L22)

```py
( )
```

参数

+   `label` (`str`) — 模型预测的标签。

+   `score` (`float`) — 模型预测的标签的分数。

包含 audio_classification() 和 image_classification() 任务的输出的字典。

### ConversationalOutputConversation

### `class huggingface_hub.inference._types.ConversationalOutputConversation`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_types.py#L36)

```py
( )
```

参数

+   `generated_responses` (`List[str]`) — 模型的响应列表。

+   `past_user_inputs` (`List[str]`) — 用户的输入列表。必须与 `generated_responses` 的长度相同。

包含 conversational() 任务的“对话”部分的字典。

### ConversationalOutput

### `class huggingface_hub.inference._types.ConversationalOutput`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_types.py#L50)

```py
( )
```

参数

+   `generated_text` (`str`) — 模型的最后一个响应。

+   `conversation` (`ConversationalOutputConversation`) — 过去的对话。

+   `warnings` (`List[str]`) — 与过程相关的警告列表。

包含 conversational() 任务的输出的字典。

### ImageSegmentationOutput

### `class huggingface_hub.inference._types.ImageSegmentationOutput`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_types.py#L87)

```py
( )
```

参数

+   `label` (`str`) — 对应于掩码的标签。

+   `mask` (`Image`) — 代表模型预测的掩码的图像对象。

+   `score` (`float`) — 与此掩码的标签相关联的分数。

包含有关 image_segmentation() 任务的信息的字典。实际上，图像分割返回一个包含每个掩码的 `ImageSegmentationOutput` 的列表。

### ModelStatus

### `class huggingface_hub.inference._common.ModelStatus`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_common.py#L71)

```py
( loaded: bool state: str compute_type: str framework: str )
```

参数

+   `loaded` (`bool`) — 如果模型当前加载到 Hugging Face 的推理 API 中。模型是按需加载的，导致用户的第一个请求需要更长时间。如果模型已加载，您可以确保它处于健康状态。

+   `state` (`str`) — 模型的当前状态。可以是‘Loaded’、‘Loadable’、‘TooBig’。如果模型的状态是‘Loadable’，那么它不会太大并且有一个支持的后端。当用户首次请求端点上的推理时，可加载的模型会自动加载。这意味着对用户来说加载模型是透明的，只是第一次调用需要更长时间才能完成。

+   `compute_type` (`str`) — 模型正在使用或将使用的计算资源类型，如‘gpu’或‘cpu’。

+   `framework` (`str`) — 模型构建时使用的框架名称，如‘transformers’或‘text-generation-inference’。

这个数据类代表了 Hugging Face 推理 API 中模型的状态。

### TokenClassificationOutput

### `class huggingface_hub.inference._types.TokenClassificationOutput`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_types.py#L163)

```py
( )
```

参数

+   `entity_group` (`str`) — 被识别的实体类型（特定于模型）。

+   `score` (`float`) — 模型预测的标签得分。

+   `word` (`str`) — 被捕获的字符串。

+   `start` (`int`) — 答案所在的字符串偏移量。如果单词出现多次，这个信息很有用。

+   `end` (`int`) — 答案所在的字符串偏移量。如果单词出现多次，这个信息很有用。

包含 token_classification()任务输出的字典。

### 文本生成类型

text_generation()任务在`InferenceClient`中有比其他任务更好的支持。特别是，如果安装了此包，用户输入和服务器输出将使用[Pydantic](https://docs.pydantic.dev/latest/)进行验证。因此，我们建议安装它（`pip install pydantic`）以获得更好的用户体验。

以下是用于验证数据的数据类，特别是 TextGenerationParameters（输入）、TextGenerationResponse（输出）和 TextGenerationStreamResponse（流式输出）。

### `class huggingface_hub.inference._text_generation.TextGenerationParameters`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_text_generation.py#L59)

```py
( do_sample: bool = False max_new_tokens: int = 20 repetition_penalty: Optional = None return_full_text: bool = False stop: List = <factory> seed: Optional = None temperature: Optional = None top_k: Optional = None top_p: Optional = None truncate: Optional = None typical_p: Optional = None best_of: Optional = None watermark: bool = False details: bool = False decoder_input_details: bool = False )
```

参数

+   `do_sample` (`bool`, *可选*) — 激活 logits 抽样。默认为 False。

+   `max_new_tokens` (`int`, *可选*) — 生成的标记的最大数量。默认为 20。

+   `repetition_penalty` (`Optional[float]`, *可选*) — 重复惩罚的参数。值为 1.0 表示没有惩罚。有关更多详细信息，请参阅[此论文](https://arxiv.org/pdf/1909.05858.pdf)。默认为 None。

+   `return_full_text` (`bool`, *可选*) — 是否将提示添加到生成的文本之前。默认为 False。

+   `stop` (`List[str]`, *可选*) — 如果生成了`stop_sequences`的成员，则停止生成标记。默认为空列表。

+   `seed` (`Optional[int]`, *可选*) — 随机抽样种子。默认为 None。

+   `temperature` (`Optional[float]`, *可选*) — 用于调节 logits 分布的值。默认为 None。

+   `top_k` (`Optional[int]`, *optional*) — 保留最高概率词汇标记的数量以进行 top-k 过滤。默认为 None。

+   `top_p` (`Optional[float]`, *optional*) — 如果设置为小于 1 的值，则仅保留概率相加达到`top_p`或更高的最可能标记集。默认为 None。

+   `truncate` (`Optional[int]`, *optional*) — 将输入标记截断到给定大小。默认为 None。

+   `typical_p` (`Optional[float]`, *optional*) — 典型解码质量。有关更多信息，请参阅[Typical Decoding for Natural Language Generation](https://arxiv.org/abs/2202.00666)。默认为 None。

+   `best_of` (`Optional[int]`, *optional*) — 生成`best_of`序列并返回具有最高标记对数概率的序列。默认为 None。

+   `watermark` (`bool`, *optional*) — 使用[A Watermark for Large Language Models](https://arxiv.org/abs/2301.10226)进行水印处理。默认为 False。

+   `details` (`bool`, *optional*) — 获取生成细节。默认为 False。

+   `decoder_input_details` (`bool`, *optional*) — 获取解码器输入标记的对数概率和 ID。默认为 False。

文本生成的参数。

### `class huggingface_hub.inference._text_generation.TextGenerationResponse`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_text_generation.py#L394)

```py
( generated_text: str details: Optional = None )
```

参数

+   `generated_text` (`str`) — 生成的文本。

+   `details` (`Optional[Details]`) — 生成细节。仅在向服务器发送`details=True`时返回。

表示文本生成的响应。

仅在`details=True`时返回，否则返回一个字符串。

### `class huggingface_hub.inference._text_generation.TextGenerationStreamResponse`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_text_generation.py#L444)

```py
( token: Token generated_text: Optional = None details: Optional = None )
```

参数

+   `token` (`Token`) — 生成的标记。

+   `generated_text` (`Optional[str]`, *optional*) — 完整生成的文本。仅在生成完成时可用。

+   `details` (`Optional[StreamDetails]`, *optional*) — 生成细节。仅在生成完成时可用。

表示用于流式文本生成的响应。

仅在`details=True`和`stream=True`时返回。

### `class huggingface_hub.inference._text_generation.InputToken`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_text_generation.py#L238)

```py
( id: int text: str logprob: Optional = None )
```

参数

+   `id` (`int`) — 模型标记器中的标记 ID。

+   `text` (`str`) — 标记文本。

+   `logprob` (`float`或`None`) — 标记的对数概率。由于无法计算第一个标记的对数概率，因此是可选的。

表示一个输入标记。

### `class huggingface_hub.inference._text_generation.Token`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_text_generation.py#L262)

```py
( id: int text: str logprob: float special: bool )
```

参数

+   `id` (`int`) — 模型标记器中的标记 ID。

+   `text` (`str`) — 标记文本。

+   `logprob` (`float`) — 标记的对数概率。

+   `special` (`bool`) — 指示标记是否为特殊标记。在连接时可用于忽略标记。

表示一个标记。

### `class huggingface_hub.inference._text_generation.FinishReason`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_text_generation.py#L291)

```py
( value names = None module = None qualname = None type = None start = 1 )
```

一个枚举。

### `class huggingface_hub.inference._text_generation.BestOfSequence`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_text_generation.py#L301)

```py
( generated_text: str finish_reason: FinishReason generated_tokens: int seed: Optional = None prefill: List = <factory> tokens: List = <factory> )
```

参数

+   `generated_text` (`str`) — 生成的文本。

+   `finish_reason` (`FinishReason`) — 生成结束的原因，由`FinishReason`值表示。

+   `generated_tokens` (`int`) — 序列中生成的标记数量。

+   `seed` (`Optional[int]`) — 如果启用了抽样，则为抽样种子。

+   `prefill`（`List[InputToken]`）— 解码器输入标记。如果`decoder_input_details`为 False，则为空。默认为空列表。

+   `tokens`（`List[Token]`）— 生成的标记。默认为空列表。

表示在文本生成过程中生成的最佳序列。

### `class huggingface_hub.inference._text_generation.Details`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_text_generation.py#L345)

```py
( finish_reason: FinishReason generated_tokens: int seed: Optional = None prefill: List = <factory> tokens: List = <factory> best_of_sequences: Optional = None )
```

参数

+   `finish_reason`（`FinishReason`）— 生成结束的原因，由`FinishReason`值表示。

+   `generated_tokens`（`int`）— 生成的标记数。

+   `seed`（`Optional[int]`）— 如果激活了采样，则为采样种子。

+   `prefill`（`List[InputToken]`，*可选*）— 解码器输入标记。如果`decoder_input_details`为 False，则为空。默认为空列表。

+   `tokens`（`List[Token]`）— 生成的标记。默认为空列表。

+   `best_of_sequences`（`Optional[List[BestOfSequence]]`）— 使用`best_of`参数时的额外序列。

表示文本生成的详细信息。

### `class huggingface_hub.inference._text_generation.StreamDetails`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference/_text_generation.py#L421)

```py
( finish_reason: FinishReason generated_tokens: int seed: Optional = None )
```

参数

+   `finish_reason`（`FinishReason`）— 生成结束的原因，由`FinishReason`值表示。

+   `generated_tokens`（`int`）— 生成的标记数。

+   `seed`（`Optional[int]`）— 如果激活了采样，则为采样种子。

表示文本生成流的详细信息。

## InferenceAPI

`InferenceAPI`是调用推理 API 的传统方式。该接口更简单，需要知道每个任务的输入参数和输出格式。它还缺乏连接到其他服务（如推理端点或 AWS SageMaker）的能力。`InferenceAPI`将很快被弃用，因此我们建议尽可能使用 InferenceClient。查看此指南以了解如何从`InferenceAPI`切换到 InferenceClient 在您的脚本中。

### `class huggingface_hub.InferenceApi`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference_api.py#L46)

```py
( repo_id: str task: Optional = None token: Optional = None gpu: bool = False )
```

用于配置请求并调用 HuggingFace 推理 API 的客户端。

示例：

```py
>>> from huggingface_hub.inference_api import InferenceApi

>>> # Mask-fill example
>>> inference = InferenceApi("bert-base-uncased")
>>> inference(inputs="The goal of life is [MASK].")
[{'sequence': 'the goal of life is life.', 'score': 0.10933292657136917, 'token': 2166, 'token_str': 'life'}]

>>> # Question Answering example
>>> inference = InferenceApi("deepset/roberta-base-squad2")
>>> inputs = {
...     "question": "What's my name?",
...     "context": "My name is Clara and I live in Berkeley.",
... }
>>> inference(inputs)
{'score': 0.9326569437980652, 'start': 11, 'end': 16, 'answer': 'Clara'}

>>> # Zero-shot example
>>> inference = InferenceApi("typeform/distilbert-base-uncased-mnli")
>>> inputs = "Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!"
>>> params = {"candidate_labels": ["refund", "legal", "faq"]}
>>> inference(inputs, params)
{'sequence': 'Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!', 'labels': ['refund', 'faq', 'legal'], 'scores': [0.9378499388694763, 0.04914155602455139, 0.013008488342165947]}

>>> # Overriding configured task
>>> inference = InferenceApi("bert-base-uncased", task="feature-extraction")

>>> # Text-to-image
>>> inference = InferenceApi("stabilityai/stable-diffusion-2-1")
>>> inference("cat")
<PIL.PngImagePlugin.PngImageFile image (...)>

>>> # Return as raw response to parse the output yourself
>>> inference = InferenceApi("mio/amadeus")
>>> response = inference("hello world", raw_response=True)
>>> response.headers
{"Content-Type": "audio/flac", ...}
>>> response.content # raw bytes from server
b'(...)'
```

#### `__init__`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference_api.py#L93)

```py
( repo_id: str task: Optional = None token: Optional = None gpu: bool = False )
```

参数

+   `repo_id`（`str`）— 仓库的 ID（例如*user/bert-base-uncased*）。

+   `task`（`str`，*可选*，默认值`None`）— 是否强制执行任务，而不是使用存储库中指定的任务。

+   `token`（*str*，*可选*）— 用作 HTTP bearer 授权的 API 令牌。这不是身份验证令牌。您可以在[`huggingface.co/settings/token`](https://huggingface.co/settings/token)中找到令牌。或者，您可以使用*HfApi().whoami(token)*找到您的组织和个人 API 令牌。

+   `gpu`（*布尔*，*可选*，默认值*False*）— 是否在推理时使用 GPU 而不是 CPU（至少需要启动计划）。

初始化标题和 API 调用信息。

#### `__call__`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/inference_api.py#L158)

```py
( inputs: Union = None params: Optional = None data: Optional = None raw_response: bool = False )
```

参数

+   `inputs`（`str`或`Dict`或`List[str]`或`List[List[str]]`，*可选*）— 用于预测的输入。

+   `params`（`Dict`，*可选*）— 模型的额外参数。将作为有效载荷中的`parameters`发送。

+   `data`（`bytes`，*可选*）— 请求的字节内容。在这种情况下，将`inputs`和`params`留空。

+   `raw_response` (`bool`, 默认为 `False`) — 如果为 `True`，则返回原始的 `Response` 对象。您可以根据需要解析其内容。默认情况下，内容会被解析为更实用的格式（例如 json 字典或 PIL 图像）。

调用推理 API。
