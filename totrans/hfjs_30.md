# 接口：TokenClassificationOutputValue

> 原文链接：[`huggingface.co/docs/huggingface.js/inference/interfaces/TokenClassificationOutputValue`](https://huggingface.co/docs/huggingface.js/inference/interfaces/TokenClassificationOutputValue)

## 属性

### 结束

• `end`: `number`

答案所在的字符串偏移位置。有助于消除单词多次出现的歧义。

#### 定义在

[inference/src/tasks/nlp/tokenClassification.ts:33](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tokenClassification.ts#L33)

* * *

### 实体 _ 组

• `entity_group`: `string`

被识别实体的类型（特定模型）。

#### 定义在

[inference/src/tasks/nlp/tokenClassification.ts:37](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tokenClassification.ts#L37)

* * *

### 分数

• `score`: `number`

实体被识别的可能性有多大。

#### 定义在

[inference/src/tasks/nlp/tokenClassification.ts:41](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tokenClassification.ts#L41)

* * *

### 开始

• `start`: `number`

答案所在的字符串偏移位置。有助于消除单词多次出现的歧义。

#### 定义在

[inference/src/tasks/nlp/tokenClassification.ts:45](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tokenClassification.ts#L45)

* * *

### 单词

• `word`: `string`

被捕获的字符串

#### 定义在

[inference/src/tasks/nlp/tokenClassification.ts:49](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tokenClassification.ts#L49)
