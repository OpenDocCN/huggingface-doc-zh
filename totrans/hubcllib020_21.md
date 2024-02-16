# Webhooks 服务器

> 原文链接：[`huggingface.co/docs/huggingface_hub/guides/webhooks_server`](https://huggingface.co/docs/huggingface_hub/guides/webhooks_server)

Webhooks 是 MLOps 相关功能的基础。它们允许您监听特定存储库或您感兴趣的特定用户/组织的所有存储库的新更改。本指南将解释如何利用`huggingface_hub`创建一个监听 webhooks 并将其部署到 Space 的服务器。它假定您熟悉 Huggingface Hub 上的 webhooks 概念。要了解有关 webhooks 本身的更多信息，您可以先阅读此指南。

我们在本指南中将使用的基类是 WebhooksServer()。这是一个用于轻松配置可以接收来自 Huggingface Hub 的 webhooks 的服务器的类。该服务器基于[Gradio](https://gradio.app/)应用程序。它具有用于显示指导信息的 UI，供您或您的用户使用，并具有用于监听 webhooks 的 API。

要查看一个正在运行的 webhook 服务器的示例，请查看[Spaces CI Bot](https://huggingface.co/spaces/spaces-ci-bot/webhook)。这是一个 Space，当在 Space 上打开 PR 时，它会启动临时环境。

这是一个实验性功能。这意味着我们仍在努力改进 API。未来可能会引入破坏性更改，而不事先通知。请确保在您的要求中固定`huggingface_hub`的版本。

## 创建一个端点

实现 webhook 端点就像装饰一个函数一样简单。让我们看一个第一个示例来解释主要概念：

```py
# app.py
from huggingface_hub import webhook_endpoint, WebhookPayload

@webhook_endpoint
async def trigger_training(payload: WebhookPayload) -> None:
    if payload.repo.type == "dataset" and payload.event.action == "update":
        # Trigger a training job if a dataset is updated
        ...
```

将此片段保存在名为`'app.py'`的文件中，并使用`'python app.py'`运行它。您应该会看到类似以下的消息：

```py
Webhook secret is not defined. This means your webhook endpoints will be open to everyone.
To add a secret, set `WEBHOOK_SECRET` as environment variable or pass it at initialization: 
        `app = WebhooksServer(webhook_secret='my_secret', ...)`
For more details about webhook secrets, please refer to https://huggingface.co/docs/hub/webhooks#webhook-secret.
Running on local URL:  http://127.0.0.1:7860
Running on public URL: https://1fadb0f52d8bf825fc.gradio.live

This share link expires in 72 hours. For free permanent hosting and GPU upgrades (NEW!), check out Spaces: https://huggingface.co/spaces

Webhooks are correctly setup and ready to use:
  - POST https://1fadb0f52d8bf825fc.gradio.live/webhooks/trigger_training
Go to https://huggingface.co/settings/webhooks to setup your webhooks.
```

干得好！您刚刚启动了一个 webhook 服务器！让我们详细解释一下发生了什么：

1.  通过使用 webhook_endpoint()装饰函数，后台已创建了一个 WebhooksServer()对象。正如您所看到的，此服务器是在[`127.0.0.1:7860`](http://127.0.0.1:7860)上运行的 Gradio 应用程序。如果在浏览器中打开此 URL，您将看到一个包含有关已注册 webhooks 的说明的登陆页面。

1.  Gradio 应用程序是 FastAPI 服务器的底层。它添加了一个新的 POST 路由`/webhooks/trigger_training`。这是将监听 webhooks 并在触发时运行`trigger_training`函数的路由。FastAPI 将自动解析有效负载并将其作为 WebhookPayload 对象传递给函数。这是一个包含有关触发 webhook 的事件的所有信息的`pydantic`对象。

1.  Gradio 应用程序还打开了一个隧道以接收来自互联网的请求。这是有趣的部分：您可以在[`huggingface.co/settings/webhooks`](https://huggingface.co/settings/webhooks)上配置一个指向本地机器的 Webhook。这对于调试您的 webhook 服务器并在部署到 Space 之前快速迭代非常有用。

1.  最后，日志还告诉您，您的服务器当前未受到秘密的保护。这对于本地调试并不成问题，但以后要记住。

默认情况下，服务器在脚本结束时启动。如果您在笔记本中运行它，可以通过调用`decorated_function.run()`手动启动服务器。由于使用唯一服务器，即使有多个端点，您也只需启动服务器一次。

## 配置 Webhook

现在您有一个正在运行的 webhook 服务器，您希望配置一个 Webhook 来开始接收消息。前往[`huggingface.co/settings/webhooks`](https://huggingface.co/settings/webhooks)，点击“添加新的 webhook”并配置您的 Webhook。设置您想要监视的目标存储库和 Webhook URL，例如`https://1fadb0f52d8bf825fc.gradio.live/webhooks/trigger_training`。

![](img/615611df3cad938ddc39a892ef73e17d.png)

就是这样！您现在可以通过更新目标存储库（例如推送提交）来触发该 webhook。查看您的 Webhook 的活动选项卡，查看已触发的事件。现在您有一个工作的设置，您可以测试并快速迭代。如果您修改了代码并重新启动服务器，您的公共 URL 可能会更改。如有需要，请确保在 Hub 上更新 webhook 配置。

## 部署到 Space

现在您已经有一个工作的 webhook 服务器，目标是将其部署到一个 Space。前往[`huggingface.co/new-space`](https://huggingface.co/new-space)创建一个 Space。给它一个名称，选择 Gradio SDK，然后点击“创建 Space”。将您的代码上传到名为`app.py`的 Space 文件中。您的 Space 将自动启动！有关 Spaces 的更多详细信息，请参考这个[指南](https://huggingface.co/docs/hub/spaces-overview)。

您的 webhook 服务器现在正在一个公共 Space 上运行。在大多数情况下，您会希望用一个密钥来保护它。前往您的 Space 设置 > “存储库密钥”部分 > “添加密钥”。将`WEBHOOK_SECRET`环境变量设置为您选择的值。返回到[Webhooks 设置](https://huggingface.co/settings/webhooks)并在 webhook 配置中设置密钥。现在，只有带有正确密钥的请求才会被服务器接受。

就是这样！您的 Space 现在已准备好从 Hub 接收 webhooks。请记住，如果您在免费的‘cpu-basic’硬件上运行 Space，在 48 小时内没有活动后，它将被关闭。如果您需要一个永久的 Space，您应该考虑升级硬件设置到[升级硬件](https://huggingface.co/docs/hub/spaces-gpus#hardware-specs)。

## 高级用法

上面的指南解释了设置 WebhooksServer()的最快方法。在本节中，我们将看到如何进一步自定义它。

### 多个端点

您可以在同一个服务器上注册多个端点。例如，您可能希望有一个端点来触发训练作业，另一个端点来触发模型评估。您可以通过添加多个`@webhook_endpoint`装饰器来实现这一点：

```py
# app.py
from huggingface_hub import webhook_endpoint, WebhookPayload

@webhook_endpoint
async def trigger_training(payload: WebhookPayload) -> None:
    if payload.repo.type == "dataset" and payload.event.action == "update":
        # Trigger a training job if a dataset is updated
        ...

@webhook_endpoint
async def trigger_evaluation(payload: WebhookPayload) -> None:
    if payload.repo.type == "model" and payload.event.action == "update":
        # Trigger an evaluation job if a model is updated
        ...
```

这将创建两个端点：

```py
(...)
Webhooks are correctly setup and ready to use:
  - POST https://1fadb0f52d8bf825fc.gradio.live/webhooks/trigger_training
  - POST https://1fadb0f52d8bf825fc.gradio.live/webhooks/trigger_evaluation
```

### 自定义服务器

为了获得更多的灵活性，您也可以直接创建一个 WebhooksServer()对象。如果您想要自定义服务器的登陆页面，这将非常有用。您可以通过传递一个[Gradio UI](https://gradio.app/docs/#blocks)来覆盖默认页面。例如，您可以为用户添加说明或添加一个表单来手动触发 webhooks。在创建 WebhooksServer()时，您可以使用`add_webhook()`装饰器注册新的 webhooks。

这里是一个完整的例子：

```py
import gradio as gr
from fastapi import Request
from huggingface_hub import WebhooksServer, WebhookPayload

# 1\. Define  UI
with gr.Blocks() as ui:
    ...

# 2\. Create WebhooksServer with custom UI and secret
app = WebhooksServer(ui=ui, webhook_secret="my_secret_key")

# 3\. Register webhook with explicit name
@app.add_webhook("/say_hello")
async def hello(payload: WebhookPayload):
    return {"message": "hello"}

# 4\. Register webhook with implicit name
@app.add_webhook
async def goodbye(payload: WebhookPayload):
    return {"message": "goodbye"}

# 5\. Start server (optional)
app.run()
```

1.  我们使用 Gradio blocks 定义了一个自定义 UI。这个 UI 将显示在服务器的登陆页面上。

1.  我们创建了一个带有自定义 UI 和密钥的 WebhooksServer()对象。密钥是可选的，可以使用`WEBHOOK_SECRET`环境变量设置。

1.  我们注册了一个显式名称的 webhook。这将在`/webhooks/say_hello`创建一个端点。

1.  我们注册了一个隐式名称的 webhook。这将在`/webhooks/goodbye`创建一个端点。

1.  我们启动服务器。这是可选的，因为在脚本结束时，您的服务器将自动启动。
