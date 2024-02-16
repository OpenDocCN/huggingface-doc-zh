# 接口：Options

> 原始文本：[`huggingface.co/docs/huggingface.js/inference/interfaces/Options`](https://huggingface.co/docs/huggingface.js/inference/interfaces/Options)

## 属性

### dont _ load _ model

• `Optional` `dont_load_model`: `boolean`

（默认值：false）。布尔值。如果模型尚未可用，则不加载模型。

#### 定义在

[inference/src/types.ts:15](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/types.ts#L15)

* * *

### fetch

• `Optional` `fetch`: (`input`: `RequestInfo` | `URL`, `init?`: `RequestInit`) => `Promise`<`Response`>

#### 类型声明

▸ (`input`, `init?`): `Promise`<`Response`>

自定义的 fetch 函数，用于替代默认的 fetch 函数，例如使用代理或编辑标头。

##### 参数

| 名称 | 类型 |
| :-- | :-- |
| `input` | `RequestInfo` &#124; `URL` |
| `init?` | `RequestInit` |

##### 返回

`Promise`<`Response`>

#### 定义在

[inference/src/types.ts:28](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/types.ts#L28)

* * *

### includeCredentials

• `Optional` `includeCredentials`: `string` | `boolean`

（默认值：“same-origin”）。字符串 | 布尔值。用于请求的凭据。如果是字符串，将直接传递。如果是布尔值，true 将是“include”，false 将不发送任何凭据。

#### 定义在

[inference/src/types.ts:37](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/types.ts#L37)

* * *

### retry _ on _ error

• `Optional` `retry_on_error`: `boolean`

（默认值：true）布尔值。如果请求返回 503 并且 wait_for_model 设置为 false，则将使用相同的参数重试请求，但 wait_for_model 设置为 true。

#### 定义在

[inference/src/types.ts:7](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/types.ts#L7)

* * *

### signal

• `Optional` `signal`: `AbortSignal`

用于请求中断的 Abort Controller 信号。

#### 定义在

[inference/src/types.ts:32](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/types.ts#L32)

* * *

### use _ cache

• `Optional` `use_cache`: `boolean`

（默认值：true）。在推断端点（无服务器）上有一个缓存层，以加快我们已经看到的请求。大多数模型可以直接使用这些结果，因为模型是确定性的（意味着结果无论如何都将相同）。但是，如果使用非确定性模型，可以设置此参数以防止使用缓存机制，从而导致进行真正的新查询。

#### 定义在

[inference/src/types.ts:11](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/types.ts#L11)

* * *

### use _ gpu

• `Optional` `use_gpu`: `boolean`

（默认值：false）。布尔值，用 GPU 而不是 CPU 进行推断（至少需要 Startup 计划）。

#### 定义在

[inference/src/types.ts:19](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/types.ts#L19)

* * *

### wait _ for _ model

• `Optional` `wait_for_model`: `boolean`

（默认值：false）布尔值。如果模型尚未准备好，等待它而不是收到 503。它限制了获取推断结果所需的请求数量。建议只在收到 503 错误后将此标志设置为 true，因为它将限制应用程序中的挂起到已知位置。

#### 定义在

[inference/src/types.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/types.ts#L24)
