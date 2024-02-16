# @huggingface/agents

> 原始文本: [https://huggingface.co/docs/huggingface.js/agents/modules](https://huggingface.co/docs/huggingface.js/agents/modules)

## 类

+   [HfAgent](classes/HfAgent)

## 变量

### 默认工具

• `Const` `defaultTools`: `Tool`[]

#### 定义于

[tools/index.ts:7](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/tools/index.ts#L7)

* * *

### imageToTextTool

• `Const` `imageToTextTool`: `Tool`

#### 定义于

[tools/imageToText.ts:3](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/tools/imageToText.ts#L3)

* * *

### speechToTextTool

• `Const` `speechToTextTool`: `Tool`

#### 定义于

[tools/speechToText.ts:3](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/tools/speechToText.ts#L3)

* * *

### textToImageTool

• `Const` `textToImageTool`: `Tool`

#### 定义于

[tools/textToImage.ts:3](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/tools/textToImage.ts#L3)

* * *

### textToSpeechTool

• `Const` `textToSpeechTool`: `Tool`

#### 定义于

[tools/textToSpeech.ts:3](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/tools/textToSpeech.ts#L3)

## 函数

### LLMFromEndpoint

▸ `LLMFromEndpoint`(`accessToken`, `endpoint`): `LLM`

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `accessToken` | `string` |
| `endpoint` | `string` |

#### 返回

`LLM`

#### 定义于

[llms/LLMHF.ts:22](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/llms/LLMHF.ts#L22)

* * *

### LLMFromHub

▸ `LLMFromHub`(`accessToken?`, `model?`): `LLM`

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `accessToken?` | `string` |
| `model?` | `string` |

#### 返回

`LLM`

#### 定义于

[llms/LLMHF.ts:4](https://github.com/huggingface/huggingface.js/blob/main/packages/agents/src/llms/LLMHF.ts#L4)
