# 消费文本生成推理

> 原始文本：[`huggingface.co/docs/text-generation-inference/basic_tutorials/consuming_tgi`](https://huggingface.co/docs/text-generation-inference/basic_tutorials/consuming_tgi)

有许多方法可以在应用程序中使用文本生成推理服务器。启动后，您可以使用`/generate`路由并发出`POST`请求以从服务器获取结果。如果希望 TGI 返回标记流，则还可以使用`/generate_stream`路由。您可以使用您喜欢的工具（如 curl、Python 或 TypeScrpt）发出请求。为了获得最终的端到端体验，我们还开源了 ChatUI，这是一个用于开源模型的聊天界面。

## curl

启动后，可以使用`/generate`或`/generate_stream`路由查询模型：

```py
curl 127.0.0.1:8080/generate \
    -X POST \
    -d '{"inputs":"What is Deep Learning?","parameters":{"max_new_tokens":20}}' \
    -H 'Content-Type: application/json'
```

## 推理客户端

[`huggingface-hub`](https://huggingface.co/docs/huggingface_hub/main/en/index)是一个用于与 Hugging Face Hub 交互的 Python 库，包括其端点。它提供了一个很好的高级类[`~huggingface_hub.InferenceClient`]，使得调用 TGI 端点变得容易。`InferenceClient`还负责参数验证并提供一个简单易用的接口。您可以通过 pip 简单安装`huggingface-hub`包。

```py
pip install huggingface-hub
```

一旦启动 TGI 服务器，请使用提供模型的端点的 URL 实例化`InferenceClient()`。然后可以通过 Python 调用`text_generation()`来访问端点。

```py
from huggingface_hub import InferenceClient

client = InferenceClient(model="http://127.0.0.1:8080")
client.text_generation(prompt="Write a code for snake game")
```

通过传递`stream=True`，您可以使用`InferenceClient`进行流式处理。流式处理将在服务器生成标记时返回标记。要使用流式处理，可以按照以下步骤操作：

```py
for token in client.text_generation("How do you make cheese?", max_new_tokens=12, stream=True):
    print(token)
```

您可以在 TGI 后端中使用的另一个参数是`details`。通过将`details`设置为`True`，可以获取有关生成（标记、概率等）的更多详细信息。当指定时，TGI 将返回`TextGenerationResponse`或`TextGenerationStreamResponse`而不是字符串或流。

```py
output = client.text_generation(prompt="Meaning of life is", details=True)
print(output)

# TextGenerationResponse(generated_text=' a complex concept that is not always clear to the individual. It is a concept that is not always', details=Details(finish_reason=<FinishReason.Length: 'length'>, generated_tokens=20, seed=None, prefill=[], tokens=[Token(id=267, text=' a', logprob=-2.0723474, special=False), Token(id=11235, text=' complex', logprob=-3.1272552, special=False), Token(id=17908, text=' concept', logprob=-1.3632495, special=False),..))
```

您可以查看下面的流式处理方式。

```py
output = client.text_generation(prompt="Meaning of life is", stream=True, details=True)
print(next(iter(output)))

# TextGenerationStreamResponse(token=Token(id=267, text=' a', logprob=-2.0723474, special=False), generated_text=None, details=None)
```

您可以在[此处](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/inference_client#huggingface_hub.InferenceClient.text_generation)查看该函数的详细信息。还有一个基于`asyncio`和`aiohttp`的客户端的异步版本`AsyncInferenceClient`。您可以在[此处](https://huggingface.co/docs/huggingface_hub/package_reference/inference_client#huggingface_hub.AsyncInferenceClient)找到其文档。

## ChatUI

ChatUI 是为 LLM 服务构建的开源界面。它提供许多自定义选项，例如使用 SERP API 进行网络搜索等。ChatUI 可以自动消费 TGI 服务器，甚至提供在不同 TGI 端点之间切换的选项。您可以在[Hugging Chat](https://huggingface.co/chat/)尝试它，或使用[ChatUI Docker Space](https://huggingface.co/new-space?template=huggingchat/chat-ui-template)部署自己的 Hugging Chat 到 Spaces。

要在同一环境中同时提供 ChatUI 和 TGI 服务，只需将您自己的端点添加到`chat-ui`存储库中的`.env.local`文件中的`MODELS`变量中。提供指向 TGI 服务位置的端点。

```py
{
// rest of the model config here
"endpoints": [{"url": "https://HOST:PORT/generate_stream"}]
}
```

![ChatUI](img/ce268cbdae69aa7842502b673821bebb.png)

## Gradio

Gradio 是一个 Python 库，可以帮助您使用几行代码为机器学习模型构建 Web 应用程序。它具有一个`ChatInterface`包装器，可帮助创建聊天机器人的整洁 UI。让我们先安装 Gradio 和 Hub Python 库。

```py
pip install huggingface-hub gradio
```

假设您在端口 8080 上提供模型，我们将通过 InferenceClient 进行查询。

```py
import gradio as gr
from huggingface_hub import InferenceClient

client = InferenceClient(model="http://127.0.0.1:8080")

def inference(message, history):
    partial_message = ""
    for token in client.text_generation(message, max_new_tokens=20, stream=True):
        partial_message += token
        yield partial_message

gr.ChatInterface(
    inference,
    chatbot=gr.Chatbot(height=300),
    textbox=gr.Textbox(placeholder="Chat with me!", container=False, scale=7),
    description="This is the demo for Gradio UI consuming TGI endpoint with LLaMA 7B-Chat model.",
    title="Gradio 🤝 TGI",
    examples=["Are tomatoes vegetables?"],
    retry_btn="Retry",
    undo_btn="Undo",
    clear_btn="Clear",
).queue().launch()
```

UI 如下👇

![](img/145df95e03868e6e18c9588c0878b82a.png) ![](img/af0024df3a67614ca6a101e75f4cdb9c.png)

您可以直接在此处尝试演示👇

[`merve-gradio-tgi-2.hf.space?__theme=light`](https://merve-gradio-tgi-2.hf.space?__theme=light)

[`merve-gradio-tgi-2.hf.space?__theme=dark`](https://merve-gradio-tgi-2.hf.space?__theme=dark)

您可以在推断函数中使用`return`而不是`yield`来禁用流式模式，就像下面这样。

```py
def inference(message, history):
    return client.text_generation(message, max_new_tokens=20)
```

您可以阅读有关如何自定义`ChatInterface`的更多信息[这里](https://www.gradio.app/guides/creating-a-chatbot-fast)。

## API 文档

您可以使用`/docs`路由查阅`text-generation-inference` REST API 的 OpenAPI 文档。Swagger UI 也可在[这里](https://huggingface.github.io/text-generation-inference)找到。
