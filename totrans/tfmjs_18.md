# 管道

> 原始文本: [`huggingface.co/docs/transformers.js/api/pipelines`](https://huggingface.co/docs/transformers.js/api/pipelines)

管道提供了一个高级、易于使用的 API，用于运行机器学习模型。

**示例:** 使用`pipeline`函数实例化管道。

```py
import { pipeline } from '@xenova/transformers';

const classifier = await pipeline('sentiment-analysis');
const output = await classifier('I love transformers!');
// [{'label': 'POSITIVE', 'score': 0.999817686}]
```

+   管道

    +   *static*

        +   .Pipeline ⇐ `可调用`

            +   `new Pipeline(options)` : `管道`

            +   `.dispose()` : `释放类型`

        +   .TextClassificationPipeline : `文本分类管道`

            +   `new TextClassificationPipeline(options)` : `文本分类管道`

            +   `._call()` : `文本分类管道回调`

        +   .TokenClassificationPipeline : `标记分类管道`

            +   `new TokenClassificationPipeline(options)` : `标记分类管道`

            +   `._call()` : `标记分类管道回调`

        +   .QuestionAnsweringPipeline : `问答管道`

            +   `new QuestionAnsweringPipeline(options)` : `问答管道`

            +   `._call()` : `问答管道回调`

        +   .FillMaskPipeline : `填充掩码管道`

            +   `new FillMaskPipeline(options)` : `填充掩码管道`

            +   `._call()` : `填充掩码管道回调`

        +   .Text2TextGenerationPipeline : `文本生成管道`

            +   `new Text2TextGenerationPipeline(options)` : `文本生成管道`

            +   `._key` : `生成的文本`

            +   `._call()` : `文本生成管道回调`

        +   .SummarizationPipeline : `摘要生成管道`

            +   `new SummarizationPipeline(options)` : `摘要生成管道`

            +   `._key` : `摘要文本`

        +   .TranslationPipeline : `翻译管道`

            +   `new TranslationPipeline(options)` : `翻译管道`

            +   `._key` : `翻译文本`

        +   .TextGenerationPipeline : `文本生成管道`

            +   `new TextGenerationPipeline(options)` : `文本生成管道`

            +   `._call()` : `文本生成管道回调`

        +   .ZeroShotClassificationPipeline : `零样本分类管道`

            +   `new ZeroShotClassificationPipeline(options)` : `零样本分类管道`

            +   `.model` : `任意`

            +   `._call()` : `零样本分类管道回调`

        +   .FeatureExtractionPipeline : `特征提取管道`

            +   `new FeatureExtractionPipeline(options)` : `特征提取管道`

            +   `._call()` : `特征提取管道回调`

        +   .AudioClassificationPipeline : `音频分类管道`

            +   `new AudioClassificationPipeline(options)` : `音频分类管道`

            +   `._call()` : `音频分类管道回调`

        +   .ZeroShotAudioClassificationPipeline : `零样本音频分类管道`

            +   `new ZeroShotAudioClassificationPipeline(options)` : `零样本音频分类管道`

            +   `._call()` : `零样本音频分类管道回调`

        +   .AutomaticSpeechRecognitionPipeline : `自动语音识别管道`

            +   `new AutomaticSpeechRecognitionPipeline(options)` => 新的自动语音识别管道

            +   `._call()` : `AutomaticSpeechRecognitionPipelineCallback` => 调用

        +   .ImageToTextPipeline => 图像到文本管道

            +   `new ImageToTextPipeline(options)` => 新的图像转文本管道

            +   `._call()` : `ImageToTextPipelineCallback` => 调用

        +   .ImageClassificationPipeline => 图像分类管道

            +   `new ImageClassificationPipeline(options)` => 新的图像分类管道

            +   `._call()` : `ImageClassificationPipelineCallback` => 调用

        +   .ImageSegmentationPipeline => 图像分割管道

            +   `new ImageSegmentationPipeline(options)` => 新的图像分割管道

            +   `._call()` : `ImageSegmentationPipelineCallback` => 调用

        +   .ZeroShotImageClassificationPipeline => 零样本图像分类管道

            +   `new ZeroShotImageClassificationPipeline(options)` => 新的零样本图像分类管道

            +   `._call()` : `ZeroShotImageClassificationPipelineCallback` => 调用

        +   .ObjectDetectionPipeline => 目标检测管道

            +   `new ObjectDetectionPipeline(options)` => 新的目标检测管道

            +   `._call()` : `ObjectDetectionPipelineCallback` => 调用

        +   .ZeroShotObjectDetectionPipeline => 零样本目标检测管道

            +   `new ZeroShotObjectDetectionPipeline(options)` => 新的零样本目标检测管道

            +   `._call()` : `ZeroShotObjectDetectionPipelineCallback` => 调用

        +   .DocumentQuestionAnsweringPipeline => 文档问答管道

            +   `new DocumentQuestionAnsweringPipeline(options)` => 新的文档问答管道

            +   `._call()` : `DocumentQuestionAnsweringPipelineCallback` => 调用

        +   .TextToAudioPipeline => 文本转音频管道

            +   `new TextToAudioPipeline(options)` => 新的文本转音频管道

            +   `._call()` : `TextToAudioPipelineCallback` => 调用

        +   .ImageToImagePipeline => 图像到图像管道

            +   `new ImageToImagePipeline(options)` => 新的图像到图像管道

            +   `._call()` : `ImageToImagePipelineCallback` => 调用

        +   .DepthEstimationPipeline => 深度估计管道

            +   `new DepthEstimationPipeline(options)` => 新的深度估计管道

            +   `._call()` : `DepthEstimationPipelineCallback` => 调用

        +   [`.pipeline(task, [model], [options])`](#module_pipelines.pipeline) ⇒ `*` => 管道

    +   *inner* => 内部

        +   [`~x[1]`](#module_pipelines..x%5B1%5D) : `number` => x[1] : 数字

        +   `~ImagePipelineInputs` : `string` | `RawImage` | `URL` => 图像管道输入

        +   `~AudioPipelineInputs` : `string` | `URL` | `Float32Array` | `Float64Array` => 音频管道输入

        +   `~BoundingBox` : `Object` => 边界框

        +   `~Disposable` ⇒ `Promise.<void>` => 一次性的

        +   `~TextPipelineConstructorArgs` : `Object` => 文本管道构造参数

        +   `~ImagePipelineConstructorArgs` : `Object` => 图像管道构造参数

        +   `~TextImagePipelineConstructorArgs` : `Object` => 文本图像管道构造参数

        +   `~TextClassificationPipelineType` ⇒ `Promise.<(TextClassificationOutput|Array<TextClassificationOutput>)>` => 文本分类管道类型

        +   `~TokenClassificationPipelineType` ⇒ `Promise.<(TokenClassificationOutput|Array<TokenClassificationOutput>)>`

        +   `~QuestionAnsweringPipelineType` ⇒ `Promise.<(QuestionAnsweringOutput|Array<QuestionAnsweringOutput>)>`

        +   `~FillMaskPipelineType` ⇒ `Promise.<(FillMaskOutput|Array<FillMaskOutput>)>`

        +   `~Text2TextGenerationPipelineType` ⇒ `Promise.<(Text2TextGenerationOutput|Array<Text2TextGenerationOutput>)>`

        +   `~SummarizationPipelineType` ⇒ `Promise.<(SummarizationOutput|Array<SummarizationOutput>)>`

        +   `~TranslationPipelineType` ⇒ `Promise.<(TranslationOutput|Array<TranslationOutput>)>`

        +   `~TextGenerationPipelineType` ⇒ `Promise.<(TextGenerationOutput|Array<TextGenerationOutput>)>`

        +   `~ZeroShotClassificationPipelineType` ⇒ `Promise.<(ZeroShotClassificationOutput|Array<ZeroShotClassificationOutput>)>`

        +   `~FeatureExtractionPipelineType` ⇒ `Promise.<Tensor>`

        +   `~AudioClassificationPipelineType` ⇒ `Promise.<(AudioClassificationOutput|Array<AudioClassificationOutput>)>`

        +   `~ZeroShotAudioClassificationPipelineType` ⇒ `Promise.<(Array<ZeroShotAudioClassificationOutput>|Array<Array<ZeroShotAudioClassificationOutput>>)>`

        +   `~ChunkCallback` : `function`

        +   `~Chunk` : `Object`

        +   `~AutomaticSpeechRecognitionPipelineType` ⇒ `Promise.<(AutomaticSpeechRecognitionOutput|Array<AutomaticSpeechRecognitionOutput>)>`

        +   `~ImageToTextPipelineType` ⇒ `Promise.<(ImageToTextOutput|Array<ImageToTextOutput>)>`

        +   `~ImageClassificationPipelineType` ⇒ `Promise.<(ImageClassificationOutput|Array<ImageClassificationOutput>)>`

        +   `~ImageSegmentationPipelineType` ⇒ `Promise.<Array<ImageSegmentationPipelineOutput>>`

        +   `~ZeroShotImageClassificationPipelineType` ⇒ `Promise.<(Array<ZeroShotImageClassificationOutput>|Array<Array<ZeroShotImageClassificationOutput>>)>`

        +   `~ObjectDetectionPipelineType` ⇒ `Promise.<(ObjectDetectionPipelineOutput|Array<ObjectDetectionPipelineOutput>)>`

        +   `~ZeroShotObjectDetectionPipelineType` ⇒ `Promise.<(Array<ZeroShotObjectDetectionOutput>|Array<Array<ZeroShotObjectDetectionOutput>>)>`

        +   `~DocumentQuestionAnsweringPipelineType` ⇒ `Promise.<(DocumentQuestionAnsweringOutput|Array<DocumentQuestionAnsweringOutput>)>`

        +   `~TextToAudioPipelineConstructorArgs` : `Object`

        +   `~TextToAudioPipelineType` ⇒ `Promise.<TextToAudioOutput>`

        +   `~ImageToImagePipelineType` ⇒ `Promise.<(RawImage|Array<RawImage>)>`

        +   `~DepthEstimationPipelineType` ⇒ `Promise.<(DepthEstimationPipelineOutput|Array<DepthEstimationPipelineOutput>)>`

        +   `~AllTasks` : `*`

* * *

## pipelines.Pipeline ⇐ <code> Callable </code>

Pipeline 类是所有流水线继承的类。请参考此类以获取不同流水线共享的方法。

**Kind**: static class of `pipelines`

**Extends**: `Callable`

+   .Pipeline ⇐ `Callable`

    +   `new Pipeline(options)`

    +   `.dispose()`：`DisposeType`

* * *

### 新管道(options)

创建一个新的 Pipeline。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| options | `Object` |  | 包含以下属性的对象： |
| [options.task] | `string` |  | 管道的任务。用于指定子任务。 |
| [options.model] | `PreTrainedModel` |  | 管道使用的模型。 |
| [options.tokenizer] | `PreTrainedTokenizer` |  | 管道使用的分词器（如果有）。 |
| [options.processor] | `Processor` |  | 管道使用的处理器（如果有）。 |

* * *

### pipeline.dispose()： <code> DisposeType </code>

**种类**：`Pipeline`的实例方法

* * *

## pipelines.TextClassificationPipeline

使用任何 `ModelForSequenceClassification` 的文本分类管道。

**示例：** 使用 `Xenova/distilbert-base-uncased-finetuned-sst-2-english` 进行情感分析。

```py
const classifier = await pipeline('sentiment-analysis', 'Xenova/distilbert-base-uncased-finetuned-sst-2-english');
const output = await classifier('I love transformers!');
// [{ label: 'POSITIVE', score: 0.999788761138916 }]
```

**示例：** 使用 `Xenova/bert-base-multilingual-uncased-sentiment` 进行多语言情感分析（并返回前 5 个类）。

```py
const classifier = await pipeline('sentiment-analysis', 'Xenova/bert-base-multilingual-uncased-sentiment');
const output = await classifier('Le meilleur film de tous les temps.', { topk: 5 });
// [
//   { label: '5 stars', score: 0.9610759615898132 },
//   { label: '4 stars', score: 0.03323351591825485 },
//   { label: '3 stars', score: 0.0036155181005597115 },
//   { label: '1 star', score: 0.0011325967498123646 },
//   { label: '2 stars', score: 0.0009423971059732139 }
// ]
```

**示例：** 使用 `Xenova/toxic-bert` 进行有毒评论分类（并返回所有类）。

```py
const classifier = await pipeline('text-classification', 'Xenova/toxic-bert');
const output = await classifier('I hate you!', { topk: null });
// [
//   { label: 'toxic', score: 0.9593140482902527 },
//   { label: 'insult', score: 0.16187334060668945 },
//   { label: 'obscene', score: 0.03452680632472038 },
//   { label: 'identity_hate', score: 0.0223250575363636 },
//   { label: 'threat', score: 0.019197041168808937 },
//   { label: 'severe_toxic', score: 0.005651099607348442 }
// ]
```

**种类**：`pipelines`的静态类

+   .TextClassificationPipeline

    +   `new TextClassificationPipeline(options)`

    +   `._call()`：`TextClassificationPipelineCallback`

* * *

### 新的 TextClassificationPipeline(options)

创建一个新的 TextClassificationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 选项 | `TextPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### textClassificationPipeline._call()： <code> TextClassificationPipelineCallback </code>

**种类**：`TextClassificationPipeline`的实例方法

* * *

## pipelines.TokenClassificationPipeline

使用任何 `ModelForTokenClassification` 的命名实体识别管道。

**示例：** 使用 `Xenova/bert-base-NER` 执行命名实体识别。

```py
const classifier = await pipeline('token-classification', 'Xenova/bert-base-NER');
const output = await classifier('My name is Sarah and I live in London');
// [
//   { entity: 'B-PER', score: 0.9980202913284302, index: 4, word: 'Sarah' },
//   { entity: 'B-LOC', score: 0.9994474053382874, index: 9, word: 'London' }
// ]
```

**示例：** 使用 `Xenova/bert-base-NER` 执行命名实体识别（并返回所有标签）。

```py
const classifier = await pipeline('token-classification', 'Xenova/bert-base-NER');
const output = await classifier('Sarah lives in the United States of America', { ignore_labels: [] });
// [
//   { entity: 'B-PER', score: 0.9966587424278259, index: 1, word: 'Sarah' },
//   { entity: 'O', score: 0.9987385869026184, index: 2, word: 'lives' },
//   { entity: 'O', score: 0.9990072846412659, index: 3, word: 'in' },
//   { entity: 'O', score: 0.9988298416137695, index: 4, word: 'the' },
//   { entity: 'B-LOC', score: 0.9995510578155518, index: 5, word: 'United' },
//   { entity: 'I-LOC', score: 0.9990395307540894, index: 6, word: 'States' },
//   { entity: 'I-LOC', score: 0.9986724853515625, index: 7, word: 'of' },
//   { entity: 'I-LOC', score: 0.9975294470787048, index: 8, word: 'America' }
// ]
```

**种类**：`pipelines`的静态类

+   .TokenClassificationPipeline

    +   `new TokenClassificationPipeline(options)`

    +   `._call()`：`TokenClassificationPipelineCallback`

* * *

### 新的 TokenClassificationPipeline(options)

创建一个新的 TokenClassificationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 选项 | `TextPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### tokenClassificationPipeline._call()： <code> TokenClassificationPipelineCallback </code>

**种类**：`TokenClassificationPipeline`的实例方法

* * *

## pipelines.QuestionAnsweringPipeline

使用任何 `ModelForQuestionAnswering` 的问答管道。

**示例：** 使用 `Xenova/distilbert-base-uncased-distilled-squad` 运行问答。

```py
const answerer = await pipeline('question-answering', 'Xenova/distilbert-base-uncased-distilled-squad');
const question = 'Who was Jim Henson?';
const context = 'Jim Henson was a nice puppet.';
const output = await answerer(question, context);
// {
//   answer: "a nice puppet",
//   score: 0.5768911502526741
// }
```

**种类**：`pipelines`的静态类

+   .QuestionAnsweringPipeline

    +   `new QuestionAnsweringPipeline(options)`

    +   `._call()`：`QuestionAnsweringPipelineCallback`

* * *

### 新的 QuestionAnsweringPipeline(options)

创建一个新的 QuestionAnsweringPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 选项 | `TextPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### questionAnsweringPipeline._call()： <code> QuestionAnsweringPipelineCallback </code>

**种类**：`QuestionAnsweringPipeline`的实例方法

* * *

## pipelines.FillMaskPipeline

使用任何`ModelWithLMHead`进行掩码语言建模预测管道。

**示例：** 使用`Xenova/bert-base-uncased`进行掩码语言建模（也称为“填充掩码”）。

```py
const unmasker = await pipeline('fill-mask', 'Xenova/bert-base-cased');
const output = await unmasker('The goal of life is [MASK].');
// [
//   { token_str: 'survival', score: 0.06137419492006302, token: 8115, sequence: 'The goal of life is survival.' },
//   { token_str: 'love', score: 0.03902450203895569, token: 1567, sequence: 'The goal of life is love.' },
//   { token_str: 'happiness', score: 0.03253183513879776, token: 9266, sequence: 'The goal of life is happiness.' },
//   { token_str: 'freedom', score: 0.018736306577920914, token: 4438, sequence: 'The goal of life is freedom.' },
//   { token_str: 'life', score: 0.01859794743359089, token: 1297, sequence: 'The goal of life is life.' }
// ]
```

**示例：** 使用`Xenova/bert-base-cased`进行掩码语言建模（并返回顶部结果）。

```py
const unmasker = await pipeline('fill-mask', 'Xenova/bert-base-cased');
const output = await unmasker('The Milky Way is a [MASK] galaxy.', { topk: 1 });
// [{ token_str: 'spiral', score: 0.6299987435340881, token: 14061, sequence: 'The Milky Way is a spiral galaxy.' }]
```

**种类**：`pipelines`的静态类

+   .FillMaskPipeline

    +   `new FillMaskPipeline(options)`

    +   `._call()`：`FillMaskPipelineCallback`

* * *

### new FillMaskPipeline(options)

创建一个新的 FillMaskPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 选项 | `TextPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### fillMaskPipeline._call()：`<code> FillMaskPipelineCallback </code>`

**种类**：`FillMaskPipeline`的实例方法

* * *

## pipelines.Text2TextGenerationPipeline

Text2TextGenerationPipeline 类用于使用执行文本到文本生成任务的模型生成文本。

**示例：** 使用`Xenova/LaMini-Flan-T5-783M`进行文本到文本生成。

```py
const generator = await pipeline('text2text-generation', 'Xenova/LaMini-Flan-T5-783M');
const output = await generator('how can I become more healthy?', {
  max_new_tokens: 100,
});
// [{ generated_text: "To become more healthy, you can: 1\. Eat a balanced diet with plenty of fruits, vegetables, whole grains, lean proteins, and healthy fats. 2\. Stay hydrated by drinking plenty of water. 3\. Get enough sleep and manage stress levels. 4\. Avoid smoking and excessive alcohol consumption. 5\. Regularly exercise and maintain a healthy weight. 6\. Practice good hygiene and sanitation. 7\. Seek medical attention if you experience any health issues." }]
```

**种类**：`pipelines`的静态类

+   .Text2TextGenerationPipeline

    +   `new Text2TextGenerationPipeline(options)`

    +   `._key`：`’generated_text’`

    +   `._call()`：`Text2TextGenerationPipelineCallback`

* * *

### new Text2TextGenerationPipeline(options)

创建一个新的 Text2TextGenerationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 选项 | `TextPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### text2TextGenerationPipeline._key：`<code> ’ generated_text ’ </code>`

**种类**：`Text2TextGenerationPipeline`的实例属性

* * *

### text2TextGenerationPipeline._call()：`<code> Text2TextGenerationPipelineCallback </code>`

**种类**：`Text2TextGenerationPipeline`的实例方法

* * *

## pipelines.SummarizationPipeline

一个用于摘要任务的管道，继承自 Text2TextGenerationPipeline。

**示例：** 使用`Xenova/distilbart-cnn-6-6`进行摘要。

```py
const generator = await pipeline('summarization', 'Xenova/distilbart-cnn-6-6');
const text = 'The tower is 324 metres (1,063 ft) tall, about the same height as an 81-storey building, ' +
  'and the tallest structure in Paris. Its base is square, measuring 125 metres (410 ft) on each side. ' +
  'During its construction, the Eiffel Tower surpassed the Washington Monument to become the tallest ' +
  'man-made structure in the world, a title it held for 41 years until the Chrysler Building in New ' +
  'York City was finished in 1930\. It was the first structure to reach a height of 300 metres. Due to ' +
  'the addition of a broadcasting aerial at the top of the tower in 1957, it is now taller than the ' +
  'Chrysler Building by 5.2 metres (17 ft). Excluding transmitters, the Eiffel Tower is the second ' +
  'tallest free-standing structure in France after the Millau Viaduct.';
const output = await generator(text, {
  max_new_tokens: 100,
});
// [{ summary_text: ' The Eiffel Tower is about the same height as an 81-storey building and the tallest structure in Paris. It is the second tallest free-standing structure in France after the Millau Viaduct.' }]
```

**种类**：`pipelines`的静态类

+   .SummarizationPipeline

    +   `new SummarizationPipeline(options)`

    +   `._key`：`’summary_text’`

* * *

### new SummarizationPipeline(options)

创建一个新的 SummarizationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 选项 | `TextPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### summarizationPipeline._key：`<code> ’ summary_text ’ </code>`

**种类**：`SummarizationPipeline`的实例属性

* * *

## pipelines.TranslationPipeline

将文本从一种语言翻译为另一种语言。

**示例：** 使用`Xenova/nllb-200-distilled-600M`进行多语言翻译。

有关语言和对应代码的完整列表，请参见[这里](https://github.com/facebookresearch/flores/blob/main/flores200/README.md#languages-in-flores-200)。

```py
const translator = await pipeline('translation', 'Xenova/nllb-200-distilled-600M');
const output = await translator('जीवन एक चॉकलेट बॉक्स की तरह है।', {
  src_lang: 'hin_Deva', // Hindi
  tgt_lang: 'fra_Latn', // French
});
// [{ translation_text: 'La vie est comme une boîte à chocolat.' }]
```

**示例：** 使用`Xenova/m2m100_418M`进行多语言翻译。

有关语言和对应代码的完整列表，请参见[这里](https://huggingface.co/facebook/m2m100_418M#languages-covered)。

```py
const translator = await pipeline('translation', 'Xenova/m2m100_418M');
const output = await translator('生活就像一盒巧克力。', {
  src_lang: 'zh', // Chinese
  tgt_lang: 'en', // English
});
// [{ translation_text: 'Life is like a box of chocolate.' }]
```

**示例：** 使用`Xenova/mbart-large-50-many-to-many-mmt`进行多语言翻译。

有关语言和对应代码的完整列表，请参见[这里](https://huggingface.co/facebook/mbart-large-50-many-to-many-mmt#languages-covered)。

```py
const translator = await pipeline('translation', 'Xenova/mbart-large-50-many-to-many-mmt');
const output = await translator('संयुक्त राष्ट्र के प्रमुख का कहना है कि सीरिया में कोई सैन्य समाधान नहीं है', {
  src_lang: 'hi_IN', // Hindi
  tgt_lang: 'fr_XX', // French
});
// [{ translation_text: 'Le chef des Nations affirme qu 'il n 'y a military solution in Syria.' }]
```

**类型**：`pipelines`的静态类

+   .TranslationPipeline

    +   `new TranslationPipeline(options)`

    +   `._key`：`’translation_text’`

* * *

### new TranslationPipeline(options)

创建一个新的 TranslationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `TextPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### translationPipeline._key：<code>’translation_text’</code>

**类型**：`TranslationPipeline`的实例属性

* * *

## pipelines.TextGenerationPipeline

使用任何`ModelWithLMHead`或`ModelForCausalLM`的语言生成管道。该管道预测将跟随指定文本提示的单词。注意：有关生成参数的完整列表，请参阅`GenerationConfig`。

**示例：** 使用`Xenova/distilgpt2`（默认设置）进行文本生成。

```py
const generator = await pipeline('text-generation', 'Xenova/distilgpt2');
const text = 'I enjoy walking with my cute dog,';
const output = await generator(text);
// [{ generated_text: "I enjoy walking with my cute dog, and I love to play with the other dogs." }]
```

**示例：** 使用`Xenova/distilgpt2`（自定义设置）进行文本生成。

```py
const generator = await pipeline('text-generation', 'Xenova/distilgpt2');
const text = 'Once upon a time, there was';
const output = await generator(text, {
  temperature: 2,
  max_new_tokens: 10,
  repetition_penalty: 1.5,
  no_repeat_ngram_size: 2,
  num_beams: 2,
  num_return_sequences: 2,
});
// [{
//   "generated_text": "Once upon a time, there was an abundance of information about the history and activities that"
// }, {
//   "generated_text": "Once upon a time, there was an abundance of information about the most important and influential"
// }]
```

**示例：** 使用`Xenova/codegen-350M-mono`进行代码生成。

```py
const generator = await pipeline('text-generation', 'Xenova/codegen-350M-mono');
const text = 'def fib(n):';
const output = await generator(text, {
  max_new_tokens: 44,
});
// [{
//   generated_text: 'def fib(n):\n' +
//     '    if n == 0:\n' +
//     '        return 0\n' +
//     '    elif n == 1:\n' +
//     '        return 1\n' +
//     '    else:\n' +
//     '        return fib(n-1) + fib(n-2)\n'
// }]
```

**类型**：`pipelines`的静态类

+   .TextGenerationPipeline

    +   `new TextGenerationPipeline(options)`

    +   `._call()`：`TextGenerationPipelineCallback`

* * *

### new TextGenerationPipeline(options)

创建一个新的 TextGenerationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `TextPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### textGenerationPipeline._call()：<code>TextGenerationPipelineCallback</code>

**类型**：`TextGenerationPipeline`的实例方法

* * *

## pipelines.ZeroShotClassificationPipeline

基于 NLI 的零样本分类管道，使用在 NLI（自然语言推理）任务上训练的`ModelForSequenceClassification`。相当于`text-classification`管道，但这些模型不需要预先确定的潜在类别数量，可以在运行时选择。通常意味着它更慢，但**更加**灵活。

**示例：** 使用`Xenova/mobilebert-uncased-mnli`进行零样本分类。

```py
const classifier = await pipeline('zero-shot-classification', 'Xenova/mobilebert-uncased-mnli');
const text = 'Last week I upgraded my iOS version and ever since then my phone has been overheating whenever I use your app.';
const labels = [ 'mobile', 'billing', 'website', 'account access' ];
const output = await classifier(text, labels);
// {
//   sequence: 'Last week I upgraded my iOS version and ever since then my phone has been overheating whenever I use your app.',
//   labels: [ 'mobile', 'website', 'billing', 'account access' ],
//   scores: [ 0.5562091040482018, 0.1843621307860853, 0.13942646639336376, 0.12000229877234923 ]
// }
```

**示例：** 使用`Xenova/nli-deberta-v3-xsmall`（多标签）进行零样本分类。

```py
const classifier = await pipeline('zero-shot-classification', 'Xenova/nli-deberta-v3-xsmall');
const text = 'I have a problem with my iphone that needs to be resolved asap!';
const labels = [ 'urgent', 'not urgent', 'phone', 'tablet', 'computer' ];
const output = await classifier(text, labels, { multi_label: true });
// {
//   sequence: 'I have a problem with my iphone that needs to be resolved asap!',
//   labels: [ 'urgent', 'phone', 'computer', 'tablet', 'not urgent' ],
//   scores: [ 0.9958870956360275, 0.9923963400697035, 0.002333537946160235, 0.0015134138567598765, 0.0010699384208377163 ]
// }
```

**类型**：`pipelines`的静态类

+   .ZeroShotClassificationPipeline

    +   `new ZeroShotClassificationPipeline(options)`

    +   `.model`：`any`

    +   `._call()`：`ZeroShotClassificationPipelineCallback`

* * *

### new ZeroShotClassificationPipeline(options)

创建一个新的 ZeroShotClassificationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `TextPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### zeroShotClassificationPipeline.model：<code>any</code>

**类型**：`ZeroShotClassificationPipeline`的实例属性

* * *

### zeroShotClassificationPipeline._call()：<code>ZeroShotClassificationPipelineCallback</code>

**类型**：`ZeroShotClassificationPipeline`的实例方法

* * *

## pipelines.FeatureExtractionPipeline

使用没有模型头的特征提取管道。该管道从基础变换器中提取隐藏状态，可以用作下游任务中的特征。

**示例：** 使用`bert-base-uncased`进行特征提取（不使用池化/归一化）。

```py
const extractor = await pipeline('feature-extraction', 'Xenova/bert-base-uncased', { revision: 'default' });
const output = await extractor('This is a simple test.');
// Tensor {
//   type: 'float32',
//   data: Float32Array [0.05939924716949463, 0.021655935794115067, ...],
//   dims: [1, 8, 768]
// }
```

**示例：** 使用`bert-base-uncased`进行特征提取（使用池化/归一化）。

```py
const extractor = await pipeline('feature-extraction', 'Xenova/bert-base-uncased', { revision: 'default' });
const output = await extractor('This is a simple test.', { pooling: 'mean', normalize: true });
// Tensor {
//   type: 'float32',
//   data: Float32Array [0.03373778983950615, -0.010106077417731285, ...],
//   dims: [1, 768]
// }
```

**示例**：使用`sentence-transformers`模型计算嵌入。

```py
const extractor = await pipeline('feature-extraction', 'Xenova/all-MiniLM-L6-v2');
const output = await extractor('This is a simple test.', { pooling: 'mean', normalize: true });
// Tensor {
//   type: 'float32',
//   data: Float32Array [0.09094982594251633, -0.014774246141314507, ...],
//   dims: [1, 384]
// }
```

**类型**：`pipelines`的静态类

+   .FeatureExtractionPipeline

    +   `new FeatureExtractionPipeline(options)`

    +   `._call()`：`FeatureExtractionPipelineCallback`

* * *

### 新的 FeatureExtractionPipeline(options)

创建一个新的 FeatureExtractionPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `TextPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### featureExtractionPipeline._call()：<code> FeatureExtractionPipelineCallback </code>

**类型**：`FeatureExtractionPipeline`的实例方法

* * *

## pipelines.AudioClassificationPipeline

使用任何`AutoModelForAudioClassification`的音频分类管道。该管道预测原始波形或音频文件的类别。

**示例**：使用`Xenova/wav2vec2-large-xlsr-53-gender-recognition-librispeech`进行音频分类。

```py
const classifier = await pipeline('audio-classification', 'Xenova/wav2vec2-large-xlsr-53-gender-recognition-librispeech');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/jfk.wav';
const output = await classifier(url);
// [
//   { label: 'male', score: 0.9981542229652405 },
//   { label: 'female', score: 0.001845747814513743 }
// ]
```

**示例**：使用`Xenova/ast-finetuned-audioset-10-10-0.4593`进行音频分类并返回前 4 个结果。

```py
const classifier = await pipeline('audio-classification', 'Xenova/ast-finetuned-audioset-10-10-0.4593');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/cat_meow.wav';
const output = await classifier(url, { topk: 4 });
// [
//   { label: 'Meow', score: 0.5617874264717102 },
//   { label: 'Cat', score: 0.22365376353263855 },
//   { label: 'Domestic animals, pets', score: 0.1141069084405899 },
//   { label: 'Animal', score: 0.08985692262649536 },
// ]
```

**类型**：`pipelines`的静态类

+   .AudioClassificationPipeline

    +   `new AudioClassificationPipeline(options)`

    +   `._call()`：`AudioClassificationPipelineCallback`

* * *

### 新的 AudioClassificationPipeline(options)

创建一个新的 AudioClassificationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `AudioPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### audioClassificationPipeline._call()：<code> AudioClassificationPipelineCallback </code>

**类型**：`AudioClassificationPipeline`的实例方法

* * *

## pipelines.ZeroShotAudioClassificationPipeline

使用`ClapModel`的零射击音频分类管道。当您提供音频和一组`candidate_labels`时，该管道预测音频的类别。

**示例**：使用`Xenova/clap-htsat-unfused`进行零射击音频分类。

```py
const classifier = await pipeline('zero-shot-audio-classification', 'Xenova/clap-htsat-unfused');
const audio = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/dog_barking.wav';
const candidate_labels = ['dog', 'vaccum cleaner'];
const scores = await classifier(audio, candidate_labels);
// [
//   { score: 0.9993992447853088, label: 'dog' },
//   { score: 0.0006007603369653225, label: 'vaccum cleaner' }
// ]
```

**类型**：`pipelines`的静态类

+   .ZeroShotAudioClassificationPipeline

    +   `new ZeroShotAudioClassificationPipeline(options)`

    +   `._call()`：`ZeroShotAudioClassificationPipelineCallback`

* * *

### 新的 ZeroShotAudioClassificationPipeline(options)

创建一个新的 ZeroShotAudioClassificationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `TextAudioPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### zeroShotAudioClassificationPipeline._call()：<code> ZeroShotAudioClassificationPipelineCallback </code>

**类型**：`ZeroShotAudioClassificationPipeline`的实例方法

* * *

## pipelines.AutomaticSpeechRecognitionPipeline

旨在提取音频中包含的口头文本的管道。

**示例**：转录英语。

```py
const transcriber = await pipeline('automatic-speech-recognition', 'Xenova/whisper-tiny.en');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/jfk.wav';
const output = await transcriber(url);
// { text: " And so my fellow Americans ask not what your country can do for you, ask what you can do for your country." }
```

**示例**：转录英语并附有时间戳。

```py
const transcriber = await pipeline('automatic-speech-recognition', 'Xenova/whisper-tiny.en');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/jfk.wav';
const output = await transcriber(url, { return_timestamps: true });
// {
//   text: " And so my fellow Americans ask not what your country can do for you, ask what you can do for your country."
//   chunks: [
//     { timestamp: [0, 8],  text: " And so my fellow Americans ask not what your country can do for you" }
//     { timestamp: [8, 11], text: " ask what you can do for your country." }
//   ]
// }
```

**示例**：转录英语并附有单词级时间戳。

```py
const transcriber = await pipeline('automatic-speech-recognition', 'Xenova/whisper-tiny.en');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/jfk.wav';
const output = await transcriber(url, { return_timestamps: 'word' });
// {
//   "text": " And so my fellow Americans ask not what your country can do for you ask what you can do for your country.",
//   "chunks": [
//     { "text": " And", "timestamp": [0, 0.78] },
//     { "text": " so", "timestamp": [0.78, 1.06] },
//     { "text": " my", "timestamp": [1.06, 1.46] },
//     ...
//     { "text": " for", "timestamp": [9.72, 9.92] },
//     { "text": " your", "timestamp": [9.92, 10.22] },
//     { "text": " country.", "timestamp": [10.22, 13.5] }
//   ]
// }
```

**示例**：转录法语。

```py
const transcriber = await pipeline('automatic-speech-recognition', 'Xenova/whisper-small');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/french-audio.mp3';
const output = await transcriber(url, { language: 'french', task: 'transcribe' });
// { text: " J'adore, j'aime, je n'aime pas, je déteste." }
```

**示例**：将法语翻译成英语。

```py
const transcriber = await pipeline('automatic-speech-recognition', 'Xenova/whisper-small');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/french-audio.mp3';
const output = await transcriber(url, { language: 'french', task: 'translate' });
// { text: " I love, I like, I don't like, I hate." }
```

**示例**：转录/翻译超过 30 秒的音频。

```py
const transcriber = await pipeline('automatic-speech-recognition', 'Xenova/whisper-tiny.en');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/ted_60.wav';
const output = await transcriber(url, { chunk_length_s: 30, stride_length_s: 5 });
// { text: " So in college, I was a government major, which means [...] So I'd start off light and I'd bump it up" }
```

**类型**：`pipelines`的静态类

+   .AutomaticSpeechRecognitionPipeline

    +   `new AutomaticSpeechRecognitionPipeline(options)`

    +   `._call()`：`AutomaticSpeechRecognitionPipelineCallback`

* * *

### new AutomaticSpeechRecognitionPipeline(options)

创建一个新的 AutomaticSpeechRecognitionPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `TextAudioPipelineConstructorArgs` | 用于实例化流水线的对象。 |

* * *

### automaticSpeechRecognitionPipeline._call()： <code> AutomaticSpeechRecognitionPipelineCallback </code>

**类型**：`AutomaticSpeechRecognitionPipeline`的实例方法

* * *

## pipelines.ImageToTextPipeline

使用`AutoModelForVision2Seq`的 Image To Text 流水线。该流水线为给定图像预测标题。

**示例：**使用`Xenova/vit-gpt2-image-captioning`为图像生成标题。

```py
const captioner = await pipeline('image-to-text', 'Xenova/vit-gpt2-image-captioning');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/cats.jpg';
const output = await captioner(url);
// [{ generated_text: 'a cat laying on a couch with another cat' }]
```

**示例：**使用`Xenova/trocr-small-handwritten`进行光学字符识别（OCR）。

```py
const captioner = await pipeline('image-to-text', 'Xenova/trocr-small-handwritten');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/handwriting.jpg';
const output = await captioner(url);
// [{ generated_text: 'Mr. Brown commented icily.' }]
```

**类型**：`pipelines`的静态类

+   .ImageToTextPipeline

    +   `new ImageToTextPipeline(options)`

    +   `._call()`：`ImageToTextPipelineCallback`

* * *

### new ImageToTextPipeline(options)

创建一个新的 ImageToTextPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `TextImagePipelineConstructorArgs` | 用于实例化流水线的对象。 |

* * *

### imageToTextPipeline._call()： <code> ImageToTextPipelineCallback </code>

**类型**：`ImageToTextPipeline`的实例方法

* * *

## pipelines.ImageClassificationPipeline

使用任何`AutoModelForImageClassification`的图像分类流水线。该流水线预测图像的类别。

**示例：**对图像进行分类。

```py
const classifier = await pipeline('image-classification', 'Xenova/vit-base-patch16-224');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/tiger.jpg';
const output = await classifier(url);
// [
//   { label: 'tiger, Panthera tigris', score: 0.632695734500885 },
// ]
```

**示例：**对图像进行分类并返回前`n`个类别。

```py
const classifier = await pipeline('image-classification', 'Xenova/vit-base-patch16-224');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/tiger.jpg';
const output = await classifier(url, { topk: 3 });
// [
//   { label: 'tiger, Panthera tigris', score: 0.632695734500885 },
//   { label: 'tiger cat', score: 0.3634825646877289 },
//   { label: 'lion, king of beasts, Panthera leo', score: 0.00045060308184474707 },
// ]
```

**示例：**对图像进行分类并返回所有类别。

```py
const classifier = await pipeline('image-classification', 'Xenova/vit-base-patch16-224');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/tiger.jpg';
const output = await classifier(url, { topk: 0 });
// [
//   { label: 'tiger, Panthera tigris', score: 0.632695734500885 },
//   { label: 'tiger cat', score: 0.3634825646877289 },
//   { label: 'lion, king of beasts, Panthera leo', score: 0.00045060308184474707 },
//   { label: 'jaguar, panther, Panthera onca, Felis onca', score: 0.00035465499968267977 },
//   ...
// ]
```

**类型**：`pipelines`的静态类

+   .ImageClassificationPipeline

    +   `new ImageClassificationPipeline(options)`

    +   `._call()`：`ImageClassificationPipelineCallback`

* * *

### new ImageClassificationPipeline(options)

创建一个新的 ImageClassificationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `ImagePipelineConstructorArgs` | 用于实例化流水线的对象。 |

* * *

### imageClassificationPipeline._call()： <code> ImageClassificationPipelineCallback </code>

**类型**：`ImageClassificationPipeline`的实例方法

* * *

## pipelines.ImageSegmentationPipeline

使用任何`AutoModelForXXXSegmentation`的图像分割流水线。该流水线预测对象的掩模和它们的类别。

**示例：**使用`Xenova/detr-resnet-50-panoptic`进行图像分割。

```py
const segmenter = await pipeline('image-segmentation', 'Xenova/detr-resnet-50-panoptic');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/cats.jpg';
const output = await segmenter(url);
// [
//   { label: 'remote', score: 0.9984649419784546, mask: RawImage { ... } },
//   { label: 'cat', score: 0.9994316101074219, mask: RawImage { ... } }
// ]
```

**类型**：`pipelines`的静态类

+   .ImageSegmentationPipeline

    +   `new ImageSegmentationPipeline(options)`

    +   `._call()`：`ImageSegmentationPipelineCallback`

* * *

### new ImageSegmentationPipeline(options)

创建一个新的 ImageSegmentationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `ImagePipelineConstructorArgs` | 用于实例化流水线的对象。 |

* * *

### imageSegmentationPipeline._call()： <code> ImageSegmentationPipelineCallback </code>

**类型**：`ImageSegmentationPipeline`的实例方法

* * *

## pipelines.ZeroShotImageClassificationPipeline

零样本图像分类流水线。当您提供一张图像和一组`candidate_labels`时，该流水线预测图像的类别。

**示例：**使用`Xenova/clip-vit-base-patch32`进行零样本图像分类。

```py
const classifier = await pipeline('zero-shot-image-classification', 'Xenova/clip-vit-base-patch32');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/tiger.jpg';
const output = await classifier(url, ['tiger', 'horse', 'dog']);
// [
//   { score: 0.9993917942047119, label: 'tiger' },
//   { score: 0.0003519294841680676, label: 'horse' },
//   { score: 0.0002562698791734874, label: 'dog' }
// ]
```

**类型**：`pipelines`的静态类

+   .ZeroShotImageClassificationPipeline

    +   `new ZeroShotImageClassificationPipeline(options)`

    +   `._call()` : `ZeroShotImageClassificationPipelineCallback`

* * *

### 新建一个 ZeroShotImageClassificationPipeline。

创建一个新的 ZeroShotImageClassificationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 参数 | `TextImagePipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### zeroShotImageClassificationPipeline._call() : <code> ZeroShotImageClassificationPipelineCallback </code>

**种类**: `ZeroShotImageClassificationPipeline`的实例方法

* * *

## pipelines.ObjectDetectionPipeline

使用任何`AutoModelForObjectDetection`的目标检测管道。该管道预测对象的边界框和它们的类别。

**示例:** 使用`Xenova/detr-resnet-50`运行目标检测。

```py
const detector = await pipeline('object-detection', 'Xenova/detr-resnet-50');
const img = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/cats.jpg';
const output = await detector(img, { threshold: 0.9 });
// [{
//   score: 0.9976370930671692,
//   label: "remote",
//   box: { xmin: 31, ymin: 68, xmax: 190, ymax: 118 }
// },
// ...
// {
//   score: 0.9984092116355896,
//   label: "cat",
//   box: { xmin: 331, ymin: 19, xmax: 649, ymax: 371 }
// }]
```

**种类**: `pipelines`的静态类

+   .ObjectDetectionPipeline

    +   `new ObjectDetectionPipeline(options)`

    +   `._call()` : `ObjectDetectionPipelineCallback`

* * *

### 新建一个 ObjectDetectionPipeline。

创建一个新的 ObjectDetectionPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `ImagePipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### objectDetectionPipeline._call() : <code> ObjectDetectionPipelineCallback </code>

**种类**: `ObjectDetectionPipeline`的实例方法

* * *

## pipelines.ZeroShotObjectDetectionPipeline

零样本目标检测管道。当您提供图像和一组`candidate_labels`时，此管道预测对象的边界框。

**示例:** 零样本目标检测，使用`Xenova/owlvit-base-patch32`。

```py
const detector = await pipeline('zero-shot-object-detection', 'Xenova/owlvit-base-patch32');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/astronaut.png';
const candidate_labels = ['human face', 'rocket', 'helmet', 'american flag'];
const output = await detector(url, candidate_labels);
// [
//   {
//     score: 0.24392342567443848,
//     label: 'human face',
//     box: { xmin: 180, ymin: 67, xmax: 274, ymax: 175 }
//   },
//   {
//     score: 0.15129457414150238,
//     label: 'american flag',
//     box: { xmin: 0, ymin: 4, xmax: 106, ymax: 513 }
//   },
//   {
//     score: 0.13649864494800568,
//     label: 'helmet',
//     box: { xmin: 277, ymin: 337, xmax: 511, ymax: 511 }
//   },
//   {
//     score: 0.10262022167444229,
//     label: 'rocket',
//     box: { xmin: 352, ymin: -1, xmax: 463, ymax: 287 }
//   }
// ]
```

**示例:** 零样本目标检测，使用`Xenova/owlvit-base-patch32`（返回前 4 个匹配项并设置阈值）。

```py
const detector = await pipeline('zero-shot-object-detection', 'Xenova/owlvit-base-patch32');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/beach.png';
const candidate_labels = ['hat', 'book', 'sunglasses', 'camera'];
const output = await detector(url, candidate_labels, { topk: 4, threshold: 0.05 });
// [
//   {
//     score: 0.1606510728597641,
//     label: 'sunglasses',
//     box: { xmin: 347, ymin: 229, xmax: 429, ymax: 264 }
//   },
//   {
//     score: 0.08935828506946564,
//     label: 'hat',
//     box: { xmin: 38, ymin: 174, xmax: 258, ymax: 364 }
//   },
//   {
//     score: 0.08530698716640472,
//     label: 'camera',
//     box: { xmin: 187, ymin: 350, xmax: 260, ymax: 411 }
//   },
//   {
//     score: 0.08349756896495819,
//     label: 'book',
//     box: { xmin: 261, ymin: 280, xmax: 494, ymax: 425 }
//   }
// ]
```

**种类**: `pipelines`的静态类

+   .ZeroShotObjectDetectionPipeline

    +   `new ZeroShotObjectDetectionPipeline(options)`

    +   `._call()` : `ZeroShotObjectDetectionPipelineCallback`

* * *

### 新建一个 ZeroShotObjectDetectionPipeline。

创建一个新的 ZeroShotObjectDetectionPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 参数 | `TextImagePipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### zeroShotObjectDetectionPipeline._call() : <code> ZeroShotObjectDetectionPipelineCallback </code>

**种类**: `ZeroShotObjectDetectionPipeline`的实例方法

* * *

## pipelines.DocumentQuestionAnsweringPipeline

使用任何`AutoModelForDocumentQuestionAnswering`的文档问答管道。输入/输出与（抽取式）问答管道类似；但是，该管道以图像（和可选的 OCR 单词/框）作为输入，而不是文本上下文。

**示例:** 使用`Xenova/donut-base-finetuned-docvqa`回答关于文档的问题。

```py
const qa_pipeline = await pipeline('document-question-answering', 'Xenova/donut-base-finetuned-docvqa');
const image = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/invoice.png';
const question = 'What is the invoice number?';
const output = await qa_pipeline(image, question);
// [{ answer: 'us-001' }]
```

**种类**: `pipelines`的静态类

+   .DocumentQuestionAnsweringPipeline

    +   `new DocumentQuestionAnsweringPipeline(options)`

    +   `._call()` : `DocumentQuestionAnsweringPipelineCallback`

* * *

### 新建一个 DocumentQuestionAnsweringPipeline。

创建一个新的 DocumentQuestionAnsweringPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `TextImagePipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### 文档问答管道._call()： <code> DocumentQuestionAnsweringPipelineCallback </code>

**类型**：`DocumentQuestionAnsweringPipeline`的实例方法

* * *

## pipelines.TextToAudioPipeline

使用任何`AutoModelForTextToWaveform`或`AutoModelForTextToSpectrogram`的文本到音频生成管道。该管道从输入文本和可选的其他条件输入生成音频文件。

**示例：** 使用`Xenova/speecht5_tts`从文本生成音频。

```py
const synthesizer = await pipeline('text-to-speech', 'Xenova/speecht5_tts', { quantized: false });
const speaker_embeddings = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/speaker_embeddings.bin';
const out = await synthesizer('Hello, my dog is cute', { speaker_embeddings });
// {
//   audio: Float32Array(26112) [-0.00005657337896991521, 0.00020583874720614403, ...],
//   sampling_rate: 16000
// }
```

然后，您可以使用`wavefile`包将音频保存为.wav 文件：

```py
import wavefile from 'wavefile';
import fs from 'fs';

const wav = new wavefile.WaveFile();
wav.fromScratch(1, out.sampling_rate, '32f', out.audio);
fs.writeFileSync('out.wav', wav.toBuffer());
```

**示例：** 使用`Xenova/mms-tts-fra`进行多语言语音生成。查看[此处](https://huggingface.co/models?pipeline_tag=text-to-speech&other=vits&sort=trending)获取可用语言的完整列表（1107）。

```py
const synthesizer = await pipeline('text-to-speech', 'Xenova/mms-tts-fra');
const out = await synthesizer('Bonjour');
// {
//   audio: Float32Array(23808) [-0.00037693005288019776, 0.0003325853613205254, ...],
//   sampling_rate: 16000
// }
```

**类型**：`pipelines`的静态类

+   .TextToAudioPipeline

    +   `new TextToAudioPipeline(options)`

    +   `._call()`：`TextToAudioPipelineCallback`

* * *

### new TextToAudioPipeline(options)

创建一个新的 TextToAudioPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `TextToAudioPipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### textToAudioPipeline._call()： <code> TextToAudioPipelineCallback </code>

**类型**：`TextToAudioPipeline`的实例方法

* * *

## pipelines.ImageToImagePipeline

使用任何`AutoModelForImageToImage`的图像到图像管道。该管道基于先前的图像输入生成图像。

**示例：** 使用`Xenova/swin2SR-classical-sr-x2-64`进行超分辨率

```py
const upscaler = await pipeline('image-to-image', 'Xenova/swin2SR-classical-sr-x2-64');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/butterfly.jpg';
const output = await upscaler(url);
// RawImage {
//   data: Uint8Array(786432) [ 41, 31, 24,  43, ... ],
//   width: 512,
//   height: 512,
//   channels: 3
// }
```

**类型**：`pipelines`的静态类

+   .ImageToImagePipeline

    +   `new ImageToImagePipeline(options)`

    +   `._call()`：`ImageToImagePipelineCallback`

* * *

### new ImageToImagePipeline(options)

创建一个新的 ImageToImagePipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `ImagePipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### imageToImagePipeline._call()： <code> ImageToImagePipelineCallback </code>

**类型**：`ImageToImagePipeline`的实例方法

* * *

## pipelines.DepthEstimationPipeline

使用任何`AutoModelForDepthEstimation`的深度估计管道。该管道预测图像的深度。

**示例：** 使用`Xenova/dpt-hybrid-midas`进行深度估计

```py
const depth_estimator = await pipeline('depth-estimation', 'Xenova/dpt-hybrid-midas');
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/cats.jpg';
const out = await depth_estimator(url);
// {
//   predicted_depth: Tensor {
//     dims: [ 384, 384 ],
//     type: 'float32',
//     data: Float32Array(147456) [ 542.859130859375, 545.2833862304688, 546.1649169921875, ... ],
//     size: 147456
//   },
//   depth: RawImage {
//     data: Uint8Array(307200) [ 86, 86, 86, ... ],
//     width: 640,
//     height: 480,
//     channels: 1
//   }
// }
```

**类型**：`pipelines`的静态类

+   .DepthEstimationPipeline

    +   `new DepthEstimationPipeline(options)`

    +   `._call()`：`DepthEstimationPipelineCallback`

* * *

### new DepthEstimationPipeline(options)

创建一个新的 DepthEstimationPipeline。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| options | `ImagePipelineConstructorArgs` | 用于实例化管道的对象。 |

* * *

### depthEstimationPipeline._call()： <code> DepthEstimationPipelineCallback </code>

**类型**：`DepthEstimationPipeline`的实例方法

* * *

## pipelines.pipeline(task, [model], [options]) ⇒ <code> * </code>

构建`Pipeline`对象的实用工厂方法。

**类型**：`pipelines`的静态方法

**返回**：`*` - 指定任务的 Pipeline 对象。

**抛出**：

+   `错误`：如果请求了不受支持的管道。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |

| 任务 | `T` |  | 定义将返回哪个管道的任务。当前接受的任务是：

+   `"audio-classification"`：将返回一个`AudioClassificationPipeline`。

+   `"automatic-speech-recognition"`：将返回一个`AutomaticSpeechRecognitionPipeline`。

+   `"depth-estimation"`：将返回一个`DepthEstimationPipeline`。

+   `"document-question-answering"`：将返回一个`DocumentQuestionAnsweringPipeline`。

+   `"feature-extraction"`：将返回一个`FeatureExtractionPipeline`。

+   `"fill-mask"`：将返回一个`FillMaskPipeline`。

+   `"image-classification"`：将返回一个`ImageClassificationPipeline`。

+   `"image-segmentation"`：将返回一个`ImageSegmentationPipeline`。

+   `"image-to-text"`：将返回一个`ImageToTextPipeline`。

+   `"object-detection"`：将返回一个`ObjectDetectionPipeline`。

+   `"question-answering"`：将返回一个`QuestionAnsweringPipeline`。

+   `"summarization"`：将返回一个`SummarizationPipeline`。

+   `"text2text-generation"`：将返回一个`Text2TextGenerationPipeline`。

+   `"text-classification"`（别名"sentiment-analysis"可用）：将返回一个`TextClassificationPipeline`。

+   `"text-generation"`：将返回一个`TextGenerationPipeline`。

+   `"token-classification"`（别名"ner"可用）：将返回一个`TokenClassificationPipeline`。

+   `"translation"`：将返回一个`TranslationPipeline`。

+   `"translation_xx_to_yy"`：将返回一个`TranslationPipeline`。

+   `"zero-shot-classification"`：将返回一个`ZeroShotClassificationPipeline`。

+   `"zero-shot-audio-classification"`：将返回一个`ZeroShotAudioClassificationPipeline`。

+   `"zero-shot-image-classification"`：将返回一个`ZeroShotImageClassificationPipeline`。

+   `"zero-shot-object-detection"`：将返回一个`ZeroShotObjectDetectionPipeline`。

|

| [model] | `string` | `null` | 要使用的预训练模型的名称。如果未指定，将使用任务的默认模型。 |
| --- | --- | --- | --- |
| [options] | `*` |  | 管道的可选参数。 |

* * *

## pipelines~x[1]： <code> number </code>

**种类**：`pipelines`的内部属性。

* * *

## pipelines~ImagePipelineInputs： <code> string </code> | <code> RawImage </code> | <code> URL </code>

**种类**：`pipelines`的内部 typedef。

* * *

## pipelines~AudioPipelineInputs： <code> string </code> | <code> URL </code> | <code> Float32Array </code> | <code> Float64Array </code>

**种类**：`pipelines`的内部 typedef。

* * *

## pipelines~BoundingBox： <code> Object </code>

**种类**：`pipelines`的内部 typedef。

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| xmin | `number` | 边界框的最小 x 坐标。 |
| ymin | `number` | 边界框的最小 y 坐标。 |
| xmax | `number` | 边界框的最大 x 坐标。 |
| ymax | `number` | 边界框的最大 y 坐标。 |

* * *

## pipelines~Disposable ⇒ <code> Promise. < void > </code>

**种类**：`pipelines`的内部 typedef。

**返回值**：`Promise.<void>` - 一个在项目被处理后解决的 Promise。

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| dispose | `DisposeType` | 一个在管道被处理后解决的 Promise。 |

* * *

## pipelines~TextPipelineConstructorArgs： <code> Object </code>

用于实例化基于文本的管道的对象。

**种类**：`pipelines`的内部 typedef。

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| 任务 | `string` | 管道的任务。用于指定子任务。 |
| model | `PreTrainedModel` | 管道使用的模型。 |
| tokenizer | `PreTrainedTokenizer` | 管道使用的分词器。 |

* * *

## pipelines~ImagePipelineConstructorArgs： <code> Object </code>

用于实例化基于音频的管道的对象。

**种类**：`pipelines`的内部 typedef。

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| 任务 | `string` | 管道的任务。用于指定子任务。 |
| model | `PreTrainedModel` | 管道使用的模型。 |
| processor | `Processor` | 管道使用的处理器。 |

* * *

## pipelines~TextImagePipelineConstructorArgs： <code> Object </code>

用于实例化文本和音频管道的对象。

**种类**：`pipelines`的内部类型

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| 任务 | `字符串` | 管道的任务。用于指定子任务。 |
| 模型 | `PreTrainedModel` | 管道使用的模型。 |
| 分词器 | `PreTrainedTokenizer` | 管道使用的分词器。 |
| 处理器 | `处理器` | 管道使用的处理器。 |

* * *

## pipelines~TextClassificationPipelineType ⇒ <code> Promise. < (TextClassificationOutput|Array < TextClassificationOutput > ) > </code>

特定于文本分类管道的参数。

**种类**：`pipelines`

**返回**: `Promise.<(TextClassificationOutput|Array<TextClassificationOutput>)>` - 包含预测标签和分数的数组或对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `字符串` &#124; `Array<string>` | 要分类的输入文本。 |
| [选项] | `TextClassificationPipelineOptions` | 用于文本分类的选项。 |

**属性**

| 名称 | 类型 | 默认 | 描述 |
| --- | --- | --- | --- |
| 标签 | `字符串` |  | 预测的标签。 |
| 分数 | `数字` |  | 相应的概率。 |
| [topk] | `数字` | `1` | 要返回的前几个预测数。 |

* * *

## pipelines~TokenClassificationPipelineType ⇒ <code> Promise. < (TokenClassificationOutput|Array < TokenClassificationOutput > ) > </code>

特定于标记分类管道的参数。

**种类**：`pipelines`的内部类型

**返回**: `Promise.<(TokenClassificationOutput|Array<TokenClassificationOutput>)>` - 结果。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `字符串` &#124; `Array<string>` | 用于标记分类的一个或多个文本（或一个文本列表）。 |
| [选项] | `TokenClassificationPipelineOptions` | 用于标记分类的选项。 |

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| 单词 | `字符串` | 被分类的标记/单词。这是通过解码所选标记获得的。 |
| 分数 | `数字` | `实体`的相应概率。 |
| 实体 | `字符串` | 预测的实体。 |
| 索引 | `数字` | 句子中相应标记的索引。 |
| [开始] | `数字` | 相应实体在句子中的起始索引。 |
| [结束] | `数字` | 相应实体在句子中的结束索引。 |
| [忽略标签] | `Array.<字符串>` | 要忽略的标签列表。 |

* * *

## pipelines~QuestionAnsweringPipelineType ⇒ <code> Promise. < (QuestionAnsweringOutput|Array < QuestionAnsweringOutput > ) > </code>

特定于问答管道的参数。

**种类**：`pipelines`的内部类型

**返回**: `Promise.<(QuestionAnsweringOutput|Array<QuestionAnsweringOutput>)>` - 包含预测答案和分数的数组或对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 问题 | `字符串` &#124; `Array<string>` | 一个或多个问题（必须与`context`参数一起使用）。 |
| 上下文 | `字符串` &#124; `Array<string>` | 与问题相关联的一个或多个上下文（必须与`question`参数一起使用）。 |
| [选项] | `QuestionAnsweringPipelineOptions` | 用于问答的选项。 |

**属性**

| 名称 | 类型 | 默认 | 描述 |
| --- | --- | --- | --- |
| 分数 | `数字` |  | 与答案相关联的概率。 |
| [开始] | `数字` |  | 答案的字符开始索引（在输入的标记化版本中）。 |
| [结束] | `数字` |  | 答案的字符结束索引（在输入的标记化版本中）。 |
| 答案 | `字符串` |  | 问题的答案。 |
| [topk] | `数字` | `1` | 要返回的前几个答案预测数。 |

* * *

## 管道~FillMaskPipelineType ⇒ <code> Promise. < (FillMaskOutput|Array < FillMaskOutput > ) > </code>

特定于填充掩码管道的参数。

**种类**：`pipelines`的内部 typedef

**返回**：`Promise.<(FillMaskOutput|Array<FillMaskOutput>)>` - 包含得分、预测标记、预测标记字符串以及带有预测标记填充的序列的对象数组，或这样的数组（每个输入文本一个）。如果只提供一个输入文本，则输出将是一个对象数组。

**抛出**：

+   `错误` 当在输入文本中找不到掩码标记时。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| texts | `string` &#124; `Array<string>` | 一个或多个带有掩码标记的文本（或一个提示列表）。 |
| [options] | `FillMaskPipelineOptions` | 用于掩码语言建模的选项。 |

**属性**

| 名称 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| sequence | `string` |  | 带有掩码标记预测的相应输入。 |
| score | `number` |  | 相应的概率。 |
| token | `number` |  | 预测的标记 id（用于替换掩码标记）。 |
| token_str | `string` |  | 预测的标记（用于替换掩码标记）。 |
| [topk] | `number` | `5` | 当传递时，覆盖要返回的预测数量。 |

* * *

## 管道~Text2TextGenerationPipelineType ⇒ <code> Promise. < (Text2TextGenerationOutput|Array < Text2TextGenerationOutput > ) > </code>

**种类**：`pipelines`的内部 typedef

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| texts | `string` &#124; `Array<string>` | 编码器的输入文本。 |
| [options] | `*` | 传递给模型的 generate 方法的额外关键字参数。 |

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| generated_text | `string` | 生成的文本。 |

* * *

## 管道~SummarizationPipelineType ⇒ <code> Promise. < (SummarizationOutput|Array < SummarizationOutput > ) > </code>

**种类**：`pipelines`的内部 typedef

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| texts | `string` &#124; `Array<string>` | 一个或多个文章（或一个文章列表）要总结。 |
| [options] | `*` | 传递给模型的 generate 方法的额外关键字参数。 |

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| summary_text | `string` | 摘要文本。 |

* * *

## 管道~TranslationPipelineType ⇒ <code> Promise. < (TranslationOutput|Array < TranslationOutput > ) > </code>

**种类**：`pipelines`的内部 typedef

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| texts | `string` &#124; `Array<string>` | 要翻译的文本。 |
| [options] | `*` | 传递给模型的 generate 方法的额外关键字参数。 |

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| translation_text | `string` | 翻译后的文本。 |

* * *

## 管道~TextGenerationPipelineType ⇒ <code> Promise. < (TextGenerationOutput|Array < TextGenerationOutput > ) > </code>

特定于文本生成管道的参数。

**种类**：`pipelines`的内部 typedef

**返回**：`Promise.<(TextGenerationOutput|Array<TextGenerationOutput>)>` - 包含生成文本的数组或对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| texts | `string` &#124; `Array<string>` | 一个或多个提示（或一个提示列表）以完成。 |
| [options] | `TextGenerationConfig` | 传递给模型的 generate 方法的额外关键字参数。 |

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| generated_text | `string` | 生成的文本。 |
| [add_special_tokens] | `boolean` | 在对序列进行标记化时是否添加特殊标记。 |

* * *

## 管道~ZeroShotClassificationPipelineType ⇒ <code> Promise. < (ZeroShotClassificationOutput|Array < ZeroShotClassificationOutput > ) > </code>

特定于零样本分类管道的参数。

**种类**: `pipelines` 的内部类型

**返回**: `Promise.<(零样本分类输出|Array<零样本分类输出>)>` - 包含预测标签和分数的数组或对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `字符串` &#124; `Array<string>` | 要分类的序列，如果模型输入过大将被截断。 |
| 候选标签 | `字符串` &#124; `Array<string>` | 将每个序列分类到的可能类标签集。可以是单个标签、逗号分隔的标签字符串或标签列表。 |
| [选项] | `零样本分类管道选项` | 用于零样本分类的选项。 |

**属性**

| 名称 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| 序列 | `字符串` |  | 输出所属的序列。 |
| 标签 | `Array.<字符串>` |  | 按可能性顺序排序的标签。 |
| 分数 | `Array.<数字>` |  | 每个标签的概率。 |
| [假设模板] | `字符串` | `""这个例子是{}。""` | 用于将每个候选标签转换为 NLI 风格假设的模板。候选标签将替换 `{}` 占位符。 |
| [多标签] | `布尔值` | `false` | 是否可以有多个候选标签为真。如果为 `false`，则对每个序列的标签可能性之和进行归一化为 1。如果为 `true`，则认为标签是独立的，并通过对蕴涵分数与矛盾分数进行 softmax，为每个候选标签进行概率归一化。 |

* * *

## pipelines~FeatureExtractionPipelineType ⇒ <code> Promise. < 张量 > </code>

特定于特征提取管道的参数。

**种类**: `pipelines` 的内部类型

**返回**: `Promise.<张量>` - 模型计算的特征。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `字符串` &#124; `Array<string>` | 一个或多个文本（或一个文本列表）以获取特征。 |
| [选项] | `特征提取管道选项` | 用于特征提取的选项。 |

**属性**

| 名称 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| [池化] | `'none'` &#124; `'mean'` &#124; `'cls'` | `"none"` | 要使用的池化方法。 |
| [归一化] | `布尔值` | `false` | 是否对最后一维的嵌入进行归一化。 |

* * *

## pipelines~AudioClassificationPipelineType ⇒ <code> Promise. < (音频分类输出|Array < 音频分类输出 > ) > </code>

特定于音频分类管道的参数。

**种类**: `pipelines` 的内部类型

**返回**: `Promise.<(音频分类输出|Array<音频分类输出>)>` - 包含预测标签和分数的数组或对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |

| 音频 | `音频管道输入` | 要分类的输入音频文件。输入可以是：

+   `字符串` 或 `URL`，是音频文件的文件名/URL，文件将以处理器的采样率读取，使用 [`AudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext) API 获取波形。如果 `AudioContext` 不可用，应将原始波形作为形状为 `(n, )` 的 `Float32Array` 传递。

+   `Float32Array` 或 `Float64Array`，形状为 `(n, )`，表示正确采样率下的原始音频（不会进行进一步检查）。

|

| [选项] | `音频分类管道选项` | 用于音频分类的选项。 |
| --- | --- | --- |

**属性**

| 名称 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| 标签 | `字符串` |  | 预测的标签。 |
| 分数 | `数字` |  | 相应的概率。 |
| [topk] | `number` | 返回的前 k 个标签的数量。如果提供的数字为`null`或高于模型配置中可用标签的数量，则默认为标签的数量。 |

* * *

## pipelines~ZeroShotAudioClassificationPipelineType ⇒ <code> Promise. < (Array < ZeroShotAudioClassificationOutput > |Array < Array < ZeroShotAudioClassificationOutput > > ) > </code>

特定于零样本音频分类管道的参数。

**种类**：`pipelines`的内部 typedef

**返回**：`Promise.<(Array<ZeroShotAudioClassificationOutput>|Array<Array<ZeroShotAudioClassificationOutput>>)>` - 包含预测标签和分数的对象数组。

| Param | Type | Description |
| --- | --- | --- |

| audio | `AudioPipelineInputs` | 要分类的输入音频文件。输入可以是：

+   `string`或`URL`，是音频文件的文件名/URL，文件将以处理器的采样率读取，以获取使用[`AudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext) API 获取波形。如果`AudioContext`不可用，应将原始波形作为形状为`(n, )`的 Float32Array 传递。

+   形状为`(n, )`的`Float32Array`或`Float64Array`，表示以正确采样率的原始音频（不会进行进一步检查）。

|

| candidate_labels | `Array.<string>` | 此音频的候选标签。 |
| --- | --- | --- |
| [options] | `ZeroShotAudioClassificationPipelineOptions` | 用于零样本音频分类的选项。 |

**属性**

| 名称 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| 标签 | `string` | 标签由模型识别。它是建议的`candidate_label`之一。 |
| 分数 | `number` | 模型为该标签分配的分数（介于 0 和 1 之间）。 |
| [hypothesis_template] | `string` | `""这是一个{}的声音。""` | 与`candidate_labels`一起使用的句子，通过将占位符替换为 candidate_labels 尝试音频分类，然后使用`logits_per_audio`来估计可能性。 |

* * *

## pipelines~ChunkCallback : <code> function </code>

**种类**：`pipelines`的内部 typedef

| Param | 类型 | 描述 |
| --- | --- | --- |
| chunk | `ChunkCallbackItem` | 要处理的块。 |

* * *

## pipelines~Chunk : <code> Object </code>

**种类**：`pipelines`的内部 typedef

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| 时间戳 | `*` | 片段的开始和结束时间戳（以秒为单位）。 |
| text | `string` | 识别的文本。 |

* * *

## pipelines~AutomaticSpeechRecognitionPipelineType ⇒ <code> Promise. < (AutomaticSpeechRecognitionOutput|Array < AutomaticSpeechRecognitionOutput > ) > </code>

特定于自动语音识别管道的参数。

**种类**：`pipelines`的内部 typedef

**返回**：`Promise.<(AutomaticSpeechRecognitionOutput|Array<AutomaticSpeechRecognitionOutput>)>` - 包含转录文本和可选时间戳的对象，如果`return_timestamps`为`true`。

| Param | 类型 | 描述 |
| --- | --- | --- |

| audio | `AudioPipelineInputs` | 要转录的输入音频文件。输入可以是：

+   `string`或`URL`，是音频文件的文件名/URL，文件将以处理器的采样率读取，以获取使用[`AudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext) API 获取波形。如果`AudioContext`不可用，应将原始波形作为形状为`(n, )`的 Float32Array 传递。

+   形状为`(n, )`的`Float32Array`或`Float64Array`，表示以正确采样率的原始音频（不会进行进一步检查）。

|

| [options] | `AutomaticSpeechRecognitionConfig` | 传递给模型的 generate 方法的附加关键字参数。 |
| --- | --- | --- |

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 识别的文本。 |
| [chunks] | `Array.<Chunk>` | 当使用 `return_timestamps` 时，`chunks` 将变成一个包含模型识别的各种文本块的列表。 |
| [kwargs.return_timestamps] | `boolean` &#124; `'word'` | 是否返回时间戳。默认为 `false`。 |
| [kwargs.chunk_length_s] | `number` | 要处理的音频块的长度（以秒为单位）。默认为 0（不分块）。 |
| [kwargs.stride_length_s] | `number` | 连续音频块之间的重叠长度（以秒为单位）。如果未提供，默认为 `chunk_length_s / 6`。 |
| [kwargs.chunk_callback] | `ChunkCallback` | 要在处理每个块时调用的回调函数。 |
| [kwargs.force_full_sequences] | `boolean` | 是否强制输出完整序列。默认为 `false`。 |
| [kwargs.language] | `string` | 源语言。默认为 `null`，表示应自动检测。如果已知源语言，可以使用此选项来潜在地提高性能。 |
| [kwargs.task] | `string` | 要执行的任务。默认为 `null`，表示应自动检测。 |
| [kwargs.forced_decoder_ids] | `Array.<Array<number>>` | 一对整数的列表，指示在采样之前将强制的生成索引映射到标记索引。例如，[[1, 123]] 表示第二个生成的标记将始终是索引为 123 的标记。 |
| [num_frames] | `number` | 输入音频中的帧数。 |

* * *

## pipelines~ImageToTextPipelineType ⇒ <code> Promise. < (ImageToTextOutput|Array < ImageToTextOutput > ) > </code>

**种类**：`pipelines` 的内部类型

**返回值**：`Promise.<(ImageToTextOutput|Array<ImageToTextOutput>)>` - 包含生成的文本的对象（或对象数组）。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| texts | `ImagePipelineInputs` | 要加注的图像。 |
| [options] | `*` | 要传递给模型的 generate 方法的附加关键字参数。 |

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| generated_text | `string` | 生成的文本。 |

* * *

## pipelines~ImageClassificationPipelineType ⇒ <code> Promise. < (ImageClassificationOutput|Array < ImageClassificationOutput > ) > </code>

特定于图像分类流程的参数。

**种类**：`pipelines` 的内部类型

**返回值**：`Promise.<(ImageClassificationOutput|Array<ImageClassificationOutput>)>` - 包含预测标签和分数的数组或对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| images | `ImagePipelineInputs` | 要分类的输入图像。 |
| [options] | `ImageClassificationPipelineOptions` | 用于图像分类的选项。 |

**属性**

| 名称 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| label | `string` |  | 模型识别的标签。 |
| score | `number` |  | 模型为该标签赋予的分数。 |
| [topk] | `number` | `1` | 流程将返回的前几个标签的数量。 |

* * *

## pipelines~ImageSegmentationPipelineType ⇒ <code> Promise. < Array < ImageSegmentationPipelineOutput > > </code>

特定于图像分割流程的参数。

**种类**：`pipelines` 的内部类型

**返回值**：`Promise.<Array<ImageSegmentationPipelineOutput>>` - 带有注释的分段。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| images | `ImagePipelineInputs` | 输入图像。 |
| [options] | `ImageSegmentationPipelineOptions` | 用于图像分割的选项。 |

**属性**

| 名称 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| label | `string` |  | 分段的标签。 |
| score | `number` &#124; `null` |  | 分段的分数。 |
| mask | `RawImage` |  | 分段的蒙版。 |
| [threshold] | `number` | `0.5` | 用于过滤预测蒙版的概率阈值。 |
| [mask_threshold] | `number` | `0.5` | 将预测的掩模转换为二进制值时使用的阈值。 |
| [overlap_mask_area_threshold] | `number` | `0.8` | 排除小的、不连续的段的掩模重叠阈值。 |
| [subtask] | `null` &#124; `string` |  | 要执行的分割任务。根据模型的能力，其中之一是[`panoptic`、`instance`和`semantic`]。如果未设置，管道将尝试按照这个顺序解决。 |
| [label_ids_to_fuse] | `Array.<number>` |  | 要融合的标签 id 列表。如果未设置，则不融合任何标签。 |
| [target_sizes] | `Array.<Array<number>>` |  | 输入图像的目标尺寸列表。如果未设置，则使用原始图像尺寸。 |

* * *

## pipelines~ZeroShotImageClassificationPipelineType ⇒ <code> Promise. < (Array < ZeroShotImageClassificationOutput > |Array < Array < ZeroShotImageClassificationOutput > > ) > </code>

特定于零样本图像分类管道的参数。

**种类**: `pipelines`的内部类型定义

**返回**: `Promise.<(Array<ZeroShotImageClassificationOutput>|Array<Array<ZeroShotImageClassificationOutput>>)>` - 包含预测标签和分数的对象数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| images | `ImagePipelineInputs` | 输入图像。 |
| candidate_labels | `Array.<string>` | 此图像的候选标签。 |
| [options] | `ZeroShotImageClassificationPipelineOptions` | 用于零样本图像分类的选项。 |

**属性**

| 名称 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| label | `string` |  | 模型识别的标签。它是建议的`candidate_label`之一。 |
| score | `number` |  | 模型为该标签分配的分数（介于 0 和 1 之间）。 |
| [hypothesis_template] | `string` | `""这是一张{}的照片""` | 与`candidate_labels`一起使用的句子，通过将占位符替换为候选标签来尝试图像分类。然后使用`logits_per_image`来估计可能性。 |

* * *

## pipelines~ObjectDetectionPipelineType ⇒ <code> Promise. < (ObjectDetectionPipelineOutput|Array < ObjectDetectionPipelineOutput > ) > </code>

特定于目标检测管道的参数。

**种类**: `pipelines`的内部类型定义

**返回**: `Promise.<(ObjectDetectionPipelineOutput|Array<ObjectDetectionPipelineOutput>)>` - 一组对象或一组对象的列表。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| images | `ImagePipelineInputs` | 输入图像。 |
| [options] | `ObjectDetectionPipelineOptions` | 用于目标检测的选项。 |

**属性**

| 名称 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| label | `string` |  | 模型识别的类别标签。 |
| score | `number` |  | 模型为该标签分配的分数。 |
| box | `BoundingBox` |  | 图像原始尺寸中检测到的对象的边界框，如果`percentage`设置为 true，则为百分比。 |
| [threshold] | `number` | `0.9` | 用于按分数筛选框的阈值。 |
| [percentage] | `boolean` | `false` | 是否以百分比（true）或像素（false）返回框坐标。 |

* * *

## pipelines~ZeroShotObjectDetectionPipelineType ⇒ <code> Promise. < (Array < ZeroShotObjectDetectionOutput > |Array < Array < ZeroShotObjectDetectionOutput > > ) > </code>

特定于零样本目标检测管道的参数。

**种类**: `pipelines`的内部类型定义

**返回**: `Promise.<(Array<ZeroShotObjectDetectionOutput>|Array<Array<ZeroShotObjectDetectionOutput>>)>` - 包含预测标签、分数和边界框的对象数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| images | `ImagePipelineInputs` | 输入图像。 |
| candidate_labels | `Array.<string>` | 模型应该在图像中识别的内容。 |
| [options] | `ZeroShotObjectDetectionPipelineOptions` | 用于零样本对象检测的选项。 |

**属性**

| 名称 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| 标签 | `string` |  | 找到对象对应的文本查询。 |
| 分数 | `number` |  | 与对象对应的分数（介于 0 和 1 之间）。 |
| 包围盒 | `BoundingBox` |  | 图像原始尺寸中检测到的对象的边界框，如果`percentage`设置为 true，则为百分比。 |
| [threshold] | `number` | `0.1` | 进行预测所需的概率。 |
| [topk] | `number` |  | 管道返回的前几个预测的数量。如果提供的数字为`null`或高于可用的预测数量，则默认为预测数量。 |
| [百分比] | `boolean` | `false` | 是否以百分比（true）或像素（false）返回框坐标。 |

* * *

## pipelines~DocumentQuestionAnsweringPipelineType ⇒ <code> Promise. < (DocumentQuestionAnsweringOutput|Array < DocumentQuestionAnsweringOutput > ) > </code>

**种类**: `pipelines`的内部类型定义

**返回**: `Promise.<(DocumentQuestionAnsweringOutput|Array<DocumentQuestionAnsweringOutput>)>` - 包含答案的对象（或对象数组）。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 图像 | `ImageInput` | 要使用的文档图像。 |
| 问题 | `string` | 要问文档的问题。 |
| [options] | `*` | 传递给模型生成方法的其他关键字参数。 |

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| 答案 | `string` | 生成的文本。 |

* * *

## pipelines~TextToAudioPipelineConstructorArgs : <code> Object </code>

**种类**: `pipelines`的内部类型定义

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| [vocoder] | `PreTrainedModel` | 管道使用的声码器（如果模型使用）。如果未提供，则使用默认的 HifiGan 声码器。 |

* * *

## pipelines~TextToAudioPipelineType ⇒ <code> Promise. < TextToAudioOutput > </code>

特定于文本到音频管道的参数。

**种类**: `pipelines`的内部类型定义

**返回**: `Promise.<TextToAudioOutput>` - 包含生成的音频和采样率的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `string` &#124; `Array<string>` | 要生成的文本。 |
| 选项 | `TextToAudioPipelineOptions` | 传递给模型生成/前向方法的参数。 |

**属性**

| 名称 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| audio | `Float32Array` |  | 生成的音频波形。 |
| 采样率 | `number` |  | 生成的音频波形的采样率。 |
| [speaker_embeddings] | `Tensor` &#124; `Float32Array` &#124; `string` &#124; `URL` |  | 说话者嵌入（如果模型需要）。 |

* * *

## pipelines~ImageToImagePipelineType ⇒ <code> Promise. < (RawImage|Array < RawImage > ) > </code>

**种类**: `pipelines`的内部类型定义

**返回**: `Promise.<(RawImage|Array<RawImage>)>` - 转换后的图像或图像列表。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 图像 | `ImagePipelineInputs` | 要转换的图像。 |

* * *

## pipelines~DepthEstimationPipelineType ⇒ <code> Promise. < (DepthEstimationPipelineOutput|Array < DepthEstimationPipelineOutput > ) > </code>

**种类**: `pipelines`的内部类型定义

**返回**: `Promise.<(DepthEstimationPipelineOutput|Array<DepthEstimationPipelineOutput>)>` - 包含结果的图像或图像列表。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 图像 | `ImagePipelineInputs` | 要计算深度的图像。 |

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| 预测深度 | `Tensor` | 模型预测的原始深度图。 |
| 深度 | `RawImage` | 处理后的深度图像（与输入图像大小相同）。 |

* * *

## pipelines~AllTasks: <code> * </code>

所有可能的管道类型。

**种类**：`pipelines`模块的内部 typedef

* * *
