# Node.js中的服务器端推断

> 原始文本：[https://huggingface.co/docs/transformers.js/tutorials/node](https://huggingface.co/docs/transformers.js/tutorials/node)

尽管Transformers.js最初设计用于在浏览器中使用，但它也能够在服务器上运行推断。在本教程中，我们将设计一个简单的Node.js API，该API使用Transformers.js进行情感分析。

我们还将向您展示如何在CommonJS和ECMAScript模块中使用该库，因此您可以选择最适合您项目的模块系统：

+   [ECMAScript模块（ESM）](#ecmascript-modules-esm) - 用于打包JavaScript代码以供重用的官方标准格式。这是现代浏览器中的默认模块系统，使用`import`导入模块并使用`export`导出模块。幸运的是，从版本13.2.0开始，Node.js稳定支持ES模块。

+   [CommonJS](#commonjs) - Node.js中的默认模块系统。在此系统中，使用`require()`导入模块，并使用`module.exports`导出模块。

尽管您始终可以使用[Python库](https://github.com/huggingface/transformers)进行服务器端推断，但使用Transformers.js意味着您可以将所有代码都写在JavaScript中（而不必设置和与单独的Python进程通信）。

**有用的链接：**

+   源代码（[ESM](https://github.com/xenova/transformers.js/tree/main/examples/node/esm/app.js)或[CommonJS](https://github.com/xenova/transformers.js/tree/main/examples/node/commonjs/app.js)）

+   [文档](https://huggingface.co/docs/transformers.js)

## 先决条件

+   [Node.js](https://nodejs.org/en/)版本18+

+   [npm](https://www.npmjs.com/)版本9+

## 入门指南

让我们首先创建一个新的Node.js项目，并通过[NPM](https://www.npmjs.com/package/@xenova/transformers)安装Transformers.js：

```py
npm init -y
npm i @xenova/transformers
```

接下来，创建一个名为`app.js`的新文件，这将是我们应用程序的入口点。根据您是使用[ECMAScript模块](#ecmascript-modules-esm)还是[CommonJS](#commonjs)，您需要做一些不同的事情（见下文）。

我们还将创建一个名为`MyClassificationPipeline`的辅助类来控制管道的加载。它使用[单例模式](https://en.wikipedia.org/wiki/Singleton_pattern)来在首次调用`getInstance`时懒惰地创建管道的单个实例，并在随后的所有调用中使用此管道：

### ECMAScript模块（ESM）

为了指示您的项目使用ECMAScript模块，您需要将`"type": "module"`添加到您的`package.json`中：

```py
{
  ...
  "type": "module",
  ...
}
```

接下来，您需要将以下导入添加到`app.js`的顶部：

```py
import http from 'http';
import querystring from 'querystring';
import url from 'url';
```

接下来，让我们导入Transformers.js并定义`MyClassificationPipeline`类。

```py
import { pipeline, env } from '@xenova/transformers';

class MyClassificationPipeline {
  static task = 'text-classification';
  static model = 'Xenova/distilbert-base-uncased-finetuned-sst-2-english';
  static instance = null;

  static async getInstance(progress_callback = null) {
    if (this.instance === null) {
      // NOTE: Uncomment this to change the cache directory
      // env.cacheDir = './.cache';

      this.instance = pipeline(this.task, this.model, { progress_callback });
    }

    return this.instance;
  }
}
```

### CommonJS

首先，在`app.js`的顶部添加以下导入：

```py
const http = require('http');
const querystring = require('querystring');
const url = require('url');
```

接下来，让我们导入Transformers.js并定义`MyClassificationPipeline`类。由于Transformers.js是一个ESM模块，我们将需要使用[`import()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import)函数动态导入库：

```py
class MyClassificationPipeline {
  static task = 'text-classification';
  static model = 'Xenova/distilbert-base-uncased-finetuned-sst-2-english';
  static instance = null;

  static async getInstance(progress_callback = null) {
    if (this.instance === null) {
      // Dynamically import the Transformers.js library
      let { pipeline, env } = await import('@xenova/transformers');

      // NOTE: Uncomment this to change the cache directory
      // env.cacheDir = './.cache';

      this.instance = pipeline(this.task, this.model, { progress_callback });
    }

    return this.instance;
  }
}
```

## 创建一个基本的HTTP服务器

接下来，让我们使用内置的[HTTP](https://nodejs.org/api/http.html#http)模块创建一个基本服务器。我们将监听发送到服务器的请求（使用`/classify`端点），提取`text`查询参数，并通过管道运行此参数。

```py
// Define the HTTP server
const server = http.createServer();
const hostname = '127.0.0.1';
const port = 3000;

// Listen for requests made to the server
server.on('request', async (req, res) => {
  // Parse the request URL
  const parsedUrl = url.parse(req.url);

  // Extract the query parameters
  const { text } = querystring.parse(parsedUrl.query);

  // Set the response headers
  res.setHeader('Content-Type', 'application/json');

  let response;
  if (parsedUrl.pathname === '/classify' && text) {
    const classifier = await MyClassificationPipeline.getInstance();
    response = await classifier(text);
    res.statusCode = 200;
  } else {
    response = { 'error': 'Bad request' }
    res.statusCode = 400;
  }

  // Send the JSON response
  res.end(JSON.stringify(response));
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});

```

由于我们使用延迟加载，第一次向服务器发出的请求也将负责加载管道。如果您希望在服务器启动运行时立即开始加载管道，可以在定义`MyClassificationPipeline`后添加以下代码行：

```py
MyClassificationPipeline.getInstance();
```

要启动服务器，请运行以下命令：

```py
node app.js
```

服务器应该在[http://127.0.0.1:3000/](http://127.0.0.1:3000/)上运行，您可以在Web浏览器中访问。您应该看到以下消息：

```py
{"error":"Bad request"}
```

这是因为我们没有使用有效的`text`查询参数来定位`/classify`端点。让我们再试一次，这次使用有效的请求。例如，您可以访问[http://127.0.0.1:3000/classify?text=I%20love%20Transformers.js](http://127.0.0.1:3000/classify?text=I%20love%20Transformers.js)，您应该会看到：

```py
[{"label":"POSITIVE","score":0.9996721148490906}]
```

太棒了！我们已成功创建了一个使用Transformers.js对文本进行分类的基本HTTP服务器。

## （可选）自定义

### 模型缓存

默认情况下，第一次运行应用程序时，它将下载模型文件并将它们缓存在您的文件系统上（在`./node_modules/@xenova/transformers/.cache/`中）。然后所有后续请求将使用这个模型。您可以通过设置`env.cacheDir`来更改缓存位置。例如，要在当前工作目录中的`.cache`目录中缓存模型，您可以添加：

```py
env.cacheDir = './.cache';
```

### 使用本地模型

如果您想使用本地模型文件，可以设置`env.localModelPath`如下：

```py
// Specify a custom location for models (defaults to '/models/').
env.localModelPath = '/path/to/models/';
```

您还可以通过将`env.allowRemoteModels`设置为`false`来禁用加载远程模型：

```py
// Disable the loading of remote models from the Hugging Face Hub:
env.allowRemoteModels = false;
```
