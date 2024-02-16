# 向您的 Space 添加“使用 HF 登录”按钮

> 原始文本：[`huggingface.co/docs/hub/spaces-oauth`](https://huggingface.co/docs/hub/spaces-oauth)

您可以通过无缝创建和关联[OAuth/OpenID 连接](https://developer.okta.com/blog/2019/10/21/illustrated-guide-to-oauth-and-oidc)应用程序，使用户可以使用其 HF 帐户登录，从而在您的 Space 中启用内置登录流程。

这为您的 Space 提供了新的用例。例如，与[持久存储](https://huggingface.co/docs/hub/spaces-storage)结合使用时，生成式 AI Space 可以允许用户登录以访问他们以前的生成，仅对他们可见。

本指南将带您完成将*使用 HF 登录*按钮集成到任何 Space 中的过程。如果您正在寻找一种快速简单的方法在**Gradio** Space 中实现此功能，请查看其[内置集成](https://www.gradio.app/guides/sharing-your-app#o-auth-login-via-hugging-face)。

您还可以使用 HF OAuth 流程在任何网站或应用程序中创建“使用 HF 登录”流程，而不仅限于 Spaces。阅读我们的通用 OAuth 页面。

## 创建一个 OAuth 应用程序

只需在您的`README.md`文件中的 Space 元数据中添加`hf_oauth: true`即可。

这是一个 Gradio Space 的元数据示例：

```py
title: Gradio Oauth Test
emoji: 🏆
colorFrom: pink
colorTo: pink
sdk: gradio
sdk_version: 3.40.0
python_version: 3.10.6
app_file: app.py

hf_oauth: true
# optional, see "Scopes" below. "openid profile" is always included.
hf_oauth_scopes:
 - read-repos
 - write-repos
 - manage-repos
 - inference-api
```

您可以查看更多信息的配置参考文档。

这将向您的 Space 添加以下[环境变量](https://huggingface.co/docs/hub/spaces-overview#helper-environment-variables)：

+   `OAUTH_CLIENT_ID`：您的 OAuth 应用程序的客户端 ID（公共）

+   `OAUTH_CLIENT_SECRET`：您的 OAuth 应用程序的客户端密钥

+   `OAUTH_SCOPES`：您的 OAuth 应用程序可访问的范围。

+   `OPENID_PROVIDER_URL`：OpenID 提供者的 URL。 OpenID 元数据将在[`{OPENID_PROVIDER_URL}/.well-known/openid-configuration`](https://huggingface.co/.well-known/openid-configuration)中可用。

对于任何其他环境变量，您可以通过使用`os.getenv("OAUTH_CLIENT_ID")`在您的代码中使用它们，例如。

## 重定向 URL

您可以使用任何重定向 URL，只要它指向您的 Space 即可。

请注意，`SPACE_HOST`作为[环境变量](https://huggingface.co/docs/hub/spaces-overview#helper-environment-variables)可用。

例如，您可以使用`https://{SPACE_HOST}/login/callback`作为重定向 URI。

## 范围

以下范围始终包含在 Spaces 中：

+   `openid`：除了访问令牌外，还获取 ID 令牌。

+   `profile`：获取用户的个人资料信息（用户名、头像等）。

这些范围是可选的，可以通过在 Space 的元数据中设置`hf_oauth_scopes`来添加：

+   `email`：获取用户的电子邮件地址。

+   `read-repos`：获取用户个人仓库的读取权限。

+   `write-repos`：获取用户个人仓库的写入权限。单独不授予读取权限，您还需要包括`read-repos`。

+   `manage-repos`：获取对仓库设置的访问权限。还授予仓库创建和删除权限。

+   `inference-api`：获取访问[推理 API](https://huggingface.co/docs/api-inference/index)，您将能够代表用户进行推理请求。

## 将按钮添加到您的 Space

您现在拥有所有信息，可以将“使用 HF 登录”按钮添加到您的 Space 中。一些库（[Python](https://github.com/lepture/authlib)，[NodeJS](https://github.com/panva/node-openid-client)）可以帮助您实现 OpenID/OAuth 协议。

Gradio 和 hugginface.js 还提供**内置支持**，使得实现“使用 HF 登录”按钮变得轻而易举；您可以查看与[gradio](https://www.gradio.app/guides/sharing-your-app#o-auth-login-via-hugging-face)和[hugginface.js](https://huggingface.co/docs/huggingface.js/hub/README#oauth-login)相关的指南。

基本上，您需要：

+   将用户重定向到`https://huggingface.co/oauth/authorize?redirect_uri={REDIRECT_URI}&scope=openid%20profile&client_id={CLIENT_ID}&state={STATE}`，其中`STATE`是一个随机字符串，稍后您需要验证。

+   处理`/auth/callback`或`/login/callback`（或您自己的自定义回调 URL）上的回调，并验证`state`参数。

+   使用`code`查询参数从`https://huggingface.co/oauth/token`获取访问令牌和 id 令牌（使用`client_id`，`code`，`grant_type=authorization_code`和`redirect_uri`作为表单数据进行 POST 请求，并使用`Authorization: Basic {base64(client_id:client_secret)}`作为标头）。

在按钮上使用`target=_blank`，在新标签页中打开登录页面，除非您在其`iframe`之外运行空间。否则，您可能会在某些浏览器上遇到有关 cookie 的问题。

## 示例：

+   [Gradio 测试应用程序](https://huggingface.co/spaces/Wauplin/gradio-oauth-test)

+   [Hugging Chat（NodeJS/SvelteKit）](https://huggingface.co/spaces/huggingchat/chat-ui)

+   [推理小部件（Auth.js/SvelteKit）](https://huggingface.co/spaces/huggingfacejs/inference-widgets)，使用`inference-api`范围代表用户进行推理请求。

+   [客户端在静态空间中（huggingface.js）](https://huggingface.co/spaces/huggingfacejs/client-side-oauth) - 非常简单的 JavaScript 示例。

JS 代码示例：

```py
import { oauthLoginUrl, oauthHandleRedirectIfPresent } from "@huggingface/hub";

const oauthResult = await oauthHandleRedirectIfPresent();

if (!oauthResult) {
  // If the user is not logged in, redirect to the login page
  window.location.href = await oauthLoginUrl();
}

// You can use oauthResult.accessToken, oauthResult.userInfo among other things
console.log(oauthResult);
```
