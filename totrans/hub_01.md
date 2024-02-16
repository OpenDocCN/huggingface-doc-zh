# Hugging Face Hub 文档

> 原文：[https://huggingface.co/docs/hub/index](https://huggingface.co/docs/hub/index)

Hugging Face Hub 是一个平台，拥有超过 35 万个模型、7.5 万个数据集和 15 万个演示应用（空间），全部开源且公开可用，在这个在线平台上，人们可以轻松合作并共同构建机器学习。Hub 作为一个中心地带，任何人都可以探索、实验、合作和使用机器学习技术。您准备好加入通往开源机器学习之路了吗？🤗

仓库[介绍](./repositories) [入门指南](./repositories-getting-started) [仓库设置](./repositories-settings) [拉取请求和讨论](./repositories-pull-requests-discussions) [通知](./notifications) [集合](./collections) [Webhooks](./webhooks) [下一步](./repositories-next-steps) [许可证](./repositories-licenses)模型[介绍](./models) [模型中心](./models-the-hub) [模型卡片](./model-cards) [受限模型](./models-gated) [上传模型](./models-uploading) [下载模型](./models-downloading) [库](./models-libraries) [任务](./models-tasks) [小部件](./models-widgets) [推理 API](./models-inference) [下载统计](./models-download-stats)数据集[介绍](./datasets) [数据集概述](./datasets-overview) [数据集卡片](./datasets-cards) [受限数据集](./datasets-gated) [上传数据集](./datasets-adding) [下载数据集](./datasets-downloading) [库](./datasets-libraries) [数据集查看器](./datasets-viewer) [下载统计](./datasets-download-stats) [数据文件配置](./datasets-data-files-configuration)空间[介绍](./spaces) [空间概述](./spaces-overview) [Gradio 空间](./spaces-sdks-gradio) [Streamlit 空间](./spaces-sdks-streamlit) [静态 HTML 空间](./spaces-sdks-static) [Docker 空间](./spaces-sdks-docker) [嵌入您的空间](./spaces-embed) [使用 Docker 运行](./spaces-run-with-docker) [参考](./spaces-config-reference) [更改日志](./spaces-changelog) [高级主题](./spaces-advanced) [使用 HF 登录](./spaces-oauth)其他[组织](./organizations) [企业中心](./enterprise-hub) [计费](./billing) [安全](./security) [管理](./moderation) [论文页面](./paper-pages) [搜索](./search) [数字对象标识符（DOI）](./doi) [Hub API 端点](./api) [使用 HF 登录](./oauth) [贡献者行为准则](https://huggingface.co/code-of-conduct) [内容指南](https://huggingface.co/content-guidelines)

## Hugging Face Hub 是什么？

我们正在帮助社区共同努力实现推进机器学习的目标🔥。

Hugging Face Hub 是一个平台，拥有超过 12 万个模型、2 万个数据集和 5 万个演示，人们可以轻松在其中合作进行机器学习工作流。Hub 作为一个中心地带，任何人都可以分享、探索、发现和实验开源机器学习。

没有任何一家公司，包括科技巨头，能够独自“解决 AI” - 我们实现这一目标的唯一途径是通过以社区为中心的方式分享知识和资源。我们正在构建 Hugging Face Hub 上最大的开源模型、数据集、演示和指标集合，以使机器学习普及和发展 🚀。

我们鼓励您阅读[行为准则](https://huggingface.co/code-of-conduct)和[内容指南](https://huggingface.co/content-guidelines)，以熟悉我们期望社区成员遵守的价值观 🤗。

## 在 Hub 上可以找到什么？

Hugging Face Hub 托管基于 Git 的仓库，这些仓库是版本控制的存储桶，可以包含所有您的文件。💾

在其中，您可以上传和发现…

+   模型，*托管最新的自然语言处理、视觉和音频任务的最先进模型*

+   数据集，*提供不同领域和模态的各种数据*。

+   空间，*直接在浏览器中演示ML模型的交互式应用程序*。

Hub提供**版本控制、提交历史记录、差异、分支和十几个库集成**！您可以在[**存储库文档**](./repositories)中了解所有存储库共享的功能。

## 模型

您可以发现并使用社区共享的数以万计的开源ML模型。为了促进负责任的模型使用和开发，模型存储库配备了[模型卡片](./model-cards)，以通知用户每个模型的限制和偏见。还可以包括有关任务、语言和指标等信息的[元数据](./model-cards#model-card-metadata)，如果存储库包含[TensorBoard跟踪](./tensorboard)，还可以添加训练指标图。还可以轻松地为您的模型添加一个[**推理小部件**](./models-widgets)，使任何人都可以直接在浏览器中与模型互动！为了进行程序化访问，提供了一个API来[**立即提供您的模型**](./models-inference)。

要将模型上传到Hub，或下载模型并将其集成到您的工作中，请探索[**模型文档**](./models)。您还可以选择来自[**十几个库**](./models-libraries)（如🤗 Transformers、Asteroid和ESPnet）来支持Hub。

## 数据集

Hub拥有超过5,000个数据集，涵盖100多种语言，可用于NLP、计算机视觉和音频等广泛任务。Hub使查找、下载和上传数据集变得简单。数据集配有详尽的文档，以[**数据集卡片**](./model-cards)和[**数据集预览**](./datasets-overview#datasets-on-the-hub)的形式，让您可以直接在浏览器中探索数据。虽然许多数据集是公开的，[**组织**](./organizations)和个人可以创建私人数据集以符合许可或隐私问题。您可以在[Hugging Face Hub文档中了解更多关于**数据集**的信息](./datasets-overview)。

[🤗 `数据集`](https://huggingface.co/docs/datasets/index)库允许您以编程方式与数据集进行交互，因此您可以轻松地在项目中使用Hub中的数据集。只需一行代码，您就可以访问数据集；即使它们太大而无法适应您的计算机，您也可以使用流式传输来高效地访问数据。

## 空间

[空间](https://huggingface.co/spaces)是在Hub上托管ML演示应用程序的简单方法。它们允许您构建您的ML作品集，在会议上展示您的项目或向利益相关者展示，并与ML生态系统中的其他人合作。

我们目前支持两个出色的Python SDK（**[Gradio](https://gradio.app/)**和**[Streamlit](https://streamlit.io/)**），让您在几分钟内构建出色的应用程序。用户还可以创建静态空间，这是一个简单的HTML/CSS/JavaScript页面在一个空间内。

在探索了一些空间之后（看看我们的[本周空间！](https://huggingface.co/spaces)），深入了解[**空间文档**](./spaces-overview)，了解如何创建自己的空间。您还可以升级您的空间以在GPU或其他加速硬件上运行。⚡️

## 组织

公司、大学和非营利组织是Hugging Face社区的重要组成部分！Hub提供[**组织**](./organizations)，可用于分组帐户和管理数据集、模型和空间。教育工作者还可以使用[Hugging Face for Classrooms](https://huggingface.co/classrooms)为学生创建协作组织。组织的存储库将显示在组织的页面上，组织的每个成员都将有能力为存储库做出贡献。除了方便地将组织的所有工作分组在一起，Hub还允许管理员设置角色以[**控制对存储库的访问**](./organizations-security)，并管理他们组织的[付款方式和账单信息](https://huggingface.co/pricing)。在协作时，机器学习更有趣！🔥

[探索现有组织](https://huggingface.co/organizations)，在这里创建一个新组织[here](https://huggingface.co/organizations/new)，然后访问[**组织文档**](./organizations)以了解更多信息。

## 安全性

Hugging Face Hub支持安全性和访问控制功能，让您放心您的代码、模型和数据是安全的。访问这些文档中的[**安全**](./security)部分，了解以下内容：

+   用户访问令牌

+   组织的访问控制

+   使用GPG签署提交

+   恶意软件扫描

![](../Images/5ff53729a41500fde15477ec3f4b596c.png)
