# 接口：TextGenerationStreamOutput

> 原始文本：[https://huggingface.co/docs/huggingface.js/inference/interfaces/TextGenerationStreamOutput](https://huggingface.co/docs/huggingface.js/inference/interfaces/TextGenerationStreamOutput)

## 属性

### 细节

• `details`: `null` | [`TextGenerationStreamDetails`](TextGenerationStreamDetails)

生成细节 仅在生成完成时可用

#### 定义在

[inference/src/tasks/nlp/textGenerationStream.ts:81](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L81)

* * *

### 生成的文本

• `generated_text`: `null` | `string`

完整生成的文本 仅在生成完成时可用

#### 定义在

[inference/src/tasks/nlp/textGenerationStream.ts:76](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L76)

* * *

### 令牌

• `token`: [`TextGenerationStreamToken`](TextGenerationStreamToken)

逐个生成的令牌

#### 定义在

[inference/src/tasks/nlp/textGenerationStream.ts:71](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/textGenerationStream.ts#L71)
