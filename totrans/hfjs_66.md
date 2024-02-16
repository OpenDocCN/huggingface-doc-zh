# 类: HfAgent

> 原文链接: [`huggingface.co/docs/huggingface.js/agents/classes/HfAgent`](https://huggingface.co/docs/huggingface.js/agents/classes/HfAgent)

## 构造函数

### 构造函数

• `新 HfAgent`(`访问令牌?`, `LLM?`, `工具?`)

#### 参数

| 名称 | 类型 | 默认值 |
| :-- | :-- | :-- |
| `访问令牌` | `字符串` | `""` |
| `LLM?` | `LLM` | `未定义` |
| `工具?` | `工具`[] | `未定义` |

#### 定义在

[HfAgent.ts:14](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/HfAgent.ts#L14)

## 属性

### 访问令牌

• `私有` `访问令牌`: `字符串`

#### 定义在

[HfAgent.ts:10](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/HfAgent.ts#L10)

* * *

### llm

• `私有` `llm`: `LLM`

#### 定义在

[HfAgent.ts:11](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/HfAgent.ts#L11)

* * *

### 工具

• `私有` `工具`: `工具`[]

#### 定义在

[HfAgent.ts:12](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/HfAgent.ts#L12)

## 方法

### 评估代码

▸ `evaluateCode`(`代码`, `文件?`): `Promise`<`Update`[]>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `代码` | `字符串` |
| `文件?` | `文件列表` |

#### 返回

`Promise`<`Update`[]>

#### 定义在

[HfAgent.ts:31](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/HfAgent.ts#L31)

* * *

### 生成代码

▸ `generateCode`(`提示`, `文件?`): `Promise`<`字符串`>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `提示` | `字符串` |
| `文件?` | `文件列表` |

#### 返回

`Promise`<`字符串`>

#### 定义在

[HfAgent.ts:27](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/HfAgent.ts#L27)

* * *

### generatePrompt

▸ `generatePrompt`(`提示`, `文件?`): `字符串`

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `提示` | `字符串` |
| `文件?` | `文件列表` |

#### 返回

`字符串`

#### 定义在

[HfAgent.ts:20](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/HfAgent.ts#L20)

* * *

### 运行

▸ `运行`(`提示`, `文件?`): `Promise`<`Update`[]>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `提示` | `字符串` |
| `files?` | `FileList` |

#### 返回

`Promise`<`Update`[]>

#### 定义在

[HfAgent.ts:51](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/HfAgent.ts#L51)
