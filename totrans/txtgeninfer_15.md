# 流式

> 原始文本：[https://huggingface.co/docs/text-generation-inference/conceptual/streaming](https://huggingface.co/docs/text-generation-inference/conceptual/streaming)

## 什么是流式？

令牌流是服务器逐个返回令牌的模式，当模型生成它们时。这使得用户可以逐步看到生成的结果，而不必等待整个生成。流式是端用户体验的一个重要方面，因为它减少了延迟，这是流畅体验中最关键的方面之一。

![](../Images/da4a03ca9d5a29fb32448f9771ddb359.png) ![](../Images/e9e351d288d9a7be620080222481534f.png)

使用令牌流，服务器可以在生成整个响应之前逐个返回令牌。用户可以在生成结束之前更早地感知到生成的质量。这具有不同的积极影响：

+   用户可以更早地获得数量级更高的结果，用于极长的查询。

+   在进行中看到某些内容使用户可以停止生成，如果生成不符合他们的期望。

+   当在早期阶段显示结果时，感知延迟较低。

+   在对话式用户界面中使用时，体验感觉更加自然。

例如，系统每秒可以生成100个令牌。如果系统生成1000个令牌，使用非流式设置，用户需要等待10秒才能获得结果。另一方面，使用流式设置，用户立即获得初始结果，尽管端到端延迟将是相同的，但他们可以在五秒后看到一半的生成。下面您可以看到一个交互式演示，显示非流式与流式并排。点击下面的**generate**。

[https://osanseviero-streaming-vs-non-streaming.hf.space?__theme=light](https://osanseviero-streaming-vs-non-streaming.hf.space?__theme=light)

[https://osanseviero-streaming-vs-non-streaming.hf.space?__theme=dark](https://osanseviero-streaming-vs-non-streaming.hf.space?__theme=dark)

## 如何使用流式传输？

### 使用Python进行流式传输

要使用`InferenceClient`流式传输令牌，只需传递`stream=True`并迭代响应。

```py
from huggingface_hub import InferenceClient

client = InferenceClient("http://127.0.0.1:8080")
for token in client.text_generation("How do you make cheese?", max_new_tokens=12, stream=True):
    print(token)

# To
# make
# cheese
#,
# you
# need
# to
# start
# with
# milk
#.
```

如果您想要更多详细信息，可以添加`details=True`。在这种情况下，您将获得一个包含额外信息（如概率和令牌）的`TextGenerationStreamResponse`。对于流中的最终响应，它还会返回完整生成的文本。

```py
for details in client.text_generation("How do you make cheese?", max_new_tokens=12, details=True, stream=True):
    print(details)

#TextGenerationStreamResponse(token=Token(id=193, text='\n', logprob=-0.007358551, special=False), generated_text=None, details=None)
#TextGenerationStreamResponse(token=Token(id=2044, text='To', logprob=-1.1357422, special=False), generated_text=None, details=None)
#TextGenerationStreamResponse(token=Token(id=717, text=' make', logprob=-0.009841919, special=False), generated_text=None, details=None)
#...
#TextGenerationStreamResponse(token=Token(id=25, text='.', logprob=-1.3408203, special=False), generated_text='\nTo make cheese, you need to start with milk.', details=StreamDetails(finish_reason=<FinishReason.Length: 'length'>, generated_tokens=12, seed=None))
```

`huggingface_hub`库还提供了`AsyncInferenceClient`，以便您可以同时处理请求。

```py
from huggingface_hub import AsyncInferenceClient

client = AsyncInferenceClient("http://127.0.0.1:8080")
async for token in await client.text_generation("How do you make cheese?", stream=True):
    print(token)

# To
# make
# cheese
#,
# you
# need
# to
# start
# with
# milk
#.
```

### 使用cURL进行流式传输

使用`generate_stream`端点与curl一起使用时，您可以添加`-N`标志，该标志禁用curl的默认缓冲并在数据从服务器到达时显示数据

```py
curl -N 127.0.0.1:8080/generate_stream \
    -X POST \
    -d '{"inputs":"What is Deep Learning?","parameters":{"max_new_tokens":20}}' \
    -H 'Content-Type: application/json'
```

### 使用JavaScript进行流式传输

首先，我们需要安装`@huggingface/inference`库。`npm install @huggingface/inference`

如果您正在使用免费的推理API，您可以使用`HfInference`。如果您正在使用推理端点，您可以使用`HfInferenceEndpoint`。让我们

我们可以创建一个提供我们端点URL和凭据的`HfInferenceEndpoint`。

```py
import { HfInferenceEndpoint } from '@huggingface/inference'

const hf = new HfInferenceEndpoint('https://YOUR_ENDPOINT.endpoints.huggingface.cloud', 'hf_YOUR_TOKEN')

// prompt
const prompt = 'What can you do in Nuremberg, Germany? Give me 3 Tips'

const stream = hf.textGenerationStream({ inputs: prompt })
for await (const r of stream) { 
  // yield the generated token
  process.stdout.write(r.token.text)
}
```

## 流式工作原理是什么？

在幕后，TGI使用服务器发送事件（SSE）。在SSE设置中，客户端发送带有数据的请求，打开HTTP连接并订阅更新。之后，服务器将数据发送给客户端。不需要进一步的请求；服务器将继续发送数据。SSE是单向的，这意味着客户端不会向服务器发送其他请求。SSE通过HTTP发送数据，使用起来很容易。

SSE与以下内容不同：

+   轮询：客户端不断调用服务器以获取数据。这意味着服务器可能会返回空响应并导致开销。

+   Webhooks：其中存在双向连接。服务器可以向客户端发送信息，但客户端也可以在第一次请求后向服务器发送数据。Webhooks的操作更复杂，因为它们不仅使用HTTP。

如果同时有太多请求，TGI会返回一个带有`overloaded`错误类型的HTTP错误（`huggingface_hub`返回`OverloadedError`）。这允许客户端管理过载的服务器（例如，它可以向用户显示繁忙错误或使用新请求重试）。要配置最大并发请求数，可以指定`--max_concurrent_requests`，允许客户端处理背压。
