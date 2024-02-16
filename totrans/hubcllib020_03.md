# 快速入门

> 原文链接：[https://huggingface.co/docs/huggingface_hub/quick-start](https://huggingface.co/docs/huggingface_hub/quick-start)

[Hugging Face Hub](https://huggingface.co/)是分享机器学习模型、演示、数据集和指标的首选地点。`huggingface_hub`库帮助您在不离开开发环境的情况下与Hub进行交互。您可以轻松创建和管理存储库，下载和上传文件，并从Hub获取有用的模型和数据集元数据。

## 安装

要开始，请安装`huggingface_hub`库：

```py
pip install --upgrade huggingface_hub
```

有关更多详细信息，请查看[安装](installation)指南。

## 下载文件

Hub上的存储库是git版本控制的，用户可以下载单个文件或整个存储库。您可以使用[hf_hub_download()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.hf_hub_download)函数来下载文件。此函数将在本地磁盘上下载和缓存文件。下次需要该文件时，它将从缓存中加载，因此无需重新下载。

您将需要存储库ID和要下载的文件的文件名。例如，要下载[Pegasus](https://huggingface.co/google/pegasus-xsum)模型配置文件：

```py
>>> from huggingface_hub import hf_hub_download
>>> hf_hub_download(repo_id="google/pegasus-xsum", filename="config.json")
```

要下载文件的特定版本，请使用`revision`参数指定分支名称、标签或提交哈希。如果选择使用提交哈希，必须是完整长度的哈希，而不是较短的7字符提交哈希：

```py
>>> from huggingface_hub import hf_hub_download
>>> hf_hub_download(
...     repo_id="google/pegasus-xsum", 
...     filename="config.json", 
...     revision="4d33b01d79672f27f001f6abade33f22d993b151"
... )
```

有关更多详细信息和选项，请参阅[hf_hub_download()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.hf_hub_download)的API参考。

## 认证

在许多情况下，您必须使用Hugging Face帐户进行身份验证才能与Hub进行交互：下载私有存储库、上传文件、创建PR等。如果您还没有帐户，请[创建一个帐户](https://huggingface.co/join)，然后登录以从您的[设置页面](https://huggingface.co/settings/tokens)获取您的[用户访问令牌](https://huggingface.co/docs/hub/security-tokens)。用户访问令牌用于向Hub验证您的身份。

令牌可以具有`read`或`write`权限。如果要创建或编辑存储库，请确保具有`write`访问令牌。否则，最好生成一个`read`令牌以减少令牌意外泄漏的风险。

### 登录命令

认证的最简单方法是将令牌保存在您的计算机上。您可以使用终端从终端使用[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)命令来执行此操作：

```py
huggingface-cli login
```

该命令将告诉您是否已经登录，并提示您输入令牌。然后验证令牌并保存在您的`HF_HOME`目录中（默认为`~/.cache/huggingface/token`）。与Hub交互的任何脚本或库在发送请求时将使用此令牌。

或者，您可以在笔记本或脚本中使用[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)进行程序化登录：

```py
>>> from huggingface_hub import login
>>> login()
```

您一次只能登录一个帐户。登录新帐户将自动注销您之前的帐户。要确定当前活动的帐户，只需运行`huggingface-cli whoami`命令。

一旦登录，Hub的所有请求 - 即使不一定需要身份验证的方法 - 将默认使用您的访问令牌。如果您想禁用对您令牌的隐式使用，您应该将`HF_HUB_DISABLE_IMPLICIT_TOKEN=1`设置为一个环境变量（参见[参考](../package_reference/environment_variables#hfhubdisableimplicittoken)）。

### 环境变量

环境变量`HF_TOKEN`也可以用于身份验证。这在一个空间中特别有用，您可以将`HF_TOKEN`设置为[Space secret](https://huggingface.co/docs/hub/spaces-overview#managing-secrets)。

**新功能：** Google Colaboratory允许您为您的笔记本定义[私钥](https://twitter.com/GoogleColab/status/1719798406195867814)。定义一个`HF_TOKEN`秘密以自动进行身份验证！

通过环境变量或秘密进行身份验证优先于存储在您的计算机上的令牌。

### 方法参数

最后，也可以通过将令牌传递给接受`token`作为参数的任何方法来进行身份验证。

```py
from transformers import whoami
 user = whoami(token=...)
```

除非在您不希望永久存储令牌或需要同时处理多个令牌的环境中，通常不建议这样做。

在将令牌作为参数传递时请小心。最佳实践是始终从安全保险库中加载令牌，而不是在代码库或笔记本中硬编码。如果您无意中分享代码，则硬编码的令牌会带来重大泄漏风险。

## 创建存储库

注册并登录后，使用[create_repo()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_repo)函数创建存储库：

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> api.create_repo(repo_id="super-cool-model")
```

如果您希望您的存储库是私有的，则：

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> api.create_repo(repo_id="super-cool-model", private=True)
```

私有存储库除您之外不会对任何人可见。

要创建存储库或将内容推送到Hub，您必须提供具有`write`权限的用户访问令牌。您可以在您的[设置页面](https://huggingface.co/settings/tokens)中创建令牌时选择权限。

## 上传文件

使用[upload_file()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.upload_file)函数将文件添加到您新创建的存储库中。您需要指定：

1.  要上传的文件路径。

1.  存储库中文件的路径。

1.  您想要添加文件的存储库ID。

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> api.upload_file(
...     path_or_fileobj="/home/lysandre/dummy-test/README.md",
...     path_in_repo="README.md",
...     repo_id="lysandre/test-model",
... )
```

要一次上传多个文件，请查看[上传](./guides/upload)指南，该指南将向您介绍几种上传文件的方法（带有或不带有git）。

## 下一步

`huggingface_hub`库为用户提供了一种使用Python与Hub进行交互的简单方法。要了解有关如何在Hub上管理文件和存储库的更多信息，我们建议阅读我们的[操作指南](./guides/overview)：

+   [管理您的存储库](./guides/repository)。

+   从Hub中[下载](./guides/download)文件。

+   [上传](./guides/upload)文件到Hub。

+   [在Hub中搜索](./guides/search)您所需的模型或数据集。

+   [访问推理API](./guides/inference)以进行快速推理。
