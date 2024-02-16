# 构建Next.js应用程序

> 原始文本：[https://huggingface.co/docs/transformers.js/tutorials/next](https://huggingface.co/docs/transformers.js/tutorials/next)

在本教程中，我们将构建一个简单的Next.js应用程序，使用Transformers.js执行情感分析！由于Transformers.js可以在浏览器或Node.js中运行，您可以选择是在[客户端](#client-side-inference)还是[服务器端](#server-side-inference)执行推理（我们将向您展示如何执行两者）。在任何情况下，我们将使用新的[App Router](https://nextjs.org/docs/app)范式进行开发。最终产品将如下所示：

![演示](../Images/263f9827f1a37479fa322dad0df05303.png)

有用链接：

+   演示站点：[客户端](https://huggingface.co/spaces/Xenova/next-example-app) 或 [服务器端](https://huggingface.co/spaces/Xenova/next-server-example-app)

+   源代码：[客户端](https://github.com/xenova/transformers.js/tree/main/examples/next-client) 或 [服务器端](https://github.com/xenova/transformers.js/tree/main/examples/next-server)

## 先决条件

+   [Node.js](https://nodejs.org/en/) 版本 18+

+   [npm](https://www.npmjs.com/) 版本 9+

## 客户端推理

### 步骤1：初始化项目

首先使用`create-next-app`创建一个新的Next.js应用程序：

```py
npx create-next-app@latest
```

安装时，您将看到各种提示。对于此演示，我们将选择下面显示的那些内容：

```py
√ What is your project named? ... next
√ Would you like to use TypeScript? ... No / Yes
√ Would you like to use ESLint? ... No / Yes
√ Would you like to use Tailwind CSS? ... No / Yes
√ Would you like to use `src/` directory? ... No / Yes
√ Would you like to use App Router? (recommended) ... No / Yes
√ Would you like to customize the default import alias? ... No / Yes

```

### 步骤2：安装和配置Transformers.js

您可以使用以下命令从[NPM](https://www.npmjs.com/package/@xenova/transformers)安装Transformers.js：

```py
npm i @xenova/transformers
```

我们还需要更新`next.config.js`文件，以在为浏览器打包时忽略特定于节点的模块：

```py
/** @type {import('next').NextConfig} */
const nextConfig = {
    // (Optional) Export as a static site
    // See https://nextjs.org/docs/pages/building-your-application/deploying/static-exports#configuration
    output: 'export', // Feel free to modify/remove this option

    // Override the default webpack configuration
    webpack: (config) => {
        // See https://webpack.js.org/configuration/resolve/#resolvealias
        config.resolve.alias = {
            ...config.resolve.alias,
            "sharp$": false,
            "onnxruntime-node$": false,
        }
        return config;
    },
}

module.exports = nextConfig
```

接下来，我们将创建一个新的[Web Worker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)脚本，我们将在其中放置所有与ML相关的代码。这是为了确保在模型加载和执行推理时不会阻塞主线程。对于此应用程序，我们将使用[`Xenova/distilbert-base-uncased-finetuned-sst-2-english`](https://huggingface.co/Xenova/distilbert-base-uncased-finetuned-sst-2-english)，这是一个在[Stanford Sentiment Treebank](https://huggingface.co/datasets/sst)数据集上微调的约67M参数模型。将以下代码添加到`./src/app/worker.js`中：

```py
import { pipeline, env } from "@xenova/transformers";

// Skip local model check
env.allowLocalModels = false;

// Use the Singleton pattern to enable lazy construction of the pipeline.
class PipelineSingleton {
    static task = 'text-classification';
    static model = 'Xenova/distilbert-base-uncased-finetuned-sst-2-english';
    static instance = null;

    static async getInstance(progress_callback = null) {
        if (this.instance === null) {
            this.instance = pipeline(this.task, this.model, { progress_callback });
        }
        return this.instance;
    }
}

// Listen for messages from the main thread
self.addEventListener('message', async (event) => {
    // Retrieve the classification pipeline. When called for the first time,
    // this will load the pipeline and save it for future use.
    let classifier = await PipelineSingleton.getInstance(x => {
        // We also add a progress callback to the pipeline so that we can
        // track model loading.
        self.postMessage(x);
    });

    // Actually perform the classification
    let output = await classifier(event.data.text);

    // Send the output back to the main thread
    self.postMessage({
        status: 'complete',
        output: output,
    });
});

```

### 步骤3：设计用户界面

现在我们将修改默认的`./src/app/page.js`文件，以便连接到我们的工作线程。由于我们将只在浏览器中执行推理，我们可以选择使用[`'use client'`指令](https://nextjs.org/docs/getting-started/react-essentials#the-use-client-directive)来使用客户端组件。

```py
'use client'

import { useState, useEffect, useRef, useCallback } from 'react'

export default function Home() {
  /* TODO: Add state variables */

  // Create a reference to the worker object.
  const worker = useRef(null);

  // We use the `useEffect` hook to set up the worker as soon as the `App` component is mounted.
  useEffect(() => {
    if (!worker.current) {
      // Create the worker if it does not yet exist.
      worker.current = new Worker(new URL('./worker.js', import.meta.url), {
        type: 'module'
      });
    }

    // Create a callback function for messages from the worker thread.
    const onMessageReceived = (e) => { /* TODO: See below */};

    // Attach the callback function as an event listener.
    worker.current.addEventListener('message', onMessageReceived);

    // Define a cleanup function for when the component is unmounted.
    return () => worker.current.removeEventListener('message', onMessageReceived);
  });

  const classify = useCallback((text) => {
    if (worker.current) {
      worker.current.postMessage({ text });
    }
  }, []);

  return ( /* TODO: See below */ )
}
```

在`Home`组件的开头初始化以下状态变量：

```py
// Keep track of the classification result and the model loading status.
const [result, setResult] = useState(null);
const [ready, setReady] = useState(null);
```

并填写`onMessageReceived`函数，以在工作线程发送消息时更新这些变量：

```py
const onMessageReceived = (e) => {
  switch (e.data.status) {
    case 'initiate':
      setReady(false);
      break;
    case 'ready':
      setReady(true);
      break;
    case 'complete':
      setResult(e.data.output[0])
      break;
  }
};
```

最后，我们可以向`Home`组件添加一个简单的UI，包括一个输入文本框和一个预格式化文本元素，用于显示分类结果：

```py
<main className="flex min-h-screen flex-col items-center justify-center p-12">
  <h1 className="text-5xl font-bold mb-2 text-center">Transformers.js</h1>
  <h2 className="text-2xl mb-4 text-center">Next.js template</h2>

  <input
    className="w-full max-w-xs p-2 border border-gray-300 rounded mb-4"
    type="text"
    placeholder="Enter text here"
    onInput={e => {
        classify(e.target.value);
    }}
  />

  {ready !== null && (
    <pre className="bg-gray-100 p-2 rounded">
      { (!ready || !result) ? 'Loading...' : JSON.stringify(result, null, 2) }
    </pre>
  )}
</main>
```

现在，您可以使用以下命令运行应用程序：

```py
npm run dev
```

访问终端中显示的URL（例如，[http://localhost:3000/](http://localhost:3000/)）以查看应用程序的运行情况！

### （可选）步骤4：构建和部署

要构建您的应用程序，只需运行：

```py
npm run build
```

这将打包您的应用程序并将静态文件输出到`out`文件夹。

对于此演示，我们将将应用程序部署为静态[Hugging Face Space](https://huggingface.co/docs/hub/spaces)，但您可以将其部署到任何您喜欢的地方！如果尚未创建免费的Hugging Face帐户，您可以在[此处](https://huggingface.co/join)创建。

1.  访问[https://huggingface.co/new-space](https://huggingface.co/new-space)并填写表格。记得选择“静态”作为空间类型。

1.  点击页面底部的“创建空间”按钮。

1.  转到“文件”→“添加文件”→“上传文件”。将`out`文件夹中的文件拖到上传框中，然后点击“上传”。上传完成后，向下滚动到按钮处，然后点击“提交更改到主分支”。

**就是这样！**您的应用现在应该在`https://huggingface.co/spaces/<your-username>/<your-space-name>`上运行！

## 服务器端推断

虽然有许多不同的方法可以执行服务器端推断，但最简单的方法（我们将在本教程中讨论）是使用新的[路由处理程序](https://nextjs.org/docs/app/building-your-application/routing/router-handlers)功能。

### 步骤1：初始化项目

首先使用`create-next-app`创建一个新的Next.js应用程序：

```py
npx create-next-app@latest
```

安装时，您会看到各种提示。对于此演示，我们将选择下面显示的那些加粗的选项：

```py
√ What is your project named? ... next
√ Would you like to use TypeScript? ... No / Yes
√ Would you like to use ESLint? ... No / Yes
√ Would you like to use Tailwind CSS? ... No / Yes
√ Would you like to use `src/` directory? ... No / Yes
√ Would you like to use App Router? (recommended) ... No / Yes
√ Would you like to customize the default import alias? ... No / Yes

```

### 步骤2：安装和配置Transformers.js

您可以使用以下命令从[NPM](https://www.npmjs.com/package/@xenova/transformers)安装Transformers.js：

```py
npm i @xenova/transformers
```

我们还需要更新`next.config.js`文件，以防止Webpack捆绑某些包：

```py
/** @type {import('next').NextConfig} */
const nextConfig = {
    // (Optional) Export as a standalone site
    // See https://nextjs.org/docs/pages/api-reference/next-config-js/output#automatically-copying-traced-files
    output: 'standalone', // Feel free to modify/remove this option

    // Indicate that these packages should not be bundled by webpack
    experimental: {
        serverComponentsExternalPackages: ['sharp', 'onnxruntime-node'],
    },
};

module.exports = nextConfig
```

接下来，让我们设置我们的路由处理程序。我们可以通过在新的`./src/app/classify/`目录中创建两个文件来实现这一点：

1.  `pipeline.js` - 用于处理我们的流水线构建。

    ```py
    import { pipeline } from "@xenova/transformers";

    // Use the Singleton pattern to enable lazy construction of the pipeline.
    // NOTE: We wrap the class in a function to prevent code duplication (see below).
    const P = () => class PipelineSingleton {
        static task = 'text-classification';
        static model = 'Xenova/distilbert-base-uncased-finetuned-sst-2-english';
        static instance = null;

        static async getInstance(progress_callback = null) {
            if (this.instance === null) {
                this.instance = pipeline(this.task, this.model, { progress_callback });
            }
            return this.instance;
        }
    }

    let PipelineSingleton;
    if (process.env.NODE_ENV !== 'production') {
        // When running in development mode, attach the pipeline to the
        // global object so that it's preserved between hot reloads.
        // For more information, see https://vercel.com/guides/nextjs-prisma-postgres
        if (!global.PipelineSingleton) {
            global.PipelineSingleton = P();
        }
        PipelineSingleton = global.PipelineSingleton;
    } else {
        PipelineSingleton = P();
    }
    export default PipelineSingleton;
    ```

1.  `route.js` - 用于处理发送到`/classify`路由的请求。

    ```py
    import { NextResponse } from 'next/server'
    import PipelineSingleton from './pipeline.js';

    export async function GET(request) {
        const text = request.nextUrl.searchParams.get('text');
        if (!text) {
            return NextResponse.json({
                error: 'Missing text parameter',
            }, { status: 400 });
        }
        // Get the classification pipeline. When called for the first time,
        // this will load the pipeline and cache it for future use.
        const classifier = await PipelineSingleton.getInstance();

        // Actually perform the classification
        const result = await classifier(text);

        return NextResponse.json(result);
    }
    ```

### 步骤3：设计用户界面

我们现在将修改默认的`./src/app/page.js`文件，以向我们新创建的路由处理程序发出请求。

```py
'use client'

import { useState } from 'react'

export default function Home() {

  // Keep track of the classification result and the model loading status.
  const [result, setResult] = useState(null);
  const [ready, setReady] = useState(null);

  const classify = async (text) => {
    if (!text) return;
    if (ready === null) setReady(false);

    // Make a request to the /classify route on the server.
    const result = await fetch(`/classify?text=${encodeURIComponent(text)}`);

    // If this is the first time we've made a request, set the ready flag.
    if (!ready) setReady(true);

    const json = await result.json();
    setResult(json);
  };
  return (
    <main className="flex min-h-screen flex-col items-center justify-center p-12">
      <h1 className="text-5xl font-bold mb-2 text-center">Transformers.js</h1>
      <h2 className="text-2xl mb-4 text-center">Next.js template (server-side)</h2>
      <input
        type="text"
        className="w-full max-w-xs p-2 border border-gray-300 rounded mb-4"
        placeholder="Enter text here"
        onInput={e => {
          classify(e.target.value);
        }}
      />

      {ready !== null && (
        <pre className="bg-gray-100 p-2 rounded">
          {
            (!ready || !result) ? 'Loading...' : JSON.stringify(result, null, 2)}
        </pre>
      )}
    </main>
  )
}
```

您现在可以使用以下命令运行您的应用程序：

```py
npm run dev
```

访问终端中显示的URL（例如，[http://localhost:3000/](http://localhost:3000/)）以查看您的应用程序运行情况！

### （可选）步骤4：构建和部署

对于此演示，我们将构建并部署我们的应用程序到[Hugging Face Spaces](https://huggingface.co/docs/hub/spaces)。如果您还没有，可以在[此处](https://huggingface.co/join)创建一个免费的Hugging Face帐户。

1.  在项目的根目录中创建一个新的`Dockerfile`。您可以使用我们的[示例Dockerfile](https://github.com/xenova/transformers.js/blob/main/examples/next-server/Dockerfile)作为模板。

1.  访问[https://huggingface.co/new-space](https://huggingface.co/new-space)并填写表格。记得选择“Docker”作为空间类型（您可以选择“空白”Docker模板）。

1.  点击页面底部的“创建空间”按钮。

1.  转到“文件”→“添加文件”→“上传文件”。将项目文件夹中的文件（如果存在，则不包括`node_modules`和`.next`）拖到上传框中，然后点击“上传”。上传完成后，向下滚动到按钮处，然后点击“提交更改到主分支”。

1.  将以下行添加到您的`README.md`的顶部：

    ```py
    ---
    title: Next Server Example App
    emoji: 🔥
    colorFrom: yellow
    colorTo: red
    sdk: docker
    pinned: false
    app_port: 3000
    ---
    ```

**就是这样！**您的应用现在应该在`https://huggingface.co/spaces/<your-username>/<your-space-name>`上运行！
