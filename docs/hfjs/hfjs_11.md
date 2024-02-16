# 接口：AudioToAudioOutputValue

> 原文：[`huggingface.co/docs/huggingface.js/inference/interfaces/AudioToAudioOutputValue`](https://huggingface.co/docs/huggingface.js/inference/interfaces/AudioToAudioOutputValue)

## 属性

### blob

• `blob`: `string`

Base64 编码的音频输出。

#### 定义在

[inference/src/tasks/audio/audioToAudio.ts:21](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/audioToAudio.ts#L21)

* * *

### 内容类型

• `content-type`: `string`

blob 的内容类型，例如音频/flac

#### 定义在

[inference/src/tasks/audio/audioToAudio.ts:26](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/audioToAudio.ts#L26)

* * *

### 标签

• `label`: `string`

音频输出的标签（特定于模型）

#### 定义在

[inference/src/tasks/audio/audioToAudio.ts:16](https://github.com/huggingface/huggingface.js/blob/main/packages/inference/src/tasks/audio/audioToAudio.ts#L16)
