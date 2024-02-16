# Hugging Face JS 库

> 原文：[`huggingface.co/docs/huggingface.js/index`](https://huggingface.co/docs/huggingface.js/index)

<picture>![huggingface javascript library logo](img/e8baa957d442bb16c916903538c80a40.png)</picture>

```py
await inference.translation({
  model: 't5-base',
  inputs: 'My name is Wolfgang and I live in Berlin'
})

await hf.translation({
  model: "facebook/nllb-200-distilled-600M",
  inputs: "how is the weather like in Gaborone",
  parameters : {
    src_lang: "eng_Latn",
    tgt_lang: "sot_Latn"
  }
})

await inference.textToImage({
  model: 'stabilityai/stable-diffusion-2',
  inputs: 'award winning high resolution photo of a giant tortoise/((ladybird)) hybrid, [trending on artstation]',
  parameters: {
    negative_prompt: 'blurry',
  }
})
```

这是一组与 Hugging Face API 交互的 JS 库，包含 TS 类型。

+   @huggingface/inference：使用推理端点（无服务器）调用 100,000 多个机器学习模型

+   @huggingface/hub：与 huggingface.co 交互以创建或删除存储库并提交/下载文件

+   @huggingface/agents：通过自然语言界面与 HF 模型交互

我们使用现代功能来避免填充和依赖关系，因此这些库仅适用于现代浏览器/Node.js >= 18 / Bun / Deno。

这些库仍然非常年轻，请通过提出问题来帮助我们！

## 安装

### 从 NPM

要通过 NPM 安装，您可以根据需要下载库：

```py
npm install @huggingface/inference
npm install @huggingface/hub
npm install @huggingface/agents
```

然后在您的代码中导入库：

```py
import { HfInference } from "@huggingface/inference";
import { HfAgent } from "@huggingface/agents";
import { createRepo, commit, deleteRepo, listFiles } from "@huggingface/hub";
import type { RepoId, Credentials } from "@huggingface/hub";
```

### 从 CDN 或静态主机

您可以在不使用任何捆绑器的情况下使用纯净的 JS 运行我们的软件包，通过使用 CDN 或静态主机。使用[ES 模块](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/)，即`<script type="module">`，您可以在您的代码中导入库：

```py
<script type="module"> import { HfInference } from 'https://cdn.jsdelivr.net/npm/@huggingface/inference@2.6.4/+esm';
    import { createRepo, commit, deleteRepo, listFiles } from "https://cdn.jsdelivr.net/npm/@huggingface/hub@0.13.0/+esm"; </script>
```

### Deno

```py
// esm.sh
import { HfInference } from "https://esm.sh/@huggingface/inference"
import { HfAgent } from "https://esm.sh/@huggingface/agents";

import { createRepo, commit, deleteRepo, listFiles } from "https://esm.sh/@huggingface/hub"
// or npm:
import { HfInference } from "npm:@huggingface/inference"
import { HfAgent } from "npm:@huggingface/agents";

import { createRepo, commit, deleteRepo, listFiles } from "npm:@huggingface/hub"
```

## 使用示例

在您的[帐户设置](https://huggingface.co/settings/tokens)中获取您的 HF 访问令牌。

### @huggingface/inference 示例

```py
import { HfInference } from "@huggingface/inference";

const HF_TOKEN = "hf_...";

const inference = new HfInference(HF_TOKEN);

// You can also omit "model" to use the recommended model for the task
await inference.translation({
  model: 't5-base',
  inputs: 'My name is Wolfgang and I live in Amsterdam'
})

await inference.textToImage({
  model: 'stabilityai/stable-diffusion-2',
  inputs: 'award winning high resolution photo of a giant tortoise/((ladybird)) hybrid, [trending on artstation]',
  parameters: {
    negative_prompt: 'blurry',
  }
})

await inference.imageToText({
  data: await (await fetch('https://picsum.photos/300/300')).blob(),
  model: 'nlpconnect/vit-gpt2-image-captioning',  
})

// Using your own dedicated inference endpoint: https://hf.co/docs/inference-endpoints/
const gpt2 = inference.endpoint('https://xyz.eu-west-1.aws.endpoints.huggingface.cloud/gpt2');
const { generated_text } = await gpt2.textGeneration({inputs: 'The answer to the universe is'});
```

### @huggingface/agents 示例

```py
import {HfAgent, LLMFromHub, defaultTools} from '@huggingface/agents';

const HF_TOKEN = "hf_...";

const agent = new HfAgent(
  HF_TOKEN,
  LLMFromHub(HF_TOKEN),
  [...defaultTools]
);

// you can generate the code, inspect it and then run it
const code = await agent.generateCode("Draw a picture of a cat wearing a top hat. Then caption the picture and read it out loud.");
console.log(code);
const messages = await agent.evaluateCode(code)
console.log(messages); // contains the data

// or you can run the code directly, however you can't check that the code is safe to execute this way, use at your own risk.
const messages = await agent.run("Draw a picture of a cat wearing a top hat. Then caption the picture and read it out loud.")
console.log(messages); 
```

### @huggingface/hub 示例

```py
import { createRepo, uploadFile, deleteFiles } from "@huggingface/hub";

const HF_TOKEN = "hf_...";

await createRepo({
  repo: "my-user/nlp-model", // or {type: "model", name: "my-user/nlp-test"},
  credentials: {accessToken: HF_TOKEN}
});

await uploadFile({
  repo: "my-user/nlp-model",
  credentials: {accessToken: HF_TOKEN},
  // Can work with native File in browsers
  file: {
    path: "pytorch_model.bin",
    content: new Blob(...) 
  }
});

await deleteFiles({
  repo: {type: "space", name: "my-user/my-space"}, // or "spaces/my-user/my-space"
  credentials: {accessToken: HF_TOKEN},
  paths: ["README.md", ".gitattributes"]
});
```

当然还有更多功能，请查看每个库的 README！

## 格式化和测试

```py
sudo corepack enable
pnpm install

pnpm -r format:check
pnpm -r lint:check
pnpm -r test
```

## 构建

```py
pnpm -r build
```

这将在`packages/*/dist`中生成 ESM 和 CJS javascript 文件，例如`packages/inference/dist/index.mjs`。
