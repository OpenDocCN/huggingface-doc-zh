# 接口: ObjectDetectionOutputValue

> 原文链接: [`huggingface.co/docs/huggingface.js/inference/interfaces/ObjectDetectionOutputValue`](https://huggingface.co/docs/huggingface.js/inference/interfaces/ObjectDetectionOutputValue)

## 属性

### 框

• `box`: `对象`

一个字典（带有键[xmin,ymin,xmax,ymax]），表示检测到的对象的边界框。

#### 类型声明

| 名称 | 类型 |
| :-- | :-- |
| `xmax` | `数字` |
| `xmin` | `数字` |
| `ymax` | `数字` |
| `ymin` | `数字` |

#### 定义在

[推断/src/tasks/cv/objectDetection.ts:16](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/objectDetection.ts#L16)

* * *

### 标签

• `label`: `字符串`

检测到的对象的类别标签（特定于模型）。

#### 定义在

[推断/src/tasks/cv/objectDetection.ts:25](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/objectDetection.ts#L25)

* * *

### 分数

• `score`: `数字`

一个浮点数，表示检测到的对象属于给定类的可能性有多大。

#### 定义在

[推断/src/tasks/cv/objectDetection.ts:30](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/objectDetection.ts#L30)
