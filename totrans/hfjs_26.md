# 接口：TextGenerationStreamDetails

> 原始文本：[`huggingface.co/docs/huggingface.js/inference/interfaces/TextGenerationStreamDetails`](https://huggingface.co/docs/huggingface.js/inference/interfaces/TextGenerationStreamDetails)

## 属性

### 最佳序列

• `可选` `best_of_sequences`: `TextGenerationStreamBestOfSequence`[]

在使用 `best_of` 参数时的额外序列

#### 定义于

[inference/src/tasks/nlp/textGenerationStream.ts:66](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L66)

* * *

### 完成原因

• `finish_reason`: `TextGenerationStreamFinishReason`

生成完成原因

#### 定义于

[inference/src/tasks/nlp/textGenerationStream.ts:56](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L56)

* * *

### 生成的令牌

• `generated_tokens`: `number`

生成的令牌数量

#### 定义于

[inference/src/tasks/nlp/textGenerationStream.ts:58](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L58)

* * *

### 预填

• `prefill`: `TextGenerationStreamPrefillToken`[]

提示令牌

#### 定义于

[inference/src/tasks/nlp/textGenerationStream.ts:62](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L62)

* * *

### 种子

• `可选` `seed`: `number`

如果启用了抽样，则抽样种子

#### 定义于

[inference/src/tasks/nlp/textGenerationStream.ts:60](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L60)

* * *

### 令牌

• `tokens`: `TextGenerationStreamToken`[]

#### 定义于

[inference/src/tasks/nlp/textGenerationStream.ts:64](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L64)
