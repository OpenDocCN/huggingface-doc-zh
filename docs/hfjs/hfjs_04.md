# @huggingface/inference

> 原文链接: [`huggingface.co/docs/huggingface.js/inference/modules`](https://huggingface.co/docs/huggingface.js/inference/modules)

## Classes

+   HfInference

+   HfInferenceEndpoint

+   InferenceOutputError

## Interfaces

+   AudioClassificationOutputValue

+   AudioToAudioOutputValue

+   AutomaticSpeechRecognitionOutput

+   BaseArgs

+   ConversationalOutput

+   DocumentQuestionAnsweringOutput

+   ImageClassificationOutputValue

+   ImageSegmentationOutputValue

+   ImageToTextOutput

+   ObjectDetectionOutputValue

+   Options

+   QuestionAnsweringOutput

+   SummarizationOutput

+   TableQuestionAnsweringOutput

+   TextGenerationOutput

+   TextGenerationStreamBestOfSequence

+   TextGenerationStreamDetails

+   TextGenerationStreamOutput

+   TextGenerationStreamPrefillToken

+   TextGenerationStreamToken

+   TokenClassificationOutputValue

+   TranslationOutputValue

+   VisualQuestionAnsweringOutput

+   ZeroShotClassificationOutputValue

+   ZeroShotImageClassificationOutputValue

## Type Aliases

### AudioClassificationArgs

Ƭ `AudioClassificationArgs`: `BaseArgs` & { `data`: `Blob` | `ArrayBuffer` }

#### Defined in

[inference/src/tasks/audio/audioClassification.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/audioClassification.ts#L5)

* * *

### AudioClassificationReturn

Ƭ `AudioClassificationReturn`: `AudioClassificationOutputValue`[]

#### Defined in

[inference/src/tasks/audio/audioClassification.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/audioClassification.ts#L24)

* * *

### AudioToAudioArgs

Ƭ `AudioToAudioArgs`: `BaseArgs` & { `data`: `Blob` | `ArrayBuffer` }

#### Defined in

[inference/src/tasks/audio/audioToAudio.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/audioToAudio.ts#L5)

* * *

### AudioToAudioReturn

Ƭ `AudioToAudioReturn`: `AudioToAudioOutputValue`[]

#### Defined in

[inference/src/tasks/audio/audioToAudio.ts:29](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/audioToAudio.ts#L29)

* * *

### AutomaticSpeechRecognitionArgs

Ƭ `AutomaticSpeechRecognitionArgs`: `BaseArgs` & { `data`: `Blob` | `ArrayBuffer` }

#### Defined in

[inference/src/tasks/audio/automaticSpeechRecognition.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/automaticSpeechRecognition.ts#L5)

* * *

### ConversationalArgs

Ƭ `ConversationalArgs`: `BaseArgs` & { `inputs`: { `generated_responses?`: `string`[] ; `past_user_inputs?`: `string`[] ; `text`: `string` } ; `parameters?`: { `max_length?`: `number` ; `max_time?`: `number` ; `min_length?`: `number` ; `repetition_penalty?`: `number` ; `temperature?`: `number` ; `top_k?`: `number` ; `top_p?`: `number` } }

#### Defined in

[inference/src/tasks/nlp/conversational.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/conversational.ts#L5)

* * *

### DocumentQuestionAnsweringArgs

Ƭ `DocumentQuestionAnsweringArgs`: `BaseArgs` & { `inputs`: { `image`: `Blob` | `ArrayBuffer` ; `question`: `string` } }

#### 定义在

[inference/src/tasks/multimodal/documentQuestionAnswering.ts:8](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/multimodal/documentQuestionAnswering.ts#L8)

* * *

### FeatureExtractionArgs

Ƭ `FeatureExtractionArgs`: `BaseArgs` & { `inputs`: `string` | `string`[] }

#### Defined in

[inference/src/tasks/nlp/featureExtraction.ts:6](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/featureExtraction.ts#L6)

* * *

### FeatureExtractionOutput

Ƭ `FeatureExtractionOutput`: (`number` | `number`[] | `number`[][])[]

返回的值是浮点数的多维数组（维数取决于您发送的是字符串还是字符串列表，以及是否为您应用了自动缩减，通常是均值池化等）。这应该在模型的 README 中有解释。

#### 定义在

[inference/src/tasks/nlp/featureExtraction.ts:19](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/featureExtraction.ts#L19)

* * *

### FillMaskArgs

Ƭ `FillMaskArgs`: `BaseArgs` & { `inputs`: `string` }

#### 定义在

[inference/src/tasks/nlp/fillMask.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/fillMask.ts#L5)

* * *

### FillMaskOutput

Ƭ `FillMaskOutput`: { `score`: `number` ; `sequence`: `string` ; `token`: `number` ; `token_str`: `string` }[]

#### 定义在

[inference/src/tasks/nlp/fillMask.ts:9](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/fillMask.ts#L9)

* * *

### ImageClassificationArgs

Ƭ `ImageClassificationArgs`: `BaseArgs` & { `data`: `Blob` | `ArrayBuffer` }

#### 定义在

[inference/src/tasks/cv/imageClassification.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageClassification.ts#L5)

* * *

### ImageClassificationOutput

Ƭ `ImageClassificationOutput`: `ImageClassificationOutputValue`[]

#### 定义在

[inference/src/tasks/cv/imageClassification.ts:23](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageClassification.ts#L23)

* * *

### ImageSegmentationArgs

Ƭ `ImageSegmentationArgs`: `BaseArgs` & { `data`: `Blob` | `ArrayBuffer` }

#### 定义在

[inference/src/tasks/cv/imageSegmentation.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageSegmentation.ts#L5)

* * *

### ImageSegmentationOutput

Ƭ `ImageSegmentationOutput`: `ImageSegmentationOutputValue`[]

#### 定义在

[inference/src/tasks/cv/imageSegmentation.ts:27](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageSegmentation.ts#L27)

* * *

### ImageToImageArgs

Ƭ `ImageToImageArgs`: `BaseArgs` & { `inputs`: `Blob` | `ArrayBuffer` ; `parameters?`: { `guess_mode?`: `boolean` ; `guidance_scale?`: `number` ; `height?`: `number` ; `negative_prompt?`: `string` ; `num_inference_steps?`: `number` ; `prompt?`: `string` ; `strength?`: `number` ; `width?`: `number` } }

#### 定义在

[inference/src/tasks/cv/imageToImage.ts:6](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageToImage.ts#L6)

* * *

### ImageToImageOutput

Ƭ `ImageToImageOutput`: `Blob`

#### 定义在

[inference/src/tasks/cv/imageToImage.ts:55](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageToImage.ts#L55)

* * *

### ImageToTextArgs

Ƭ `ImageToTextArgs`: `BaseArgs` & { `data`: `Blob` | `ArrayBuffer` }

#### 在以下位置定义：

[inference/src/tasks/cv/imageToText.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageToText.ts#L5)

* * *

### InferenceTask

`InferenceTask`: `Exclude`<`PipelineType`, `"other"`>

#### 在以下位置定义：

[inference/src/types.ts:40](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/types.ts#L40)

* * *

### ObjectDetectionArgs

`ObjectDetectionArgs`: `BaseArgs` & { `data`: `Blob` | `ArrayBuffer` }

#### 在以下位置定义：

[inference/src/tasks/cv/objectDetection.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/objectDetection.ts#L5)

* * *

### ObjectDetectionOutput

`ObjectDetectionOutput`: `ObjectDetectionOutputValue`[]

#### 在以下位置定义：

[inference/src/tasks/cv/objectDetection.ts:33](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/objectDetection.ts#L33)

* * *

### QuestionAnsweringArgs

`QuestionAnsweringArgs`: `BaseArgs` & { `inputs`: { `context`: `string` ; `question`: `string` } }

#### 在以下位置定义：

[inference/src/tasks/nlp/questionAnswering.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/questionAnswering.ts#L5)

* * *

### RequestArgs

`RequestArgs`: `BaseArgs` & { `data`: `Blob` | `ArrayBuffer` } | { `inputs`: `unknown` } & { `accessToken?`: `string` ; `parameters?`: `Record`<`string`, `unknown`> }

#### 在以下位置定义：

[inference/src/types.ts:57](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/types.ts#L57)

* * *

### SentenceSimilarityArgs

`SentenceSimilarityArgs`: `BaseArgs` & { `inputs`: `Record`<`string`, `unknown`> | `Record`<`string`, `unknown`>[] }

#### 在以下位置定义：

[inference/src/tasks/nlp/sentenceSimilarity.ts:6](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/sentenceSimilarity.ts#L6)

* * *

### SentenceSimilarityOutput

`SentenceSimilarityOutput`: `number`[]

返回值是一组浮点数列表

#### 在以下位置定义：

[inference/src/tasks/nlp/sentenceSimilarity.ts:19](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/sentenceSimilarity.ts#L19)

* * *

### SummarizationArgs

`SummarizationArgs`: `BaseArgs` & { `inputs`: `string` ; `parameters?`: { `max_length?`: `number` ; `max_time?`: `number` ; `min_length?`: `number` ; `repetition_penalty?`: `number` ; `temperature?`: `number` ; `top_k?`: `number` ; `top_p?`: `number` } }

#### 在以下位置定义：

[inference/src/tasks/nlp/summarization.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/summarization.ts#L5)

* * *

### TableQuestionAnsweringArgs

`TableQuestionAnsweringArgs`: `BaseArgs` & { `inputs`: { `query`: `string` ; `table`: `Record`<`string`, `string`[]> } }

#### 在以下位置定义：

[inference/src/tasks/nlp/tableQuestionAnswering.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tableQuestionAnswering.ts#L5)

* * *

### TabularClassificationArgs

`TabularClassificationArgs`: `BaseArgs` & { `inputs`: { `data`: `Record`<`string`, `string`[]> } }

#### 在以下位置定义：

[inference/src/tasks/tabular/tabularClassification.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/tabular/tabularClassification.ts#L5)

* * *

### TabularClassificationOutput

`TabularClassificationOutput`: `number`[]

每行的预测标签列表

#### 在以下位置定义：

[inference/src/tasks/tabular/tabularClassification.ts:17](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/tabular/tabularClassification.ts#L17)

* * *

### TabularRegressionArgs

`TabularRegressionArgs`: `BaseArgs` & { `inputs`: { `data`: `Record`<`string`, `string`[]> } }

#### 在以下位置定义：

[inference/src/tasks/tabular/tabularRegression.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/tabular/tabularRegression.ts#L5)

* * *

### TabularRegressionOutput

Ƭ `TabularRegressionOutput`: `number`[]

每行的预测值列表

#### 定义在

[inference/src/tasks/tabular/tabularRegression.ts:17](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/tabular/tabularRegression.ts#L17)

* * *

### TextClassificationArgs

Ƭ `TextClassificationArgs`: `BaseArgs` & { `inputs`: `string` }

#### 定义在

[inference/src/tasks/nlp/textClassification.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textClassification.ts#L5)

* * *

### TextClassificationOutput

Ƭ `TextClassificationOutput`: { `label`: `string` ; `score`: `number` }[]

#### 定义在

[inference/src/tasks/nlp/textClassification.ts:12](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textClassification.ts#L12)

* * *

### TextGenerationArgs

Ƭ `TextGenerationArgs`: `BaseArgs` & { `inputs`: `string` ; `parameters?`: { `do_sample?`: `boolean` ; `max_new_tokens?`: `number` ; `max_time?`: `number` ; `num_return_sequences?`: `number` ; `repetition_penalty?`: `number` ; `return_full_text?`: `boolean` ; `stop_sequences?`: `string`[] ; `temperature?`: `number` ; `top_k?`: `number` ; `top_p?`: `number` ; `truncate?`: `number` }

#### 定义在

[inference/src/tasks/nlp/textGeneration.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGeneration.ts#L5)

* * *

### TextGenerationStreamFinishReason

Ƭ `TextGenerationStreamFinishReason`: `"length"` | `"eos_token"` | `"stop_sequence"`

#### 定义在

[inference/src/tasks/nlp/textGenerationStream.ts:46](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L46)

* * *

### TextToImageArgs

Ƭ `TextToImageArgs`: `BaseArgs` & { `inputs`: `string` ; `parameters?`: { `guidance_scale?`: `number` ; `height?`: `number` ; `negative_prompt?`: `string` ; `num_inference_steps?`: `number` ; `width?`: `number` } }

#### 定义在

[inference/src/tasks/cv/textToImage.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/textToImage.ts#L5)

* * *

### TextToImageOutput

Ƭ `TextToImageOutput`: `Blob`

#### 定义在

[inference/src/tasks/cv/textToImage.ts:35](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/textToImage.ts#L35)

* * *

### TextToSpeechArgs

Ƭ `TextToSpeechArgs`: `BaseArgs` & { `inputs`: `string` }

#### 定义在

[inference/src/tasks/audio/textToSpeech.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/textToSpeech.ts#L5)

* * *

### TextToSpeechOutput

Ƭ `TextToSpeechOutput`: `Blob`

#### 定义在

[inference/src/tasks/audio/textToSpeech.ts:12](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/textToSpeech.ts#L12)

* * *

### TokenClassificationArgs

Ƭ `TokenClassificationArgs`: `BaseArgs` & { `inputs`: `string` ; `parameters?`: { `aggregation_strategy?`: `"none"` | `"simple"` | `"first"` | `"average"` | `"max"` } }

#### 定义在

[inference/src/tasks/nlp/tokenClassification.ts:6](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tokenClassification.ts#L6)

* * *

### TokenClassificationOutput

Ƭ `TokenClassificationOutput`: `TokenClassificationOutputValue`[]

#### 定义在

[inference/src/tasks/nlp/tokenClassification.ts:52](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tokenClassification.ts#L52)

* * *

### TranslationArgs

Ƭ `TranslationArgs`: `BaseArgs` & { `inputs`: `string` | `string`[] }

#### 定义在

[inference/src/tasks/nlp/translation.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/translation.ts#L5)

* * *

### 翻译输出

Ƭ `TranslationOutput`: `TranslationOutputValue` | `TranslationOutputValue`[]

#### 定义于

[inference/src/tasks/nlp/translation.ts:19](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/translation.ts#L19)

* * *

### VisualQuestionAnsweringArgs

Ƭ `VisualQuestionAnsweringArgs`: `BaseArgs` & { `inputs`: { `image`: `Blob` | `ArrayBuffer` ; `question`: `string` } }

#### 定义于

[inference/src/tasks/multimodal/visualQuestionAnswering.ts:6](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/multimodal/visualQuestionAnswering.ts#L6)

* * *

### ZeroShotClassificationArgs

Ƭ `ZeroShotClassificationArgs`: `BaseArgs` & { `inputs`: `string` | `string`[] ; `parameters`: { `candidate_labels`: `string`[] ; `multi_label?`: `boolean` } }

#### 定义于

[inference/src/tasks/nlp/zeroShotClassification.ts:6](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/zeroShotClassification.ts#L6)

* * *

### 零样本分类输出

Ƭ `ZeroShotClassificationOutput`: `ZeroShotClassificationOutputValue`[]

#### 定义于

[inference/src/tasks/nlp/zeroShotClassification.ts:29](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/zeroShotClassification.ts#L29)

* * *

### ZeroShotImageClassificationArgs

Ƭ `ZeroShotImageClassificationArgs`: `BaseArgs` & { `inputs`: { `image`: `Blob` | `ArrayBuffer` } ; `parameters`: { `candidate_labels`: `string`[] } }

#### 定义于

[inference/src/tasks/cv/zeroShotImageClassification.ts:7](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/zeroShotImageClassification.ts#L7)

* * *

### ZeroShotImageClassificationOutput

Ƭ `ZeroShotImageClassificationOutput`: `ZeroShotImageClassificationOutputValue`[]

#### 定义于

[inference/src/tasks/cv/zeroShotImageClassification.ts:27](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/zeroShotImageClassification.ts#L27)

## 函数

### 音频分类

▸ `audioClassification`(`args`, `options?`): `Promise`<`AudioClassificationReturn`>

此任务读取一些音频输入并输出类别的可能性。推荐模型：superb/hubert-large-superb-er

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `AudioClassificationArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`AudioClassificationReturn`>

#### 定义于

[inference/src/tasks/audio/audioClassification.ts:30](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/audioClassification.ts#L30)

* * *

### 音频到音频

▸ `audioToAudio`(`args`, `options?`): `Promise`<`AudioToAudioReturn`>

此任务读取一些音频输入并输出一个或多个音频文件。示例模型：speechbrain/sepformer-wham 用于音频源分离。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `AudioToAudioArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`AudioToAudioReturn`>

#### 定义于

[inference/src/tasks/audio/audioToAudio.ts:35](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/audioToAudio.ts#L35)

* * *

### 自动语音识别

▸ `automaticSpeechRecognition`(`args`, `options?`): `Promise`<`AutomaticSpeechRecognitionOutput`>

这项任务读取一些音频输入，并输出音频文件中的文字。推荐模型（英语语言）：facebook/wav2vec2-large-960h-lv60-self

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `AutomaticSpeechRecognitionArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`AutomaticSpeechRecognitionOutput`>

#### 定义在

[inference/src/tasks/audio/automaticSpeechRecognition.ts:23](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/automaticSpeechRecognition.ts#L23)

* * *

### 对话

▸ `conversational`(`args`, `options?`): `Promise`<`ConversationalOutput`>

这项任务对应于任何类似聊天机器人的结构。模型往往具有较短的 max_length，因此在使用给定模型时，请谨慎检查是否需要长距离依赖。推荐模型：microsoft/DialoGPT-large。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `ConversationalArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`ConversationalOutput`>

#### 定义在

[inference/src/tasks/nlp/conversational.ts:65](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/conversational.ts#L65)

* * *

### 文档问答

▸ `documentQuestionAnswering`(`args`, `options?`): `Promise`<`DocumentQuestionAnsweringOutput`>

回答文档图像上的问题。推荐模型：impira/layoutlm-document-qa。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `DocumentQuestionAnsweringArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`DocumentQuestionAnsweringOutput`>

#### 定义在

[inference/src/tasks/multimodal/documentQuestionAnswering.ts:42](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/multimodal/documentQuestionAnswering.ts#L42)

* * *

### 特征提取

▸ `featureExtraction`(`args`, `options?`): `Promise`<`FeatureExtractionOutput`>

这项任务读取一些文本，并输出原始浮点值，通常作为语义数据库/语义搜索的一部分。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `FeatureExtractionArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`FeatureExtractionOutput`>

#### 定义在

[inference/src/tasks/nlp/featureExtraction.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/featureExtraction.ts#L24)

* * *

### 填充掩码

▸ `fillMask`(`args`, `options?`): `Promise`<`FillMaskOutput`>

尝试用缺失的单词（确切地说是标记）填补一个空洞。这是 BERT 模型的基本任务。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `FillMaskArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`FillMaskOutput`>

#### 定义在

[inference/src/tasks/nlp/fillMask.ts:31](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/fillMask.ts#L31)

* * *

### 图像分类

▸ `imageClassification`(`args`, `options?`): `Promise`<`ImageClassificationOutput`>

这项任务读取一些图像输入，并输出类别的可能性。推荐模型：google/vit-base-patch16-224

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `ImageClassificationArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`ImageClassificationOutput`>

#### 定义在

[inference/src/tasks/cv/imageClassification.ts:29](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageClassification.ts#L29)

* * *

### imageSegmentation

▸ `imageSegmentation`(`args`, `options?`): `Promise`<`ImageSegmentationOutput`>

这个任务读取一些图像输入并输出检测到的对象的类别和边界框的可能性。推荐模型：facebook/detr-resnet-50-panoptic

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `ImageSegmentationArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`ImageSegmentationOutput`>

#### 定义在

[inference/src/tasks/cv/imageSegmentation.ts:33](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageSegmentation.ts#L33)

* * *

### imageToImage

▸ `imageToImage`(`args`, `options?`): `Promise`<`ImageToImageOutput`>

这个任务读取一些文本输入并输出图像。推荐模型：lllyasviel/sd-controlnet-depth

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `ImageToImageArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`ImageToImageOutput`>

#### 定义在

[inference/src/tasks/cv/imageToImage.ts:61](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageToImage.ts#L61)

* * *

### imageToText

▸ `imageToText`(`args`, `options?`): `Promise`<`ImageToTextOutput`>

这个任务读取一些图像输入并输出文本标题。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `ImageToTextArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`ImageToTextOutput`>

#### 定义在

[inference/src/tasks/cv/imageToText.ts:22](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageToText.ts#L22)

* * *

### objectDetection

▸ `objectDetection`(`args`, `options?`): `Promise`<`ObjectDetectionOutput`>

这个任务读取一些图像输入并输出检测到的对象的类别和边界框的可能性。推荐模型：facebook/detr-resnet-50

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `ObjectDetectionArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`ObjectDetectionOutput`>

#### 定义在

[inference/src/tasks/cv/objectDetection.ts:39](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/objectDetection.ts#L39)

* * *

### questionAnswering

▸ `questionAnswering`(`args`, `options?`): `Promise`<`QuestionAnsweringOutput`>

想要一个可以回答任何问题的聪明全知机器人吗？推荐模型：deepset/roberta-base-squad2

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `QuestionAnsweringArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`QuestionAnsweringOutput`>

#### 定义在

[inference/src/tasks/nlp/questionAnswering.ts:34](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/questionAnswering.ts#L34)

* * *

### 请求

▸ `request`<`T`>(`args`, `options?`): `Promise`<`T`>

用于自定义调用推理端点的原语

#### 类型参数

| 名称 |
| :-- |
| `T` |

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `RequestArgs` |
| `options?` | `Options` & { `task?`: `string` ; `taskHint?`: `InferenceTask` } |

#### 返回

`Promise`<`T`>

#### 定义在

[inference/src/tasks/custom/request.ts:7](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/custom/request.ts#L7)

* * *

### 句子相似度

▸ `sentenceSimilarity`(`args`, `options?`): `Promise`<`SentenceSimilarityOutput`>

通过比较它们的嵌入，计算一个文本与一系列其他句子之间的语义相似度。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `SentenceSimilarityArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`SentenceSimilarityOutput`>

#### 定义于

[inference/src/tasks/nlp/sentenceSimilarity.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/sentenceSimilarity.ts#L24)

* * *

### streamingRequest

`streamingRequest`<`T`>(`args`, `options?`): `AsyncGenerator`<`T`>

用于进行自定义推断调用，期望服务器发送事件，并通过生成器返回响应

#### 类型参数

| 名称 |
| :-- |
| `T` |

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `RequestArgs` |
| `options?` | `Options` & { `task?`: `string` ; `taskHint?`: `InferenceTask` } |

#### 返回

`AsyncGenerator`<`T`>

#### 定义于

[inference/src/tasks/custom/streamingRequest.ts:9](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/custom/streamingRequest.ts#L9)

* * *

### 摘要

▸ `summarization`(`args`, `options?`): `Promise`<`SummarizationOutput`>

这个任务被广泛用于将较长的文本总结为较短的文本。请注意，一些模型有输入的最大长度。这意味着摘要不能处理完整的书籍等。在选择模型时要小心。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `SummarizationArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`SummarizationOutput`>

#### 定义于

[inference/src/tasks/nlp/summarization.ts:52](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/summarization.ts#L52)

* * *

### 表格问答

▸ `tableQuestionAnswering`(`args`, `options?`): `Promise`<`TableQuestionAnsweringOutput`>

不懂 SQL？不想深入研究大型电子表格？用简单的英语提问！推荐模型：google/tapas-base-finetuned-wtq。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `TableQuestionAnsweringArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`TableQuestionAnsweringOutput`>

#### 定义于

[inference/src/tasks/nlp/tableQuestionAnswering.ts:40](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tableQuestionAnswering.ts#L40)

* * *

### 表格分类

▸ `tabularClassification`(`args`, `options?`): `Promise`<`TabularClassificationOutput`>

预测表格形式中给定特征集的目标标签。通常，您会希望在训练数据上训练分类模型，并在具有相同格式的新数据上使用它。示例模型：vvmnnnkv/wine-quality

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `TabularClassificationArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`TabularClassificationOutput`>

#### 定义于

[inference/src/tasks/tabular/tabularClassification.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/tabular/tabularClassification.ts#L24)

* * *

### 表格回归

▸ `tabularRegression`(`args`, `options?`): `Promise`<`TabularRegressionOutput`>

对给定特征集的目标值进行预测。通常，您会想要在训练数据上训练回归模型，并将其与相同格式的新数据一起使用。示例模型：scikit-learn/Fish-Weight

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `TabularRegressionArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`TabularRegressionOutput`>

#### 定义在

[inference/src/tasks/tabular/tabularRegression.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/tabular/tabularRegression.ts#L24)

* * *

### 文本分类

▸ `textClassification`(`args`, `options?`): `Promise`<`TextClassificationOutput`>

通常用于情感分析，这将输出输入类别的可能性。推荐模型：distilbert-base-uncased-finetuned-sst-2-english

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `TextClassificationArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`TextClassificationOutput`>

#### 定义在

[inference/src/tasks/nlp/textClassification.ts:26](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textClassification.ts#L26)

* * *

### 文本生成

▸ `textGeneration`(`args`, `options?`): `Promise`<`TextGenerationOutput`>

用于从提示中继续文本。这是一个非常通用的任务。推荐模型：gpt2（这是一个简单的模型，但很有趣玩）。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `TextGenerationArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`TextGenerationOutput`>

#### 定义在

[inference/src/tasks/nlp/textGeneration.ts:68](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGeneration.ts#L68)

* * *

### 文本生成流

▸ `textGenerationStream`(`args`, `options?`): `AsyncGenerator`<`TextGenerationStreamOutput`>

用于从提示中继续文本。与`textGeneration`相同，但返回一个可以逐个读取标记的生成器

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `TextGenerationArgs` |
| `options?` | `Options` |

#### 返回

`AsyncGenerator`<`TextGenerationStreamOutput`>

#### 定义在

[inference/src/tasks/nlp/textGenerationStream.ts:87](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L87)

* * *

### 文本转图像

▸ `textToImage`(`args`, `options?`): `Promise`<`TextToImageOutput`>

这个任务读取一些文本输入并输出一幅图像。推荐模型：stabilityai/stable-diffusion-2

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `TextToImageArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`TextToImageOutput`>

#### 定义在

[inference/src/tasks/cv/textToImage.ts:41](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/textToImage.ts#L41)

* * *

### 文本转语音

▸ `textToSpeech`(`args`, `options?`): `Promise`<`TextToSpeechOutput`>

这个任务合成一个发音给定文本的声音。推荐模型：espnet/kan-bayashi_ljspeech_vits

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `TextToSpeechArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`TextToSpeechOutput`>

#### 定义在

[inference/src/tasks/audio/textToSpeech.ts:18](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/textToSpeech.ts#L18)

* * *

### 标记分类

▸ `tokenClassification`(`args`, `options?`): `Promise`<`TokenClassificationOutput`>

通常用于句子解析，无论是语法还是命名实体识别（NER），以理解文本中包含的关键字。推荐模型：dbmdz/bert-large-cased-finetuned-conll03-english

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `TokenClassificationArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`TokenClassificationOutput`>

#### 定义在

[inference/src/tasks/nlp/tokenClassification.ts:57](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tokenClassification.ts#L57)

* * *

### 翻译

▸ `翻译`(`args`, `options?`): `Promise`<`TranslationOutput`>

这个任务被广泛用于将文本从一种语言翻译成另一种语言。推荐模型：Helsinki-NLP/opus-mt-ru-en。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `TranslationArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`TranslationOutput`>

#### 定义在

[inference/src/tasks/nlp/translation.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/translation.ts#L24)

* * *

### 视觉问答

▸ `visualQuestionAnswering`(`args`, `options?`): `Promise`<`VisualQuestionAnsweringOutput`>

回答图像上的问题。推荐模型：dandelin/vilt-b32-finetuned-vqa。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `VisualQuestionAnsweringArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`VisualQuestionAnsweringOutput`>

#### 定义在

[inference/src/tasks/multimodal/visualQuestionAnswering.ts:32](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/multimodal/visualQuestionAnswering.ts#L32)

* * *

### zeroShotClassification

▸ `zeroShotClassification`(`args`, `options?`): `Promise`<`ZeroShotClassificationOutput`>

这个任务非常有用，可以尝试使用零代码进行分类，只需传递一个句子/段落和该句子的可能标签，就可以得到结果。推荐模型：facebook/bart-large-mnli。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `ZeroShotClassificationArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`ZeroShotClassificationOutput`>

#### 定义在

[inference/src/tasks/nlp/zeroShotClassification.ts:34](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/zeroShotClassification.ts#L34)

* * *

### zeroShotImageClassification

▸ `zeroShotImageClassification`(`args`, `options?`): `Promise`<`ZeroShotImageClassificationOutput`>

将图像分类到指定的类别。推荐模型：openai/clip-vit-large-patch14-336

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `args` | `ZeroShotImageClassificationArgs` |
| `options?` | `Options` |

#### 返回

`Promise`<`ZeroShotImageClassificationOutput`>

#### 定义在

[inference/src/tasks/cv/zeroShotImageClassification.ts:33](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/zeroShotImageClassification.ts#L33)
