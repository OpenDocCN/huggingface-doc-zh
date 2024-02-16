# Webhooks服务器

> 原文链接：[https://huggingface.co/docs/huggingface_hub/guides/webhooks_server](https://huggingface.co/docs/huggingface_hub/guides/webhooks_server)

Webhooks是MLOps相关功能的基础。它们允许您监听特定存储库或您感兴趣的特定用户/组织的所有存储库的新更改。本指南将解释如何利用`huggingface_hub`创建一个监听webhooks并将其部署到Space的服务器。它假定您熟悉Huggingface Hub上的webhooks概念。要了解有关webhooks本身的更多信息，您可以先阅读此指南。

我们在本指南中将使用的基类是[WebhooksServer()](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.WebhooksServer)。这是一个用于轻松配置可以接收来自Huggingface Hub的webhooks的服务器的类。该服务器基于[Gradio](https://gradio.app/)应用程序。它具有用于显示指导信息的UI，供您或您的用户使用，并具有用于监听webhooks的API。

要查看一个正在运行的webhook服务器的示例，请查看[Spaces CI Bot](https://huggingface.co/spaces/spaces-ci-bot/webhook)。这是一个Space，当在Space上打开PR时，它会启动临时环境。

这是一个实验性功能。这意味着我们仍在努力改进API。未来可能会引入破坏性更改，而不事先通知。请确保在您的要求中固定`huggingface_hub`的版本。

## 创建一个端点

实现webhook端点就像装饰一个函数一样简单。让我们看一个第一个示例来解释主要概念：

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

干得好！您刚刚启动了一个webhook服务器！让我们详细解释一下发生了什么：

1.  通过使用[webhook_endpoint()](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.webhook_endpoint)装饰函数，后台已创建了一个[WebhooksServer()](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.WebhooksServer)对象。正如您所看到的，此服务器是在[http://127.0.0.1:7860](http://127.0.0.1:7860)上运行的Gradio应用程序。如果在浏览器中打开此URL，您将看到一个包含有关已注册webhooks的说明的登陆页面。

1.  Gradio应用程序是FastAPI服务器的底层。它添加了一个新的POST路由`/webhooks/trigger_training`。这是将监听webhooks并在触发时运行`trigger_training`函数的路由。FastAPI将自动解析有效负载并将其作为[WebhookPayload](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.WebhookPayload)对象传递给函数。这是一个包含有关触发webhook的事件的所有信息的`pydantic`对象。

1.  Gradio应用程序还打开了一个隧道以接收来自互联网的请求。这是有趣的部分：您可以在[https://huggingface.co/settings/webhooks](https://huggingface.co/settings/webhooks)上配置一个指向本地机器的Webhook。这对于调试您的webhook服务器并在部署到Space之前快速迭代非常有用。

1.  最后，日志还告诉您，您的服务器当前未受到秘密的保护。这对于本地调试并不成问题，但以后要记住。

默认情况下，服务器在脚本结束时启动。如果您在笔记本中运行它，可以通过调用`decorated_function.run()`手动启动服务器。由于使用唯一服务器，即使有多个端点，您也只需启动服务器一次。

## 配置Webhook

现在您有一个正在运行的webhook服务器，您希望配置一个Webhook来开始接收消息。前往[https://huggingface.co/settings/webhooks](https://huggingface.co/settings/webhooks)，点击“添加新的webhook”并配置您的Webhook。设置您想要监视的目标存储库和Webhook URL，例如`https://1fadb0f52d8bf825fc.gradio.live/webhooks/trigger_training`。

![](../Images/615611df3cad938ddc39a892ef73e17d.png)

就是这样！您现在可以通过更新目标存储库（例如推送提交）来触发该webhook。查看您的Webhook的活动选项卡，查看已触发的事件。现在您有一个工作的设置，您可以测试并快速迭代。如果您修改了代码并重新启动服务器，您的公共URL可能会更改。如有需要，请确保在Hub上更新webhook配置。

## 部署到Space

现在您已经有一个工作的webhook服务器，目标是将其部署到一个Space。前往[https://huggingface.co/new-space](https://huggingface.co/new-space)创建一个Space。给它一个名称，选择Gradio SDK，然后点击“创建Space”。将您的代码上传到名为`app.py`的Space文件中。您的Space将自动启动！有关Spaces的更多详细信息，请参考这个[指南](https://huggingface.co/docs/hub/spaces-overview)。

您的webhook服务器现在正在一个公共Space上运行。在大多数情况下，您会希望用一个密钥来保护它。前往您的Space设置 > “存储库密钥”部分 > “添加密钥”。将`WEBHOOK_SECRET`环境变量设置为您选择的值。返回到[Webhooks设置](https://huggingface.co/settings/webhooks)并在webhook配置中设置密钥。现在，只有带有正确密钥的请求才会被服务器接受。

就是这样！您的Space现在已准备好从Hub接收webhooks。请记住，如果您在免费的‘cpu-basic’硬件上运行Space，在48小时内没有活动后，它将被关闭。如果您需要一个永久的Space，您应该考虑升级硬件设置到[升级硬件](https://huggingface.co/docs/hub/spaces-gpus#hardware-specs)。

## 高级用法

上面的指南解释了设置[WebhooksServer()](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.WebhooksServer)的最快方法。在本节中，我们将看到如何进一步自定义它。

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

为了获得更多的灵活性，您也可以直接创建一个[WebhooksServer()](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.WebhooksServer)对象。如果您想要自定义服务器的登陆页面，这将非常有用。您可以通过传递一个[Gradio UI](https://gradio.app/docs/#blocks)来覆盖默认页面。例如，您可以为用户添加说明或添加一个表单来手动触发webhooks。在创建[WebhooksServer()](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.WebhooksServer)时，您可以使用`add_webhook()`装饰器注册新的webhooks。

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

1.  我们使用Gradio blocks定义了一个自定义UI。这个UI将显示在服务器的登陆页面上。

1.  我们创建了一个带有自定义UI和密钥的[WebhooksServer()](/docs/huggingface_hub/v0.20.3/en/package_reference/webhooks_server#huggingface_hub.WebhooksServer)对象。密钥是可选的，可以使用`WEBHOOK_SECRET`环境变量设置。

1.  我们注册了一个显式名称的webhook。这将在`/webhooks/say_hello`创建一个端点。

1.  我们注册了一个隐式名称的webhook。这将在`/webhooks/goodbye`创建一个端点。

1.  我们启动服务器。这是可选的，因为在脚本结束时，您的服务器将自动启动。
