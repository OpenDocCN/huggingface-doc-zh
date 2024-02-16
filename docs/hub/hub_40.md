# Static HTML Spaces

> 原始文本：[`huggingface.co/docs/hub/spaces-sdks-static`](https://huggingface.co/docs/hub/spaces-sdks-static)

Spaces 还可以为您的应用程序提供自定义 HTML，而不是使用 Streamlit 或 Gradio。在 Spaces **README.md** 文件的顶部的 `YAML` 块内设置 `sdk: static`。然后，您可以将 HTML 代码放在一个 **index.html** 文件中。

以下是一些使用自定义 HTML 的 Spaces 示例：

+   [Smarter NPC](https://huggingface.co/spaces/mishig/smarter_npc): 在 Spaces 中使用 iframe 显示 PlayCanvas 项目。

+   [Huggingfab](https://huggingface.co/spaces/pierreant-p/huggingfab): 在 Spaces 中显示一个 Sketchfab 模型。

## Space 变量

可以将自定义环境变量传递给您的 Space。如果为您的 Space 启用了 OAuth，那么 OAuth 信息，如客户端 ID 和范围，也可以作为环境变量使用。

要在 JavaScript 中使用这些变量，可以使用 `window.huggingface.variables` 对象。例如，要访问 `OAUTH_CLIENT_ID` 变量，可以使用 `window.huggingface.variables.OAUTH_CLIENT_ID`。

以下是一个使用自定义环境变量和启用 oauth 并在 HTML 中显示变量的 Space 示例：

+   [Static Variables](https://huggingface.co/spaces/huggingfacejs/static-variables)
