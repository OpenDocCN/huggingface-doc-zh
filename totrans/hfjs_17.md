# 接口: ImageSegmentationOutputValue

> 原始文本: [https://huggingface.co/docs/huggingface.js/inference/interfaces/ImageSegmentationOutputValue](https://huggingface.co/docs/huggingface.js/inference/interfaces/ImageSegmentationOutputValue)

## 属性

### 标签

• `label`: `string`

分割的类别标签（特定于模型）。

#### 定义于

[inference/src/tasks/cv/imageSegmentation.ts:16](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageSegmentation.ts#L16)

* * *

### 掩模

• `mask`: `string`

一个表示分割掩模的字符串（单通道黑白图像的base64字符串）。

#### 定义于

[inference/src/tasks/cv/imageSegmentation.ts:20](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageSegmentation.ts#L20)

* * *

### 分数

• `score`: `number`

表示检测到的对象属于给定类别的可能性的浮点数。

#### 定义于

[inference/src/tasks/cv/imageSegmentation.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/cv/imageSegmentation.ts#L24)
