# 接口: CommitParams

> 原文链接: [`huggingface.co/docs/huggingface.js/hub/interfaces/CommitParams`](https://huggingface.co/docs/huggingface.js/hub/interfaces/CommitParams)

## 属性

### abortSignal

• `可选` `abortSignal`: `AbortSignal`

#### 定义在

[hub/src/lib/commit.ts:83](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L83)

* * *

### 分支

• `可选` `branch`: `字符串`

**`默认`**

“main”

#### 定义在

[hub/src/lib/commit.ts:63](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L63)

* * *

### 凭证

• `可选` `credentials`: `凭证`

#### 定义在

[hub/src/lib/commit.ts:61](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L61)

* * *

### 描述

• `可选` `description`: `字符串`

#### 定义在

[hub/src/lib/commit.ts:58](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L58)

* * *

### 获取

• `可选` `fetch`: (`input`: `URL` | `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`>

#### 类型声明

▸ (`input`, `init?`): `Promise`<`Response`>

自定义 fetch 函数，用于替代默认函数，例如使用代理或编辑标头。

##### 参数

| 名称 | 类型 |
| :-- | :-- |
| `input` | `URL` &#124; `RequestInfo` |
| `init?` | `RequestInit` |

##### 返回

`Promise`<`Response`>

#### 定义在

[hub/src/lib/commit.ts:82](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L82)

* * *

### hubUrl

• `可选` `hubUrl`: `字符串`

#### 定义在

[hub/src/lib/commit.ts:72](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L72)

* * *

### isPullRequest

• `可选` `isPullRequest`: `布尔值`

#### 定义在

[hub/src/lib/commit.ts:71](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L71)

* * *

### 操作

• `操作`: `CommitOperation`[]

#### 定义在

[hub/src/lib/commit.ts:60](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L60)

* * *

### 父提交

• `可选` `parentCommit`: `字符串`

父提交。可选

+   在打开 PR 时: 将使用 parentCommit 作为父提交

+   在提交分支时: 确保没有中间提交

#### 定义在

[hub/src/lib/commit.ts:70](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L70)

* * *

### 仓库

• `repo`: `RepoDesignation`

#### 定义在

[hub/src/lib/commit.ts:59](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L59)

* * *

### 标题

• `标题`: `字符串`

#### 定义在

[hub/src/lib/commit.ts:57](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L57)

* * *

### useWebWorkers

• `可选` `useWebWorkers`: `布尔值` | { `minSize?`: `数字` ; `poolSize?`: `数字` }

是否使用 web workers 计算 SHA256 哈希值。

我们在 web worker 中从 CDN 加载 hash-wasm。不确定如何以其他方式实现并仍然拥有“干净”的捆绑包。

#### 定义在

[hub/src/lib/commit.ts:78](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L78)
