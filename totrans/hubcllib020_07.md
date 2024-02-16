# 从 Hub 下载文件

> 原文：[https://huggingface.co/docs/huggingface_hub/guides/download](https://huggingface.co/docs/huggingface_hub/guides/download)

`huggingface_hub` 库提供了从存储在 Hub 上的存储库下载文件的函数。您可以独立使用这些函数，也可以将它们集成到您自己的库中，使用户更方便地与 Hub 交互。本指南将向您展示如何：

+   下载并缓存单个文件。

+   下载并缓存整个存储库。

+   下载文件到本地文件夹。

## 下载单个文件

[hf_hub_download()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.hf_hub_download) 函数是从 Hub 下载文件的主要函数。它下载远程文件，在磁盘上缓存它（以版本感知方式），并返回其本地文件路径。

返回的文件路径是指向 HF 本地缓存的指针。因此，重要的是不要修改文件以避免缓存损坏。如果您想了解有关文件如何被缓存的更多信息，请参考我们的 [缓存指南](./manage-cache)。

### 从最新版本开始

使用 `repo_id`、`repo_type` 和 `filename` 参数选择要下载的文件。默认情况下，该文件将被视为 `model` 存储库的一部分。

```py
>>> from huggingface_hub import hf_hub_download
>>> hf_hub_download(repo_id="lysandre/arxiv-nlp", filename="config.json")
'/root/.cache/huggingface/hub/models--lysandre--arxiv-nlp/snapshots/894a9adde21d9a3e3843e6d5aeaaf01875c7fade/config.json'

# Download from a dataset
>>> hf_hub_download(repo_id="google/fleurs", filename="fleurs.py", repo_type="dataset")
'/root/.cache/huggingface/hub/datasets--google--fleurs/snapshots/199e4ae37915137c555b1765c01477c216287d34/fleurs.py'
```

### 从特定版本开始

默认情况下，从 `main` 分支下载最新版本。但是，在某些情况下，您可能希望下载特定版本的文件（例如，从特定分支、PR、标签或提交哈希）。为此，请使用 `revision` 参数：

```py
# Download from the `v1.0` tag
>>> hf_hub_download(repo_id="lysandre/arxiv-nlp", filename="config.json", revision="v1.0")

# Download from the `test-branch` branch
>>> hf_hub_download(repo_id="lysandre/arxiv-nlp", filename="config.json", revision="test-branch")

# Download from Pull Request #3
>>> hf_hub_download(repo_id="lysandre/arxiv-nlp", filename="config.json", revision="refs/pr/3")

# Download from a specific commit hash
>>> hf_hub_download(repo_id="lysandre/arxiv-nlp", filename="config.json", revision="877b84a8f93f2d619faa2a6e514a32beef88ab0a")
```

**注意：** 当使用提交哈希时，必须使用完整长度的哈希而不是 7 个字符的提交哈希。

### 构建下载 URL

如果您想构建用于从存储库下载文件的 URL，可以使用 [hf_hub_url()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.hf_hub_url) 返回一个 URL。请注意，它在内部被 [hf_hub_download()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.hf_hub_download) 使用。

## 下载整个存储库

[snapshot_download()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.snapshot_download) 在给定的修订版本下载整个存储库。它内部使用 [hf_hub_download()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.hf_hub_download)，这意味着所有下载的文件也会被缓存在本地磁盘上。下载是并发进行的以加快进程速度。

要下载整个存储库，只需传递 `repo_id` 和 `repo_type`：

```py
>>> from huggingface_hub import snapshot_download
>>> snapshot_download(repo_id="lysandre/arxiv-nlp")
'/home/lysandre/.cache/huggingface/hub/models--lysandre--arxiv-nlp/snapshots/894a9adde21d9a3e3843e6d5aeaaf01875c7fade'

# Or from a dataset
>>> snapshot_download(repo_id="google/fleurs", repo_type="dataset")
'/home/lysandre/.cache/huggingface/hub/datasets--google--fleurs/snapshots/199e4ae37915137c555b1765c01477c216287d34'
```

[snapshot_download()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.snapshot_download) 默认下载最新的修订版本。如果您想要特定的存储库修订版本，请使用 `revision` 参数：

```py
>>> from huggingface_hub import snapshot_download
>>> snapshot_download(repo_id="lysandre/arxiv-nlp", revision="refs/pr/1")
```

### 筛选要下载的文件

[snapshot_download()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.snapshot_download) 提供了一个简单的方法来下载存储库。但是，并不总是希望下载整个存储库的内容。例如，如果您知道只会使用 `.safetensors` 权重而不是所有 `.bin` 文件，则可能希望阻止下载所有 `.bin` 文件。您可以使用 `allow_patterns` 和 `ignore_patterns` 参数来实现这一点。

这些参数接受单个模式或模式列表。模式是标准通配符（globbing patterns），如此处所述 [here](https://tldp.org/LDP/GNU-Linux-Tools-Summary/html/x11655.htm)。模式匹配基于 [`fnmatch`](https://docs.python.org/3/library/fnmatch.html)。

例如，您可以使用 `allow_patterns` 仅下载 JSON 配置文件：

```py
>>> from huggingface_hub import snapshot_download
>>> snapshot_download(repo_id="lysandre/arxiv-nlp", allow_patterns="*.json")
```

另一方面，`ignore_patterns` 可以排除某些文件不被下载。以下示例忽略了 `.msgpack` 和 `.h5` 文件扩展名：

```py
>>> from huggingface_hub import snapshot_download
>>> snapshot_download(repo_id="lysandre/arxiv-nlp", ignore_patterns=["*.msgpack", "*.h5"])
```

最后，您可以结合两者来精确过滤您的下载。以下是一个示例，用于下载所有json和markdown文件，除了`vocab.json`。

```py
>>> from huggingface_hub import snapshot_download
>>> snapshot_download(repo_id="gpt2", allow_patterns=["*.md", "*.json"], ignore_patterns="vocab.json")
```

## 下载文件到本地文件夹

从Hub下载文件的推荐（也是默认）方式是使用[缓存系统](./manage-cache)。您可以通过设置`cache_dir`参数（在[hf_hub_download()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.hf_hub_download)和[snapshot_download()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.snapshot_download)中）来定义缓存位置。

然而，在某些情况下，您可能希望下载文件并将它们移动到特定文件夹。这对于获得与`git`命令提供的工作流程更接近的工作流程是有用的。您可以使用`local_dir`和`local_dir_use_symlinks`参数来实现这一点：

+   `local_dir`必须是系统上文件夹的路径。下载的文件将保持与存储库中相同的文件结构。例如，如果`filename="data/train.csv"`和`local_dir="path/to/folder"`，那么返回的文件路径将是`"path/to/folder/data/train.csv"`。

+   `local_dir_use_symlinks`定义了文件在本地文件夹中如何保存。

    +   默认行为(`"auto"`)是复制小文件(<5MB)并对较大文件使用符号链接。符号链接可以优化带宽和磁盘使用。但是手动编辑符号链接文件可能会损坏缓存，因此对于小文件会进行复制。5MB的阈值可以通过`HF_HUB_LOCAL_DIR_AUTO_SYMLINK_THRESHOLD`环境变量进行配置。

    +   如果设置了`local_dir_use_symlinks=True`，则所有文件都将链接为最佳磁盘空间优化。例如，在下载包含成千上万个小文件的大型数据集时，这是非常有用的。

    +   最后，如果您根本不想要符号链接，可以禁用它们(`local_dir_use_symlinks=False`)。缓存目录仍将用于检查文件是否已缓存。如果已经缓存，文件将从缓存中**复制**（即节省带宽但增加磁盘使用）。如果文件尚未缓存，它将被直接下载并移动到本地目录。这意味着如果以后需要在其他地方重用它，它将被**重新下载**。

以下是一个总结不同选项的表格，以帮助您选择最适合您用例的参数。

| 参数 | 文件已缓存 | 返回的路径 | 可读取路径？ | 可保存到路径？ | 优化带宽 | 优化磁盘使用 |
| --- | :-: | :-: | :-: | :-: | :-: | :-: |
| `local_dir=None` |  | 缓存中的符号链接 | ✅ | ❌ *(保存会损坏缓存)* | ✅ | ✅ |
| `local_dir="path/to/folder"` `local_dir_use_symlinks="auto"` |  | 文件或符号链接在文件夹中 | ✅ | ✅ *(对于小文件)* ⚠️ *(对于大文件在保存前不要解析路径)* | ✅ | ✅ |
| `local_dir="path/to/folder"` `local_dir_use_symlinks=True` |  | 文件夹中的符号链接 | ✅ | ⚠️ *(保存前不要解析路径)* | ✅ | ✅ |
| `local_dir="path/to/folder"` `local_dir_use_symlinks=False` | 否 | 文件在文件夹中 | ✅ | ✅ | ❌ *(重新运行时，文件将被重新下载)* | ⚠️ (如果在多个文件夹中运行，则会有多个副本) |
| `local_dir="path/to/folder"` `local_dir_use_symlinks=False` | 是 | 文件在文件夹中 | ✅ | ✅ | ⚠️ *(文件必须先缓存)* | ❌ *(文件会重复)* |

**注意：**如果您使用的是Windows机器，您需要启用开发者模式或以管理员身份运行`huggingface_hub`以启用符号链接。查看[缓存限制](../guides/manage-cache#limitations)部分以获取更多详细信息。

## 从CLI下载

您可以使用终端中的`huggingface-cli download`命令直接从Hub下载文件。在内部，它使用相同的[hf_hub_download()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.hf_hub_download)和[snapshot_download()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.snapshot_download)助手描述的方法，并将返回的路径打印到终端。

```py
>>> huggingface-cli download gpt2 config.json
/home/wauplin/.cache/huggingface/hub/models--gpt2/snapshots/11c5a3d5811f50298f278a704980280950aedb10/config.json
```

您可以同时下载多个文件，显示进度条并返回文件所在的快照路径：

```py
>>> huggingface-cli download gpt2 config.json model.safetensors
Fetching 2 files: 100%|████████████████████████████████████████████| 2/2 [00:00<00:00, 23831.27it/s]
/home/wauplin/.cache/huggingface/hub/models--gpt2/snapshots/11c5a3d5811f50298f278a704980280950aedb10
```

有关CLI下载命令的更多详细信息，请参考[CLI指南](./cli#huggingface-cli-download)。

## 更快的下载速度

如果您在带宽较高的机器上运行，您可以通过[`hf_transfer`](https://github.com/huggingface/hf_transfer)增加下载速度，这是一个基于Rust开发的库，用于加快与Hub的文件传输。要启用它，请安装该软件包（`pip install hf_transfer`）并将`HF_HUB_ENABLE_HF_TRANSFER=1`设置为环境变量。

进度条在`hf_transfer`版本`0.1.4`开始支持。如果您打算启用更快的下载，请考虑升级（`pip install -U hf-transfer`）。

`hf_transfer`是一款强大的用户工具！经过测试并已准备投入生产使用，但缺乏像高级错误处理或代理等用户友好功能。有关更多详细信息，请查看此[部分](https://huggingface.co/docs/huggingface_hub/hf_transfer)。
