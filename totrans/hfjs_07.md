# 类: HfInferenceEndpoint

> [https://huggingface.co/docs/huggingface.js/inference/classes/HfInferenceEndpoint](https://huggingface.co/docs/huggingface.js/inference/classes/HfInferenceEndpoint)

## 层次结构

+   `无访问令牌无模型任务`

    ↳ **`HfInferenceEndpoint`**

## 构造函数

### 构造函数

• `new HfInferenceEndpoint`(`endpointUrl`, `accessToken?`, `defaultOptions?`)

#### 参数

| 名称 | 类型 | 默认值 |
| :-- | :-- | :-- |
| `endpointUrl` | `字符串` | `未定义` |
| `accessToken` | `字符串` | `""` |
| `defaultOptions` | [`选项`](../interfaces/Options) | `{}` |

#### 定义于

[inference/src/HfInference.ts:48](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/HfInference.ts#L48)

## 属性

### 音频分类

• `音频分类`: (`args`: { `data`: `Blob` | `ArrayBuffer` }, `options?`: [`选项`](../interfaces/Options)) => `Promise`<[`音频分类返回`](../modules#audioclassificationreturn)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`音频分类返回`](../modules#audioclassificationreturn)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `对象` | - |
| `args.data` | `Blob` &#124; `ArrayBuffer` | 二进制音频数据 |
| `options?` | [`选项`](../interfaces/Options) | - |

##### 返回

`Promise`<[`音频分类返回`](../modules#audioclassificationreturn)>

#### 定义于

[inference/src/tasks/audio/audioClassification.ts:30](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/audioClassification.ts#L30)

* * *

### 音频到音频

• `音频到音频`: (`args`: { `data`: `Blob` | `ArrayBuffer` }, `options?`: [`选项`](../interfaces/Options)) => `Promise`<[`音频到音频返回`](../modules#audiotoaudioreturn)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`音频到音频返回`](../modules#audiotoaudioreturn)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `对象` | - |
| `args.data` | `Blob` &#124; `ArrayBuffer` | 二进制音频数据 |
| `options?` | [`选项`](../interfaces/Options) | - |

##### 返回

`Promise`<[`音频到音频返回`](../modules#audiotoaudioreturn)>

#### 定义于

[inference/src/tasks/audio/audioToAudio.ts:35](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/audioToAudio.ts#L35)

* * *

### 自动语音识别

• `自动语音识别`: (`args`: { `data`: `Blob` | `ArrayBuffer` }, `options?`: [`选项`](../interfaces/Options)) => `Promise`<[`自动语音识别输出`](../interfaces/AutomaticSpeechRecognitionOutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`自动语音识别输出`](../interfaces/AutomaticSpeechRecognitionOutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `对象` | - |
| `args.data` | `Blob` &#124; `ArrayBuffer` | 二进制音频数据 |
| `options?` | [`选项`](../interfaces/Options) | - |

##### 返回

`Promise`<[`自动语音识别输出`](../interfaces/AutomaticSpeechRecognitionOutput)>

#### 定义于

[inference/src/tasks/audio/automaticSpeechRecognition.ts:23](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/automaticSpeechRecognition.ts#L23)

* * *

### 对话

• `对话`: (`args`: { `inputs`: { `generated_responses?`: `string`[] ; `past_user_inputs?`: `string`[] ; `text`: `string` } ; `parameters?`: { `max_length?`: `number` ; `max_time?`: `number` ; `min_length?`: `number` ; `repetition_penalty?`: `number` ; `temperature?`: `number` ; `top_k?`: `number` ; `top_p?`: `number` } }, `options?`: [`选项`](../interfaces/Options)) => `Promise`<[`对话输出`](../interfaces/ConversationalOutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`对话输出`](../interfaces/ConversationalOutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `对象` | - |
| `args.inputs` | `对象` | - |
| `args.inputs.generated_responses?` | `string`[] | 一个字符串列表，对应模型先前的回复。 |
| `args.inputs.past_user_inputs?` | `string`[] | 一个字符串列表，对应用户先前的回复。应该与生成的回复长度相同。 |
| `args.inputs.text` | `string` | 会话中用户的最后输入。 |
| `args.parameters?` | `Object` | - |
| `args.parameters.max_length?` | `number` | (默认: 无). 整数，定义输出摘要的最大标记长度。 |
| `args.parameters.max_time?` | `number` | (默认: 无). 浮点数 (0-120.0). 查询应该最多花费的时间量，以秒为单位。网络可能会导致一些开销，因此这将是一个软限制。 |
| `args.parameters.min_length?` | `number` | (默认: 无). 整数，定义输出摘要的最小标记长度。 |
| `args.parameters.repetition_penalty?` | `number` | (默认: 无). 浮点数 (0.0-100.0). 在生成过程中一个标记被使用得越多，它在后续生成过程中被惩罚的程度就越大。 |
| `args.parameters.temperature?` | `number` | (默认: 1.0). 浮点数 (0.0-100.0). 采样操作的温度。1表示常规采样，0表示始终选择最高分数，100.0接近均匀概率。 |
| `args.parameters.top_k?` | `number` | (默认: 无). 整数，定义在样本操作中考虑的前k个标记，以创建新文本。 |
| `args.parameters.top_p?` | `number` | (默认: 无). 浮点数，定义在文本生成的样本操作中的标记。将标记添加到样本中，以使概率从最可能到最不可能，直到概率之和大于top_p。 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`ConversationalOutput`](../interfaces/ConversationalOutput)>

#### 定义于

[inference/src/tasks/nlp/conversational.ts:65](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/conversational.ts#L65)

* * *

### documentQuestionAnswering

• `documentQuestionAnswering`: (`args`: { `inputs`: { `image`: `Blob` | `ArrayBuffer` ; `question`: `string` } }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`DocumentQuestionAnsweringOutput`](../interfaces/DocumentQuestionAnsweringOutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`DocumentQuestionAnsweringOutput`](../interfaces/DocumentQuestionAnsweringOutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `Object` | - |
| `args.inputs.image` | `Blob` &#124; `ArrayBuffer` | 原始图像。您可以在浏览器中使用原生的`File`，或在节点中使用`new Blob([buffer])`，或对于base64图像`new Blob([btoa(base64String)])`，甚至`await (await fetch('...)).blob()` |
| `args.inputs.question` | `string` | - |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`DocumentQuestionAnsweringOutput`](../interfaces/DocumentQuestionAnsweringOutput)>

#### 定义于

[inference/src/tasks/multimodal/documentQuestionAnswering.ts:42](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/multimodal/documentQuestionAnswering.ts#L42)

* * *

### featureExtraction

• `featureExtraction`: (`args`: { `inputs`: `string` | `string`[] }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`FeatureExtractionOutput`](../modules#featureextractionoutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`FeatureExtractionOutput`](../modules#featureextractionoutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `string` &#124; `string`[] | 输入是一个字符串或一个字符串列表，用于获取特征。输入: “那是一个快乐的人”， |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`FeatureExtractionOutput`](../modules#featureextractionoutput)>

#### 定义于

[inference/src/tasks/nlp/featureExtraction.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/featureExtraction.ts#L24)

* * *

### fillMask

• `fillMask`: (`args`: { `inputs`: `string` }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`FillMaskOutput`](../modules#fillmaskoutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`FillMaskOutput`](../modules#fillmaskoutput)>

##### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `Object` |
| `args.inputs` | `string` |
| `options?` | [`Options`](../interfaces/Options) |

##### 返回

`Promise`<[`FillMaskOutput`](../modules#fillmaskoutput)>

#### 定义在

[inference/src/tasks/nlp/fillMask.ts:31](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/fillMask.ts#L31)

* * *

### imageClassification

• `imageClassification`: (`args`: { `data`: `Blob` | `ArrayBuffer` }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`ImageClassificationOutput`](../modules#imageclassificationoutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`ImageClassificationOutput`](../modules#imageclassificationoutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.data` | `Blob` &#124; `ArrayBuffer` | 二进制图像数据 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`ImageClassificationOutput`](../modules#imageclassificationoutput)>

#### 定义在

[inference/src/tasks/cv/imageClassification.ts:29](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageClassification.ts#L29)

* * *

### 图像分割

• `imageSegmentation`: (`args`: { `data`: `Blob` | `ArrayBuffer` }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`ImageSegmentationOutput`](../modules#imagesegmentationoutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`ImageSegmentationOutput`](../modules#imagesegmentationoutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.data` | `Blob` &#124; `ArrayBuffer` | 二进制图像数据 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`ImageSegmentationOutput`](../modules#imagesegmentationoutput)>

#### 定义在

[inference/src/tasks/cv/imageSegmentation.ts:33](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageSegmentation.ts#L33)

* * *

### imageToImage

• `imageToImage`: (`args`: { `inputs`: `Blob` | `ArrayBuffer` ; `parameters?`: { `guess_mode?`: `boolean` ; `guidance_scale?`: `number` ; `height?`: `number` ; `negative_prompt?`: `string` ; `num_inference_steps?`: `number` ; `prompt?`: `string` ; `strength?`: `number` ; `width?`: `number` } }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<`Blob`>

#### 类型声明

▸ (`args`, `options?`): `Promise`<`Blob`>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `Blob` &#124; `ArrayBuffer` | 初始图像条件 |
| `args.parameters?` | `Object` | - |
| `args.parameters.guess_mode?` | `boolean` | 猜测模式仅适用于ControlNet模型，默认为False，在此模式下，ControlNet编码器将尽力识别输入图像的内容，即使您删除所有提示。建议在3.0到5.0之间的`guidance_scale`。 |
| `args.parameters.guidance_scale?` | `number` | 引导比例：更高的引导比例鼓励生成与文本“提示”紧密相关的图像，通常以降低图像质量为代价。 |
| `args.parameters.height?` | `number` | 生成图像的像素高度 |
| `args.parameters.negative_prompt?` | `string` | 用于图像生成的可选负面提示 |
| `args.parameters.num_inference_steps?` | `number` | 降噪步骤的数量。更多的降噪步骤通常会导致更高质量的图像，但会降低推理速度。 |
| `args.parameters.prompt?` | `字符串` | 引导图像生成的文本提示。 |
| `args.parameters.strength?` | `数字` | 强度参数仅适用于SD img2img和alt扩散img2img模型。在概念上，指示要转换参考`图像`的程度。必须在0和1之间。`图像`将被用作起点，添加更多噪音，`强度`越大。去噪步骤的数量取决于最初添加的噪音量。当`强度`为1时，添加的噪音将是最大的，并且去噪过程将运行指定的`num_inference_steps`的完整迭代次数。因此，值为1基本上忽略了`图像`。 |
| `args.parameters.width?` | `数字` | 生成图像的像素宽度 |
| `选项？` | [`选项`](../interfaces/Options) | - |

##### 返回

`Promise`<`Blob`>

#### 定义于

[inference/src/tasks/cv/imageToImage.ts:61](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageToImage.ts#L61)

* * *

### imageToText

• `imageToText`：(`参数`：{ `数据`：`Blob` | `ArrayBuffer` }, `选项？`：[`选项`](../interfaces/Options)) => `Promise`<[`图像到文本输出`](../interfaces/ImageToTextOutput]>

#### 类型声明

▸ (`参数`，`选项？`): `Promise`<[`图像到文本输出`](../interfaces/ImageToTextOutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `对象` | - |
| `args.data` | `Blob` &#124; `ArrayBuffer` | 二进制图像数据 |
| `选项？` | [`选项`](../interfaces/Options) | - |

##### 返回

`Promise`<[`图像到文本输出`](../interfaces/ImageToTextOutput)>

#### 定义于

[inference/src/tasks/cv/imageToText.ts:22](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageToText.ts#L22)

* * *

### objectDetection

• `objectDetection`：(`参数`：{ `数据`：`Blob` | `ArrayBuffer` }, `选项？`：[`选项`](../interfaces/Options)) => `Promise`<[`对象检测输出`](../modules#objectdetectionoutput)>

#### 类型声明

▸ (`参数`，`选项？`): `Promise`<[`对象检测输出`](../modules#objectdetectionoutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `对象` | - |
| `args.data` | `Blob` &#124; `ArrayBuffer` | 二进制图像数据 |
| `选项？` | [`选项`](../interfaces/Options) | - |

##### 返回

`Promise`<[`对象检测输出`](../modules#objectdetectionoutput)>

#### 定义于

[inference/src/tasks/cv/objectDetection.ts:39](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/objectDetection.ts#L39)

* * *

### questionAnswering

• `questionAnswering`：(`参数`：{ `输入`：{ `上下文`：`字符串` ; `问题`：`字符串` } }, `选项？`：[`选项`](../interfaces/Options)) => `Promise`<[`问答输出`](../interfaces/QuestionAnsweringOutput)>

#### 类型声明

▸ (`参数`，`选项？`): `Promise`<[`问答输出`](../interfaces/QuestionAnsweringOutput)>

##### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `对象` |
| `args.inputs` | `对象` |
| `args.inputs.context` | `字符串` |
| `args.inputs.question` | `字符串` |
| `选项？` | [`选项`](../interfaces/Options) |

##### 返回

`Promise`<[`问答输出`](../interfaces/QuestionAnsweringOutput)>

#### 定义于

[inference/src/tasks/nlp/questionAnswering.ts:34](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/questionAnswering.ts#L34)

* * *

### 请求

• `请求`：(`参数`：{ `数据`：`Blob` | `ArrayBuffer` ; `参数？`：`Record`<`字符串`，`未知`> } | { `输入`：`未知` ; `参数？`：`Record`<`字符串`，`未知`> }，`选项？`：[`选项`](../interfaces/Options) & { `任务？`：`字符串` ; `任务提示？`：[`推断任务`](../modules#inferencetask) }) => `Promise`<`未知`>

#### 类型声明

▸ (`参数`，`选项？`): `Promise`<`未知`>

##### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | { `data`: `Blob` &#124; `ArrayBuffer` ; `parameters?`: `Record`<`string`, `unknown`> } &#124; { `inputs`: `unknown` ; `parameters?`: `Record`<`string`, `unknown`> } |
| `options?` | [`Options`](../interfaces/Options) & { `task?`: `string` ; `taskHint?`: [`InferenceTask`](../modules#inferencetask) } |

##### 返回

`Promise`<`unknown`>

#### 定义在

[inference/src/tasks/custom/request.ts:7](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/custom/request.ts#L7)

* * *

### sentenceSimilarity

• `sentenceSimilarity`: (`args`: { `inputs`: `Record`<`string`, `unknown`> | `Record`<`string`, `unknown`>[] }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`SentenceSimilarityOutput`](../modules#sentencesimilarityoutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`SentenceSimilarityOutput`](../modules#sentencesimilarityoutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `Record`<`string`, `unknown`> &#124; `Record`<`string`, `unknown`>[] | 输入根据模型而异。例如，当使用sentence-transformers/paraphrase-xlm-r-multilingual-v1时，输入将包含一个`source_sentence`字符串和一个字符串数组`sentences` |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`SentenceSimilarityOutput`](../modules#sentencesimilarityoutput)>

#### 定义在

[inference/src/tasks/nlp/sentenceSimilarity.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/sentenceSimilarity.ts#L24)

* * *

### streamingRequest

• `streamingRequest`: (`args`: { `data`: `Blob` | `ArrayBuffer` ; `parameters?`: `Record`<`string`, `unknown`> } | { `inputs`: `unknown` ; `parameters?`: `Record`<`string`, `unknown`> }, `options?`: [`Options`](../interfaces/Options) & { `task?`: `string` ; `taskHint?`: [`InferenceTask`](../modules#inferencetask) }) => `AsyncGenerator`<`unknown`, `any`, `unknown`>

#### 类型声明

▸ (`args`, `options?`): `AsyncGenerator`<`unknown`, `any`, `unknown`>

##### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | { `data`: `Blob` &#124; `ArrayBuffer` ; `parameters?`: `Record`<`string`, `unknown`> } &#124; { `inputs`: `unknown` ; `parameters?`: `Record`<`string`, `unknown`> } |
| `options?` | [`Options`](../interfaces/Options) & { `task?`: `string` ; `taskHint?`: [`InferenceTask`](../modules#inferencetask) } |

##### 返回

`AsyncGenerator`<`unknown`, `any`, `unknown`>

#### 定义在

[inference/src/tasks/custom/streamingRequest.ts:9](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/custom/streamingRequest.ts#L9)

* * *

### summarization

• `summarization`: (`args`: { `inputs`: `string` ; `parameters?`: { `max_length?`: `number` ; `max_time?`: `number` ; `min_length?`: `number` ; `repetition_penalty?`: `number` ; `temperature?`: `number` ; `top_k?`: `number` ; `top_p?`: `number` } }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`SummarizationOutput`](../interfaces/SummarizationOutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`SummarizationOutput`](../interfaces/SummarizationOutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `string` | 要总结的字符串 |
| `args.parameters?` | `Object` | - |
| `args.parameters.max_length?` | `number` | (默认值：无). 整数，定义输出摘要的最大长度（以标记为单位）。 |
| `args.parameters.max_time?` | `number` | (默认值：无). 浮点数 (0-120.0). 查询应该最多花费的时间（以秒为单位）。网络可能会导致一些额外开销，因此这将是一个软限制。 |
| `args.parameters.min_length?` | `number` | (默认值：无). 整数，定义输出摘要的最小长度（以标记为单位）。 |
| `args.parameters.repetition_penalty?` | `number` | (默认值：无)。浮点数（0.0-100.0）。在生成过程中一个标记被使用得越多，它在连续的生成过程中被惩罚的可能性就越大。 |
| `args.parameters.temperature?` | `number` | (默认值：1.0)。浮点数（0.0-100.0）。采样操作的温度。1表示常规采样，0表示始终选择最高分数，100.0接近均匀概率。 |
| `args.parameters.top_k?` | `number` | (默认值：无)。整数，用于定义在样本操作中考虑的前几个标记，以创建新文本。 |
| `args.parameters.top_p?` | `number` | (默认值：无)。浮点数，用于定义在文本生成的样本操作中的标记。在样本中添加标记，以便从最可能到最不可能，直到概率之和大于top_p。 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`SummarizationOutput`](../interfaces/SummarizationOutput)>

#### 定义在

[inference/src/tasks/nlp/summarization.ts:52](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/summarization.ts#L52)

* * *

### tableQuestionAnswering

• `tableQuestionAnswering`: (`args`: { `inputs`: { `query`: `string` ; `table`: `Record`<`string`, `string`[]> } }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`TableQuestionAnsweringOutput`](../interfaces/TableQuestionAnsweringOutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`TableQuestionAnsweringOutput`](../interfaces/TableQuestionAnsweringOutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `Object` | - |
| `args.inputs.query` | `string` | 您想要询问表格的纯文本查询 |
| `args.inputs.table` | `Record`<`string`, `string`[]> | 以字典列表表示的数据表，其中条目是标题，列表是所有值，所有列表必须具有相同的大小。 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`TableQuestionAnsweringOutput`](../interfaces/TableQuestionAnsweringOutput)>

#### 定义在

[inference/src/tasks/nlp/tableQuestionAnswering.ts:40](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tableQuestionAnswering.ts#L40)

* * *

### tabularClassification

• `tabularClassification`: (`args`: { `inputs`: { `data`: `Record`<`string`, `string`[]> } }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`TabularClassificationOutput`](../modules#tabularclassificationoutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`TabularClassificationOutput`](../modules#tabularclassificationoutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `Object` | - |
| `args.inputs.data` | `Record`<`string`, `string`[]> | 以字典列表表示的数据表，其中条目是标题，列表是所有值，所有列表必须具有相同的大小。 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`TabularClassificationOutput`](../modules#tabularclassificationoutput)>

#### 定义在

[inference/src/tasks/tabular/tabularClassification.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/tabular/tabularClassification.ts#L24)

* * *

### tabularRegression

• `tabularRegression`: (`args`: { `inputs`: { `data`: `Record`<`string`, `string`[]> } }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`TabularRegressionOutput`](../modules#tabularregressionoutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`TabularRegressionOutput`](../modules#tabularregressionoutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `Object` | - |
| `args.inputs.data` | `Record`<`string`, `string`[]> | 以字典列表表示的数据表，其中条目为标题，列表为所有值，所有列表必须具有相同的大小。 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`TabularRegressionOutput`](../modules#tabularregressionoutput)>

#### 定义于

[inference/src/tasks/tabular/tabularRegression.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/tabular/tabularRegression.ts#L24)

* * *

### 文本分类

• `文本分类`: (`args`: { `inputs`: `string` }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`TextClassificationOutput`](../modules#textclassificationoutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`TextClassificationOutput`](../modules#textclassificationoutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `string` | 要分类的字符串 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`TextClassificationOutput`](../modules#textclassificationoutput)>

#### 定义于

[inference/src/tasks/nlp/textClassification.ts:26](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textClassification.ts#L26)

* * *

### 文本生成

• `文本生成`: (`args`: { `inputs`: `string` ; `parameters?`: { `do_sample?`: `boolean` ; `max_new_tokens?`: `number` ; `max_time?`: `number` ; `num_return_sequences?`: `number` ; `repetition_penalty?`: `number` ; `return_full_text?`: `boolean` ; `stop_sequences?`: `string`[] ; `temperature?`: `number` ; `top_k?`: `number` ; `top_p?`: `number` ; `truncate?`: `number` } }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`TextGenerationOutput`](../interfaces/TextGenerationOutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`TextGenerationOutput`](../interfaces/TextGenerationOutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `string` | 要生成的字符串 |
| `args.parameters?` | `Object` | - |
| `args.parameters.do_sample?` | `boolean` | (可选：True). 布尔值。是否使用采样，否则使用贪婪解码。 |
| `args.parameters.max_new_tokens?` | `number` | (默认值：无). 整数 (0-250). 要生成的新标记数量，这不包括输入长度，它是您想要生成的文本大小的估计。每个新标记都会减慢请求速度，因此要在响应时间和生成文本长度之间寻找平衡。 |
| `args.parameters.max_time?` | `number` | (默认值：无). 浮点数 (0-120.0). 查询应该花费的最长时间（以秒为单位）。网络可能会造成一些开销，因此这将是一个软限制。与 max_new_tokens 结合使用可获得最佳结果。 |
| `args.parameters.num_return_sequences?` | `number` | (默认值：1). 整数。要返回的建议数量。 |
| `args.parameters.repetition_penalty?` | `number` | (默认值：无). 浮点数 (0.0-100.0). 在生成过程中一个标记被使用得越多，它在连续生成过程中被惩罚的越多。 |
| `args.parameters.return_full_text?` | `boolean` | (默认值：True). 布尔值。如果设置为 False，则返回结果将不包含原始查询，使提示更容易。 |
| `args.parameters.stop_sequences?` | `string`[] | (默认值：[]) 字符串列表。当生成列表中的一个字符串时，模型将停止生成文本。 * |
| `args.parameters.temperature?` | `number` | (默认值：1.0). 浮点数 (0.0-100.0). 采样操作的温度。1 表示常规采样，0 表示始终选择最高分数，100.0 表示接近均匀概率。 |
| `args.parameters.top_k?` | `number` | (默认值：无). 整数，用于定义在样本操作中考虑的前几个标记，以生成新文本。 |
| `args.parameters.top_p?` | `number` | (默认值：无). 浮点数，用于定义在文本生成的采样操作中的标记。将标记添加到采样中，以使概率从最可能到最不可能，直到概率之和大于 top_p。 |
| `args.parameters.truncate?` | `number` | (默认值：无). 整数。输入中的最大标记数。 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`TextGenerationOutput`](../interfaces/TextGenerationOutput)>

#### 定义在

[inference/src/tasks/nlp/textGeneration.ts:68](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGeneration.ts#L68)

* * *

### textGenerationStream

• `textGenerationStream`: (`args`: { `inputs`: `string` ; `parameters?`: { `do_sample?`: `boolean` ; `max_new_tokens?`: `number` ; `max_time?`: `number` ; `num_return_sequences?`: `number` ; `repetition_penalty?`: `number` ; `return_full_text?`: `boolean` ; `stop_sequences?`: `string`[] ; `temperature?`: `number` ; `top_k?`: `number` ; `top_p?`: `number` ; `truncate?`: `number` } }, `options?`: [`Options`](../interfaces/Options)) => `AsyncGenerator`<[`TextGenerationStreamOutput`](../interfaces/TextGenerationStreamOutput), `any`, `unknown`>

#### 类型声明

▸ (`args`, `options?`): `AsyncGenerator`<[`TextGenerationStreamOutput`](../interfaces/TextGenerationStreamOutput), `any`, `unknown`>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `string` | 要生成的字符串 |
| `args.parameters?` | `Object` | - |
| `args.parameters.do_sample?` | `boolean` | (可选：True). 布尔值。是否使用采样，否则使用贪婪解码。 |
| `args.parameters.max_new_tokens?` | `number` | (默认值：无). 整数 (0-250). 要生成的新标记数量，这不包括输入长度，它是您想要生成的文本大小的估计。每个新标记都会减慢请求速度，因此需要在响应时间和生成文本长度之间寻找平衡。 |
| `args.parameters.max_time?` | `number` | (默认值：无). 浮点数 (0-120.0). 查询应该最多花费的时间量（以秒为单位）。网络可能会导致一些额外开销，因此这将是一个软限制。与 max_new_tokens 结合使用以获得最佳结果。 |
| `args.parameters.num_return_sequences?` | `number` | (默认值：1). 整数。要返回的建议数量。 |
| `args.parameters.repetition_penalty?` | `number` | (默认值：无). 浮点数 (0.0-100.0). 在生成过程中一个标记被使用得越多，它在连续的生成过程中被惩罚的可能性就越大。 |
| `args.parameters.return_full_text?` | `boolean` | (默认值：True). 布尔值。如果设置为 False，则返回结果将不包含原始查询，使提示更容易。 |
| `args.parameters.stop_sequences?` | `string`[] | (默认值：[]) 字符串列表。当列表中的一个字符串被生成时，模型将停止生成文本。* |
| `args.parameters.temperature?` | `number` | (默认值：1.0). 浮点数 (0.0-100.0). 采样操作的温度。1 表示常规采样，0 表示始终选择最高分数，100.0 表示接近均匀概率。 |
| `args.parameters.top_k?` | `number` | (默认值：无). 整数，用于定义在采样操作中考虑的前几个标记，以创建新文本。 |
| `args.parameters.top_p?` | `number` | (默认值：无). 浮点数，用于定义在文本生成的采样操作中的标记。将标记添加到采样中，以使概率从最可能到最不可能，直到概率之和大于 top_p。 |
| `args.parameters.truncate?` | `number` | (默认值：无). 整数。输入中的最大标记数。 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`AsyncGenerator`<[`TextGenerationStreamOutput`](../interfaces/TextGenerationStreamOutput), `any`, `unknown`>

#### 定义在

[inference/src/tasks/nlp/textGenerationStream.ts:87](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L87)

* * *

### textToImage

• `textToImage`: (`args`: { `inputs`: `string` ; `parameters?`: { `guidance_scale?`: `number` ; `height?`: `number` ; `negative_prompt?`: `string` ; `num_inference_steps?`: `number` ; `width?`: `number` } }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<`Blob`>

#### 类型声明

▸ (`args`, `options?`): `Promise`<`Blob`>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `string` | 从中生成图像的文本 |
| `args.parameters?` | `Object` | - |
| `args.parameters.guidance_scale?` | `number` | 指导比例：更高的指导比例鼓励生成与文本“提示”紧密相关的图像，通常以降低图像质量为代价。 |
| `args.parameters.height?` | `number` | 生成图像的像素高度 |
| `args.parameters.negative_prompt?` | `string` | 用于图像生成的可选负面提示 |
| `args.parameters.num_inference_steps?` | `number` | 降噪步骤的数量。更多的降噪步骤通常会导致更高质量的图像，但会降低推理速度。 |
| `args.parameters.width?` | `number` | 生成图像的像素宽度 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<`Blob`>

#### 定义在

[inference/src/tasks/cv/textToImage.ts:41](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/textToImage.ts#L41)

* * *

### textToSpeech

• `textToSpeech`: (`args`: { `inputs`: `string` }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<`Blob`>

#### 类型声明

▸ (`args`, `options?`): `Promise`<`Blob`>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `string` | 从中生成音频的文本 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<`Blob`>

#### 定义在

[inference/src/tasks/audio/textToSpeech.ts:18](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/textToSpeech.ts#L18)

* * *

### tokenClassification

• `tokenClassification`: (`args`: { `inputs`: `string` ; `parameters?`: { `aggregation_strategy?`: `"none"` | `"simple"` | `"first"` | `"average"` | `"max"` } }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`TokenClassificationOutput`](../modules#tokenclassificationoutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`TokenClassificationOutput`](../modules#tokenclassificationoutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `string` | 要分类的字符串 |
| `args.parameters?` | `Object` | - |
| `args.parameters.aggregation_strategy?` | `"none"` &#124; `"simple"` &#124; `"first"` &#124; `"average"` &#124; `"max"` | （默认值：简单）。有几种聚合策略：none：每个标记都会被分类，不会有进一步的聚合。simple：实体根据默认模式进行分组（当标记相似时，B-、I-标记会合并）。first：与简单策略相同，除非单词不能以不同的标记结束。当存在歧义时，单词将使用第一个标记的标记。average：与简单策略相同，除非单词不能以不同的标记结束。分数将在标记上平均，然后应用最大标签。max：与简单策略相同，除非单词不能以不同的标记结束。单词实体将是具有最大分数的标记。 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`TokenClassificationOutput`](../modules#tokenclassificationoutput)>

#### 定义在

[inference/src/tasks/nlp/tokenClassification.ts:57](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tokenClassification.ts#L57)

* * *

### 翻译

• `translation`: (`args`: { `inputs`: `string` | `string`[] }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`TranslationOutput`](../modules#translationoutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`TranslationOutput`](../modules#translationoutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `string` &#124; `string`[] | 要翻译的字符串 |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`TranslationOutput`](../modules#translationoutput)>

#### 定义于

[inference/src/tasks/nlp/translation.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/translation.ts#L24)

* * *

### 视觉问答

• `visualQuestionAnswering`: (`args`: { `inputs`: { `image`: `Blob` | `ArrayBuffer` ; `question`: `string` } }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`VisualQuestionAnsweringOutput`](../interfaces/VisualQuestionAnsweringOutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`VisualQuestionAnsweringOutput`](../interfaces/VisualQuestionAnsweringOutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `Object` | - |
| `args.inputs.image` | `Blob` &#124; `ArrayBuffer` | 原始图像，您可以在浏览器中使用本机`File`，或在节点中使用`new Blob([buffer])`，或对于base64图像`new Blob([btoa(base64String)])`，甚至`await (await fetch('...)).blob()` |
| `args.inputs.question` | `string` | - |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`VisualQuestionAnsweringOutput`](../interfaces/VisualQuestionAnsweringOutput)>

#### 定义于

[inference/src/tasks/multimodal/visualQuestionAnswering.ts:32](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/multimodal/visualQuestionAnswering.ts#L32)

* * *

### zeroShotClassification

• `zeroShotClassification`: (`args`: { `inputs`: `string` | `string`[] ; `parameters`: { `candidate_labels`: `string`[] ; `multi_label?`: `boolean` } }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`ZeroShotClassificationOutput`](../modules#zeroshotclassificationoutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`ZeroShotClassificationOutput`](../modules#zeroshotclassificationoutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `string` &#124; `string`[] | 一个字符串或字符串列表 |
| `args.parameters` | `Object` | - |
| `args.parameters.candidate_labels` | `string`[] | 一个字符串列表，是输入的潜在类别。（最多10个候选标签，如果需要更多，可以简单地运行多个请求，如果使用太多候选标签，结果会产生误导。如果想保持完全相同，可以简单地运行multi_label=True，并在您的端上进行缩放。） |
| `args.parameters.multi_label?` | `boolean` | （默认值：false）如果类别可以重叠，则设置为True |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`ZeroShotClassificationOutput`](../modules#zeroshotclassificationoutput)>

#### 定义于

[inference/src/tasks/nlp/zeroShotClassification.ts:34](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/zeroShotClassification.ts#L34)

* * *

### zeroShotImageClassification

• `zeroShotImageClassification`: (`args`: { `inputs`: { `image`: `Blob` | `ArrayBuffer` } ; `parameters`: { `candidate_labels`: `string`[] } }, `options?`: [`Options`](../interfaces/Options)) => `Promise`<[`ZeroShotImageClassificationOutput`](../modules#zeroshotimageclassificationoutput)>

#### 类型声明

▸ (`args`, `options?`): `Promise`<[`ZeroShotImageClassificationOutput`](../modules#zeroshotimageclassificationoutput)>

##### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `args` | `Object` | - |
| `args.inputs` | `Object` | - |
| `args.inputs.image` | `Blob` &#124; `ArrayBuffer` | 二进制图像数据 |
| `args.parameters` | `Object` | - |
| `args.parameters.candidate_labels` | `string`[] | 一个潜在的输入类别字符串列表（最多10个） |
| `options?` | [`Options`](../interfaces/Options) | - |

##### 返回

`Promise`<[`ZeroShotImageClassificationOutput`](../modules#zeroshotimageclassificationoutput)>

#### 定义在

[inference/src/tasks/cv/zeroShotImageClassification.ts:33](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/zeroShotImageClassification.ts#L33)
