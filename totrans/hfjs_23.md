# 接口: TableQuestionAnsweringOutput

> 原始文本: [https://huggingface.co/docs/huggingface.js/inference/interfaces/TableQuestionAnsweringOutput](https://huggingface.co/docs/huggingface.js/inference/interfaces/TableQuestionAnsweringOutput)

## 属性

### 聚合器

• `聚合器`: `字符串`

用于获取答案的聚合器

#### 定义在

[inference/src/tasks/nlp/tableQuestionAnswering.ts:22](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tableQuestionAnswering.ts#L22)

* * *

### 答案

• `答案`: `字符串`

明文答案

#### 定义在

[inference/src/tasks/nlp/tableQuestionAnswering.ts:26](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tableQuestionAnswering.ts#L26)

* * *

### 单元格

• `单元格`: `字符串`[]

单元格内容的坐标列表

#### 定义在

[inference/src/tasks/nlp/tableQuestionAnswering.ts:30](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tableQuestionAnswering.ts#L30)

* * *

### 坐标

• `坐标`: `数字`[][]

答案中引用的单元格的坐标列表

#### 定义在

[inference/src/tasks/nlp/tableQuestionAnswering.ts:34](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/nlp/tableQuestionAnswering.ts#L34)
