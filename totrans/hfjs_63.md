# 🤗 Hugging Face Agents.js

> 原始文本：[https://huggingface.co/docs/huggingface.js/agents/README](https://huggingface.co/docs/huggingface.js/agents/README)

一种从自然语言中调用Hugging Face模型和推理端点的方法，使用LLM。

## 安装

```py
pnpm add @huggingface/agents

npm add @huggingface/agents

yarn add @huggingface/agents
```

### Deno

```py
// esm.sh
import { HfAgent } from "https://esm.sh/@huggingface/agent"
// or npm:
import { HfAgent } from "npm:@huggingface/agent"
```

## 用法

Agents.js利用在HF上托管的LLM作为推理端点，因此您需要创建一个帐户并生成一个[访问令牌](https://huggingface.co/settings/tokens)。

```py
import { HfAgent } from "@huggingface/agents";

const agent = new HfAgent("hf_...");

const code = await agent.generateCode("Draw a picture of a cat, wearing a top hat.")
console.log(code) // always good to check the generated code before running it
const outputs = await agent.evaluateCode(code);
console.log(outputs) 
```

### 选择您的LLM

您还可以通过调用`LLMFrom*`函数之一来使用自己的LLM。

#### 来自hub

只要它们有API，您可以指定hub上的任何有效模型。

```py
import { HfAgent, LLMFromHub } from "@huggingface/agents";

const agent = new HfAgent(
  "hf_...",
  LLMFromHub("hf_...", "OpenAssistant/oasst-sft-4-pythia-12b-epoch-3.5")
);
```

#### 从您自己的端点

您还可以指定自己的端点，只要它实现相同的API，例如使用[text generation inference](https://github.com/huggingface/text-generation-inference)和[Inference Endpoints](https://huggingface.co/inference-endpoints)。

```py
import { HfAgent, LLMFromEndpoint } from "@huggingface/agents";

const agent = new HfAgent(
  "hf_...",
  LLMFromEndpoint("hf_...", "http://...")
);
```

#### 自定义LLM

在这种情况下，LLM被定义为任何异步函数，它接受一个字符串输入并返回一个字符串。 例如，如果您想要使用OpenAI API，您可以这样做：

```py
import { HfAgent } from "@huggingface/agents";
import { Configuration, OpenAIApi } from "openai";

const api = new OpenAIApi(new Configuration({ apiKey: "sk-..." }));

const llmOpenAI = async (prompt: string): Promise<string> => {
  return (
    (
      await api.createCompletion({
        model: "text-davinci-003",
        prompt: prompt,
        max_tokens: 1000,
      })
    ).data.choices[0].text ?? ""
  );
};

const agent = new HfAgent(
  "hf_...",
  llmOpenAI
);

// do anything you want with the agent here

```

### 工具

默认情况下，代理程序配备有4种工具。 （textToImage，textToSpeech，imageToText，speechToText）

但是您可以通过创建新工具并在初始化时传递它们来轻松扩展工具列表。

```py
import { HfAgent, defaultTools, LLMFromHub } from "@huggingface/agents";
import type { Tool } from "@huggingface/agents/src/types";

// define the tool
const uppercaseTool: Tool = {
    name: "uppercase",
    description: "uppercase the input string and returns it ",
    examples: [
        {
            prompt: "uppercase the string: hello world",
            code: `const output = uppercase("hello world")`,
            tools: ["uppercase"],
        },
    ],
    call: async (input) => {
        const data = await input;
        if (typeof data !== "string") {
            throw new Error("Input must be a string");
        }
        return data.toUpperCase();
    },
};

// pass it in the agent
const agent = new HfAgent(process.env.HF_TOKEN,
                LLMFromHub("hf_...", "OpenAssistant/oasst-sft-4-pythia-12b-epoch-3.5"),
                [uppercaseTool, ...defaultTools]);
```

## 依赖

+   `@huggingface/inference`：需要调用推理端点本身。
