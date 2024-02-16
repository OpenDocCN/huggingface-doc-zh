# Git vs HTTP 范式

> 原文链接：[`huggingface.co/docs/huggingface_hub/concepts/git_vs_http`](https://huggingface.co/docs/huggingface_hub/concepts/git_vs_http)

`huggingface_hub` 库是用于与 Hugging Face Hub 交互的库，该库是一组基于 git 的仓库（模型、数据集或 Spaces）。使用 `huggingface_hub` 访问 Hub 的两种主要方式。

第一种方法，所谓的“基于 git 的”方法，由 Repository 类领导。这种方法使用围绕 `git` 命令的包装器，具有专门设计用于与 Hub 交互的附加功能。第二个选项，称为“基于 HTTP 的”方法，涉及使用 HfApi 客户端进行 HTTP 请求。让我们来看看每种方法的优缺点。

## Repository：历史悠久的基于 git 的方法

最初，`huggingface_hub` 主要围绕 Repository 类构建。它提供了常见 `git` 命令的 Python 封装，如 `"git add"`, `"git commit"`, `"git push"`, `"git tag"`, `"git checkout"` 等。

该库还有助于设置凭据和跟踪大型文件，这些文件通常在机器学习仓库中使用。此外，该库允许您在后台执行其方法，这对在训练期间上传数据很有用。

使用 Repository 的主要优势是可以在本地机器上维护整个仓库的本地副本。这也可能是一个缺点，因为需要您不断更新和维护这个本地副本。这类似于传统软件开发，每个开发人员都维护自己的本地副本，并在开发功能时推送更改。然而，在机器学习的背景下，这并不总是必要的，因为用户可能只需要下载权重进行推理或将权重从一种格式转换为另一种格式，而无需克隆整个仓库。

Repository 现在已被弃用，推荐使用基于 HTTP 的替代方案。鉴于它在传统代码中的广泛应用，完全移除 Repository 将仅在发布 `v1.0` 中发生。

## HfApi：灵活便捷的 HTTP 客户端

HfApi 类是为提供替代本地 git 仓库而开发的，特别是在处理大型模型或数据集时可能会很繁琐。HfApi 类提供了与基于 git 的方法相同的功能，例如下载和推送文件，创建分支和标签，但无需保持同步的本地文件夹。

除了已由 `git` 提供的功能外，HfApi 类还提供了额外功能，例如管理仓库、使用缓存下载文件以实现高效重用、在 Hub 中搜索仓库和元数据、访问社区功能如讨论、PR 和评论，以及配置 Spaces 硬件和秘密。

## 我应该使用什么？以及何时使用？

总的来说，**基于 HTTP 的方法是在所有情况下使用`huggingface_hub`的推荐方式**。HfApi 允许拉取和推送更改，处理 PR、标签和分支，参与讨论等等。自`0.16`版本发布以来，基于 http 的方法也可以在后台运行，这是 Repository 类的最后一个主要优势。

然而，并非所有 git 命令都可以通过 HfApi 使用。有些可能永远不会被实现，但我们始终在努力改进和缩小差距。如果您的使用情况没有被覆盖，请在 Github 上提出[问题](https://github.com/huggingface/huggingface_hub)！我们欢迎反馈，以帮助与我们的用户一起构建🤗生态系统。

在 Hugging Face Hub 中，基于 HTTP 的 HfApi 优先于基于 git 的 Repository，但这并不意味着 git 版本控制会很快从 Hugging Face Hub 中消失。在有意义的工作流中，始终可以使用`git`命令。
