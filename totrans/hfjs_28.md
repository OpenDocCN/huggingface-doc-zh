# 接口：TextGenerationStreamPrefillToken

> 原始文本: [https://huggingface.co/docs/huggingface.js/inference/interfaces/TextGenerationStreamPrefillToken](https://huggingface.co/docs/huggingface.js/inference/interfaces/TextGenerationStreamPrefillToken)

## 属性

### id

• `id`: `number`

来自模型分词器的标记ID

#### 定义在

[推理/src/tasks/nlp/textGenerationStream.ts:21](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L21)

* * *

### 对数概率

• `可选` `logprob`: `number`

对数概率 可选，因为第一个标记的对数概率无法计算

#### 定义在

[推理/src/tasks/nlp/textGenerationStream.ts:28](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L28)

* * *

### 文本

• `text`: `string`

标记文本

#### 定义在

[推理/src/tasks/nlp/textGenerationStream.ts:23](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L23)
