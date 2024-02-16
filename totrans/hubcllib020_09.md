# 命令行界面（CLI）

> [`huggingface.co/docs/huggingface_hub/guides/cli`](https://huggingface.co/docs/huggingface_hub/guides/cli)

`huggingface_hub` Python 包带有一个名为 `huggingface-cli` 的内置 CLI。此工具允许您直接从终端与 Hugging Face Hub 交互。例如，您可以登录到您的帐户，创建存储库，上传和下载文件等。它还提供方便的功能来配置您的机器或管理缓存。在本指南中，我们将查看 CLI 的主要功能以及如何使用它们。

## 入门

首先，让我们安装 CLI：

```py
>>> pip install -U "huggingface_hub[cli]"
```

在上面的代码片段中，我们还安装了 `[cli]` 额外依赖项，以改善用户体验，特别是在使用 `delete-cache` 命令时。

安装后，您可以检查 CLI 是否正确设置：

```py
>>> huggingface-cli --help
usage: huggingface-cli <command> [<args>]

positional arguments:
  {env,login,whoami,logout,repo,upload,download,lfs-enable-largefiles,lfs-multipart-upload,scan-cache,delete-cache}
                        huggingface-cli command helpers
    env                 Print information about the environment.
    login               Log in using a token from huggingface.co/settings/tokens
    whoami              Find out which huggingface.co account you are logged in as.
    logout              Log out
    repo                {create} Commands to interact with your huggingface.co repos.
    upload              Upload a file or a folder to a repo on the Hub
    download            Download files from the Hub
    lfs-enable-largefiles
                        Configure your repository to enable upload of files > 5GB.
    scan-cache          Scan cache directory.
    delete-cache        Delete revisions from the cache directory.

options:
  -h, --help            show this help message and exit
```

如果 CLI 正确安装，您应该看到 CLI 中所有可用选项的列表。如果您收到错误消息，例如 `command not found: huggingface-cli`，请参考安装指南。

`--help` 选项非常方便，可用于获取有关命令的更多详细信息。您可以随时使用它来列出所有可用选项及其详细信息。例如，`huggingface-cli upload --help` 提供有关如何使用 CLI 上传文件的更多信息。

## huggingface-cli login

在许多情况下，您必须登录到 Hugging Face 帐户才能与 Hub 交互（下载私有存储库、上传文件、创建 PR 等）。为此，您需要从[设置页面](https://huggingface.co/settings/tokens)获取[用户访问令牌](https://huggingface.co/docs/hub/security-tokens)。用户访问令牌用于向 Hub 验证您的身份。如果要上传或修改内容，请确保设置具有写入访问权限的令牌。

获得您的令牌后，在终端中运行以下命令：

```py
>>> huggingface-cli login
```

此命令将提示您输入令牌。复制粘贴您的令牌并按*Enter*。然后会询问您是否还应将令牌保存为 git 凭据。如果您打算在本地使用 `git`，再次按*Enter*（默认为是）。最后，它将调用 Hub 来检查您的令牌是否有效并将其保存在本地。

```py
_|    _|  _|    _|    _|_|_|    _|_|_|  _|_|_|  _|      _|    _|_|_|      _|_|_|_|    _|_|      _|_|_|  _|_|_|_|
_|    _|  _|    _|  _|        _|          _|    _|_|    _|  _|            _|        _|    _|  _|        _|
_|_|_|_|  _|    _|  _|  _|_|  _|  _|_|    _|    _|  _|  _|  _|  _|_|      _|_|_|    _|_|_|_|  _|        _|_|_|
_|    _|  _|    _|  _|    _|  _|    _|    _|    _|    _|_|  _|    _|      _|        _|    _|  _|        _|
_|    _|    _|_|      _|_|_|    _|_|_|  _|_|_|  _|      _|    _|_|_|      _|        _|    _|    _|_|_|  _|_|_|_|

To login, `huggingface_hub` requires a token generated from https://huggingface.co/settings/tokens .
Token: 
Add token as git credential? (Y/n) 
Token is valid (permission: write).
Your token has been saved in your configured git credential helpers (store).
Your token has been saved to /home/wauplin/.cache/huggingface/token
Login successful
```

如果您想在不提示的情况下登录，可以直接从命令行传递令牌。为了更安全，我们建议将您的令牌作为环境变量传递，以避免将其粘贴在命令历史记录中。

```py
# Or using an environment variable
>>> huggingface-cli login --token $HUGGINGFACE_TOKEN --add-to-git-credential 
Token is valid (permission: write).
Your token has been saved in your configured git credential helpers (store).
Your token has been saved to /home/wauplin/.cache/huggingface/token
Login successful
```

有关身份验证的更多详细信息，请查看此部分。

## huggingface-cli whoami

如果您想知道自己是否已登录，可以使用 `huggingface-cli whoami`。此命令没有任何选项，只会打印您的用户名以及您在 Hub 上所属的组织：

```py
huggingface-cli whoami                                                                     
Wauplin
orgs:  huggingface,eu-test,OAuthTesters,hf-accelerate,HFSmolCluster
```

如果您未登录，将打印错误消息。

## huggingface-cli logout

此命令将注销您。实际上，它将删除保存在您的机器上的令牌。

如果您使用 `HF_TOKEN` 环境变量登录，则此命令不会注销您（请参阅参考）。如果是这种情况，您必须在您的机器配置中取消设置环境变量。

## huggingface-cli download

使用 `huggingface-cli download` 命令直接从 Hub 下载文件。在内部，它使用与 Download 指南中描述的相同的 hf_hub_download()和 snapshot_download()助手，并将返回的路径打印到终端。在下面的示例中，我们将介绍最常见的用例。要查看所有可用选项的完整列表，您可以运行：

```py
huggingface-cli download --help
```

### 下载单个文件

要从存储库下载单个文件，只需提供 repo_id 和文件名如下：

```py
>>> huggingface-cli download gpt2 config.json
downloading https://huggingface.co/gpt2/resolve/main/config.json to /home/wauplin/.cache/huggingface/hub/tmpwrq8dm5o
(…)ingface.co/gpt2/resolve/main/config.json: 100%|██████████████████████████████████| 665/665 [00:00<00:00, 2.49MB/s]
/home/wauplin/.cache/huggingface/hub/models--gpt2/snapshots/11c5a3d5811f50298f278a704980280950aedb10/config.json
```

该命令将始终在最后一行上打印文件在本地计算机上的路径。

### 下载整个存储库

在某些情况下，您可能只想从存储库下载所有文件。只需指定存储库 ID 即可完成：

```py
>>> huggingface-cli download HuggingFaceH4/zephyr-7b-beta
Fetching 23 files:   0%|                                                | 0/23 [00:00<?, ?it/s]
...
...
/home/wauplin/.cache/huggingface/hub/models--HuggingFaceH4--zephyr-7b-beta/snapshots/3bac358730f8806e5c3dc7c7e19eb36e045bf720
```

### 下载多个文件

您还可以使用单个命令从存储库下载文件的子集。有两种方法可以做到这一点。如果您已经有一个精确的文件列表要下载，只需按顺序提供它们即可：

```py
>>> huggingface-cli download gpt2 config.json model.safetensors
Fetching 2 files:   0%|                                                                        | 0/2 [00:00<?, ?it/s]
downloading https://huggingface.co/gpt2/resolve/11c5a3d5811f50298f278a704980280950aedb10/model.safetensors to /home/wauplin/.cache/huggingface/hub/tmpdachpl3o
(…)8f278a7049802950aedb10/model.safetensors: 100%|██████████████████████████████| 8.09k/8.09k [00:00<00:00, 40.5MB/s]
Fetching 2 files: 100%|████████████████████████████████████████████████████████████████| 2/2 [00:00<00:00,  3.76it/s]
/home/wauplin/.cache/huggingface/hub/models--gpt2/snapshots/11c5a3d5811f50298f278a704980280950aedb10
```

另一种方法是提供模式来过滤您想要下载的文件，使用`--include`和`--exclude`。例如，如果您想要从[stabilityai/stable-diffusion-xl-base-1.0](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0)下载所有 safetensors 文件，除了 FP16 精度的文件：

```py
>>> huggingface-cli download stabilityai/stable-diffusion-xl-base-1.0 --include "*.safetensors" --exclude "*.fp16.*"*
Fetching 8 files:   0%|                                                                         | 0/8 [00:00<?, ?it/s]
...
...
Fetching 8 files: 100%|█████████████████████████████████████████████████████████████████████████| 8/8 (...)
/home/wauplin/.cache/huggingface/hub/models--stabilityai--stable-diffusion-xl-base-1.0/snapshots/462165984030d82259a11f4367a4eed129e94a7b
```

### 下载数据集或空间

上面的示例展示了如何从模型存储库下载。要下载数据集或空间，请使用`--repo-type`选项：

```py
# https://huggingface.co/datasets/HuggingFaceH4/ultrachat_200k
>>> huggingface-cli download HuggingFaceH4/ultrachat_200k --repo-type dataset

# https://huggingface.co/spaces/HuggingFaceH4/zephyr-chat
>>> huggingface-cli download HuggingFaceH4/zephyr-chat --repo-type space

...
```

### 下载特定修订版

上面的示例展示了如何从主分支上的最新提交下载。要从特定修订版（提交哈希、分支名称或标签）下载，请使用`--revision`选项：

```py
>>> huggingface-cli download bigcode/the-stack --repo-type dataset --revision v1.1
...
```

### 下载到本地文件夹

从 Hub 下载文件的推荐（也是默认）方式是使用缓存系统。但是，在某些情况下，您可能希望下载文件并将其移动到特定文件夹。这对于获得与 git 命令提供的工作流程更接近的工作流程很有用。您可以使用`--local_dir`选项来实现这一点。

将文件下载到本地目录会带来一些缺点。在使用`--local-dir`之前，请查看 Download 指南中的限制。

```py
>>> huggingface-cli download adept/fuyu-8b model-00001-of-00002.safetensors --local-dir .
...
./model-00001-of-00002.safetensors
```

### 指定缓存目录

默认情况下，所有文件将下载到由`HF_HOME`环境变量定义的缓存目录中。您还可以使用`--cache-dir`指定自定义缓存：

```py
>>> huggingface-cli download adept/fuyu-8b --cache-dir ./path/to/cache
...
./path/to/cache/models--adept--fuyu-8b/snapshots/ddcacbcf5fdf9cc59ff01f6be6d6662624d9c745
```

### 指定令牌

要访问私有或受限存储库，必须使用令牌。默认情况下，将使用本地保存的令牌（使用`huggingface-cli login`）。如果要明确进行身份验证，请使用`--token`选项：

```py
>>> huggingface-cli download gpt2 config.json --token=hf_****
/home/wauplin/.cache/huggingface/hub/models--gpt2/snapshots/11c5a3d5811f50298f278a704980280950aedb10/config.json
```

### 静音模式

默认情况下，`huggingface-cli download`命令将显示详细信息。它会打印警告消息、有关已下载文件的信息和进度条。如果您想要将所有这些信息静音，可以使用`--quiet`选项。只会打印最后一行（即已下载文件的路径）。如果您想要将输出传递给脚本中的另一个命令，这可能会很有用。

```py
>>> huggingface-cli download gpt2 --quiet
/home/wauplin/.cache/huggingface/hub/models--gpt2/snapshots/11c5a3d5811f50298f278a704980280950aedb10
```

## huggingface-cli upload

使用`huggingface-cli upload`命令直接将文件上传到 Hub。在内部，它使用了与 Upload 指南中描述的相同的 upload_file()和 upload_folder()辅助函数。在下面的示例中，我们将介绍最常见的用例。要查看所有可用选项的完整列表，可以运行：

```py
>>> huggingface-cli upload --help
```

### 上传整个文件夹

该命令的默认用法是：

```py
# Usage:  huggingface-cli upload [repo_id] [local_path] [path_in_repo]
```

要将当前目录上传到存储库的根目录，请使用：

```py
>>> huggingface-cli upload my-cool-model . .
https://huggingface.co/Wauplin/my-cool-model/tree/main/
```

如果存储库尚不存在，将自动创建。

您还可以上传一个特定的文件夹：

```py
>>> huggingface-cli upload my-cool-model ./models .
https://huggingface.co/Wauplin/my-cool-model/tree/main/
```

最后，您可以将文件夹上传到存储库的特定目的地：

```py
>>> huggingface-cli upload my-cool-model ./path/to/curated/data /data/train
https://huggingface.co/Wauplin/my-cool-model/tree/main/data/train
```

### 上传单个文件

您还可以通过将`local_path`设置为指向计算机上的文件来上传单个文件。如果是这种情况，`path_in_repo`是可选的，将默认为您本地文件的名称：

```py
>>> huggingface-cli upload Wauplin/my-cool-model ./models/model.safetensors
https://huggingface.co/Wauplin/my-cool-model/blob/main/model.safetensors
```

如果要将单个文件上传到特定目录，请相应设置`path_in_repo`：

```py
>>> huggingface-cli upload Wauplin/my-cool-model ./models/model.safetensors /vae/model.safetensors
https://huggingface.co/Wauplin/my-cool-model/blob/main/vae/model.safetensors
```

### 上传多个文件

要一次上传文件夹中的多个文件而不上传整个文件夹，可以使用`--include`和`--exclude`模式。还可以与`--delete`选项结合使用，在上传新文件的同时删除存储库中的文件。在下面的示例中，我们通过删除远程文件并上传除`/logs`中的文件之外的所有文件来同步本地 Space：

```py
# Sync local Space with Hub (upload new files except from logs/, delete removed files)
>>> huggingface-cli upload Wauplin/space-example --repo-type=space --exclude="/logs/*" --delete="*" --commit-message="Sync local Space with Hub"
...
```

### 上传到数据集或 Space

要上传到数据集或 Space，请使用`--repo-type`选项：

```py
>>> huggingface-cli upload Wauplin/my-cool-dataset ./data /train --repo-type=dataset
...
```

### 上传到组织

要将内容上传到组织拥有的存储库而不是个人存储库，必须在`repo_id`中明确指定：

```py
>>> huggingface-cli upload MyCoolOrganization/my-cool-model . .
https://huggingface.co/MyCoolOrganization/my-cool-model/tree/main/
```

### 上传到特定修订版

默认情况下，文件将上传到`main`分支。如果要将文件上传到另一个分支或引用，请使用`--revision`选项：

```py
# Upload files to a PR
>>> huggingface-cli upload bigcode/the-stack . . --repo-type dataset --revision refs/pr/104
...
```

**注意：**如果`revision`不存在且未设置`--create-pr`，则将自动从`main`分支创建分支。

### 上传并创建 PR

如果您没有推送到存储库的权限，则必须打开 PR 并让作者知道您想要进行的更改。这可以通过设置`--create-pr`选项来完成：

```py
# Create a PR and upload the files to it
>>> huggingface-cli upload bigcode/the-stack . . --repo-type dataset --revision refs/pr/104
https://huggingface.co/datasets/bigcode/the-stack/blob/refs%2Fpr%2F104/
```

### 定期间隔上传

在某些情况下，您可能希望定期更新存储库。例如，如果您正在训练模型并希望每隔 10 分钟上传日志文件夹，则可以使用`--every`选项：

```py
# Upload new logs every 10 minutes
huggingface-cli upload training-model logs/ --every=10
```

### 指定提交消息

使用`--commit-message`和`--commit-description`设置自定义消息和描述以替代默认消息

```py
>>> huggingface-cli upload Wauplin/my-cool-model ./models . --commit-message="Epoch 34/50" --commit-description="Val accuracy: 68%. Check tensorboard for more details."
...
https://huggingface.co/Wauplin/my-cool-model/tree/main
```

### 指定令牌

要上传文件，必须使用令牌。默认情况下，将使用本地保存的令牌（使用`huggingface-cli login`）。如果要明确进行身份验证，请使用`--token`选项：

```py
>>> huggingface-cli upload Wauplin/my-cool-model ./models . --token=hf_****
...
https://huggingface.co/Wauplin/my-cool-model/tree/main
```

### 安静模式

默认情况下，`huggingface-cli upload`命令将是冗长的。它将打印警告消息、有关上传文件的信息和进度条等详细信息。如果要将所有这些信息静音，请使用`--quiet`选项。只会打印最后一行（即上传文件的 URL）。如果您希望将输出传递给脚本中的另一个命令，则这可能会很有用。

```py
>>> huggingface-cli upload Wauplin/my-cool-model ./models . --quiet
https://huggingface.co/Wauplin/my-cool-model/tree/main
```

## huggingface-cli 扫描缓存

扫描您的缓存目录是有用的，如果您想知道您已下载的存储库以及它在磁盘上占用了多少空间。您可以通过运行`huggingface-cli scan-cache`来实现：

```py
>>> huggingface-cli scan-cache
REPO ID                     REPO TYPE SIZE ON DISK NB FILES LAST_ACCESSED LAST_MODIFIED REFS                LOCAL PATH
--------------------------- --------- ------------ -------- ------------- ------------- ------------------- -------------------------------------------------------------------------
glue                        dataset         116.3K       15 4 days ago    4 days ago    2.4.0, main, 1.17.0 /home/wauplin/.cache/huggingface/hub/datasets--glue
google/fleurs               dataset          64.9M        6 1 week ago    1 week ago    refs/pr/1, main     /home/wauplin/.cache/huggingface/hub/datasets--google--fleurs
Jean-Baptiste/camembert-ner model           441.0M        7 2 weeks ago   16 hours ago  main                /home/wauplin/.cache/huggingface/hub/models--Jean-Baptiste--camembert-ner
bert-base-cased             model             1.9G       13 1 week ago    2 years ago                       /home/wauplin/.cache/huggingface/hub/models--bert-base-cased
t5-base                     model            10.1K        3 3 months ago  3 months ago  main                /home/wauplin/.cache/huggingface/hub/models--t5-base
t5-small                    model           970.7M       11 3 days ago    3 days ago    refs/pr/1, main     /home/wauplin/.cache/huggingface/hub/models--t5-small

Done in 0.0s. Scanned 6 repo(s) for a total of 3.4G.
Got 1 warning(s) while scanning. Use -vvv to print details.
```

有关如何扫描缓存目录的更多详细信息，请参考管理您的缓存指南。

## huggingface-cli 删除缓存

`huggingface-cli delete-cache`是一个帮助您删除不再使用的缓存部分的工具。这对于节省和释放磁盘空间很有用。要了解有关使用此命令的更多信息，请参考管理您的缓存指南。

## huggingface-cli 环境

`huggingface-cli env`命令打印有关您的机器设置的详细信息。当您在[GitHub](https://github.com/huggingface/huggingface_hub)上提出问题以帮助维护人员调查您的问题时，这将非常有用。

```py
>>> huggingface-cli env

Copy-and-paste the text below in your GitHub issue.

- huggingface_hub version: 0.19.0.dev0
- Platform: Linux-6.2.0-36-generic-x86_64-with-glibc2.35
- Python version: 3.10.12
- Running in iPython ?: No
- Running in notebook ?: No
- Running in Google Colab ?: No
- Token path ?: /home/wauplin/.cache/huggingface/token
- Has saved token ?: True
- Who am I ?: Wauplin
- Configured git credential helpers: store
- FastAI: N/A
- Tensorflow: 2.11.0
- Torch: 1.12.1
- Jinja2: 3.1.2
- Graphviz: 0.20.1
- Pydot: 1.4.2
- Pillow: 9.2.0
- hf_transfer: 0.1.3
- gradio: 4.0.2
- tensorboard: 2.6
- numpy: 1.23.2
- pydantic: 2.4.2
- aiohttp: 3.8.4
- ENDPOINT: https://huggingface.co
- HF_HUB_CACHE: /home/wauplin/.cache/huggingface/hub
- HF_ASSETS_CACHE: /home/wauplin/.cache/huggingface/assets
- HF_TOKEN_PATH: /home/wauplin/.cache/huggingface/token
- HF_HUB_OFFLINE: False
- HF_HUB_DISABLE_TELEMETRY: False
- HF_HUB_DISABLE_PROGRESS_BARS: None
- HF_HUB_DISABLE_SYMLINKS_WARNING: False
- HF_HUB_DISABLE_EXPERIMENTAL_WARNING: False
- HF_HUB_DISABLE_IMPLICIT_TOKEN: False
- HF_HUB_ENABLE_HF_TRANSFER: False
- HF_HUB_ETAG_TIMEOUT: 10
- HF_HUB_DOWNLOAD_TIMEOUT: 10
```
