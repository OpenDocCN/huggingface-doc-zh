# Transformers.js

> 原文：[`huggingface.co/docs/transformers.js/index`](https://huggingface.co/docs/transformers.js/index)

网络的最新机器学习。在浏览器中直接运行🤗 Transformers，无需服务器！

Transformers.js 旨在与 Hugging Face 的[transformers](https://github.com/huggingface/transformers) Python 库在功能上等效，这意味着您可以使用非常相似的 API 运行相同的预训练模型。这些模型支持不同模态中的常见任务，例如：

+   📝 **自然语言处理**：文本分类，命名实体识别，问答，语言建模，摘要，翻译，多项选择和文本生成。

+   🖼️ **计算机视觉**：图像分类，目标检测和分割。

+   🗣️ **音频**：自动语音识别和音频分类。

+   🐙 **多模态**：零样本图像分类。

Transformers.js 使用[ONNX Runtime](https://onnxruntime.ai/)在浏览器中运行模型。最好的部分是，您可以使用[🤗 Optimum](https://github.com/huggingface/optimum#onnx--onnx-runtime)轻松地将您的预训练的 PyTorch，TensorFlow 或 JAX 模型转换为 ONNX。

要获取更多信息，请查看完整的[文档](https://huggingface.co/docs/transformers.js)。

## 快速介绍

从现有代码进行翻译非常简单！就像 Python 库一样，我们支持`pipeline` API。管道将预训练模型与输入的预处理和输出的后处理组合在一起，使其成为使用库运行模型的最简单方式。

| **Python（原始）** | **Javascript（我们的）** |
| --- | --- |

|

```py
from transformers import pipeline

# Allocate a pipeline for sentiment-analysis
pipe = pipeline('sentiment-analysis')

out = pipe('I love transformers!')
# [{'label': 'POSITIVE', 'score': 0.999806941}]
```

|

```py
import { pipeline } from '@xenova/transformers';

// Allocate a pipeline for sentiment-analysis
let pipe = await pipeline('sentiment-analysis');

let out = await pipe('I love transformers!');
// [{'label': 'POSITIVE', 'score': 0.999817686}]
```

|

您还可以通过将模型 ID 或路径指定为`pipeline`函数的第二个参数来使用不同的模型。例如：

```py
// Use a different model for sentiment-analysis
let pipe = await pipeline('sentiment-analysis', 'Xenova/bert-base-multilingual-uncased-sentiment');
```

## 目录

文档分为 4 个部分：

1.  **入门** 提供了对库的快速介绍和安装说明，以便快速上手。

1.  **教程** 是初学者开始的好地方！我们还提供了一些示例应用供您玩耍！

1.  **开发者指南** 展示了如何使用库来实现特定目标。

1.  **API 参考** 描述了所有类和函数，以及它们可用的参数和类型。

## 示例

想要立即开始吗？从我们的示例应用程序/模板开始：

| 名称 | 描述 | 链接 |
| --- | --- | --- |
| Whisper Web | Whisper 的语音识别 | [代码](https://github.com/xenova/whisper-web), [演示](https://huggingface.co/spaces/Xenova/whisper-web) |
| Doodle Dash | 实时涂鸦识别游戏 | [博客](https://huggingface.co/blog/ml-web-games), [代码](https://github.com/xenova/doodle-dash), [演示](https://huggingface.co/spaces/Xenova/doodle-dash) |
| 代码播放器 | 浏览器代码补全网站 | [代码](https://github.com/xenova/transformers.js/tree/main/examples/code-completion/), [演示](https://huggingface.co/spaces/Xenova/ai-code-playground) |
| 语义图像搜索（客户端） | 使用文本搜索图像 | [代码](https://github.com/xenova/transformers.js/tree/main/examples/semantic-image-search-client/), [演示](https://huggingface.co/spaces/Xenova/semantic-image-search-client) |
| 语义图像搜索（服务器端） | 使用文本搜索图像（Supabase） | [代码](https://github.com/xenova/transformers.js/tree/main/examples/semantic-image-search/), [演示](https://huggingface.co/spaces/Xenova/semantic-image-search) |
| Vanilla JavaScript | 浏览器对象检测 | [视频](https://scrimba.com/scrim/cKm9bDAg), [代码](https://github.com/xenova/transformers.js/tree/main/examples/vanilla-js/), [演示](https://huggingface.co/spaces/Scrimba/vanilla-js-object-detector) |
| React | 多语言翻译网站 | [代码](https://github.com/xenova/transformers.js/tree/main/examples/react-translator/), [演示](https://huggingface.co/spaces/Xenova/react-translator) |
| 文本转语音（客户端） | 浏览器内语音合成 | [代码](https://github.com/xenova/transformers.js/tree/main/examples/text-to-speech-client/)，[演示](https://huggingface.co/spaces/Xenova/text-to-speech-client) |
| 浏览器扩展 | 文本分类扩展 | [代码](https://github.com/xenova/transformers.js/tree/main/examples/extension/) |
| 电子 | 文本分类应用 | [代码](https://github.com/xenova/transformers.js/tree/main/examples/electron/) |
| Next.js（客户端） | 情感分析（浏览器推断） | [代码](https://github.com/xenova/transformers.js/tree/main/examples/next-client/)，[演示](https://huggingface.co/spaces/Xenova/next-example-app) |
| Next.js（服务器端） | 情感分析（Node.js 推断） | [代码](https://github.com/xenova/transformers.js/tree/main/examples/next-server/)，[演示](https://huggingface.co/spaces/Xenova/next-server-example-app) |
| Node.js | 情感分析 API | [代码](https://github.com/xenova/transformers.js/tree/main/examples/node/) |
| 演示站点 | 一系列演示 | [代码](https://github.com/xenova/transformers.js/tree/main/examples/demo-site/)，[演示](https://xenova.github.io/transformers.js/) |

查看 Transformers.js 在 Hugging Face 上的[模板](https://huggingface.co/new-space?template=static-templates%2Ftransformers.js)，一键开始！

## 支持的任务/模型

以下是 Transformers.js 当前支持的所有任务和架构列表。如果您在此处看不到您的任务/模型或尚未支持，请随时在[此处](https://github.com/xenova/transformers.js/issues/new/choose)提出功能请求。

要在 Hub 上找到兼容的模型，请在过滤菜单中选择“transformers.js”库标签（或访问[此链接](https://huggingface.co/models?library=transformers.js)）。您可以通过选择您感兴趣的任务（例如，[文本分类](https://huggingface.co/models?pipeline_tag=text-classification&library=transformers.js)）来细化搜索。

### 任务

#### 自然语言处理

| 任务 | ID | 描述 | 是否支持？ |
| --- | --- | --- | --- |
| [对话](https://huggingface.co/tasks/conversational) | `conversational` | 在给定提示的情况下生成相关、连贯和知识丰富的对话文本。 | ❌ |
| [填充掩码](https://huggingface.co/tasks/fill-mask) | `fill-mask` | 掩盖句子中的一些单词并预测应该替换这些掩码的单词。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.FillMaskPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=fill-mask&library=transformers.js) |
| [问答](https://huggingface.co/tasks/question-answering) | `question-answering` | 从给定文本中检索问题的答案。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.QuestionAnsweringPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=question-answering&library=transformers.js) |
| [句子相似度](https://huggingface.co/tasks/sentence-similarity) | `sentence-similarity` | 确定两个文本有多相似。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.FeatureExtractionPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=feature-extraction&library=transformers.js) |
| [摘要](https://huggingface.co/tasks/summarization) | `summarization` | 生成文档的简短版本，同时保留其重要信息。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.SummarizationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=summarization&library=transformers.js) |
| [表格问答](https://huggingface.co/tasks/table-question-answering) | `table-question-answering` | 回答关于给定表格信息的问题。 | ❌ |
| [文本分类](https://huggingface.co/tasks/text-classification) | `text-classification` 或 `sentiment-analysis` | 为给定文本分配一个标签或类别。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.TextClassificationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=text-classification&library=transformers.js) |
| [文本生成](https://huggingface.co/tasks/text-generation#completion-generation-models) | `text-generation` | 通过预测序列中的下一个单词生成新文本。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.TextGenerationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=text-generation&library=transformers.js) |
| [文本到文本生成](https://huggingface.co/tasks/text-generation#text-to-text-generation-models) | `text2text-generation` | 将一个文本序列转换为另一个文本序列。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.Text2TextGenerationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=text2text-generation&library=transformers.js) |
| [标记分类](https://huggingface.co/tasks/token-classification) | `token-classification` 或 `ner` | 为文本中的每个标记分配一个标签。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.TokenClassificationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=token-classification&library=transformers.js) |
| [翻译](https://huggingface.co/tasks/translation) | `translation` | 将文本从一种语言转换为另一种语言。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.TranslationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=translation&library=transformers.js) |
| [零样本分类](https://huggingface.co/tasks/zero-shot-classification) | `zero-shot-classification` | 将文本分类为训练期间未见过的类别。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ZeroShotClassificationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=zero-shot-classification&library=transformers.js) |

#### Vision

| 任务 | ID | 描述 | 支持？ |
| --- | --- | --- | --- |
| [深度估计](https://huggingface.co/tasks/depth-estimation) | `depth-estimation` | 预测图像中存在的对象的深度。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.DepthEstimationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=depth-estimation&library=transformers.js) |
| [图像分类](https://huggingface.co/tasks/image-classification) | `image-classification` | 为整个图像分配一个标签或类别。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ImageClassificationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=image-classification&library=transformers.js) |
| [图像分割](https://huggingface.co/tasks/image-segmentation) | `image-segmentation` | 将图像分割成每个像素映射到一个对象的段。这个任务有多个变体，如实例分割、全景分割和语义分割。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ImageSegmentationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=image-segmentation&library=transformers.js) |
| [图像到图像](https://huggingface.co/tasks/image-to-image) | `image-to-image` | 将源图像转换为与目标图像或目标图像域的特征相匹配的图像。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ImageToImagePipeline) [(模型)](https://huggingface.co/models?pipeline_tag=image-to-image&library=transformers.js) |
| [掩模生成](https://huggingface.co/tasks/mask-generation) | `mask-generation` | 为图像中的对象生成掩模。 | ❌ |
| [目标检测](https://huggingface.co/tasks/object-detection) | `object-detection` | 在图像中识别特定定义类别的对象。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ObjectDetectionPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=object-detection&library=transformers.js) |
| [视频分类](https://huggingface.co/tasks/video-classification) | n/a | 为整个视频分配标签或类别。 | ❌ |
| [无条件图像生成](https://huggingface.co/tasks/unconditional-image-generation) | n/a | 在任何情境中（如提示文本或另一图像）生成无条件的图像。 | ❌ |

#### 音频

| 任务 | ID | 描述 | 是否支持？ |
| --- | --- | --- | --- |
| [音频分类](https://huggingface.co/tasks/audio-classification) | `audio-classification` | 为给定音频分配标签或类别。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.AudioClassificationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=audio-classification&library=transformers.js) |
| [音频到音频](https://huggingface.co/tasks/audio-to-audio) | n/a | 从输入音频源生成音频。 | ❌ |
| [自动语音识别](https://huggingface.co/tasks/automatic-speech-recognition) | `automatic-speech-recognition` | 将给定音频转录为文本。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.AutomaticSpeechRecognitionPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=automatic-speech-recognition&library=transformers.js) |
| [文本到语音](https://huggingface.co/tasks/text-to-speech) | `text-to-speech` 或 `text-to-audio` | 根据文本输入生成自然音质的语音。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.TextToAudioPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=text-to-audio&library=transformers.js) |

#### 表格

| 任务 | ID | 描述 | 是否支持？ |
| --- | --- | --- | --- |
| [表格分类](https://huggingface.co/tasks/tabular-classification) | n/a | 根据属性集对目标类别（一组）进行分类。 | ❌ |
| [表格回归](https://huggingface.co/tasks/tabular-regression) | n/a | 根据属性集预测数值。 | ❌ |

#### 多模态

| 任务 | ID | 描述 | 是否支持？ |
| --- | --- | --- | --- |
| [文档问答](https://huggingface.co/tasks/document-question-answering) | `document-question-answering` | 回答文档图像上的问题。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.DocumentQuestionAnsweringPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=document-question-answering&library=transformers.js) |
| [特征提取](https://huggingface.co/tasks/feature-extraction) | `feature-extraction` | 将原始数据转换为可以处理的数值特征，同时保留原始数据集中的信息。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.FeatureExtractionPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=feature-extraction&library=transformers.js) |
| [图像到文本](https://huggingface.co/tasks/image-to-text) | `image-to-text` | 从给定图像输出文本。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ImageToTextPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=image-to-text&library=transformers.js) |
| [文本到图像](https://huggingface.co/tasks/text-to-image) | `text-to-image` | 从输入文本生成图像。 | ❌ |
| [视觉问答](https://huggingface.co/tasks/visual-question-answering) | `visual-question-answering` | 根据图像回答开放式问题。 | ❌ |
| [零样本音频分类](https://huggingface.co/learn/audio-course/chapter4/classification_models#zero-shot-audio-classification) | `zero-shot-audio-classification` | 将音频分类为在训练过程中未见过的类别。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ZeroShotAudioClassificationPipeline) [(模型)](https://huggingface.co/models?other=zero-shot-audio-classification&library=transformers.js) |
| [零样本图像分类](https://huggingface.co/tasks/zero-shot-image-classification) | `zero-shot-image-classification` | 将图像分类为在训练过程中未见过的类别。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ZeroShotImageClassificationPipeline) [(模型)](https://huggingface.co/models?pipeline_tag=zero-shot-image-classification&library=transformers.js) |
| [零样本目标检测](https://huggingface.co/tasks/zero-shot-object-detection) | `zero-shot-object-detection` | 识别在训练过程中未见过的类别的对象。 | ✅ [(文档)](https://huggingface.co/docs/transformers.js/api/pipelines#module_pipelines.ZeroShotObjectDetectionPipeline) [(模型)](https://huggingface.co/models?other=zero-shot-object-detection&library=transformers.js) |

#### 强化学习

| 任务 | ID | 描述 | 是否支持？ |
| --- | --- | --- | --- |
| [强化学习](https://huggingface.co/tasks/reinforcement-learning) | n/a | 通过与环境互动，通过试错学习行为，并根据奖励（负面或正面）作为反馈。 | ❌ |

### 模型

1.  **[ALBERT](https://huggingface.co/docs/transformers/model_doc/albert)**（来自 Google Research 和芝加哥丰田技术研究所）发布了论文[ALBERT:自监督学习语言表示的轻量 BERT](https://arxiv.org/abs/1909.11942)，作者是 Zhenzhong Lan，Mingda Chen，Sebastian Goodman，Kevin Gimpel，Piyush Sharma，Radu Soricut。

1.  **[音频频谱变换器](https://huggingface.co/docs/transformers/model_doc/audio-spectrogram-transformer)**（来自 MIT）发布了论文[AST:音频频谱变换器](https://arxiv.org/abs/2104.01778)，作者是 Yuan Gong，Yu-An Chung，James Glass。

1.  **[BART](https://huggingface.co/docs/transformers/model_doc/bart)**（来自 Facebook）发布了论文[BART:去噪序列到序列预训练用于自然语言生成、翻译和理解](https://arxiv.org/abs/1910.13461)，作者是 Mike Lewis，Yinhan Liu，Naman Goyal，Marjan Ghazvininejad，Abdelrahman Mohamed，Omer Levy，Ves Stoyanov 和 Luke Zettlemoyer。

1.  **[BEiT](https://huggingface.co/docs/transformers/model_doc/beit)**（来自微软）发布了论文[BEiT: 图像 Transformer 的 BERT 预训练](https://arxiv.org/abs/2106.08254)，作者是 Hangbo Bao，Li Dong，Furu Wei。

1.  **[BERT](https://huggingface.co/docs/transformers/model_doc/bert)**（来自 Google）发布了论文[BERT:深度双向 Transformer 的预训练用于语言理解](https://arxiv.org/abs/1810.04805)，作者是 Jacob Devlin，Ming-Wei Chang，Kenton Lee 和 Kristina Toutanova。

1.  **[Blenderbot](https://huggingface.co/docs/transformers/model_doc/blenderbot)**（来自 Facebook）发布了论文[构建开放域聊天机器人的配方](https://arxiv.org/abs/2004.13637)，作者是 Stephen Roller，Emily Dinan，Naman Goyal，Da Ju，Mary Williamson，Yinhan Liu，Jing Xu，Myle Ott，Kurt Shuster，Eric M. Smith，Y-Lan Boureau，Jason Weston。

1.  **[BlenderbotSmall](https://huggingface.co/docs/transformers/model_doc/blenderbot-small)**（来自 Facebook）由 Stephen Roller、Emily Dinan、Naman Goyal、Da Ju、Mary Williamson、Yinhan Liu、Jing Xu、Myle Ott、Kurt Shuster、Eric M. Smith、Y-Lan Boureau、Jason Weston 发布的论文[Recipes for building an open-domain chatbot](https://arxiv.org/abs/2004.13637)。

1.  **[BLOOM](https://huggingface.co/docs/transformers/model_doc/bloom)**（来自 BigScience workshop）由[BigScience Workshop](https://bigscience.huggingface.co/)发布。

1.  **[CamemBERT](https://huggingface.co/docs/transformers/model_doc/camembert)**（来自 Inria/Facebook/Sorbonne）由 Louis Martin*、Benjamin Muller*、Pedro Javier Ortiz Suárez*、Yoann Dupont、Laurent Romary、Éric Villemonte de la Clergerie、Djamé Seddah 和 Benoît Sagot 发布的论文[CamemBERT: a Tasty French Language Model](https://arxiv.org/abs/1911.03894)。

1.  **[Chinese-CLIP](https://huggingface.co/docs/transformers/model_doc/chinese_clip)**（来自 OFA-Sys）由 An Yang、Junshu Pan、Junyang Lin、Rui Men、Yichang Zhang、Jingren Zhou、Chang Zhou 发布的论文[Chinese CLIP: Contrastive Vision-Language Pretraining in Chinese](https://arxiv.org/abs/2211.01335)。

1.  **[CLAP](https://huggingface.co/docs/transformers/model_doc/clap)**（来自 LAION-AI）由 Yusong Wu、Ke Chen、Tianyu Zhang、Yuchen Hui、Taylor Berg-Kirkpatrick、Shlomo Dubnov 发布的论文[Large-scale Contrastive Language-Audio Pretraining with Feature Fusion and Keyword-to-Caption Augmentation](https://arxiv.org/abs/2211.06687)。

1.  **[CLIP](https://huggingface.co/docs/transformers/model_doc/clip)**（来自 OpenAI）由 Alec Radford、Jong Wook Kim、Chris Hallacy、Aditya Ramesh、Gabriel Goh、Sandhini Agarwal、Girish Sastry、Amanda Askell、Pamela Mishkin、Jack Clark、Gretchen Krueger、Ilya Sutskever 发布的论文[Learning Transferable Visual Models From Natural Language Supervision](https://arxiv.org/abs/2103.00020)。

1.  **[CLIPSeg](https://huggingface.co/docs/transformers/model_doc/clipseg)**（来自 Göttingen 大学）由 Timo Lüddecke 和 Alexander Ecker 发布的论文[Image Segmentation Using Text and Image Prompts](https://arxiv.org/abs/2112.10003)。

1.  **[CodeGen](https://huggingface.co/docs/transformers/model_doc/codegen)**（来自 Salesforce）由 Erik Nijkamp、Bo Pang、Hiroaki Hayashi、Lifu Tu、Huan Wang、Yingbo Zhou、Silvio Savarese、Caiming Xiong 发布的论文[A Conversational Paradigm for Program Synthesis](https://arxiv.org/abs/2203.13474)。

1.  **[CodeLlama](https://huggingface.co/docs/transformers/model_doc/llama_code)**（来自 MetaAI）由 Baptiste Rozière、Jonas Gehring、Fabian Gloeckle、Sten Sootla、Itai Gat、Xiaoqing Ellen Tan、Yossi Adi、Jingyu Liu、Tal Remez、Jérémy Rapin、Artyom Kozhevnikov、Ivan Evtimov、Joanna Bitton、Manish Bhatt、Cristian Canton Ferrer、Aaron Grattafiori、Wenhan Xiong、Alexandre Défossez、Jade Copet、Faisal Azhar、Hugo Touvron、Louis Martin、Nicolas Usunier、Thomas Scialom、Gabriel Synnaeve 发布的论文[Code Llama: Open Foundation Models for Code](https://ai.meta.com/research/publications/code-llama-open-foundation-models-for-code)。

1.  **[ConvBERT](https://huggingface.co/docs/transformers/model_doc/convbert)**（来自 YituTech）由 Zihang Jiang、Weihao Yu、Daquan Zhou、Yunpeng Chen、Jiashi Feng、Shuicheng Yan 发布的论文[ConvBERT: Improving BERT with Span-based Dynamic Convolution](https://arxiv.org/abs/2008.02496)。

1.  **[ConvNeXT](https://huggingface.co/docs/transformers/model_doc/convnext)**（来自 Facebook AI）由 Zhuang Liu、Hanzi Mao、Chao-Yuan Wu、Christoph Feichtenhofer、Trevor Darrell、Saining Xie 发布的论文[A ConvNet for the 2020s](https://arxiv.org/abs/2201.03545)。

1.  **[ConvNeXTV2](https://huggingface.co/docs/transformers/model_doc/convnextv2)**（来自 Facebook AI）与 Sanghyun Woo、Shoubhik Debnath、Ronghang Hu、Xinlei Chen、Zhuang Liu、In So Kweon、Saining Xie 的论文[ConvNeXt V2: Co-designing and Scaling ConvNets with Masked Autoencoders](https://arxiv.org/abs/2301.00808)一同发布。

1.  **[DeBERTa](https://huggingface.co/docs/transformers/model_doc/deberta)**（来自微软）与 Pengcheng He、Xiaodong Liu、Jianfeng Gao、Weizhu Chen 的论文[DeBERTa: Decoding-enhanced BERT with Disentangled Attention](https://arxiv.org/abs/2006.03654)一同发布。

1.  **[DeBERTa-v2](https://huggingface.co/docs/transformers/model_doc/deberta-v2)**（来自微软）与 Pengcheng He、Xiaodong Liu、Jianfeng Gao、Weizhu Chen 的论文[DeBERTa: Decoding-enhanced BERT with Disentangled Attention](https://arxiv.org/abs/2006.03654)一同发布。

1.  **[DeiT](https://huggingface.co/docs/transformers/model_doc/deit)**（来自 Facebook）与 Hugo Touvron、Matthieu Cord、Matthijs Douze、Francisco Massa、Alexandre Sablayrolles、Hervé Jégou 的论文[Training data-efficient image transformers & distillation through attention](https://arxiv.org/abs/2012.12877)一同发布。

1.  **[Depth Anything](https://huggingface.co/docs/transformers/main/model_doc/depth_anything)**（来自香港大学和 TikTok）与 Lihe Yang、Bingyi Kang、Zilong Huang、Xiaogang Xu、Jiashi Feng、Hengshuang Zhao 的论文[Depth Anything: Unleashing the Power of Large-Scale Unlabeled Data](https://arxiv.org/abs/2401.10891)一同发布。

1.  **[DETR](https://huggingface.co/docs/transformers/model_doc/detr)**（来自 Facebook）与 Nicolas Carion、Francisco Massa、Gabriel Synnaeve、Nicolas Usunier、Alexander Kirillov、Sergey Zagoruyko 的论文[End-to-End Object Detection with Transformers](https://arxiv.org/abs/2005.12872)一同发布。

1.  **[DINOv2](https://huggingface.co/docs/transformers/model_doc/dinov2)**（来自 Meta AI）与 Maxime Oquab、Timothée Darcet、Théo Moutakanni、Huy Vo、Marc Szafraniec、Vasil Khalidov、Pierre Fernandez、Daniel Haziza、Francisco Massa、Alaaeldin El-Nouby、Mahmoud Assran、Nicolas Ballas、Wojciech Galuba、Russell Howes、Po-Yao Huang、Shang-Wen Li、Ishan Misra、Michael Rabbat、Vasu Sharma、Gabriel Synnaeve、Hu Xu、Hervé Jegou、Julien Mairal、Patrick Labatut、Armand Joulin、Piotr Bojanowski 的论文[DINOv2: Learning Robust Visual Features without Supervision](https://arxiv.org/abs/2304.07193)一同发布。

1.  **[DistilBERT](https://huggingface.co/docs/transformers/model_doc/distilbert)**（来自 HuggingFace）与 Victor Sanh、Lysandre Debut 和 Thomas Wolf 的论文[DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter](https://arxiv.org/abs/1910.01108)一同发布。相同的方法已被应用于将 GPT2 压缩为[DistilGPT2](https://github.com/huggingface/transformers/tree/main/examples/research_projects/distillation)、RoBERTa 压缩为[DistilRoBERTa](https://github.com/huggingface/transformers/tree/main/examples/research_projects/distillation)、Multilingual BERT 压缩为[DistilmBERT](https://github.com/huggingface/transformers/tree/main/examples/research_projects/distillation)以及德语版本的 DistilBERT。

1.  **[DiT](https://huggingface.co/docs/transformers/model_doc/dit)**（来自微软研究）与 Junlong Li、Yiheng Xu、Tengchao Lv、Lei Cui、Cha Zhang、Furu Wei 的论文[DiT: Self-supervised Pre-training for Document Image Transformer](https://arxiv.org/abs/2203.02378)一同发布。

1.  **[Donut](https://huggingface.co/docs/transformers/model_doc/donut)**（来自 NAVER）与 Geewook Kim、Teakgyu Hong、Moonbin Yim、Jeongyeon Nam、Jinyoung Park、Jinyeong Yim、Wonseok Hwang、Sangdoo Yun、Dongyoon Han、Seunghyun Park 的论文[OCR-free Document Understanding Transformer](https://arxiv.org/abs/2111.15664)一同发布。

1.  **[DPT](https://huggingface.co/docs/transformers/master/model_doc/dpt)**（来自英特尔实验室）与 René Ranftl、Alexey Bochkovskiy、Vladlen Koltun 合著的论文[用于密集预测的视觉 Transformer](https://arxiv.org/abs/2103.13413)一起发布。

1.  **[ELECTRA](https://huggingface.co/docs/transformers/model_doc/electra)**（来自 Google Research/斯坦福大学）与 Kevin Clark、Minh-Thang Luong、Quoc V. Le、Christopher D. Manning 合著的论文[将文本编码器预训练为判别器而不是生成器](https://arxiv.org/abs/2003.10555)一起发布。

1.  **[ESM](https://huggingface.co/docs/transformers/model_doc/esm)**（来自 Meta AI）是变压器蛋白质语言模型。**ESM-1b**与 Alexander Rives、Joshua Meier、Tom Sercu、Siddharth Goyal、Zeming Lin、Jason Liu、Demi Guo、Myle Ott、C. Lawrence Zitnick、Jerry Ma 和 Rob Fergus 合著的论文[从扩展无监督学习到 2.5 亿蛋白质序列的生物结构和功能](https://www.pnas.org/content/118/15/e2016239118)一起发布。**ESM-1v**与 Joshua Meier、Roshan Rao、Robert Verkuil、Jason Liu、Tom Sercu 和 Alexander Rives 合著的论文[语言模型实现对蛋白质功能突变的零样本预测](https://doi.org/10.1101/2021.07.09.450648)一起发布。**ESM-2 和 ESMFold**与 Zeming Lin、Halil Akin、Roshan Rao、Brian Hie、Zhongkai Zhu、Wenting Lu、Allan dos Santos Costa、Maryam Fazel-Zarandi、Tom Sercu、Sal Candido、Alexander Rives 合著的论文[蛋白质序列的语言模型在演化尺度上实现准确的结构预测](https://doi.org/10.1101/2022.07.20.500902)一起发布。

1.  **[Falcon](https://huggingface.co/docs/transformers/model_doc/falcon)**（来自技术创新研究所）由 Almazrouei、Ebtesam、Alobeidli、Hamza、Alshamsi、Abdulaziz、Cappelli、Alessandro、Cojocaru、Ruxandra、Debbah、Merouane、Goffinet、Etienne、Heslow、Daniel、Launay、Julien、Malartic、Quentin、Noune、Badreddine、Pannier、Baptiste、Penedo、Guilherme 等人合作。

1.  **[FLAN-T5](https://huggingface.co/docs/transformers/model_doc/flan-t5)**（来自 Google AI）由 Hyung Won Chung、Le Hou、Shayne Longpre、Barret Zoph、Yi Tay、William Fedus、Eric Li、Xuezhi Wang、Mostafa Dehghani、Siddhartha Brahma、Albert Webson、Shixiang Shane Gu、Zhuyun Dai、Mirac Suzgun、Xinyun Chen、Aakanksha Chowdhery、Sharan Narang、Gaurav Mishra、Adams Yu、Vincent Zhao、Yanping Huang、Andrew Dai、Hongkun Yu、Slav Petrov、Ed H. Chi、Jeff Dean、Jacob Devlin、Adam Roberts、Denny Zhou、Quoc V. Le 和 Jason Wei 合作发布。

1.  **[GLPN](https://huggingface.co/docs/transformers/model_doc/glpn)**（来自 KAIST）与 Doyeon Kim、Woonghyun Ga、Pyungwhan Ahn、Donggyu Joo、Sehwan Chun、Junmo Kim 合著的论文[全局-局部路径网络用于单目深度估计与垂直切割深度](https://arxiv.org/abs/2201.07436)一起发布。

1.  **[GPT Neo](https://huggingface.co/docs/transformers/model_doc/gpt_neo)**（来自 EleutherAI）由 Sid Black、Stella Biderman、Leo Gao、Phil Wang 和 Connor Leahy 合作发布。

1.  **[GPT NeoX](https://huggingface.co/docs/transformers/model_doc/gpt_neox)**（来自 EleutherAI）与 Sid Black、Stella Biderman、Eric Hallahan、Quentin Anthony、Leo Gao、Laurence Golding、Horace He、Connor Leahy、Kyle McDonell、Jason Phang、Michael Pieler、USVSN Sai Prashanth、Shivanshu Purohit、Laria Reynolds、Jonathan Tow、Ben Wang、Samuel Weinbach 合著的论文[GPT-NeoX-20B：一个开源的自回归语言模型](https://arxiv.org/abs/2204.06745)一起发布。

1.  **[GPT-2](https://huggingface.co/docs/transformers/model_doc/gpt2)**（来自 OpenAI）与 Alec Radford*、Jeffrey Wu*、Rewon Child、David Luan、Dario Amodei**和 Ilya Sutskever**合著的论文[《Language Models are Unsupervised Multitask Learners》](https://blog.openai.com/better-language-models/)一同发布。

1.  **[GPT-J](https://huggingface.co/docs/transformers/model_doc/gptj)**（来自 EleutherAI）由 Ben Wang 和 Aran Komatsuzaki 在[repository kingoflolz/mesh-transformer-jax](https://github.com/kingoflolz/mesh-transformer-jax/)中发布。

1.  **[GPTBigCode](https://huggingface.co/docs/transformers/model_doc/gpt_bigcode)**（来自 BigCode）与 Loubna Ben Allal、Raymond Li、Denis Kocetkov、Chenghao Mou、Christopher Akiki、Carlos Munoz Ferrandis、Niklas Muennighoff、Mayank Mishra、Alex Gu、Manan Dey、Logesh Kumar Umapathi、Carolyn Jane Anderson、Yangtian Zi、Joel Lamy Poirier、Hailey Schoelkopf、Sergey Troshin、Dmitry Abulkhanov、Manuel Romero、Michael Lappert、Francesco De Toni、Bernardo García del Río、Qian Liu、Shamik Bose、Urvashi Bhattacharyya、Terry Yue Zhuo、Ian Yu、Paulo Villegas、Marco Zocca、Sourab Mangrulkar、David Lansky、Huu Nguyen、Danish Contractor、Luis Villa、Jia Li、Dzmitry Bahdanau、Yacine Jernite、Sean Hughes、Daniel Fried、Arjun Guha、Harm de Vries、Leandro von Werra 合著的论文[《SantaCoder: don’t reach for the stars!》](https://arxiv.org/abs/2301.03988)一同发布。

1.  **[HerBERT](https://huggingface.co/docs/transformers/model_doc/herbert)**（来自 Allegro.pl, AGH 科技大学）与 Piotr Rybak、Robert Mroczkowski、Janusz Tracz、Ireneusz Gawlik 合著的论文[《KLEJ: Comprehensive Benchmark for Polish Language Understanding》](https://www.aclweb.org/anthology/2020.acl-main.111.pdf)一同发布。

1.  **[Hubert](https://huggingface.co/docs/transformers/model_doc/hubert)**（来自 Facebook）与 Wei-Ning Hsu、Benjamin Bolte、Yao-Hung Hubert Tsai、Kushal Lakhotia、Ruslan Salakhutdinov、Abdelrahman Mohamed 合著的论文[《HuBERT: Self-Supervised Speech Representation Learning by Masked Prediction of Hidden Units》](https://arxiv.org/abs/2106.07447)一同发布。

1.  **[LongT5](https://huggingface.co/docs/transformers/model_doc/longt5)**（来自 Google AI）与 Mandy Guo、Joshua Ainslie、David Uthus、Santiago Ontanon、Jianmo Ni、Yun-Hsuan Sung、Yinfei Yang 合著的论文[《LongT5: Efficient Text-To-Text Transformer for Long Sequences》](https://arxiv.org/abs/2112.07916)一同发布。

1.  **[LLaMA](https://huggingface.co/docs/transformers/model_doc/llama)**（来自 Meta AI 的 FAIR 团队）与 Hugo Touvron、Thibaut Lavril、Gautier Izacard、Xavier Martinet、Marie-Anne Lachaux、Timothée Lacroix、Baptiste Rozière、Naman Goyal、Eric Hambro、Faisal Azhar、Aurelien Rodriguez、Armand Joulin、Edouard Grave、Guillaume Lample 合著的论文[《LLaMA: Open and Efficient Foundation Language Models》](https://arxiv.org/abs/2302.13971)一同发布。

1.  **[Llama2](https://huggingface.co/docs/transformers/model_doc/llama2)**（来自 Meta AI 的 FAIR 团队）与论文[Llama2：开放基础和精细调整的聊天模型](https://ai.meta.com/research/publications/llama-2-open-foundation-and-fine-tuned-chat-models/XXX)一起发布，作者是 Hugo Touvron，Louis Martin，Kevin Stone，Peter Albert，Amjad Almahairi，Yasmine Babaei，Nikolay Bashlykov，Soumya Batra，Prajjwal Bhargava，Shruti Bhosale，Dan Bikel，Lukas Blecher，Cristian Canton Ferrer，Moya Chen，Guillem Cucurull，David Esiobu，Jude Fernandes，Jeremy Fu，Wenyin Fu，Brian Fuller，Cynthia Gao，Vedanuj Goswami，Naman Goyal，Anthony Hartshorn，Saghar Hosseini，Rui Hou，Hakan Inan，Marcin Kardas，Viktor Kerkez Madian Khabsa，Isabel Kloumann，Artem Korenev，Punit Singh Koura，Marie-Anne Lachaux，Thibaut Lavril，Jenya Lee，Diana Liskovich，Yinghai Lu，Yuning Mao，Xavier Martinet，Todor Mihaylov，Pushka rMishra，Igor Molybog，Yixin Nie，Andrew Poulton，Jeremy Reizenstein，Rashi Rungta，Kalyan Saladi，Alan Schelten，Ruan Silva，Eric Michael Smith，Ranjan Subramanian，Xiaoqing EllenTan，Binh Tang，Ross Taylor，Adina Williams，Jian Xiang Kuan，Puxin Xu，Zheng Yan，Iliyan Zarov，Yuchen Zhang，Angela Fan，Melanie Kambadur，Sharan Narang，Aurelien Rodriguez，Robert Stojnic，Sergey Edunov，Thomas Scialom。

1.  **[M2M100](https://huggingface.co/docs/transformers/model_doc/m2m_100)**（来自 Facebook）与论文[超越以英语为中心的多语言机器翻译](https://arxiv.org/abs/2010.11125)一起发布，作者是 Angela Fan，Shruti Bhosale，Holger Schwenk，Zhiyi Ma，Ahmed El-Kishky，Siddharth Goyal，Mandeep Baines，Onur Celebi，Guillaume Wenzek，Vishrav Chaudhary，Naman Goyal，Tom Birch，Vitaliy Liptchinsky，Sergey Edunov，Edouard Grave，Michael Auli，Armand Joulin。

1.  **[MarianMT](https://huggingface.co/docs/transformers/model_doc/marian)**使用 Jörg Tiedemann 的[OPUS](http://opus.nlpl.eu/)数据训练的机器翻译模型。[Marian Framework](https://marian-nmt.github.io/)由 Microsoft Translator 团队开发。

1.  **[mBART](https://huggingface.co/docs/transformers/model_doc/mbart)**（来自 Facebook）与论文[神经机器翻译的多语言去噪预训练](https://arxiv.org/abs/2001.08210)一起发布，作者是 Yinhan Liu，Jiatao Gu，Naman Goyal，Xian Li，Sergey Edunov，Marjan Ghazvininejad，Mike Lewis，Luke Zettlemoyer。

1.  **[mBART-50](https://huggingface.co/docs/transformers/model_doc/mbart)**（来自 Facebook）与论文[具有可扩展多语言预训练和微调的多语言翻译](https://arxiv.org/abs/2008.00401)一起发布，作者是 Yuqing Tang，Chau Tran，Xian Li，Peng-Jen Chen，Naman Goyal，Vishrav Chaudhary，Jiatao Gu，Angela Fan。

1.  **[Mistral](https://huggingface.co/docs/transformers/model_doc/mistral)**（来自 Mistral AI）由[Mistral AI](https://mistral.ai)团队发布：Albert Jiang，Alexandre Sablayrolles，Arthur Mensch，Chris Bamford，Devendra Singh Chaplot，Diego de las Casas，Florian Bressand，Gianna Lengyel，Guillaume Lample，Lélio Renard Lavaud，Lucile Saulnier，Marie-Anne Lachaux，Pierre Stock，Teven Le Scao，Thibaut Lavril，Thomas Wang，Timothée Lacroix，William El Sayed。

1.  **[MMS](https://huggingface.co/docs/transformers/model_doc/mms)**（来自 Facebook）与论文[将语音技术扩展到 1000 多种语言](https://arxiv.org/abs/2305.13516)一起发布，作者是 Vineel Pratap，Andros Tjandra，Bowen Shi，Paden Tomasello，Arun Babu，Sayani Kundu，Ali Elkahky，Zhaoheng Ni，Apoorv Vyas，Maryam Fazel-Zarandi，Alexei Baevski，Yossi Adi，Xiaohui Zhang，Wei-Ning Hsu，Alexis Conneau，Michael Auli。

1.  **[MobileBERT](https://huggingface.co/docs/transformers/model_doc/mobilebert)**（来自 CMU/Google Brain）与论文[MobileBERT：适用于资源受限设备的紧凑通用 BERT](https://arxiv.org/abs/2004.02984)一起发布，作者是 Zhiqing Sun，Hongkun Yu，Xiaodan Song，Renjie Liu，Yiming Yang 和 Denny Zhou。

1.  **[MobileViT](https://huggingface.co/docs/transformers/model_doc/mobilevit)**（来自苹果）由 Sachin Mehta 和 Mohammad Rastegari 在论文[MobileViT: Light-weight, General-purpose, and Mobile-friendly Vision Transformer](https://arxiv.org/abs/2110.02178)中发布。

1.  **[MPNet](https://huggingface.co/docs/transformers/model_doc/mpnet)**（来自微软研究）由 Kaitao Song、Xu Tan、Tao Qin、Jianfeng Lu、刘铁彦在论文[MPNet: Masked and Permuted Pre-training for Language Understanding](https://arxiv.org/abs/2004.09297)中发布。

1.  **[MPT](https://huggingface.co/docs/transformers/model_doc/mpt)**（来自 MosaiML）由 MosaicML NLP 团队在[llm-foundry](https://github.com/mosaicml/llm-foundry/)存储库中发布。

1.  **[MT5](https://huggingface.co/docs/transformers/model_doc/mt5)**（来自 Google AI）由 Linting Xue、Noah Constant、Adam Roberts、Mihir Kale、Rami Al-Rfou、Aditya Siddhant、Aditya Barua、Colin Raffel 在论文[mT5: A massively multilingual pre-trained text-to-text transformer](https://arxiv.org/abs/2010.11934)中发布。

1.  **[NLLB](https://huggingface.co/docs/transformers/model_doc/nllb)**（来自 Meta）由 NLLB 团队在论文[No Language Left Behind: Scaling Human-Centered Machine Translation](https://arxiv.org/abs/2207.04672)中发布。

1.  **[Nougat](https://huggingface.co/docs/transformers/model_doc/nougat)**（来自 Meta AI）由 Lukas Blecher、Guillem Cucurull、Thomas Scialom、Robert Stojnic 在论文[Nougat: Neural Optical Understanding for Academic Documents](https://arxiv.org/abs/2308.13418)中发布。

1.  **[OPT](https://huggingface.co/docs/transformers/master/model_doc/opt)**（来自 Meta AI）由 Susan Zhang、Stephen Roller、Naman Goyal、Mikel Artetxe、Moya Chen、Shuohui Chen 等人在论文[OPT: Open Pre-trained Transformer Language Models](https://arxiv.org/abs/2205.01068)中发布。

1.  **[OWL-ViT](https://huggingface.co/docs/transformers/model_doc/owlvit)**（来自 Google AI）由 Matthias Minderer、Alexey Gritsenko、Austin Stone、Maxim Neumann、Dirk Weissenborn、Alexey Dosovitskiy、Aravindh Mahendran、Anurag Arnab、Mostafa Dehghani、Zhuoran Shen、Xiao Wang、Xiaohua Zhai、Thomas Kipf 和 Neil Houlsby 在论文[Simple Open-Vocabulary Object Detection with Vision Transformers](https://arxiv.org/abs/2205.06230)中发布。

1.  **[Phi](https://huggingface.co/docs/transformers/main/model_doc/phi)**（来自微软）由 Suriya Gunasekar、Yi Zhang、Jyoti Aneja、Caio César Teodoro Mendes、Allie Del Giorno、Sivakanth Gopi、Mojan Javaheripi、Piero Kauffmann、Gustavo de Rosa、Olli Saarikivi、Adil Salim、Shital Shah、Harkirat Singh Behl、Xin Wang、Sébastien Bubeck、Ronen Eldan、Adam Tauman Kalai、Yin Tat Lee 和 Yuanzhi Li 在论文[Textbooks Are All You Need](https://arxiv.org/abs/2306.11644)和[Yuanzhi Li、Sébastien Bubeck、Ronen Eldan、Allie Del Giorno、Suriya Gunasekar 和 Yin Tat Lee 在论文 Textbooks Are All You Need II: phi-1.5 technical report](https://arxiv.org/abs/2309.05463)中发布。

1.  **[Qwen2](https://huggingface.co/docs/transformers/model_doc/qwen2)**（来自阿里巴巴集团的 Qwen 团队）由 Jinze Bai、Shuai Bai、Yunfei Chu、Zeyu Cui、Kai Dang、Xiaodong Deng、Yang Fan、Wenbin Ge、Yu Han、Fei Huang、Binyuan Hui、Luo Ji、Mei Li、Junyang Lin、Runji Lin、Dayiheng Liu、Gao Liu、Chengqiang Lu、Keming Lu、Jianxin Ma、Rui Men、Xingzhang Ren、Xuancheng Ren、Chuanqi Tan、Sinan Tan、Jianhong Tu、Peng Wang、Shijie Wang、Wei Wang、Shengguang Wu、Benfeng Xu、Jin Xu、An Yang、Hao Yang、Jian Yang、Shusheng Yang、Yang Yao、Bowen Yu、Hongyi Yuan、Zheng Yuan、Jianwei Zhang、Xingxuan Zhang、Yichang Zhang、Zhenru Zhang、Chang Zhou、Jingren Zhou、Xiaohuan Zhou 和 Tianhang Zhu 在论文[Qwen Technical Report](https://arxiv.org/abs/2309.16609)中发布。

1.  **[ResNet](https://huggingface.co/docs/transformers/model_doc/resnet)**（来自微软研究）与论文[Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385)一起发布，作者为 Kaiming He、Xiangyu Zhang、Shaoqing Ren、Jian Sun。

1.  **[RoBERTa](https://huggingface.co/docs/transformers/model_doc/roberta)**（来自 Facebook），与论文[RoBERTa: A Robustly Optimized BERT Pretraining Approach](https://arxiv.org/abs/1907.11692)一起发布，作者为刘银涵、Myle Ott、Naman Goyal、Jingfei Du、Mandar Joshi、Danqi Chen、Omer Levy、Mike Lewis、Luke Zettlemoyer、Veselin Stoyanov。

1.  **[RoFormer](https://huggingface.co/docs/transformers/model_doc/roformer)**（来自追一科技），与论文[RoFormer: Enhanced Transformer with Rotary Position Embedding](https://arxiv.org/abs/2104.09864)一起发布，作者为苏建林、陆宇、潘胜锋、文波、刘云峰。

1.  **[SegFormer](https://huggingface.co/docs/transformers/model_doc/segformer)**（来自 NVIDIA）与论文[SegFormer: Simple and Efficient Design for Semantic Segmentation with Transformers](https://arxiv.org/abs/2105.15203)一起发布，作者为 Enze Xie、Wenhai Wang、Zhiding Yu、Anima Anandkumar、Jose M. Alvarez、Ping Luo。

1.  **[Segment Anything](https://huggingface.co/docs/transformers/model_doc/sam)**（来自 Meta AI）与论文[Segment Anything](https://arxiv.org/pdf/2304.02643v1.pdf)一起发布，作者为 Alexander Kirillov、Eric Mintun、Nikhila Ravi、Hanzi Mao、Chloe Rolland、Laura Gustafson、Tete Xiao、Spencer Whitehead、Alex Berg、Wan-Yen Lo、Piotr Dollar、Ross Girshick。

1.  **[SigLIP](https://huggingface.co/docs/transformers/main/model_doc/siglip)**（来自 Google AI）与论文[Sigmoid Loss for Language Image Pre-Training](https://arxiv.org/abs/2303.15343)一起发布，作者为 Xiaohua Zhai、Basil Mustafa、Alexander Kolesnikov、Lucas Beyer。

1.  **[SpeechT5](https://huggingface.co/docs/transformers/model_doc/speecht5)**（来自微软研究）与论文[SpeechT5: Unified-Modal Encoder-Decoder Pre-Training for Spoken Language Processing](https://arxiv.org/abs/2110.07205)一起发布，作者为 Junyi Ao、Rui Wang、Long Zhou、Chengyi Wang、Shuo Ren、Yu Wu、Shujie Liu、Tom Ko、Qing Li、Yu Zhang、Zhihua Wei、Yao Qian、Jinyu Li、Furu Wei。

1.  **[SqueezeBERT](https://huggingface.co/docs/transformers/model_doc/squeezebert)**（来自伯克利）与论文[SqueezeBERT: What can computer vision teach NLP about efficient neural networks?](https://arxiv.org/abs/2006.11316)一起发布，作者为 Forrest N. Iandola、Albert E. Shaw、Ravi Krishna、Kurt W. Keutzer。

1.  **[Swin Transformer](https://huggingface.co/docs/transformers/model_doc/swin)**（来自微软）与论文[Swin Transformer: Hierarchical Vision Transformer using Shifted Windows](https://arxiv.org/abs/2103.14030)一起发布，作者为 Ze Liu、Yutong Lin、Yue Cao、Han Hu、Yixuan Wei、Zheng Zhang、Stephen Lin、Baining Guo。

1.  **[Swin2SR](https://huggingface.co/docs/transformers/model_doc/swin2sr)**（来自 Würzburg 大学）与论文[Swin2SR: SwinV2 Transformer for Compressed Image Super-Resolution and Restoration](https://arxiv.org/abs/2209.11345)一起发布，作者为 Marcos V. Conde、Ui-Jin Choi、Maxime Burchi、Radu Timofte。

1.  **[T5](https://huggingface.co/docs/transformers/model_doc/t5)**（来自 Google AI）与论文[Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer](https://arxiv.org/abs/1910.10683)一起发布，作者为 Colin Raffel、Noam Shazeer、Adam Roberts、Katherine Lee、Sharan Narang、Michael Matena、Yanqi Zhou、Wei Li、Peter J. Liu。

1.  **[T5v1.1](https://huggingface.co/docs/transformers/model_doc/t5v1.1)**（来自 Google AI）在[google-research/text-to-text-transfer-transformer](https://github.com/google-research/text-to-text-transfer-transformer/blob/main/released_checkpoints.md#t511)仓库中发布，作者为 Colin Raffel、Noam Shazeer、Adam Roberts、Katherine Lee、Sharan Narang、Michael Matena、Yanqi Zhou、Wei Li、Peter J. Liu。

1.  **[Table Transformer](https://huggingface.co/docs/transformers/model_doc/table-transformer)**（来自微软研究院），与 Brandon Smock、Rohith Pesala、Robin Abraham 合作发布了论文[PubTables-1M: Towards Comprehensive Table Extraction From Unstructured Documents](https://arxiv.org/abs/2110.00061)。

1.  **[TrOCR](https://huggingface.co/docs/transformers/model_doc/trocr)**（来自微软），与 Minghao Li、Tengchao Lv、Lei Cui、Yijuan Lu、Dinei Florencio、Cha Zhang、Zhoujun Li、Furu Wei 合作发布了论文[TrOCR: Transformer-based Optical Character Recognition with Pre-trained Models](https://arxiv.org/abs/2109.10282)。

1.  **[Vision Transformer (ViT)](https://huggingface.co/docs/transformers/model_doc/vit)**（来自 Google AI），与 Alexey Dosovitskiy、Lucas Beyer、Alexander Kolesnikov、Dirk Weissenborn、Xiaohua Zhai、Thomas Unterthiner、Mostafa Dehghani、Matthias Minderer、Georg Heigold、Sylvain Gelly、Jakob Uszkoreit、Neil Houlsby 合作发布了论文[An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale](https://arxiv.org/abs/2010.11929)。

1.  **[ViTMatte](https://huggingface.co/docs/transformers/model_doc/vitmatte)**（来自 HUST-VL），与 Jingfeng Yao、Xinggang Wang、Shusheng Yang、Baoyuan Wang 合作发布了论文[ViTMatte: Boosting Image Matting with Pretrained Plain Vision Transformers](https://arxiv.org/abs/2305.15272)。

1.  **[VITS](https://huggingface.co/docs/transformers/model_doc/vits)**（来自 Kakao Enterprise），与 Jaehyeon Kim、Jungil Kong、Juhee Son 合作发布了论文[Conditional Variational Autoencoder with Adversarial Learning for End-to-End Text-to-Speech](https://arxiv.org/abs/2106.06103)。

1.  **[Wav2Vec2](https://huggingface.co/docs/transformers/model_doc/wav2vec2)**（来自 Facebook AI），与 Alexei Baevski、Henry Zhou、Abdelrahman Mohamed、Michael Auli 合作发布了论文[wav2vec 2.0: A Framework for Self-Supervised Learning of Speech Representations](https://arxiv.org/abs/2006.11477)。

1.  **[Wav2Vec2-BERT](https://huggingface.co/docs/transformers/main/model_doc/wav2vec2-bert)**（来自 Meta AI），与 Seamless Communication 团队合作发布了论文[Seamless: Multilingual Expressive and Streaming Speech Translation](https://ai.meta.com/research/publications/seamless-multilingual-expressive-and-streaming-speech-translation/)。

1.  **[WavLM](https://huggingface.co/docs/transformers/model_doc/wavlm)**（来自微软研究院），与 Sanyuan Chen、Chengyi Wang、Zhengyang Chen、Yu Wu、Shujie Liu、Zhuo Chen、Jinyu Li、Naoyuki Kanda、Takuya Yoshioka、Xiong Xiao、Jian Wu、Long Zhou、Shuo Ren、Yanmin Qian、Yao Qian、Jian Wu、Michael Zeng、Furu Wei 合作发布了论文[WavLM: Large-Scale Self-Supervised Pre-Training for Full Stack Speech Processing](https://arxiv.org/abs/2110.13900)。

1.  **[Whisper](https://huggingface.co/docs/transformers/model_doc/whisper)**（来自 OpenAI），与 Alec Radford、Jong Wook Kim、Tao Xu、Greg Brockman、Christine McLeavey、Ilya Sutskever 合作发布了论文[Robust Speech Recognition via Large-Scale Weak Supervision](https://cdn.openai.com/papers/whisper.pdf)。

1.  **[XLM](https://huggingface.co/docs/transformers/model_doc/xlm)**（来自 Facebook），与 Guillaume Lample 和 Alexis Conneau 合作发布了论文[Cross-lingual Language Model Pretraining](https://arxiv.org/abs/1901.07291)。

1.  **[XLM-RoBERTa](https://huggingface.co/docs/transformers/model_doc/xlm-roberta)**（来自 Facebook AI），与 Alexis Conneau、Kartikay Khandelwal、Naman Goyal、Vishrav Chaudhary、Guillaume Wenzek、Francisco Guzmán、Edouard Grave、Myle Ott、Luke Zettlemoyer 和 Veselin Stoyanov 合作发布了论文[Unsupervised Cross-lingual Representation Learning at Scale](https://arxiv.org/abs/1911.02116)。

1.  **[YOLOS](https://huggingface.co/docs/transformers/model_doc/yolos)**（来自华中科技大学）是由 Yuxin Fang、Bencheng Liao、Xinggang Wang、Jiemin Fang、Jiyang Qi、Rui Wu、Jianwei Niu、Wenyu Liu 等人发表的论文[You Only Look at One Sequence: Rethinking Transformer in Vision through Object Detection](https://arxiv.org/abs/2106.00666)发布的。
