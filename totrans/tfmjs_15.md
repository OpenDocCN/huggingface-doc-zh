# 在Node.js中进行服务器端音频处理

> 原始文本：[https://huggingface.co/docs/transformers.js/guides/node-audio-processing](https://huggingface.co/docs/transformers.js/guides/node-audio-processing)

在Web上编写代码的一个主要好处是您可以访问现代浏览器中提供的众多API。不幸的是，在编写服务器端代码时，我们无法享受到这样的便利，因此我们必须找到另一种方法。在本教程中，我们将设计一个简单的Node.js应用程序，该应用程序使用Transformers.js进行语音识别，同时学习如何在服务器上处理音频。

我们需要解决的主要问题是[Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)在Node.js中不可用，这意味着我们无法使用[`AudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext)类来处理音频。因此，我们需要安装第三方库来获取原始音频数据。在本例中，我们只考虑`.wav`文件，但相同的原则也适用于其他音频格式。

本教程将以ES模块的形式编写，但您可以轻松地将其改为使用CommonJS。更多信息，请参阅[node教程](https://huggingface.co/docs/transformers.js/tutorials/node)。

**有用链接：**

+   [源代码](https://github.com/xenova/transformers.js/tree/main/examples/node-audio-processing)

+   [文档](https://huggingface.co/docs/transformers.js)

## 先决条件

+   [Node.js](https://nodejs.org/en/) 版本 18+

+   [npm](https://www.npmjs.com/) 版本 9+

## 入门

让我们从创建一个新的Node.js项目并通过[NPM](https://www.npmjs.com/package/@xenova/transformers)安装Transformers.js开始：

```py
npm init -y
npm i @xenova/transformers
```

记得在你的`package.json`中添加`"type": "module"`，以指示你的项目使用ECMAScript模块。

接下来，让我们安装[`wavefile`](https://www.npmjs.com/package/wavefile)包，我们将用它来加载`.wav`文件：

```py
npm i wavefile
```

## 创建应用程序

首先创建一个名为`index.js`的新文件，这将是我们应用程序的入口点。让我们还导入必要的模块：

```py
import { pipeline } from '@xenova/transformers';
import wavefile from 'wavefile';
```

在本教程中，我们将使用`Xenova/whisper-tiny.en`模型，但请随意从[Hugging Face Hub](https://huggingface.co/models?library=transformers.js&search=whisper)中选择其他whisper模型。让我们创建我们的管道：

```py
let transcriber = await pipeline('automatic-speech-recognition', 'Xenova/whisper-tiny.en');
```

接下来，让我们加载一个音频文件并将其转换为Transformers.js所需的格式：

```py
// Load audio data
let url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/jfk.wav';
let buffer = Buffer.from(await fetch(url).then(x => x.arrayBuffer()))

// Read .wav file and convert it to required format
let wav = new wavefile.WaveFile(buffer);
wav.toBitDepth('32f'); // Pipeline expects input as a Float32Array
wav.toSampleRate(16000); // Whisper expects audio with a sampling rate of 16000
let audioData = wav.getSamples();
if (Array.isArray(audioData)) {
  if (audioData.length > 1) {
    const SCALING_FACTOR = Math.sqrt(2);

    // Merge channels (into first channel to save memory)
    for (let i = 0; i < audioData[0].length; ++i) {
      audioData[0][i] = SCALING_FACTOR * (audioData[0][i] + audioData[1][i]) / 2;
    }
  }

  // Select first channel
  audioData = audioData[0];
}
```

最后，让我们运行模型并测量执行持续时间。

```py
let start = performance.now();
let output = await transcriber(audioData);
let end = performance.now();
console.log(`Execution duration: ${(end - start) / 1000} seconds`);
console.log(output);
```

您现在可以使用`node index.js`运行应用程序。请注意，当首次运行脚本时，可能需要一段时间来下载和缓存模型。后续请求将使用缓存的模型，模型加载速度将更快。

您应该看到类似以下的输出：

```py
Execution duration: 0.6460317999720574 seconds
{
  text: ' And so my fellow Americans ask not what your country can do for you. Ask what you can do for your country.'
}
```

就是这样！您已成功创建了一个使用Transformers.js进行语音识别的Node.js应用程序。您现在可以将其用作自己应用程序的起点。
