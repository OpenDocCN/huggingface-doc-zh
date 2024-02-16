# 下载文件

> 原文链接：[`huggingface.co/docs/huggingface_hub/package_reference/file_download`](https://huggingface.co/docs/huggingface_hub/package_reference/file_download)

## 下载单个文件

### hf_hub_download

#### `huggingface_hub.hf_hub_download`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/file_download.py#L993)

```py
( repo_id: str filename: str subfolder: Optional = None repo_type: Optional = None revision: Optional = None library_name: Optional = None library_version: Optional = None cache_dir: Union = None local_dir: Union = None local_dir_use_symlinks: Union = 'auto' user_agent: Union = None force_download: bool = False force_filename: Optional = None proxies: Optional = None etag_timeout: float = 10 resume_download: bool = False token: Union = None local_files_only: bool = False legacy_cache_layout: bool = False endpoint: Optional = None )
```

参数

+   `repo_id`（`str`）—由用户或组织名称和由`/`分隔的存储库名称。

+   `filename`（`str`）—存储库中文件的名称。

+   `subfolder`（`str`，*可选*）—与模型存储库中的文件夹对应的可选值。

+   `repo_type`（`str`，*可选*）—如果从数据集或空间下载，则设置为`"dataset"`或`"space"`，如果从模型下载，则设置为`None`或`"model"`。默认为`None`。

+   `revision`（`str`，*可选*）—可选的 Git 修订 ID，可以是分支名称、标签或提交哈希。

+   `library_name`（`str`，*可选*）—对象对应的库的名称。

+   `library_version`（`str`，*可选*）—库的版本。

+   `cache_dir`（`str`，`Path`，*可选*）—缓存文件存储的文件夹路径。

+   `local_dir`（`str`或`Path`，*可选*）—如果提供，下载的文件将放置在此目录下，作为符号链接（默认）或常规文件（有关更多详细信息，请参阅描述）。

+   `local_dir_use_symlinks`（`"auto"`或`bool`，默认为`"auto"`）—与`local_dir`一起使用。如果设置为`"auto"`，将使用缓存目录，并且文件将根据其大小在本地目录中复制或创建符号链接。如果设置为`True`，将创建符号链接，无论文件大小如何。如果设置为`False`，文件将从缓存中复制（如果已存在）或从 Hub 下载并且不缓存。有关更多详细信息，请参阅描述。

+   `user_agent`（`dict`，`str`，*可选*）—以字典或字符串形式的用户代理信息。

+   `force_download`（`bool`，*可选*，默认为`False`）—是否应下载文件，即使它已经存在于本地缓存中。

+   `proxies`（`dict`，*可选*）—将协议映射到传递给`requests.request`的代理的 URL 的字典。

+   `etag_timeout`（`float`，*可选*，默认为`10`）—在获取 ETag 时，等待服务器发送数据的秒数，然后放弃，传递给`requests.request`。

+   `resume_download`（`bool`，*可选*，默认为`False`）—如果为`True`，则恢复先前中断的下载。

+   `token`（`str`，`bool`，*可选*）—用于下载的令牌。

    +   如果为`True`，则从 HuggingFace 配置文件夹中读取令牌。

    +   如果是字符串，则用作身份验证令牌。

+   `local_files_only`（`bool`，*可选*，默认为`False`）—如果为`True`，则避免下载文件并返回本地缓存文件的路径（如果存在）。

+   `legacy_cache_layout`（`bool`，*可选*，默认为`False`）—如果为`True`，则使用传统的文件缓存布局，即只需调用 hf_hub_url()然后`cached_download`。由于新的缓存布局更强大，因此此方法已被弃用。

如果本地缓存中尚不存在给定文件，则下载该文件。

新的缓存文件布局如下：

+   缓存目录包含每个 repo_id 的一个子文件夹（由 repo 类型命名空间）

+   在每个存储库文件夹内：

    +   refs 是最新已知的修订 => 提交哈希对列表

    +   blobs 包含实际的文件 blob（根据它们的 git-sha 或 sha256 标识，取决于它们是否为 LFS 文件）

    +   快照包含每个提交的一个子文件夹，每个“提交”包含在该特定提交中已解析的文件子集。每个文件名都是指向该特定提交的 blob 的符号链接。

如果提供了`local_dir`，则将在此位置复制来自存储库的文件结构。您可以配置如何移动这些文件：

+   如果`local_dir_use_symlinks="auto"`（默认），文件将被下载并存储在缓存目录中作为 blob 文件。小文件（<5MB）将在`local_dir`中复制，而对于更大的文件，将创建符号链接。目标是能够手动编辑和保存小文件而不会破坏缓存，同时为二进制文件节省磁盘空间。5MB 阈值可以通过`HF_HUB_LOCAL_DIR_AUTO_SYMLINK_THRESHOLD`环境变量进行配置。

+   如果`local_dir_use_symlinks=True`，文件将被下载，存储在缓存目录中，并在`local_dir`中创建符号链接。这在磁盘使用方面是最优化的，但文件不得手动编辑。

+   如果`local_dir_use_symlinks=False`且 blob 文件存在于缓存目录中，则它们将在本地目录中复制。这意味着磁盘使用没有被优化。

+   最后，如果`local_dir_use_symlinks=False`且 blob 文件不存在于缓存目录中，则文件将被下载并直接放置在`local_dir`下。这意味着如果以后需要重新下载它们，它们将被完全重新下载。

```py
[  96]  .
└── [ 160]  models--julien-c--EsperBERTo-small
    ├── [ 160]  blobs
    │   ├── [321M]  403450e234d65943a7dcf7e05a771ce3c92faa84dd07db4ac20f592037a1e4bd
    │   ├── [ 398]  7cb18dc9bafbfcf74629a4b760af1b160957a83e
    │   └── [1.4K]  d7edf6bd2a681fb0175f7735299831ee1b22b812
    ├── [  96]  refs
    │   └── [  40]  main
    └── [ 128]  snapshots
        ├── [ 128]  2439f60ef33a0d46d85da5001d52aeda5b00ce9f
        │   ├── [  52]  README.md -> ../../blobs/d7edf6bd2a681fb0175f7735299831ee1b22b812
        │   └── [  76]  pytorch_model.bin -> ../../blobs/403450e234d65943a7dcf7e05a771ce3c92faa84dd07db4ac20f592037a1e4bd
        └── [ 128]  bbc77c8132af1cc5cf678da3f1ddf2de43606d48
            ├── [  52]  README.md -> ../../blobs/7cb18dc9bafbfcf74629a4b760af1b160957a83e
            └── [  76]  pytorch_model.bin -> ../../blobs/403450e234d65943a7dcf7e05a771ce3c92faa84dd07db4ac20f592037a1e4bd
```

引发以下错误：

+   [`EnvironmentError`](https://docs.python.org/3/library/exceptions.html#EnvironmentError) 如果`token=True`且找不到令牌。

+   [`OSError`](https://docs.python.org/3/library/exceptions.html#OSError) 如果无法确定 ETag。

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果某个参数值无效

+   RepositoryNotFoundError 如果找不到要下载的存储库。这可能是因为它不存在，或者因为它被设置为`private`，而您没有访问权限。

+   RevisionNotFoundError 如果找不到要下载的修订版本。

+   EntryNotFoundError 如果找不到要下载的文件。

+   LocalEntryNotFoundError 如果网络被禁用或不可用，并且在缓存中找不到文件。

### hf_hub_url

#### `huggingface_hub.hf_hub_url`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/file_download.py#L184)

```py
( repo_id: str filename: str subfolder: Optional = None repo_type: Optional = None revision: Optional = None endpoint: Optional = None )
```

参数

+   `repo_id`（`str`）— 由用户或组织名称和存储库名称以`/`分隔的命名空间。

+   `filename`（`str`）— 存储库中文件的名称。

+   `subfolder`（`str`，*可选*）— 与存储库内部文件夹对应的可选值。

+   `repo_type`（`str`，*可选*）— 如果从数据集或空间下载，则设置为`"dataset"`或`"space"`，如果从模型下载，则设置为`None`或`"model"`。默认为`None`。

+   `revision`（`str`，*可选*）— 可选的 Git 修订 ID，可以是分支名称、标签或提交哈希。

从给定信息构建文件的 URL。

解析后的地址可以是 huggingface.co 托管的 URL，也可以是指向 Cloudfront（内容传送网络或 CDN）的链接，用于大于几 MB 的大文件。

示例：

```py
>>> from huggingface_hub import hf_hub_url

>>> hf_hub_url(
...     repo_id="julien-c/EsperBERTo-small", filename="pytorch_model.bin"
... )
'https://huggingface.co/julien-c/EsperBERTo-small/resolve/main/pytorch_model.bin'
```

注意：

Cloudfront 在全球范围内复制，因此对于最终用户来说，下载速度更快（同时也降低了我们的带宽成本）。

Cloudfront 默认会积极缓存文件（默认 TTL 为 24 小时），但这在这里不是问题，因为我们在 huggingface.co 上实现了基于 git 的版本控制系统，这意味着我们以内容可寻址的方式（即文件名为其哈希值）将文件存储在 S3/Cloudfront 上。使用内容可寻址的文件名意味着缓存永远不会过时。

在这个库的客户端缓存方面，我们基于对象的实体标签（`ETag`）进行缓存，这是资源特定版本的标识符[1]。对象的 ETag 是：如果存储在 git 中，则为其 git-sha1，如果存储在 git-lfs 中，则为其 sha256。

参考：

+   [1] [`developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag)

## 下载存储库的快照

#### `huggingface_hub.snapshot_download`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_snapshot_download.py#L34)

```py
( repo_id: str repo_type: Optional = None revision: Optional = None cache_dir: Union = None local_dir: Union = None local_dir_use_symlinks: Union = 'auto' library_name: Optional = None library_version: Optional = None user_agent: Union = None proxies: Optional = None etag_timeout: float = 10 resume_download: bool = False force_download: bool = False token: Union = None local_files_only: bool = False allow_patterns: Union = None ignore_patterns: Union = None max_workers: int = 8 tqdm_class: Optional = None endpoint: Optional = None )
```

参数

+   `repo_id` (`str`) — 由用户或组织名称和存储库名称以`/`分隔的字符串。

+   `repo_type` (`str`，*可选*) — 如果从数据集或空间下载，则设置为`"dataset"`或`"space"`，如果从模型下载，则设置为`None`或`"model"`。默认为`None`。

+   `revision` (`str`，*可选*) — 可选的 Git 修订 ID，可以是分支名称、标签或提交哈希。

+   `cache_dir` (`str`，`Path`，*可选*) — 存储缓存文件的文件夹的路径。

+   `local_dir` (`str`或`Path`，*可选*) — 如果提供，下载的文件将被放置在此目录下，可以是符号链接（默认）或常规文件（详细信息请参见描述）。

+   `local_dir_use_symlinks` (`"auto"`或`bool`，默认为`"auto"`) — 与`local_dir`一起使用。如果设置为`"auto"`，将使用缓存目录，并且文件将根据其大小复制或创建符号链接到本地目录。如果设置为`True`，将创建符号链接，无论文件大小如何。如果设置为`False`，文件将从缓存中复制（如果已存在）或从 Hub 下载并且不缓存。详细信息请参见描述。

+   `library_name` (`str`，*可选*) — 对象对应的库的名称。

+   `library_version` (`str`，*可选*) — 库的版本。

+   `user_agent` (`str`，`dict`，*可选*) — 以字典或字符串形式的用户代理信息。

+   `proxies` (`dict`，*可选*) — 将协议映射到传递给`requests.request`的代理的 URL 的字典。

+   `etag_timeout` (`float`，*可选*，默认为`10`) — 在获取 ETag 时，等待服务器发送数据的秒数，然后放弃，这将传递给`requests.request`。

+   `resume_download` (`bool`，*可选*，默认为`False`) — 如果为`True`，则恢复先前中断的下载。

+   `force_download` (`bool`，*可选*，默认为`False`) — 是否应该下载文件，即使它已经存在于本地缓存中。

+   `token` (`str`，`bool`，*可选*) — 用于下载的令牌。

    +   如果为`True`，则从 HuggingFace 配置文件夹中读取令牌。

    +   如果是字符串，则用作身份验证令牌。

+   `local_files_only` (`bool`，*可选*，默认为`False`) — 如果为`True`，避免下载文件，并返回本地缓存文件的路径（如果存在）。

+   `allow_patterns` (`List[str]`或`str`，*可选*) — 如果提供，只有匹配至少一个模式的文件才会被下载。

+   `ignore_patterns` (`List[str]`或`str`，*可选*) — 如果提供，与任何模式匹配的文件将不会被下载。

+   `max_workers` (`int`，*可选*) — 下载文件的并发线程数（1 个线程=1 个文件下载）。默认为 8。

+   `tqdm_class` (`tqdm`，*可选*) — 如果提供，将覆盖进度条的默认行为。传递的参数必须继承自`tqdm.auto.tqdm`或至少模仿其行为。请注意，`tqdm_class`不会传递给每个单独的下载。默认为自定义 HF 进度条，可以通过设置`HF_HUB_DISABLE_PROGRESS_BARS`环境变量来禁用。

下载存储库文件。

在指定的修订版本中下载存储库文件的整个快照。当您想要从存储库获取所有文件时很有用，因为您事先不知道需要哪些文件。所有文件都嵌套在一个文件夹中，以保持它们相对于该文件夹的实际文件名。您还可以使用`allow_patterns`和`ignore_patterns`来过滤要下载的文件。

如果提供了`local_dir`，则将在此位置复制来自存储库的文件结构。您可以配置如何移动这些文件：

+   如果`local_dir_use_symlinks="auto"`（默认值），文件将被下载并存储在缓存目录中作为 blob 文件。小文件（<5MB）将在`local_dir`中复制，而对于较大的文件，将创建符号链接。目标是能够手动编辑和保存小文件而不会破坏缓存，同时为二进制文件节省磁盘空间。5MB 阈值可以通过`HF_HUB_LOCAL_DIR_AUTO_SYMLINK_THRESHOLD`环境变量进行配置。

+   如果`local_dir_use_symlinks=True`，文件将被下载，存储在缓存目录中，并在`local_dir`中创建符号链接。这在磁盘使用方面是最优的，但文件不得手动编辑。

+   如果`local_dir_use_symlinks=False`并且 blob 文件存在于缓存目录中，则它们将在本地目录中复制。这意味着磁盘使用不被优化。

+   最后，如果`local_dir_use_symlinks=False`并且 blob 文件不存在于缓存目录中，则文件将被下载并直接放置在`local_dir`下。这意味着如果以后需要重新下载它们，它们将被完全重新下载。

另一种选择是克隆存储库，但这需要安装和正确配置 git 和 git-lfs。在使用 git 克隆存储库时，无法过滤要下载的文件。

引发以下错误：

+   [`EnvironmentError`](https://docs.python.org/3/library/exceptions.html#EnvironmentError) 如果`token=True`且找不到令牌。

+   [`OSError`](https://docs.python.org/3/library/exceptions.html#OSError) 如果无法确定 ETag。

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果某个参数值无效

## 获取有关文件的元数据

### get_hf_file_metadata

#### `huggingface_hub.get_hf_file_metadata`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/file_download.py#L1588)

```py
( url: str token: Union = None proxies: Optional = None timeout: Optional = 10 library_name: Optional = None library_version: Optional = None user_agent: Union = None )
```

参数

+   `url` (`str`) — 文件 url，例如由 hf_hub_url()返回。

+   `token` (`str`或`bool`, *optional*) — 用于下载的令牌。

    +   如果为`True`，则从 HuggingFace 配置文件夹中读取令牌。

    +   如果为`False`或`None`，则不提供令牌。

    +   如果是字符串，则用作身份验证令牌。

+   `proxies` (`dict`, *optional*) — 将协议映射到传递给`requests.request`的代理的 URL 的字典。

+   `timeout` (`float`, *optional*, 默认为 10) — 在放弃之前等待服务器发送元数据的秒数。

+   `library_name` (`str`, *optional*) — 对象对应的库的名称。

+   `library_version` (`str`, *optional*) — 库的版本。

+   `user_agent` (`dict`, `str`, *optional*) — 以字典或字符串形式的用户代理信息。

获取在 Hub 上版本化的文件的元数据，给定一个 url。

### HfFileMetadata

### `class huggingface_hub.HfFileMetadata`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/file_download.py#L160)

```py
( commit_hash: Optional etag: Optional location: str size: Optional )
```

参数

+   `commit_hash` (`str`, *optional*) — 与文件相关的提交哈希值。

+   `etag` (`str`, *optional*) — 服务器上文件的 ETag。

+   `location` (`str`) — 下载文件的位置。可以是 Hub 的 url 或其他位置。

+   `size` (`size`) — 文件的大小。在 LFS 文件的情况下，包含实际 LFS 文件的大小，而不是指针的大小。

包含有关在 Hub 上版本化的文件的信息的数据结构。

基于 URL 由 get_hf_file_metadata()返回。

## 缓存

上述方法旨在与防止重新下载文件的缓存系统一起使用。缓存系统在 v0.8.0 中进行了更新，成为依赖于 Hub 的库之间共享的中央缓存系统。

阅读缓存系统指南以详细介绍 HF 的缓存。
