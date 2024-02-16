# 🤗 Hugging Face 推理端点

> 原始文本：[`huggingface.co/docs/huggingface.js/inference/README`](https://huggingface.co/docs/huggingface.js/inference/README)

由 Typescript 支持的 Hugging Face 推理端点 API 包装器。了解有关推理端点的更多信息，请访问[Hugging Face](https://huggingface.co/inference-endpoints)。它可以与[无服务器](https://huggingface.co/docs/api-inference/index)和[专用](https://huggingface.co/docs/inference-endpoints/index)端点一起使用。

您还可以尝试一个实时的[交互式笔记本](https://observablehq.com/@huggingface/hello-huggingface-js-inference)，在[hf.co/huggingfacejs](https://huggingface.co/huggingfacejs)上查看一些演示，或观看一个[Scrimba 教程，解释推理端点的工作原理](https://scrimba.com/scrim/cod8248f5adfd6e129582c523)。

## 开始

### 安装

#### 节点

```py
npm install @huggingface/inference

pnpm add @huggingface/inference

yarn add @huggingface/inference
```

#### Deno

```py
// esm.sh
import { HfInference } from "https://esm.sh/@huggingface/inference"
// or npm:
import { HfInference } from "npm:@huggingface/inference"
```

### 初始化

```py
import { HfInference } from '@huggingface/inference'

const hf = new HfInference('your access token')
```

❗**重要提示：**使用访问令牌是可选的，但最终您将受到速率限制。加入[Hugging Face](https://huggingface.co/join)，然后访问[访问令牌](https://huggingface.co/settings/tokens)以**免费**生成您的访问令牌。

您的访问令牌应保持私密。如果您需要在前端应用程序中保护它，请建议设置一个存储访问令牌的代理服务器。

#### 树摇

您可以直接从模块中导入您需要的函数，而不是使用`HfInference`类。

```py
import { textGeneration } from "@huggingface/inference";

await textGeneration({
  accessToken: "hf_...",
  model: "model_or_endpoint",
  inputs: ...,
  parameters: ...
})
```

这将使您的捆绑器进行树摇。

## 自然语言处理

### 填充掩码

尝试用缺失的单词（确切地说是标记）填补一个空白。

```py
await hf.fillMask({
  model: 'bert-base-uncased',
  inputs: '[MASK] world!'
})
```

### 摘要

将较长的文本总结为较短的文本。请注意，一些模型具有输入的最大长度。

```py
await hf.summarization({
  model: 'facebook/bart-large-cnn',
  inputs:
    'The tower is 324 metres (1,063 ft) tall, about the same height as an 81-storey building, and the tallest structure in Paris. Its base is square, measuring 125 metres (410 ft) on each side. During its construction, the Eiffel Tower surpassed the Washington Monument to become the tallest man-made structure in the world, a title it held for 41 years until the Chrysler Building in New York City was finished in 1930.',
  parameters: {
    max_length: 100
  }
})
```

### 问答

根据您提供的上下文回答问题。

```py
await hf.questionAnswering({
  model: 'deepset/roberta-base-squad2',
  inputs: {
    question: 'What is the capital of France?',
    context: 'The capital of France is Paris.'
  }
})
```

### 表格问答

```py
await hf.tableQuestionAnswering({
  model: 'google/tapas-base-finetuned-wtq',
  inputs: {
    query: 'How many stars does the transformers repository have?',
    table: {
      Repository: ['Transformers', 'Datasets', 'Tokenizers'],
      Stars: ['36542', '4512', '3934'],
      Contributors: ['651', '77', '34'],
      'Programming language': ['Python', 'Python', 'Rust, Python and NodeJS']
    }
  }
})
```

### 文本分类

通常用于情感分析，此方法将为给定文本分配标签，并附带该标签的概率分数。

```py
await hf.textClassification({
  model: 'distilbert-base-uncased-finetuned-sst-2-english',
  inputs: 'I like you. I love you.'
})
```

### 文本生成

从输入提示生成文本。

[演示](https://huggingface.co/spaces/huggingfacejs/streaming-text-generation)

```py
await hf.textGeneration({
  model: 'gpt2',
  inputs: 'The answer to the universe is'
})

for await (const output of hf.textGenerationStream({
  model: "google/flan-t5-xxl",
  inputs: 'repeat "one two three four"',
  parameters: { max_new_tokens: 250 }
})) {
  console.log(output.token.text, output.generated_text);
}
```

### 标记分类

用于句子解析，无论是语法还是命名实体识别（NER），以理解文本中包含的关键字。

```py
await hf.tokenClassification({
  model: 'dbmdz/bert-large-cased-finetuned-conll03-english',
  inputs: 'My name is Sarah Jessica Parker but you can call me Jessica'
})
```

### 翻译

将文本从一种语言转换为另一种语言。

```py
await hf.translation({
  model: 't5-base',
  inputs: 'My name is Wolfgang and I live in Berlin'
})

await hf.translation({
  model: 'facebook/mbart-large-50-many-to-many-mmt',
  inputs: textToTranslate,
  parameters: {
		"src_lang": "en_XX",
		"tgt_lang": "fr_XX"
	}
})
```

### 零样本分类

检查输入文本与您提供的标签集合的匹配程度。

```py
await hf.zeroShotClassification({
  model: 'facebook/bart-large-mnli',
  inputs: [
    'Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!'
  ],
  parameters: { candidate_labels: ['refund', 'legal', 'faq'] }
})
```

### 对话

此任务对应于任何类似于聊天机器人的结构。模型往往具有较短的 max_length，因此在使用给定模型时，请谨慎检查是否需要长距离依赖。

```py
await hf.conversational({
  model: 'microsoft/DialoGPT-large',
  inputs: {
    past_user_inputs: ['Which movie is the best ?'],
    generated_responses: ['It is Die Hard for sure.'],
    text: 'Can you explain why ?'
  }
})
```

### 句子相似度

计算一个文本与其他句子列表之间的语义相似度。

```py
await hf.sentenceSimilarity({
  model: 'sentence-transformers/paraphrase-xlm-r-multilingual-v1',
  inputs: {
    source_sentence: 'That is a happy person',
    sentences: [
      'That is a happy dog',
      'That is a very happy person',
      'Today is a sunny day'
    ]
  }
})
```

## 音频

### 自动语音识别

从音频文件转录语音。

[演示](https://huggingface.co/spaces/huggingfacejs/speech-recognition-vue)

```py
await hf.automaticSpeechRecognition({
  model: 'facebook/wav2vec2-large-960h-lv60-self',
  data: readFileSync('test/sample1.flac')
})
```

### 音频分类

为给定音频分配标签，并附带该标签的概率分数。

[演示](https://huggingface.co/spaces/huggingfacejs/audio-classification-vue)

```py
await hf.audioClassification({
  model: 'superb/hubert-large-superb-er',
  data: readFileSync('test/sample1.flac')
})
```

### 文本转语音

从文本输入生成自然声音。

[交互式教程](https://scrimba.com/scrim/co8da4d23b49b648f77f4848a?pl=pkVnrP7uP)

```py
await hf.textToSpeech({
  model: 'espnet/kan-bayashi_ljspeech_vits',
  inputs: 'Hello world!'
})
```

### 音频到音频

从输入音频中输出一个或多个生成的音频，通常用于语音增强和源分离。

```py
await hf.audioToAudio({
  model: 'speechbrain/sepformer-wham',
  data: readFileSync('test/sample1.flac')
})
```

## 计算机视觉

### 图像分类

为给定图像分配标签，并附带该标签的概率分数。

[演示](https://huggingface.co/spaces/huggingfacejs/image-classification-vue)

```py
await hf.imageClassification({
  data: readFileSync('test/cheetah.png'),
  model: 'google/vit-base-patch16-224'
})
```

### 目标检测

检测图像中的对象，并返回带有相应边界框和概率分数的标签。

[演示](https://huggingface.co/spaces/huggingfacejs/object-detection-vue)

```py
await hf.objectDetection({
  data: readFileSync('test/cats.png'),
  model: 'facebook/detr-resnet-50'
})
```

### 图像分割

检测图像中的段落，并返回带有相应边界框和概率分数的标签。

```py
await hf.imageSegmentation({
  data: readFileSync('test/cats.png'),
  model: 'facebook/detr-resnet-50-panoptic'
})
```

### 图像到文本

从给定图像输出文本，通常用于字幕或光学字符识别。

```py
await hf.imageToText({
  data: readFileSync('test/cats.png'),
  model: 'nlpconnect/vit-gpt2-image-captioning'
})
```

### 文本到图像

从文本提示创建图像。

[演示](https://huggingface.co/spaces/huggingfacejs/image-to-text)

```py
await hf.textToImage({
  inputs: 'award winning high resolution photo of a giant tortoise/((ladybird)) hybrid, [trending on artstation]',
  model: 'stabilityai/stable-diffusion-2',
  parameters: {
    negative_prompt: 'blurry',
  }
})
```

### 图像到图像

图像到图像是将源图像转换为与目标图像或目标图像域特征相匹配的任务。

[交互式教程](https://scrimba.com/scrim/co4834bf9a91cc81cfab07969?pl=pkVnrP7uP)

```py
await hf.imageToImage({
  inputs: new Blob([readFileSync("test/stormtrooper_depth.png")]),
  parameters: {
    prompt: "elmo's lecture",
  },
  model: "lllyasviel/sd-controlnet-depth",
});
```

### 零射击图像分类

检查输入图像与您提供的标签集合的匹配程度。

```py
await hf.zeroShotImageClassification({
  model: 'openai/clip-vit-large-patch14-336',
  inputs: {
    image: await (await fetch('https://placekitten.com/300/300')).blob()
  },  
  parameters: {
    candidate_labels: ['cat', 'dog']
  }
})
```

## 多模态

### 特征提取

此任务读取一些文本并输出通常作为语义数据库/语义搜索的一部分消耗的原始浮点值。

```py
await hf.featureExtraction({
  model: "sentence-transformers/distilbert-base-nli-mean-tokens",
  inputs: "That is a happy person",
});
```

### 视觉问答

视觉问答是根据图像回答开放式问题的任务。它们输出自然语言回答以回答自然语言问题。

[演示](https://huggingface.co/spaces/huggingfacejs/doc-vis-qa)

```py
await hf.visualQuestionAnswering({
  model: 'dandelin/vilt-b32-finetuned-vqa',
  inputs: {
    question: 'How many cats are lying down?',
    image: await (await fetch('https://placekitten.com/300/300')).blob()
  }
})
```

### 文档问答

文档问答模型接受（文档，问题）对作为输入，并以自然语言返回答案。

[演示](https://huggingface.co/spaces/huggingfacejs/doc-vis-qa)

```py
await hf.documentQuestionAnswering({
  model: 'impira/layoutlm-document-qa',
  inputs: {
    question: 'Invoice number?',
    image: await (await fetch('https://huggingface.co/spaces/impira/docquery/resolve/2359223c1837a7587402bda0f2643382a6eefeab/invoice.png')).blob(),
  }
})
```

## 表格

### 表格回归

表格回归是根据一组属性预测数值的任务。

```py
await hf.tabularRegression({
  model: "scikit-learn/Fish-Weight",
  inputs: {
    data: {
      "Height": ["11.52", "12.48", "12.3778"],
      "Length1": ["23.2", "24", "23.9"],
      "Length2": ["25.4", "26.3", "26.5"],
      "Length3": ["30", "31.2", "31.1"],
      "Species": ["Bream", "Bream", "Bream"],
      "Width": ["4.02", "4.3056", "4.6961"]
    },
  },
})
```

### 表格分类

表格分类是根据一组属性对目标类别（一组）进行分类的任务。

```py
await hf.tabularClassification({
  model: "vvmnnnkv/wine-quality",
  inputs: {
    data: {
      "fixed_acidity": ["7.4", "7.8", "10.3"],
      "volatile_acidity": ["0.7", "0.88", "0.32"],
      "citric_acid": ["0", "0", "0.45"],
      "residual_sugar": ["1.9", "2.6", "6.4"],
      "chlorides": ["0.076", "0.098", "0.073"],
      "free_sulfur_dioxide": ["11", "25", "5"],
      "total_sulfur_dioxide": ["34", "67", "13"],
      "density": ["0.9978", "0.9968", "0.9976"],
      "pH": ["3.51", "3.2", "3.23"],
      "sulphates": ["0.56", "0.68", "0.82"],
      "alcohol": ["9.4", "9.8", "12.6"]
    },
  },
})
```

## 自定义调用

适用于具有自定义参数/输出的模型。

```py
await hf.request({
  model: 'my-custom-model',
  inputs: 'hello world',
  parameters: {
    custom_param: 'some magic',
  }
})

// Custom streaming call, for models with custom parameters / outputs
for await (const output of hf.streamingRequest({
  model: 'my-custom-model',
  inputs: 'hello world',
  parameters: {
    custom_param: 'some magic',
  }
})) {
  ...
}
```

## 自定义推理端点

了解更多关于使用您自己的推理端点的信息[这里](https://hf.co/docs/inference-endpoints/)

```py
const gpt2 = hf.endpoint('https://xyz.eu-west-1.aws.endpoints.huggingface.cloud/gpt2');
const { generated_text } = await gpt2.textGeneration({inputs: 'The answer to the universe is'});
```

## 运行测试

```py
HF_TOKEN="your access token" pnpm run test
```

## 寻找合适的模型

我们有一个信息丰富的文档项目称为[Tasks](https://huggingface.co/tasks)，列出每个任务的可用模型并详细解释每个任务的工作原理。

它还包含演示，示例输出和其他资源，以便您深入了解 ML 方面的内容。
