# 类: InvalidApiResponseFormatError

> 原始文本: [`huggingface.co/docs/huggingface.js/hub/classes/InvalidApiResponseFormatError`](https://huggingface.co/docs/huggingface.js/hub/classes/InvalidApiResponseFormatError)

## 层次结构

+   `Error`

    ↳ **`InvalidApiResponseFormatError`**

## 构造函数

### 构造函数

• `new InvalidApiResponseFormatError`(`message?`)

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `message?` | `string` |

#### 继承自

Error.constructor

#### 定义在

文档内部/node_modules/.pnpm/typescript@4.9.5/node_modules/typescript/lib/lib.es5.d.ts:1059

• `new InvalidApiResponseFormatError`(`message?`, `options?`)

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `message?` | `string` |
| `options?` | `ErrorOptions` |

#### 继承自

Error.constructor

#### 定义在

文档内部/node_modules/.pnpm/typescript@4.9.5/node_modules/typescript/lib/lib.es2022.error.d.ts:30

## 属性

### 原因

• `可选` `cause`: `unknown`

#### 继承自

Error.cause

#### 定义在

文档内部/node_modules/.pnpm/typescript@4.9.5/node_modules/typescript/lib/lib.es2022.error.d.ts:26

* * *

### 消息

• `message`: `string`

#### 继承自

Error.message

#### 定义在

文档内部/node_modules/.pnpm/typescript@4.9.5/node_modules/typescript/lib/lib.es5.d.ts:1054

* * *

### 名称

• `名称`: `string`

#### 继承自

Error.name

#### 定义在

文档内部/node_modules/.pnpm/typescript@4.9.5/node_modules/typescript/lib/lib.es5.d.ts:1053

* * *

### 堆栈

• `可选` `stack`: `string`

#### 继承自

Error.stack

#### 定义在

文档内部/node_modules/.pnpm/typescript@4.9.5/node_modules/typescript/lib/lib.es5.d.ts:1055

* * *

### prepareStackTrace

▪ `静态` `可选` `prepareStackTrace`: (`err`: `Error`, `stackTraces`: `CallSite`[]) => `any`

#### 类型声明

▸ (`err`, `stackTraces`): `any`

格式化堆栈跟踪的可选覆盖

**`查看`**

[`v8.dev/docs/stack-trace-api#customizing-stack-traces`](https://v8.dev/docs/stack-trace-api#customizing-stack-traces)

##### 参数

| 名称 | 类型 |
| :-- | :-- |
| `err` | `Error` |
| `stackTraces` | `CallSite`[] |

##### 返回

`any`

#### 继承自

Error.prepareStackTrace

#### 定义在

hub/node_modules/.pnpm/@types+node@18.13.0/node_modules/@types/node/globals.d.ts:11

* * *

### stackTraceLimit

▪ `静态` `stackTraceLimit`: `number`

#### 继承自

Error.stackTraceLimit

#### 定义在

hub/node_modules/.pnpm/@types+node@18.13.0/node_modules/@types/node/globals.d.ts:13

## 方法

### captureStackTrace

▸ `静态` `captureStackTrace`(`targetObject`, `constructorOpt?`): `void`

在目标对象上创建 .stack 属性

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `targetObject` | `object` |
| `constructorOpt?` | `Function` |

#### 返回

`void`

#### 继承自

Error.captureStackTrace

#### 定义在

hub/node_modules/.pnpm/@types+node@18.13.0/node_modules/@types/node/globals.d.ts:4
