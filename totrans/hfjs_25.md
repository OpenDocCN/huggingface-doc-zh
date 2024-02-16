# 接口: TextGenerationStreamBestOfSequence

> 原始文本: [https://huggingface.co/docs/huggingface.js/inference/interfaces/TextGenerationStreamBestOfSequence](https://huggingface.co/docs/huggingface.js/inference/interfaces/TextGenerationStreamBestOfSequence)

## 属性

### 完成_原因

• `finish_reason`: [`TextGenerationStreamFinishReason`](../modules#textgenerationstreamfinishreason)

生成完成原因

#### 定义在

[inference/src/tasks/nlp/textGenerationStream.ts:35](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L35)

* * *

### 生成的文本

• `generated_text`: `string`

生成的文本

#### 定义在

[inference/src/tasks/nlp/textGenerationStream.ts:33](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L33)

* * *

### 生成的_标记

• `generated_tokens`: `number`

生成的标记数

#### 定义在

[inference/src/tasks/nlp/textGenerationStream.ts:37](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L37)

* * *

### 预填

• `prefill`: [`TextGenerationStreamPrefillToken`](TextGenerationStreamPrefillToken)[]

提示标记

#### 定义在

[inference/src/tasks/nlp/textGenerationStream.ts:41](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L41)

* * *

### 种子

• `可选` `seed`: `number`

如果启用了抽样，则抽样种子

#### 定义在

[inference/src/tasks/nlp/textGenerationStream.ts:39](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L39)

* * *

### 标记

• `tokens`: [`TextGenerationStreamToken`](TextGenerationStreamToken)[]

生成的标记

#### 定义在

[inference/src/tasks/nlp/textGenerationStream.ts:43](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L43)
