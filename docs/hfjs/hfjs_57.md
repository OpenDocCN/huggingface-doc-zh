# 接口: SpaceRuntime

> 原始文本: [`huggingface.co/docs/huggingface.js/hub/interfaces/SpaceRuntime`](https://huggingface.co/docs/huggingface.js/hub/interfaces/SpaceRuntime)

## 属性

### errorMessage

• `可选` `errorMessage`: `字符串`

#### 在以下位置定义

[hub/src/types/public.d.ts:54](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L54)

* * *

### gcTimeout

• `可选` `gcTimeout`: `null` | `数字`

以秒为单位

#### 在以下位置定义

[hub/src/types/public.d.ts:64](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L64)

* * *

### hardware

• `可选` `hardware`: `对象`

#### 类型声明

| 名称 | 类型 |
| :-- | :-- |
| `current` | `null` &#124; `SpaceHardwareFlavor` |
| `currentPrettyName?` | `字符串` |
| `requested` | `null` &#124; `SpaceHardwareFlavor` |
| `requestedPrettyName?` | `字符串` |

#### 在以下位置定义

[hub/src/types/public.d.ts:55](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L55)

* * *

### resources

• `可选` `resources`: `SpaceResourceConfig`

在调用 /spaces 时，只有在 ?full=true 时才会获取这些属性

#### 在以下位置定义

[hub/src/types/public.d.ts:62](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L62)

* * *

### sdk

• `可选` `sdk`: `SpaceSdk`

#### 在以下位置定义

[hub/src/types/public.d.ts:52](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L52)

* * *

### sdkVersion

• `可选` `sdkVersion`: `字符串`

#### 在以下位置定义

[hub/src/types/public.d.ts:53](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L53)

* * *

### 阶段

• `阶段`: `SpaceStage`

#### 在以下位置定义

[hub/src/types/public.d.ts:51](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L51)
