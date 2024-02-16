# 在另一个网站中嵌入您的Space

> 原始文本：[https://huggingface.co/docs/hub/spaces-embed](https://huggingface.co/docs/hub/spaces-embed)

一旦您的Space运行起来，您可能希望将其嵌入到网站或博客中。嵌入或分享您的Space是让您的观众与您的工作和演示互动的好方法，而无需在他们那边进行任何设置。要嵌入一个Space，其可见性需要是公开的。

## 直接URL

Space被分配一个唯一的URL，您可以使用它来分享您的Space或将其嵌入到网站中。

此URL的形式为：`"https://<space-subdomain>.hf.space"`。例如，Space[NimaBoscarino/hotdog-gradio](https://huggingface.co/spaces/NimaBoscarino/hotdog-gradio)具有相应的URL为`"https://nimaboscarino-hotdog-gradio.hf.space"`。子域是唯一的，只有在移动或重命名Space时才会更改。

您的Space始终从此子域的根目录提供服务。

您可以在选项菜单中找到Space的URL以及如何直接嵌入它的示例片段：

![](../Images/8393d22377bd250a75a94e4ee1e63dc4.png) ![](../Images/564a74387e47cf99eed3f3727aff31ae.png)

## 使用IFrames进行嵌入

Space的默认嵌入方法是使用IFrames。在您希望嵌入Space的HTML位置中添加以下元素：

```py
<iframe
    src="https://<space-subdomain>.hf.space"
    frameborder="0"
    width="850"
    height="450"
></iframe>
```

例如，使用[NimaBoscarino/hotdog-gradio](https://huggingface.co/spaces/NimaBoscarino/hotdog-gradio) Space：

[https://nimaboscarino-hotdog-gradio.hf.space](https://nimaboscarino-hotdog-gradio.hf.space)

## 使用Web组件进行嵌入

如果您希望嵌入的Space是基于Gradio的，您可以使用Web组件来嵌入您的Space。Web组件比IFrames更快，并且会自动调整到您的网页，因此您无需为元素配置`width`或`height`。首先，您需要通过将以下脚本添加到您的HTML中来导入与Space中Gradio版本对应的Gradio JS库。

![](../Images/2ac852ea0f648ac3e209458fbb26c786.png)

然后，在您希望嵌入Space的位置添加一个`gradio-app`元素。

```py
<gradio-app src="https://<space-subdomain>.hf.space"></gradio-app>
```

查看[Gradio文档](https://gradio.app/sharing_your_app/#embedding-hosted-spaces)以获取更多详细信息。
