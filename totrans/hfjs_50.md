# 接口: ListFileEntry

> 原文链接: [https://huggingface.co/docs/huggingface.js/hub/interfaces/ListFileEntry](https://huggingface.co/docs/huggingface.js/hub/interfaces/ListFileEntry)

## 属性

### lastCommit

• `可选` `lastCommit`: `对象`

仅在`listFiles`调用中的`expand`设置为`true`时获取。

#### 类型声明

| 名称 | 类型 |
| :-- | :-- |
| `日期` | `字符串` |
| `id` | `字符串` |
| `标题` | `字符串` |

#### 定义在

[hub/src/lib/list-files.ts:23](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/list-files.ts#L23)

* * *

### lfs

• `可选` `lfs`: `对象`

#### 类型声明

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `oid` | `字符串` | - |
| `pointerSize` | `数字` | 原始指针文件的大小，100~200字节 |
| `大小` | `数字` | - |

#### 定义在

[hub/src/lib/list-files.ts:14](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/list-files.ts#L14)

* * *

### oid

• `oid`: `字符串`

#### 定义在

[hub/src/lib/list-files.ts:13](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/list-files.ts#L13)

* * *

### 路径

• `路径`: `字符串`

#### 定义在

[hub/src/lib/list-files.ts:12](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/list-files.ts#L12)

* * *

### security

• `可选` `security`: `未知`

仅在`listFiles`调用中的`expand`设置为`true`时获取。

#### 定义在

[hub/src/lib/list-files.ts:31](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/list-files.ts#L31)

* * *

### 大小

• `大小`: `数字`

#### 定义在

[hub/src/lib/list-files.ts:11](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/list-files.ts#L11)

* * *

### 类型

• `类型`: `"文件"` | `"目录"` | `"未知"`

#### 定义在

[hub/src/lib/list-files.ts:10](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/list-files.ts#L10)
