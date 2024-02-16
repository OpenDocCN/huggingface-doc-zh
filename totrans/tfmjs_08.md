# 构建一个 React 应用程序

> 原始文本：[https://huggingface.co/docs/transformers.js/tutorials/react](https://huggingface.co/docs/transformers.js/tutorials/react)

在本教程中，我们将构建一个简单的 React 应用程序，使用 Transformers.js 执行多语言翻译！最终产品将类似于这样：

![演示](../Images/04732e5c65a2c2502d231cc7491291d4.png)

有用的链接：

+   [演示网站](https://huggingface.co/spaces/Xenova/react-translator)

+   [源代码](https://github.com/xenova/transformers.js/tree/main/examples/react-translator)

## 先决条件

+   [Node.js](https://nodejs.org/en/) 版本 18+

+   [npm](https://www.npmjs.com/) 版本 9+

## 第 1 步：初始化项目

在本教程中，我们将使用 [Vite](https://vitejs.dev/) 来初始化我们的项目。Vite 是一个构建工具，允许我们快速设置一个具有最小配置的 React 应用程序。在终端中运行以下命令：

```py
npm create vite@latest react-translator -- --template react
```

如果提示安装 `create-vite`，请输入 `y` 并按 `Enter` 键。

接下来，进入项目目录并安装必要的开发依赖项：

```py
cd react-translator
npm install
```

为了测试我们的应用程序是否正常工作，我们可以运行以下命令：

```py
npm run dev
```

访问终端中显示的 URL（例如，[http://localhost:5173/](http://localhost:5173/)）应该显示默认的“React + Vite”欢迎页面。您可以通过在终端中按 `Ctrl` + `C` 来停止开发服务器。

## 第 2 步：安装和配置 Transformers.js

现在我们来到有趣的部分：向我们的应用程序添加机器学习！首先，使用以下命令从 [NPM](https://www.npmjs.com/package/@xenova/transformers) 安装 Transformers.js：

```py
npm install @xenova/transformers
```

对于这个应用程序，我们将使用 [Xenova/nllb-200-distilled-600M](https://huggingface.co/Xenova/nllb-200-distilled-600M) 模型，该模型可以在 200 种语言之间执行多语言翻译。在开始之前，有两件事情我们需要注意：

1.  ML 推断可能非常消耗计算资源，因此最好在主线程之外的单独线程中加载和运行模型。

1.  由于模型相当大（>1 GB），我们不希望在用户点击“翻译”按钮之前下载它。

我们可以通过使用 [Web Worker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers) 和一些 [React hooks](https://react.dev/reference/react) 来实现这两个目标。

1.  在 `src` 目录中创建一个名为 `worker.js` 的文件。这个脚本将为我们完成所有繁重的工作，包括加载和运行翻译管道。为了确保模型只加载一次，我们将创建 `MyTranslationPipeline` 类，该类使用 [单例模式](https://en.wikipedia.org/wiki/Singleton_pattern) 在首次调用 `getInstance` 时懒惰地创建管道的单个实例，并在所有后续调用中使用此管道：

    ```py
    import { pipeline } from '@xenova/transformers';

    class MyTranslationPipeline {
      static task = 'translation';
      static model = 'Xenova/nllb-200-distilled-600M';
      static instance = null;

      static async getInstance(progress_callback = null) {
        if (this.instance === null) {
          this.instance = pipeline(this.task, this.model, { progress_callback });
        }

        return this.instance;
      }
    }
    ```

1.  修改 `src` 目录中的 `App.jsx`。当初始化我们的 React 项目时，此文件将自动创建，并包含一些样板代码。在 `App` 函数内部，让我们创建 web worker 并使用 `useRef` hook 存储对它的引用：

    ```py
    // Remember to import the relevant hooks
    import { useEffect, useRef, useState } from 'react'

    function App() {
      // Create a reference to the worker object.
      const worker = useRef(null);

      // We use the `useEffect` hook to setup the worker as soon as the `App` component is mounted.
      useEffect(() => {
        if (!worker.current) {
          // Create the worker if it does not yet exist.
          worker.current = new Worker(new URL('./worker.js', import.meta.url), {
              type: 'module'
          });
        }

        // Create a callback function for messages from the worker thread.
        const onMessageReceived = (e) => {
          // TODO: Will fill in later
        };

        // Attach the callback function as an event listener.
        worker.current.addEventListener('message', onMessageReceived);

        // Define a cleanup function for when the component is unmounted.
        return () => worker.current.removeEventListener('message', onMessageReceived);
      });

      return (
        // TODO: Rest of our app goes here...
      )
    }

    export default App 
    ```

## 第 3 步：设计用户界面

我们建议使用 `npm run dev` 重新启动开发服务器（如果尚未运行），以便您可以实时查看您的更改。

首先，让我们定义我们的组件。在 `src` 目录中创建一个名为 `components` 的文件夹，并创建以下文件：

1.  `LanguageSelector.jsx`：这个组件将允许用户选择输入和输出语言。查看完整的语言列表 [here](https://github.com/xenova/transformers.js/blob/main/examples/react-translator/src/components/LanguageSelector.jsx)。

    ```py
    const LANGUAGES = {
      "Acehnese (Arabic script)": "ace_Arab",
      "Acehnese (Latin script)": "ace_Latn",
      "Afrikaans": "afr_Latn",
      ...
      "Zulu": "zul_Latn",
    }

    export default function LanguageSelector({ type, onChange, defaultLanguage }) {
      return (
        <div className='language-selector'>
          <label>{type}: </label>
          <select onChange={onChange} defaultValue={defaultLanguage}>
            {Object.entries(LANGUAGES).map(([key, value]) => {
              return <option key={key} value={value}>{key}</option>
            })}
          </select>
        </div>
      )
    }
    ```

1.  `Progress.jsx`：这个组件将显示每个模型文件下载的进度。

    ```py
    export default function Progress({ text, percentage }) {
      percentage = percentage ?? 0;
      return (
        <div className="progress-container">
          <div className='progress-bar' style={{ 'width': `${percentage}%` }}>
            {text} ({`${percentage.toFixed(2)}%`})
          </div>
        </div>
      );
    }
    ```

现在我们可以通过将这些导入添加到文件顶部在 `App.jsx` 中使用这些组件：

```py
import LanguageSelector from './components/LanguageSelector';
import Progress from './components/Progress';
```

让我们还添加一些状态变量来跟踪应用程序中的一些内容，比如模型加载、语言、输入文本和输出文本。将以下代码添加到 `src/App.jsx` 中 `App` 函数的开头：

```py
function App() {

  // Model loading
  const [ready, setReady] = useState(null);
  const [disabled, setDisabled] = useState(false);
  const [progressItems, setProgressItems] = useState([]);

  // Inputs and outputs
  const [input, setInput] = useState('I love walking my dog.');
  const [sourceLanguage, setSourceLanguage] = useState('eng_Latn');
  const [targetLanguage, setTargetLanguage] = useState('fra_Latn');
  const [output, setOutput] = useState('');

  // rest of the code...
}
```

接下来，我们可以将自定义组件添加到主 `App` 组件中。我们还会添加两个用于输入和输出文本的 `textarea` 元素，以及一个触发翻译的 `button`。修改 `return` 语句如下：

```py
return (
  <>
    <h1>Transformers.js</h1>
    <h2>ML-powered multilingual translation in React!</h2>

    <div className='container'>
      <div className='language-container'>
        <LanguageSelector type={"Source"} defaultLanguage={"eng_Latn"} onChange={x => setSourceLanguage(x.target.value)} />
        <LanguageSelector type={"Target"} defaultLanguage={"fra_Latn"} onChange={x => setTargetLanguage(x.target.value)} />
      </div>

      <div className='textbox-container'>
        <textarea value={input} rows={3} onChange={e => setInput(e.target.value)}></textarea>
        <textarea value={output} rows={3} readOnly></textarea>
      </div>
    </div>

    <button disabled={disabled} onClick={translate}>Translate</button>

    <div className='progress-bars-container'>
      {ready === false && (
        <label>Loading models... (only run once)</label>
      )}
      {progressItems.map(data => (
        <div key={data.file}>
          <Progress text={data.file} percentage={data.progress} />
        </div>
      ))}
    </div>
  </>
)
```

现在不用担心 `translate` 函数。我们将在下一节中定义它。

最后，我们可以添加一些 CSS 来让我们的应用程序看起来更漂亮。修改 `src` 目录中以下文件：

1.  `index.css`：

    <details><summary data-svelte-h="svelte-1hbuv8y">查看代码</summary>

    ```py
    :root {
      font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
      line-height: 1.5;
      font-weight: 400;
      color: #213547;
      background-color: #ffffff;

      font-synthesis: none;
      text-rendering: optimizeLegibility;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      -webkit-text-size-adjust: 100%;
    }

    body {
      margin: 0;
      display: flex;
      place-items: center;
      min-width: 320px;
      min-height: 100vh;
    }

    h1 {
      font-size: 3.2em;
      line-height: 1;
    }

    h1,
    h2 {
      margin: 8px;
    }

    select {
      padding: 0.3em;
      cursor: pointer;
    }

    textarea {
      padding: 0.6em;
    }

    button {
      padding: 0.6em 1.2em;
      cursor: pointer;
      font-weight: 500;
    }

    button[disabled] {
      cursor: not-allowed;
    }

    select,
    textarea,
    button {
      border-radius: 8px;
      border: 1px solid transparent;
      font-size: 1em;
      font-family: inherit;
      background-color: #f9f9f9;
      transition: border-color 0.25s;
    }

    select:hover,
    textarea:hover,
    button:not([disabled]):hover {
      border-color: #646cff;
    }

    select:focus,
    select:focus-visible,
    textarea:focus,
    textarea:focus-visible,
    button:focus,
    button:focus-visible {
      outline: 4px auto -webkit-focus-ring-color;
    }
    ```</details>

1.  `App.css`

    <details><summary data-svelte-h="svelte-1hbuv8y">查看代码</summary>

    ```py
    #root {
      max-width: 1280px;
      margin: 0 auto;
      padding: 2rem;
      text-align: center;
    }

    .language-container {
      display: flex;
      gap: 20px;
    }

    .textbox-container {
      display: flex;
      justify-content: center;
      gap: 20px;
      width: 800px;
    }

    .textbox-container>textarea, .language-selector {
      width: 50%;
    }

    .language-selector>select {
      width: 150px;
    }

    .progress-container {
      position: relative;
      font-size: 14px;
      color: white;
      background-color: #e9ecef;
      border: solid 1px;
      border-radius: 8px;
      text-align: left;
      overflow: hidden;
    }

    .progress-bar {
      padding: 0 4px;
      z-index: 0;
      top: 0;
      width: 1%;
      height: 100%;
      overflow: hidden;
      background-color: #007bff;
      white-space: nowrap;
    }

    .progress-text {
      z-index: 2;
    }

    .selector-container {
      display: flex;
      gap: 20px;
    }

    .progress-bars-container {
      padding: 8px;
      height: 140px;
    }

    .container {
      margin: 25px;
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    ```</details>

## 步骤 4：将所有内容连接在一起

现在我们已经设置了基本的用户界面，我们终于可以把所有东西连接起来了。

首先，让我们定义 `translate` 函数，当用户点击 `Translate` 按钮时将调用该函数。这会将包含输入文本、源语言和目标语言的消息发送到工作线程进行处理。我们还会禁用按钮，以防用户多次点击。在 `App` 函数的 `return` 语句之前添加以下代码：

```py
const translate = () => {
  setDisabled(true);
  worker.current.postMessage({
    text: input,
    src_lang: sourceLanguage,
    tgt_lang: targetLanguage,
  });
}
```

现在，在 `src/worker.js` 中添加一个事件侦听器，以侦听来自主线程的消息。我们将使用 `self.postMessage` 将消息（例如模型加载进度和文本流）发送回主线程。

```py
// Listen for messages from the main thread
self.addEventListener('message', async (event) => {
  // Retrieve the translation pipeline. When called for the first time,
  // this will load the pipeline and save it for future use.
  let translator = await MyTranslationPipeline.getInstance(x => {
      // We also add a progress callback to the pipeline so that we can
      // track model loading.
      self.postMessage(x);
  });

  // Actually perform the translation
  let output = await translator(event.data.text, {
      tgt_lang: event.data.tgt_lang,
      src_lang: event.data.src_lang,

      // Allows for partial output
      callback_function: x => {
          self.postMessage({
              status: 'update',
              output: translator.tokenizer.decode(x[0].output_token_ids, { skip_special_tokens: true })
          });
      }
  });

  // Send the output back to the main thread
  self.postMessage({
      status: 'complete',
      output: output,
  });
});
```

最后，让我们填写我们的 `onMessageReceived` 函数，该函数将根据来自工作线程的消息更新应用程序状态。在我们之前定义的 `useEffect` 钩子内添加以下代码：

```py
const onMessageReceived = (e) => {
  switch (e.data.status) {
    case 'initiate':
      // Model file start load: add a new progress item to the list.
      setReady(false);
      setProgressItems(prev => [...prev, e.data]);
      break;

    case 'progress':
      // Model file progress: update one of the progress items.
      setProgressItems(
        prev => prev.map(item => {
          if (item.file === e.data.file) {
            return { ...item, progress: e.data.progress }
          }
          return item;
        })
      );
      break;

    case 'done':
      // Model file loaded: remove the progress item from the list.
      setProgressItems(
        prev => prev.filter(item => item.file !== e.data.file)
      );
      break;

    case 'ready':
      // Pipeline ready: the worker is ready to accept messages.
      setReady(true);
      break;

    case 'update':
      // Generation update: update the output text.
      setOutput(e.data.output);
      break;

    case 'complete':
      // Generation complete: re-enable the "Translate" button
      setDisabled(false);
      break;
  }
};
```

现在，您可以使用 `npm run dev` 运行应用程序，并直接在浏览器中进行多语言翻译！

## （可选）步骤 5：构建和部署

要构建您的应用程序，只需运行 `npm run build`。这将打包您的应用程序并将静态文件输出到 `dist` 文件夹中。

对于这个演示，我们将将我们的应用程序部署为静态 [Hugging Face Space](https://huggingface.co/docs/hub/spaces)，但您可以将其部署到任何您喜欢的地方！如果您还没有，您可以在[这里](https://huggingface.co/join)创建一个免费的 Hugging Face 账户。

1.  访问 [https://huggingface.co/new-space](https://huggingface.co/new-space) 并填写表格。记得选择“Static”作为空间类型。

1.  转到“文件” → “添加文件” → “上传文件”。将 `dist` 文件夹中的 `index.html` 文件和 `public/` 文件夹拖到上传框中，然后点击“上传”。上传完成后，滚动到按钮下方，点击“提交更改到主分支”。

**就是这样！** 您的应用程序现在应该在 `https://huggingface.co/spaces/<your-username>/<your-space-name>` 上运行！
