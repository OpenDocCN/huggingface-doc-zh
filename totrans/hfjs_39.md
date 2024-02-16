# 类：HubApiError

> 原文: [`huggingface.co/docs/huggingface.js/hub/classes/HubApiError`](https://huggingface.co/docs/huggingface.js/hub/classes/HubApiError)

当调用 Hugging Face Hub 失败时抛出的错误。

## 层次结构

+   `错误`

    ↳ **`HubApiError`**

## 构造函数

### 构造函数

• `new HubApiError`(`url`, `statusCode`, `requestId?`, `message?`)

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `url` | `字符串` |
| `statusCode` | `数字` |
| `requestId?` | `字符串` |
| `message?` | `字符串` |

#### 覆盖

Error.constructor

#### 定义于

[hub/src/error.ts:33](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/error.ts#L33)

## 属性

### cause

• `可选` `cause`: `未知`

#### 继承自

Error.cause

#### 定义于

文档内部/node_modules/.pnpm/typescript@4.9.5/node_modules/typescript/lib/lib.es2022.error.d.ts:26

* * *

### 数据

• `可选` `数据`: `JsonObject`

#### 定义于

[hub/src/error.ts:31](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/error.ts#L31)

* * *

### message

• `message`: `字符串`

#### 继承自

Error.message

#### 定义于

文档内部/node_modules/.pnpm/typescript@4.9.5/node_modules/typescript/lib/lib.es5.d.ts:1054

* * *

### 名称

• `名称`: `字符串`

#### 继承自

Error.name

#### 定义于

文档内部/node_modules/.pnpm/typescript@4.9.5/node_modules/typescript/lib/lib.es5.d.ts:1053

* * *

### 请求 ID

• `可选` `请求 ID`: `字符串`

#### 定义于

[hub/src/error.ts:30](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/error.ts#L30)

* * *

### stack

• `可选` `stack`: `字符串`

#### 继承自

Error.stack

#### 定义于

文档内部/node_modules/.pnpm/typescript@4.9.5/node_modules/typescript/lib/lib.es5.d.ts:1055

* * *

### statusCode

• `statusCode`: `数字`

#### 定义于

[hub/src/error.ts:28](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/error.ts#L28)

* * *

### url

• `url`: `字符串`

#### 定义于

[hub/src/error.ts:29](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/error.ts#L29)

* * *

### prepareStackTrace

▪ `静态` `可选` `prepareStackTrace`: (`err`: `错误`, `stackTraces`: `CallSite`[]) => `任意`

#### 类型声明

▸ (`err`, `stackTraces`): `任意`

格式化堆栈跟踪的可选覆盖

**`查看`**

[`v8.dev/docs/stack-trace-api#customizing-stack-traces`](https://v8.dev/docs/stack-trace-api#customizing-stack-traces)

##### 参数

| 名称 | 类型 |
| :-- | :-- |
| `err` | `错误` |
| `stackTraces` | `CallSite`[] |

##### 返回

`任意`

#### 继承自

Error.prepareStackTrace

#### 定义于

hub/node_modules/.pnpm/@types+node@18.13.0/node_modules/@types/node/globals.d.ts:11

* * *

### stackTraceLimit

▪ `静态` `stackTraceLimit`: `数字`

#### 继承自

Error.stackTraceLimit

#### 定义于

hub/node_modules/.pnpm/@types+node@18.13.0/node_modules/@types/node/globals.d.ts:13

## 方法

### captureStackTrace

▸ `静态` `captureStackTrace`(`targetObject`, `constructorOpt?`): `void`

在目标对象上创建 .stack 属性

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `targetObject` | `对象` |
| `constructorOpt?` | `函数` |

#### 返回

`void`

#### 继承自

Error.captureStackTrace

#### 定义于

hub/node_modules/.pnpm/@types+node@18.13.0/node_modules/@types/node/globals.d.ts:4
