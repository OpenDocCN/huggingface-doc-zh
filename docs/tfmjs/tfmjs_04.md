# 管道 API

> 原文：[`huggingface.co/docs/transformers.js/pipelines`](https://huggingface.co/docs/transformers.js/pipelines)

就像[transformers Python 库](https://github.com/huggingface/transformers)一样，Transformers.js 为用户提供了一个简单的方式来利用 transformers 的强大功能。`pipeline()`函数是使用预训练模型进行推断的最简单最快的方式。

要查看可用任务/管道的完整列表，请查看此表。

## 基础知识

首先创建一个`pipeline()`实例，并指定要用它进行的任务。例如，要创建一个情感分析管道，您可以这样做：

```py
import { pipeline } from '@xenova/transformers';

let classifier = await pipeline('sentiment-analysis');
```

第一次运行时，`pipeline`将下载并缓存与任务相关的默认预训练模型。这可能需要一些时间，但后续调用将快得多。

默认情况下，模型将从[Hugging Face Hub](https://huggingface.co/models)下载并存储在[浏览器缓存](https://developer.mozilla.org/en-US/docs/Web/API/Cache)中，但也有方法可以指定自定义模型和缓存位置。有关更多信息，请参见这里。

现在，您可以通过将其作为函数调用来在目标文本上使用分类器：

```py
let result = await classifier('I love transformers!');
// [{'label': 'POSITIVE', 'score': 0.9998}]
```

如果您有多个输入，可以将它们作为数组传递：

```py
let result = await classifier(['I love transformers!', 'I hate transformers!']);
// [{'label': 'POSITIVE', 'score': 0.9998}, {'label': 'NEGATIVE', 'score': 0.9982}]
```

您还可以通过将其作为`pipeline()`函数的第二个参数传递来指定要用于管道的不同模型。例如，要为情感分析使用不同的模型（例如训练为预测评论情感的模型，评分为 1 到 5 颗星），您可以这样做：

```py
let reviewer = await pipeline('sentiment-analysis', 'Xenova/bert-base-multilingual-uncased-sentiment');

let result = await reviewer('The Shawshank Redemption is a true masterpiece of cinema.');
// [{label: '5 stars', score: 0.8167929649353027}]
```

Transformers.js 支持加载托管在 Hugging Face Hub 上的任何模型，前提是它具有 ONNX 权重（位于名为`onnx`的子文件夹中）。有关如何将您的 PyTorch、TensorFlow 或 JAX 模型转换为 ONNX 的更多信息，请参见转换部分。

`pipeline()`函数是快速使用预训练模型进行推断的好方法，因为它会为您处理所有的预处理和后处理。例如，如果您想使用 OpenAI 的 Whisper 模型进行自动语音识别（ASR），您可以这样做：

```py
// Allocate a pipeline for Automatic Speech Recognition
let transcriber = await pipeline('automatic-speech-recognition', 'Xenova/whisper-small.en');

// Transcribe an audio file, loaded from a URL.
let result = await transcriber('https://huggingface.co/datasets/Narsil/asr_dummy/resolve/main/mlk.flac');
// {text: ' I have a dream that one day this nation will rise up and live out the true meaning of its creed.'}
```

## 管道选项

### 加载

我们提供了各种选项来控制如何从 Hugging Face Hub（或本地）加载模型。默认情况下，使用模型的*量化*版本，这个版本更小更快，但通常不太准确。要覆盖此行为（即使用未量化的模型），您可以将自定义的`PretrainedOptions`对象作为`pipeline`函数的第三个参数：

```py
// Allocation a pipeline for feature extraction, using the unquantized model
const pipe = await pipeline('feature-extraction', 'Xenova/all-MiniLM-L6-v2', {
    quantized: false,
});
```

您还可以通过传递`revision`参数来指定要使用的模型的修订版。由于 Hugging Face Hub 使用基于 git 的版本控制系统，您可以使用任何有效的 git 修订说明符（例如分支名称或提交哈希）

```py
let transcriber = await pipeline('automatic-speech-recognition', 'Xenova/whisper-tiny.en', {
    revision: 'output_attentions',
});
```

要查看完整的选项列表，请查看 PretrainedOptions 文档。

### 运行

许多管道有额外的选项可以指定。例如，当使用支持多语言翻译的模型时，您可以像这样指定源语言和目标语言：

```py
// Allocation a pipeline for translation
let translator = await pipeline('translation', 'Xenova/nllb-200-distilled-600M');

// Translate from English to Greek
let result = await translator('I like to walk my dog.', {
    src_lang: 'eng_Latn',
    tgt_lang: 'ell_Grek'
});
// [ { translation_text: 'Μου αρέσει να περπατάω το σκυλί μου.' } ]

// Translate back to English
let result2 = await translator(result[0].translation_text, {
    src_lang: 'ell_Grek',
    tgt_lang: 'eng_Latn'
});
// [ { translation_text: 'I like to walk my dog.' } ]
```

当使用支持自回归生成的模型时，您可以指定生成参数，如新标记的数量、采样方法、温度、重复惩罚等。有关可用参数的完整列表，请参见 GenerationConfig 类。

例如，要使用`LaMini-Flan-T5-783M`生成一首诗，您可以这样做：

```py
// Allocate a pipeline for text2text-generation
let poet = await pipeline('text2text-generation', 'Xenova/LaMini-Flan-T5-783M');
let result = await poet('Write me a love poem about cheese.', {
    max_new_tokens: 200,
    temperature: 0.9,
    repetition_penalty: 2.0,
    no_repeat_ngram_size: 3,
});
```

将`result[0].generated_text`记录到控制台中会显示：

```py
Cheese, oh cheese! You're the perfect comfort food.
Your texture so smooth and creamy you can never get old.
With every bite it melts in your mouth like buttery delights
that make me feel right at home with this sweet treat of mine. 

From classic to bold flavor combinations,
I love how versatile you are as an ingredient too?
Cheddar is my go-to for any occasion or mood; 
It adds depth and richness without being overpowering its taste buds alone
```

有关每个管道可用选项的更多信息，请参阅 API 参考。如果您想对推断过程有更多控制，可以使用`AutoModel`、`AutoTokenizer`或`AutoProcessor`类。

## 可用任务

### 任务

#### 自然语言处理

| 任务 | ID | 描述 | 是否支持？ |
| --- | --- | --- | --- |
| [对话](https://huggingface.co/tasks/conversational) | `conversational` | 根据提示生成相关、连贯和知识丰富的对话文本。 | ❌ |
| [填充掩码](https://huggingface.co/tasks/fill-mask) | `fill-mask` | 对句子中的一些单词进行掩码，并预测应该替换这些掩码的单词。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.FillMaskPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=fill-mask&library=transformers.js) |
| [问答](https://huggingface.co/tasks/question-answering) | `question-answering` | 从给定文本中检索问题的答案。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.QuestionAnsweringPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=question-answering&library=transformers.js) |
| [句子相似度](https://huggingface.co/tasks/sentence-similarity) | `sentence-similarity` | 确定两个文本有多相似。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.FeatureExtractionPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=feature-extraction&library=transformers.js) |
| [摘要](https://huggingface.co/tasks/summarization) | `summarization` | 生成文档的简短版本，同时保留其重要信息。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.SummarizationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=summarization&library=transformers.js) |
| [表格问答](https://huggingface.co/tasks/table-question-answering) | `table-question-answering` | 回答关于给定表格信息的问题。 | ❌ |
| [文本分类](https://huggingface.co/tasks/text-classification) | `text-classification` 或 `sentiment-analysis` | 为给定文本分配一个标签或类别。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.TextClassificationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=text-classification&library=transformers.js) |
| [文本生成](https://huggingface.co/tasks/text-generation#completion-generation-models) | `text-generation` | 通过预测序列中的下一个单词来生成新文本。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.TextGenerationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=text-generation&library=transformers.js) |
| [文本到文本生成](https://huggingface.co/tasks/text-generation#text-to-text-generation-models) | `text2text-generation` | 将一个文本序列转换为另一个文本序列。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.Text2TextGenerationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=text2text-generation&library=transformers.js) |
| [标记分类](https://huggingface.co/tasks/token-classification) | `token-classification` 或 `ner` | 为文本中的每个标记分配一个标签。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.TokenClassificationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=token-classification&library=transformers.js) |
| [翻译](https://huggingface.co/tasks/translation) | `translation` | 将文本从一种语言转换为另一种语言。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.TranslationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=translation&library=transformers.js) |
| [零样本分类](https://huggingface.co/tasks/zero-shot-classification) | `zero-shot-classification` | 将文本分类为在训练期间未见过的类别。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ZeroShotClassificationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=zero-shot-classification&library=transformers.js) |

#### 视觉

| 任务 | ID | 描述 | 是否支持？ |
| --- | --- | --- | --- |
| [深度估计](https://huggingface.co/tasks/depth-estimation) | `depth-estimation` | 预测图像中存在的对象的深度。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.DepthEstimationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=depth-estimation&library=transformers.js) |
| [图像分类](https://huggingface.co/tasks/image-classification) | `image-classification` | 为整个图像分配标签或类别。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ImageClassificationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=image-classification&library=transformers.js) |
| [图像分割](https://huggingface.co/tasks/image-segmentation) | `image-segmentation` | 将图像分割成每个像素映射到对象的段。此任务有多个变体，如实例分割、全景分割和语义分割。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ImageSegmentationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=image-segmentation&library=transformers.js) |
| [图像对图像](https://huggingface.co/tasks/image-to-image) | `image-to-image` | 将源图像转换为与目标图像或目标图像域的特征相匹配。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ImageToImagePipeline) [(模型)](https://huggingface.co/models?pipeline_tag=image-to-image&library=transformers.js) |
| [掩膜生成](https://huggingface.co/tasks/mask-generation) | `mask-generation` | 为图像中的对象生成掩膜。 | ❌ |
| [目标检测](https://huggingface.co/tasks/object-detection) | `object-detection` | 在图像中识别特定定义类别的对象。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ObjectDetectionPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=object-detection&library=transformers.js) |
| [视频分类](https://huggingface.co/tasks/video-classification) | n/a | 为整个视频分配标签或类别。 | ❌ |
| [无条件图像生成](https://huggingface.co/tasks/unconditional-image-generation) | n/a | 在任何情境中生成没有条件的图像（如提示文本或另一幅图像）。 | ❌ |

#### 音频

| 任务 | ID | 描述 | 是否支持？ |
| --- | --- | --- | --- |
| [音频分类](https://huggingface.co/tasks/audio-classification) | `audio-classification` | 为给定音频分配标签或类别。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.AudioClassificationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=audio-classification&library=transformers.js) |
| [音频对音频](https://huggingface.co/tasks/audio-to-audio) | n/a | 从输入音频源生成音频。 | ❌ |
| [自动语音识别](https://huggingface.co/tasks/automatic-speech-recognition) | `automatic-speech-recognition` | 将给定的音频转录为文本。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.AutomaticSpeechRecognitionPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=automatic-speech-recognition&library=transformers.js) |
| [文本到语音](https://huggingface.co/tasks/text-to-speech) | `text-to-speech` 或 `text-to-audio` | 根据文本输入生成自然音质的语音。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.TextToAudioPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=text-to-audio&library=transformers.js) |

#### 表格

| 任务 | ID | 描述 | 是否支持？ |
| --- | --- | --- | --- |
| [表格分类](https://huggingface.co/tasks/tabular-classification) | n/a | 基于一组属性对目标类别（一组）进行分类。 | ❌ |
| [表格回归](https://huggingface.co/tasks/tabular-regression) | n/a | 根据一组属性预测数值。 | ❌ |

#### 多模态

| 任务 | ID | 描述 | 是否支持？ |
| --- | --- | --- | --- |
| [文档问答](https://huggingface.co/tasks/document-question-answering) | `document-question-answering` | 在文档图像上回答问题。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.DocumentQuestionAnsweringPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=document-question-answering&library=transformers.js) |
| [特征提取](https://huggingface.co/tasks/feature-extraction) | `feature-extraction` | 将原始数据转换为可以处理的数字特征，同时保留原始数据集中的信息。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.FeatureExtractionPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=feature-extraction&library=transformers.js) |
| [图像到文本](https://huggingface.co/tasks/image-to-text) | `image-to-text` | 根据给定图像输出文本。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ImageToTextPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=image-to-text&library=transformers.js) |
| [文本到图像](https://huggingface.co/tasks/text-to-image) | `text-to-image` | 从输入文本生成图像。 | ❌ |
| [视觉问答](https://huggingface.co/tasks/visual-question-answering) | `visual-question-answering` | 基于图像回答开放性问题。 | ❌ |
| [零样本音频分类](https://huggingface.co/learn/audio-course/chapter4/classification_models#zero-shot-audio-classification) | `zero-shot-audio-classification` | 将音频分类为在训练期间未见过的类别。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ZeroShotAudioClassificationPipeline) [(模型)](https://huggingface.co/models?other=zero-shot-audio-classification&library=transformers.js) |
| [零样本图像分类](https://huggingface.co/tasks/zero-shot-image-classification) | `zero-shot-image-classification` | 将图像分类为在训练期间未见过的类别。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ZeroShotImageClassificationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=zero-shot-image-classification&library=transformers.js) |
| [零样本目标检测](https://huggingface.co/tasks/zero-shot-object-detection) | `zero-shot-object-detection` | 识别在训练期间未见过的类别的对象。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ZeroShotObjectDetectionPipeline) [(模型)](https://huggingface.co/models?other=zero-shot-object-detection&library=transformers.js) |

#### 强化学习

| 任务 | ID | 描述 | 是否支持？ |
| --- | --- | --- | --- |
| [强化学习](https://huggingface.co/tasks/reinforcement-learning) | n/a | 通过与环境互动并通过试错接收奖励（负面或正面）作为反馈来学习行为。 | ❌ |
