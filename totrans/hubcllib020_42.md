# Webhooks服务器

> 原始文本：[https://huggingface.co/docs/huggingface_hub/package_reference/webhooks_server](https://huggingface.co/docs/huggingface_hub/package_reference/webhooks_server)

Webhooks是与MLOps相关功能的基础。它们允许您监听特定存储库或您感兴趣关注的特定用户/组织的所有存储库的新更改。要了解有关Huggingface Hub上webhooks的更多信息，您可以阅读Webhooks[指南](https://huggingface.co/docs/hub/webhooks)。

查看此[指南](../guides/webhooks_server)以逐步教程，了解如何设置您的webhooks服务器并将其部署为Space。

这是一个实验性功能。这意味着我们仍在努力改进API。未来可能会引入重大更改而不事先通知。请确保在您的要求中固定`huggingface_hub`的版本。在使用实验性功能时会触发警告。您可以通过将`HF_HUB_DISABLE_EXPERIMENTAL_WARNING=1`设置为环境变量来禁用它。

## 服务器

服务器是一个[Gradio](https://gradio.app/)应用程序。它具有一个用户界面，用于显示指导用户的说明，并具有一个API来监听webhooks。实现webhook端点就像装饰一个函数一样简单。您可以通过将Webhooks重定向到您的机器（使用Gradio隧道）来调试它，然后再部署到Space。

### WebhooksServer

#### `huggingface_hub.WebhooksServer`

[<源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_webhooks_server.py#L38)

```py
( *args **kwargs )
```

参数

+   `ui` (`gradio.Blocks`, 可选) — 用作Space登陆页面的Gradio UI实例。如果为`None`，则会创建一个显示有关配置的webhooks的说明的UI。

+   `webhook_secret` (`str`, 可选) — 用于验证传入webhook请求的秘钥。只要您在[webhooks设置面板](https://huggingface.co/settings/webhooks)中配置了该值，您可以将此值设置为任何您想要的秘钥。您还可以将此值设置为`WEBHOOK_SECRET`环境变量。如果未提供秘钥，则webhook端点将在没有任何安全性的情况下打开。

[WebhooksServer()](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.WebhooksServer)类允许您创建一个可以接收Huggingface webhooks的Gradio应用程序实例。可以使用`add_webhook()`装饰器注册这些webhooks。Webhook端点将作为POST端点添加到FastAPI路由器中。一旦所有webhooks都注册完毕，必须调用`run`方法来启动应用程序。

建议将[WebhookPayload](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.WebhookPayload)作为webhook函数的第一个参数。它是一个Pydantic模型，包含有关webhook事件的所有信息。数据将自动为您解析。

查看[webhooks指南](../guides/webhooks_server)以逐步教程，了解如何设置您的WebhooksServer并将其部署到Space。

`WebhooksServer`是实验性的。其API可能会在未来发生变化。

您必须安装`gradio`才能使用`WebhooksServer`（`pip install --upgrade gradio`）。

示例：

```py
import gradio as gr
from huggingface_hub import WebhooksServer, WebhookPayload

with gr.Blocks() as ui:
    ...

app = WebhooksServer(ui=ui, webhook_secret="my_secret_key")

@app.add_webhook("/say_hello")
async def hello(payload: WebhookPayload):
    return {"message": "hello"}

app.run()
```

### @webhook_endpoint

#### `huggingface_hub.webhook_endpoint`

[<源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_webhooks_server.py#L223)

```py
( path: Optional = None )
```

参数

+   `path` (`str`, 可选) — 要注册webhook函数的URL路径。如果未提供，则将使用函数名作为路径。无论如何，所有webhooks都在`/webhooks`下注册。

装饰器用于启动[WebhooksServer()](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.WebhooksServer)并将装饰的函数注册为webhook端点。

这是一个快速入门的辅助工具。如果需要更多灵活性（自定义落地页或 Webhook 密钥），可以直接使用 [WebhooksServer()](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.WebhooksServer)。您可以多次使用此装饰器注册多个 Webhook 端点（到同一服务器）。

查看 [webhooks 指南](../guides/webhooks_server) 以逐步教程如何设置服务器并在 Space 上部署它。

`webhook_endpoint` 是实验性的。其 API 可能会在将来更改。

您必须安装 `gradio` 才能使用 `webhook_endpoint` (`pip install --upgrade gradio`)。

示例：默认用法是将函数注册为 Webhook 端点。函数名称将用作路径。服务器将在退出时自动启动（即在脚本结束时）。

```py
from huggingface_hub import webhook_endpoint, WebhookPayload

@webhook_endpoint
async def trigger_training(payload: WebhookPayload):
    if payload.repo.type == "dataset" and payload.event.action == "update":
        # Trigger a training job if a dataset is updated
        ...

# Server is automatically started at the end of the script.
```

高级用法：将函数注册为 Webhook 端点并手动启动服务器。如果您在笔记本中运行它，这将非常有用。

```py
from huggingface_hub import webhook_endpoint, WebhookPayload

@webhook_endpoint
async def trigger_training(payload: WebhookPayload):
    if payload.repo.type == "dataset" and payload.event.action == "update":
        # Trigger a training job if a dataset is updated
        ...

# Start the server manually
trigger_training.run()
```

## Payload

[WebhookPayload](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.WebhookPayload) 是包含来自 Webhooks 的 payload 的主要数据结构。这是一个 `pydantic` 类，非常容易与 FastAPI 一起使用。如果将其作为参数传递给 Webhook 端点，它将自动验证并解析为 Python 对象。

有关 Webhooks payload 的更多信息，您可以参考 Webhooks Payload [指南](https://huggingface.co/docs/hub/webhooks#webhook-payloads)。

### `类 huggingface_hub.WebhookPayload`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_webhooks_payload.py#L109)

```py
( event: WebhookPayloadEvent repo: WebhookPayloadRepo discussion: Optional = None comment: Optional = None webhook: WebhookPayloadWebhook movedTo: Optional = None )
```

### WebhookPayload

### `类 huggingface_hub.WebhookPayload`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_webhooks_payload.py#L109)

```py
( event: WebhookPayloadEvent repo: WebhookPayloadRepo discussion: Optional = None comment: Optional = None webhook: WebhookPayloadWebhook movedTo: Optional = None )
```

### WebhookPayloadComment

### `类 huggingface_hub.WebhookPayloadComment`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_webhooks_payload.py#L80)

```py
( id: str author: ObjectId hidden: bool content: Optional = None url: WebhookPayloadUrl )
```

### WebhookPayloadDiscussion

### `类 huggingface_hub.WebhookPayloadDiscussion`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_webhooks_payload.py#L87)

```py
( id: str num: int author: ObjectId url: WebhookPayloadUrl title: str isPullRequest: bool status: Literal changes: Optional = None pinned: Optional = None )
```

### WebhookPayloadDiscussionChanges

### `类 huggingface_hub.WebhookPayloadDiscussionChanges`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_webhooks_payload.py#L75)

```py
( base: str mergeCommitId: Optional = None )
```

### WebhookPayloadEvent

### `类 huggingface_hub.WebhookPayloadEvent`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_webhooks_payload.py#L70)

```py
( action: Literal scope: str )
```

### WebhookPayloadMovedTo

### `类 huggingface_hub.WebhookPayloadMovedTo`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_webhooks_payload.py#L61)

```py
( name: str owner: ObjectId )
```

### WebhookPayloadRepo

### `类 huggingface_hub.WebhookPayloadRepo`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_webhooks_payload.py#L98)

```py
( id: str owner: ObjectId head_sha: Optional = None name: str private: bool subdomain: Optional = None tags: Optional = None type: Literal url: WebhookPayloadUrl )
```

### WebhookPayloadUrl

### `类 huggingface_hub.WebhookPayloadUrl`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_webhooks_payload.py#L56)

```py
( web: str api: Optional = None )
```

### WebhookPayloadWebhook

### `类 huggingface_hub.WebhookPayloadWebhook`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_webhooks_payload.py#L66)

```py
( id: str version: Literal )
```
