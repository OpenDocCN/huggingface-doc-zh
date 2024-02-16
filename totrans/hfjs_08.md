# 类：InferenceOutputError

> 原始文本：[https://huggingface.co/docs/huggingface.js/inference/classes/InferenceOutputError](https://huggingface.co/docs/huggingface.js/inference/classes/InferenceOutputError)

## 层次结构

+   `TypeError`

    ↳ **`InferenceOutputError`**

## 构造函数

### 构造函数

• `new InferenceOutputError`(`message`)

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `message` | `string` |

#### 覆盖

TypeError.constructor

#### 在以下位置定义

[inference/src/lib/InferenceOutputError.ts:2](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/lib/InferenceOutputError.ts#L2)

## 属性

### 原因

• `Optional` `cause`: `unknown`

#### 继承自

TypeError.cause

#### 在以下位置定义

doc-internal/node_modules/.pnpm/[typescript@4.9.5](mailto:typescript@4.9.5)/node_modules/typescript/lib/lib.es2022.error.d.ts:26

* * *

### 消息

• `message`: `string`

#### 继承自

TypeError.message

#### 在以下位置定义

doc-internal/node_modules/.pnpm/[typescript@4.9.5](mailto:typescript@4.9.5)/node_modules/typescript/lib/lib.es5.d.ts:1054

* * *

### 名称

• `name`: `string`

#### 继承自

TypeError.name

#### 在以下位置定义

doc-internal/node_modules/.pnpm/[typescript@4.9.5](mailto:typescript@4.9.5)/node_modules/typescript/lib/lib.es5.d.ts:1053

* * *

### 堆栈

• `Optional` `stack`: `string`

#### 继承自

TypeError.stack

#### 在以下位置定义

doc-internal/node_modules/.pnpm/[typescript@4.9.5](mailto:typescript@4.9.5)/node_modules/typescript/lib/lib.es5.d.ts:1055

* * *

### prepareStackTrace

▪ `Static` `Optional` `prepareStackTrace`: (`err`: `Error`, `stackTraces`: `CallSite`[]) => `any`

#### 类型声明

▸ (`err`, `stackTraces`): `any`

用于格式化堆栈跟踪的可选覆盖

**`查看`**

[https://v8.dev/docs/stack-trace-api#customizing-stack-traces](https://v8.dev/docs/stack-trace-api#customizing-stack-traces)

##### 参数

| 名称 | 类型 |
| :-- | :-- |
| `err` | `Error` |
| `stackTraces` | `CallSite`[] |

##### 返回

`any`

#### 继承自

TypeError.prepareStackTrace

#### 在以下位置定义

推断/ node_modules/.pnpm/@[types+node@18.13.0](mailto:types+node@18.13.0)/node_modules/@types/node/globals.d.ts:11

* * *

### stackTraceLimit

▪ `Static` `stackTraceLimit`: `number`

#### 继承自

TypeError.stackTraceLimit

#### 在以下位置定义

推断/ node_modules/.pnpm/@[types+node@18.13.0](mailto:types+node@18.13.0)/node_modules/@types/node/globals.d.ts:13

## 方法

### captureStackTrace

▸ `Static` `captureStackTrace`(`targetObject`, `constructorOpt?`): `void`

在目标对象上创建 .stack 属性

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `targetObject` | `object` |
| `constructorOpt?` | `Function` |

#### 返回

`void`

#### 继承自

TypeError.captureStackTrace

#### 在以下位置定义

推断/ node_modules/.pnpm/@[types+node@18.13.0](mailto:types+node@18.13.0)/node_modules/@types/node/globals.d.ts:4
