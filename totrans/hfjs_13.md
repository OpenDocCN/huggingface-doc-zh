# 接口：BaseArgs

> 原始文本：[https://huggingface.co/docs/huggingface.js/inference/interfaces/BaseArgs](https://huggingface.co/docs/huggingface.js/inference/interfaces/BaseArgs)

## 属性

### accessToken

• `可选` `accessToken`: `字符串`

要使用的访问令牌。没有它，您将很快受到速率限制。

可以在hf.co/settings/token免费创建

#### 定义在

[推理/src/types.ts:48](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/types.ts#L48)

* * *

### 模型

• `可选` `model`: `字符串`

要使用的模型。可以是专用推理端点的完整URL。

如果未指定，将调用huggingface.co/api/tasks来获取任务的默认模型。

#### 定义在

[推理/src/types.ts:54](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/types.ts#L54)
