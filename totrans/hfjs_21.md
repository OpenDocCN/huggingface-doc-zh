# 接口: QuestionAnsweringOutput

> 原始文本: [https://huggingface.co/docs/huggingface.js/inference/interfaces/QuestionAnsweringOutput](https://huggingface.co/docs/huggingface.js/inference/interfaces/QuestionAnsweringOutput)

## 属性

### 答案

• `answer`: `string`

文本中的答案字符串。

#### 定义于

[inference/src/tasks/nlp/questionAnswering.ts:16](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/questionAnswering.ts#L16)

* * *

### 结束

• `end`: `number`

答案在上下文中结束的索引（字符串方式）。

#### 定义于

[inference/src/tasks/nlp/questionAnswering.ts:20](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/questionAnswering.ts#L20)

* * *

### 分数

• `score`: `number`

表示答案正确的可能性的浮点数

#### 定义于

[inference/src/tasks/nlp/questionAnswering.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/questionAnswering.ts#L24)

* * *

### 开始

• `start`: `number`

答案在上下文中开始的索引（字符串方式）。

#### 定义于

[inference/src/tasks/nlp/questionAnswering.ts:28](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/questionAnswering.ts#L28)
