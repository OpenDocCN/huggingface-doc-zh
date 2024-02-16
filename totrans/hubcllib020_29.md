# HfApi客户端

> 原始文本：[https://huggingface.co/docs/huggingface_hub/package_reference/hf_api](https://huggingface.co/docs/huggingface_hub/package_reference/hf_api)

下面是`HfApi`类的文档，它作为Hugging Face Hub的API的Python包装器。

`HfApi`中的所有方法也可以直接从包的根目录访问。下面详细介绍了这两种方法。

使用根方法更为直接，但[HfApi](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi)类提供更多灵活性。特别是，您可以传递一个将在所有HTTP调用中重复使用的令牌。这与`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)不同，因为令牌不会持久保存在计算机上。还可以提供不同的端点或配置自定义用户代理。

```py
from huggingface_hub import HfApi, list_models

# Use root method
models = list_models()

# Or configure a HfApi client
hf_api = HfApi(
    endpoint="https://huggingface.co", # Can be a Private Hub endpoint.
    token="hf_xxx", # Token is not persisted on the machine.
)
models = hf_api.list_models()
```

## HfApi

### `class huggingface_hub.HfApi`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1214)

```py
( endpoint: Optional[str] = None token: Optional[str] = None library_name: Optional[str] = None library_version: Optional[str] = None user_agent: Union[Dict, str, None] = None )
```

#### `accept_access_request`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L8025)

```py
( repo_id: str user: str repo_type: Optional[str] = None token: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 要接受访问请求的存储库的ID。

+   `user` (`str`) — 应接受访问请求的用户的用户名。

+   `repo_type` (`str`, *可选*) — 要接受访问请求的存储库类型。必须是`model`、`dataset`或`space`之一。默认为`model`。

+   `token` (`str`, *可选*) — 有效的身份验证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

引发

`HTTPError`

+   `HTTPError` — 如果存储库未受限，则为HTTP 400。

+   `HTTPError` — 如果您只有对存储库的只读访问权限，则为HTTP 403。如果您在存储库所属组织中没有`write`或`admin`角色，或者如果您传递了一个`read`令牌，这可能是情况。

+   `HTTPError` — 如果用户在Hub上不存在，则为HTTP 404。

+   `HTTPError` — 如果找不到用户访问请求，则为HTTP 404。

+   `HTTPError` — 如果用户访问请求已经在接受列表中，则为HTTP 404。

接受来自用户的对给定受限存储库的访问请求。

一旦请求被接受，用户将能够下载存储库的任何文件并访问社区选项卡。如果批准模式是自动的，您不必手动接受请求。接受的请求可以随时使用[cancel_access_request()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.cancel_access_request)和[reject_access_request()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.reject_access_request)进行取消或拒绝。

有关受限存储库的更多信息，请参阅[https://huggingface.co/docs/hub/models-gated](https://huggingface.co/docs/hub/models-gated)。

#### `add_collection_item`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7607)

```py
( collection_slug: str item_id: str item_type: CollectionItemType_T note: Optional[str] = None exists_ok: bool = False token: Optional[str] = None )
```

参数

+   `collection_slug` (`str`) — 要更新的集合的Slug。示例：`"TheBloke/recent-models-64f9a55bb3115b4f513ec026"`。

+   `item_id` (`str`) — 要添加到集合中的项目的ID。它可以是Hub上存储库的ID（例如`"facebook/bart-large-mnli"`）或论文ID（例如`"2307.09288"`）。

+   `item_type` (`str`) — 要添加的项目类型。可以是`"model"`、`"dataset"`、`"space"`或`"paper"`之一。

+   `note` (`str`, *可选*) — 要附加到集合中项目的注释。注释的最大长度为500个字符。

+   `exists_ok` (`bool`, *可选*) — 如果为`True`，则如果项目已经存在，则不会引发错误。

+   `token` (`str`, *可选*) — Hugging Face令牌。如果未提供，将默认使用本地保存的令牌。

将项目添加到Hub上的集合中。

返回：[Collection](/docs/huggingface_hub/v0.20.3/en/package_reference/collections#huggingface_hub.Collection)

示例：

```py
>>> from huggingface_hub import add_collection_item
>>> collection = add_collection_item(
...     collection_slug="davanstrien/climate-64f99dc2a5067f6b65531bab",
...     item_id="pierre-loic/climate-news-articles",
...     item_type="dataset"
... )
>>> collection.items[-1].item_id
"pierre-loic/climate-news-articles"
# ^item got added to the collection on last position

# Add item with a note
>>> add_collection_item(
...     collection_slug="davanstrien/climate-64f99dc2a5067f6b65531bab",
...     item_id="datasets/climate_fever",
...     item_type="dataset"
...     note="This dataset adopts the FEVER methodology that consists of 1,535 real-world claims regarding climate-change collected on the internet."
... )
(...)
```

#### `add_space_secret`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6367)

```py
( repo_id: str key: str value: str description: Optional[str] = None token: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 要更新的仓库的ID。示例："bigcode/in-the-stack"。

+   `key` (`str`) — 秘密键。示例："GITHUB_API_KEY"

+   `value` (`str`) — 秘密值。示例："your_github_api_key"。

+   `description` (`str`, *可选*) — 秘密描述。示例："用于访问Github API的Github API密钥"。

+   `token` (`str`, *可选*) — Hugging Face令牌。如果未提供，将默认使用本地保存的令牌。

在空间中添加或更新一个秘密。

秘密允许在空间中设置秘密键或令牌，而无需将它们硬编码。有关更多详细信息，请参见[https://huggingface.co/docs/hub/spaces-overview#managing-secrets](https://huggingface.co/docs/hub/spaces-overview#managing-secrets)。

#### `add_space_variable`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6440)

```py
( repo_id: str key: str value: str description: Optional[str] = None token: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 要更新的仓库的ID。示例："bigcode/in-the-stack"。

+   `key` (`str`) — 变量键。示例："MODEL_REPO_ID"

+   `value` (`str`) — 变量值。示例："the_model_repo_id"。

+   `description` (`str`) — 变量描述。示例："实现模型的模型仓库ID"。

+   `token` (`str`, *可选*) — Hugging Face令牌。如果未提供，将默认使用本地保存的令牌。

在空间中添加或更新一个变量。

变量允许在空间中设置环境变量，而无需将它们硬编码。有关更多详细信息，请参见[https://huggingface.co/docs/hub/spaces-overview#managing-secrets-and-environment-variables](https://huggingface.co/docs/hub/spaces-overview#managing-secrets-and-environment-variables)

#### `cancel_access_request`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7988)

```py
( repo_id: str user: str repo_type: Optional[str] = None token: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 要取消访问请求的仓库的ID。

+   `user` (`str`) — 应取消访问请求的用户的用户名。

+   `repo_type` (`str`, *可选*) — 要取消访问请求的仓库类型。必须是`model`、`dataset`或`space`之一。默认为`model`。

+   `token` (`str`, *可选*) — 有效的身份验证令牌（请参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

引发

`HTTPError`

+   `HTTPError` — 如果仓库未受限，则返回HTTP 400。

+   `HTTPError` — 如果您只有对仓库的只读访问权限，则返回HTTP 403。如果您在仓库所属组织中没有`write`或`admin`角色，或者如果您传递了一个`read`令牌，则可能会出现这种情况。

+   `HTTPError` — 如果Hub上不存在用户，则返回HTTP 404。

+   `HTTPError` — 如果找不到用户访问请求，则返回HTTP 404。

+   `HTTPError` — 如果用户访问请求已在待处理列表中，则返回HTTP 404。

取消用户对特定受限仓库的访问请求。

取消的请求将返回到待处理列表，并且用户将失去对仓库的访问权限。

有关受限仓库的更多信息，请参见[https://huggingface.co/docs/hub/models-gated](https://huggingface.co/docs/hub/models-gated)。

#### `change_discussion_status`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6121)

```py
( repo_id: str discussion_num: int new_status: Literal['open', 'closed'] token: Optional[str] = None comment: Optional[str] = None repo_type: Optional[str] = None ) → export const metadata = 'undefined';DiscussionStatusChange
```

参数

+   `repo_id` (`str`) — 由命名空间（用户或组织）和由斜杠分隔的仓库名称组成。

+   `discussion_num` (`int`) — 讨论或拉取请求的编号。必须是严格正整数。

+   `new_status` (`str`) — 讨论的新状态，可以是`"open"`或`"closed"`。

+   `comment` (`str`, *可选*) — 与状态更改一起发布的可选评论。

+   `repo_type` (`str`, *可选*) — 如果上传到数据集或空间，则设置为`"dataset"`或`"space"`，如果上传到模型，则为`None`或`"model"`。默认为`None`。

+   `token` (`str`, *可选*) — 身份验证令牌（请参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）

返回

[DiscussionStatusChange](/docs/huggingface_hub/v0.20.3/en/package_reference/community#huggingface_hub.DiscussionStatusChange)

状态更改事件

关闭或重新打开讨论或拉取请求。

示例：

```py
>>> new_title = "New title, fixing a typo"
>>> HfApi().rename_discussion(
...     repo_id="username/repo_name",
...     discussion_num=34
...     new_title=new_title
... )
# DiscussionStatusChange(id='deadbeef0000000', type='status-change', ...)

```

引发以下错误：

+   [`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError) - 如果HuggingFace API返回错误

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) - 如果某个参数值无效

+   [RepositoryNotFoundError] 如果无法找到要下载的存储库。这可能是因为它不存在，或者因为它被设置为`private`，而您没有访问权限。

#### `comment_discussion`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L5978)

```py
( repo_id: str discussion_num: int comment: str token: Optional[str] = None repo_type: Optional[str] = None ) → export const metadata = 'undefined';DiscussionComment
```

参数

+   `repo_id`（`str`） - 由`/`分隔的命名空间（用户或组织）和存储库名称。

+   `discussion_num`（`int`） - 讨论或拉取请求的编号。必须是严格的正整数。

+   `comment`（`str`） - 要创建的评论内容。评论支持Markdown格式。

+   `repo_type`（`str`，*可选*） - 如果上传到数据集或空间，则设置为`"dataset"`或`"space"`，如果上传到模型，则为`None`或`"model"`。默认为`None`。

+   `token`（`str`，*可选*） - 身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）

返回

[DiscussionComment](/docs/huggingface_hub/v0.20.3/en/package_reference/community#huggingface_hub.DiscussionComment)

新创建的评论

在给定讨论上创建新评论。

示例：

```py

>>> comment = """
... Hello @otheruser!
...
... # This is a title
...
... **This is bold**, *this is italic* and ~this is strikethrough~
... And [this](http://url) is a link
... """

>>> HfApi().comment_discussion(
...     repo_id="username/repo_name",
...     discussion_num=34
...     comment=comment
... )
# DiscussionComment(id='deadbeef0000000', type='comment', ...)

```

引发以下错误：

+   [`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError) - 如果HuggingFace API返回错误

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) - 如果某个参数值无效

+   [RepositoryNotFoundError] 如果无法找到要下载的存储库。这可能是因为它不存在，或者因为它被设置为`private`，而您没有访问权限。

#### `create_branch`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L5354)

```py
( repo_id: str branch: str revision: Optional[str] = None token: Optional[str] = None repo_type: Optional[str] = None exist_ok: bool = False )
```

参数

+   `repo_id`（`str`） - 将创建分支的存储库。示例：`"user/my-cool-model"`。

+   `branch`（`str`） - 要创建的分支的名称。

+   `revision`（`str`，*可选*） - 创建分支的git修订版。可以是分支名称或提交的OID/SHA，以十六进制字符串表示。默认为`"main"`分支的头部。

+   `token`（`str`，*可选*） - 身份验证令牌。将默认为存储的令牌。

+   `repo_type`（`str`，*可选*） - 如果在数据集或空间上创建分支，则设置为`"dataset"`或`"space"`，如果标记模型，则为`None`或`"model"`。默认为`None`。

+   `exist_ok`（`bool`，*可选*，默认为`False`） - 如果为`True`，则如果分支已经存在，则不会引发错误。

引发

[RepositoryNotFoundError] 或 [BadRequestError] 或 [HfHubHTTPError]

+   [RepositoryNotFoundError] - 如果找不到存储库（错误404）：错误的repo_id/repo_type，私有但未经身份验证或存储库不存在。

+   [BadRequestError] - 如果分支的引用无效。例如：`refs/pr/5`或‘refs/foo/bar’。

+   [HfHubHTTPError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HfHubHTTPError) — 如果仓库中已经存在该分支（错误409），并且`exist_ok`设置为`False`。

在Hub上为仓库创建一个新分支，从指定的修订版本开始（默认为`main`）。要找到适合您需求的修订版本，可以使用[list_repo_refs()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_repo_refs)或[list_repo_commits()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_repo_commits)。

#### `create_collection`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7439)

```py
( title: str namespace: Optional[str] = None description: Optional[str] = None private: bool = False exists_ok: bool = False token: Optional[str] = None )
```

参数

+   `title` (`str`) — 要创建的集合的标题。示例："Recent models"。

+   `namespace` (`str`, *optional*) — 要创建的集合的命名空间（用户名或组织）。将默认为所有者名称。

+   `description` (`str`, *optional*) — 要创建的集合的描述。

+   `private` (`bool`, *optional*) — 集合是否应该是私有的。默认为`False`（即公共集合）。

+   `exists_ok` (`bool`, *optional*) — 如果为`True`，则如果集合已经存在，则不会引发错误。

+   `token` (`str`, *optional*) — Hugging Face令牌。如果未提供，将默认为本地保存的令牌。

在Hub上创建一个新的集合。

返回：[Collection](/docs/huggingface_hub/v0.20.3/en/package_reference/collections#huggingface_hub.Collection)

示例：

```py
>>> from huggingface_hub import create_collection
>>> collection = create_collection(
...     title="ICCV 2023",
...     description="Portfolio of models, papers and demos I presented at ICCV 2023",
... )
>>> collection.slug
"username/iccv-2023-64f9a55bb3115b4f513ec026"
```

#### `create_commit`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L3403)

```py
( repo_id: str operations: Iterable[CommitOperation] commit_message: str commit_description: Optional[str] = None token: Optional[str] = None repo_type: Optional[str] = None revision: Optional[str] = None create_pr: Optional[bool] = None num_threads: int = 5 parent_commit: Optional[str] = None run_as_future: bool = False ) → export const metadata = 'undefined';CommitInfo or Future
```

参数

+   `repo_id` (`str`) — 将创建提交的仓库，例如："username/custom_transformers"

+   `operations` (`Iterable` of `CommitOperation()`) — 要包含在提交中的操作的可迭代对象，可以是：

    +   [CommitOperationAdd](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitOperationAdd) 用于上传文件

    +   [CommitOperationDelete](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitOperationDelete) 用于删除文件

    +   [CommitOperationCopy](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitOperationCopy) 用于复制文件

    操作对象将被修改以包含与上传相关的信息。不要重复使用相同的对象进行多次提交。

+   `commit_message` (`str`) — 将创建的提交的摘要（第一行）。

+   `commit_description` (`str`, *optional*) — 将创建的提交的描述

+   `token` (`str`, *optional*) — 认证令牌，使用`HfApi.login`方法获取。将默认为存储的令牌。

+   `repo_type` (`str`, *optional*) — 如果上传到数据集或空间，则设置为`"dataset"`或`"space"`，如果上传到模型，则为`None`或`"model"`。默认为`None`。

+   `revision` (`str`, *optional*) — 提交的git修订版本。默认为`"main"`分支的头部。

+   `create_pr` (`boolean`, *optional*) — 是否创建一个包含该提交的Pull Request。默认为`False`。如果未设置`revision`，则PR将针对`"main"`分支打开。如果设置了`revision`并且是一个分支，则PR将针对该分支打开。如果设置了`revision`并且不是分支名称（例如：一个提交oid），服务器将返回`RevisionNotFoundError`。

+   `num_threads` (`int`, *optional*) — 用于上传文件的并发线程数。默认为5。将其设置为2意味着最多同时上传2个文件。

+   `parent_commit` (`str`, *可选*) — 父提交的 OID / SHA，以十六进制字符串表示。也支持缩写（前 7 个字符）。如果指定且 `create_pr` 为 `False`，则如果 `revision` 没有指向 `parent_commit`，提交将失败。如果指定且 `create_pr` 为 `True`，将从 `parent_commit` 创建拉取请求。指定 `parent_commit` 确保在提交更改之前存储库未更改，并且如果存储库同时更新/提交，则尤其有用。

+   `run_as_future` (`bool`, *可选*) — 是否在后台运行此方法。后台作业按顺序运行，不会阻塞主线程。传递 `run_as_future=True` 将返回一个 [Future](https://docs.python.org/3/library/concurrent.futures.html#future-objects) 对象。默认为 `False`。

返回

[CommitInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitInfo) 或 `Future`

包含有关新创建提交的信息的 [CommitInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitInfo) 实例（提交哈希、提交 URL、PR URL、提交消息等）。如果传递 `run_as_future=True`，则返回一个 Future 对象，该对象在执行时将包含结果。

引发

`ValueError` 或 [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError)

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) — 如果提交消息为空。

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) — 如果父提交不是有效的提交 OID。

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) — 如果 Hub API 返回 HTTP 400 错误（错误请求）

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) — 如果 `create_pr` 为 `True`，且修订版本既不是 `None` 也不是 `"main"`。

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果找不到存储库（错误 404）：错误的 repo_id/repo_type，私有但未经身份验证，或存储库不存在。

在给定存储库中创建提交，根据需要删除和上传文件。

在提交过程中，输入的 `CommitOperation` 列表将发生变化。不要重复使用相同的对象进行多次提交。

`create_commit` 假定存储库已经存在于 Hub 上。如果收到客户端错误 404，请确保已经进行了身份验证，并且 `repo_id` 和 `repo_type` 设置正确。如果存储库不存在，请先使用 [create_repo()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_repo) 创建它。

`create_commit` 限制为 25k 个 LFS 文件和 1GB 的常规文件负载。

#### `create_commits_on_pr`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L3625)

```py
( repo_id: str addition_commits: List[List[CommitOperationAdd]] deletion_commits: List[List[CommitOperationDelete]] commit_message: str commit_description: Optional[str] = None token: Optional[str] = None repo_type: Optional[str] = None merge_pr: bool = True num_threads: int = 5 verbose: bool = False ) → export const metadata = 'undefined';str
```

参数

+   `repo_id` (`str`) — 将提交推送到的存储库。示例："username/my-cool-model"。

+   `addition_commits` (`List` of `List` of [CommitOperationAdd](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitOperationAdd)) — 包含 [CommitOperationAdd](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitOperationAdd) 列表的列表。每个子列表将导致一个提交到 PR。

    deletion_commits — 包含 [CommitOperationDelete](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitOperationDelete) 列表的列表。每个子列表将导致一个提交到 PR。删除提交在添加提交之前推送。

+   `commit_message` (`str`) — 将要创建的提交的摘要（第一行）。也将是 PR 的标题。

+   `commit_description` (`str`, *可选*) — 将要创建的提交的描述。描述将添加到 PR 中。

+   `token` (`str`, *optional*) — 认证令牌，使用 `HfApi.login` 方法获取。将默认使用存储的令牌。

+   `repo_type` (`str`, *optional*) — 如果上传到数据集或空间，则设置为 `"dataset"` 或 `"space"`，如果上传到模型，则为 `None` 或 `"model"`。默认为 `None`。

+   `merge_pr` (`bool`) — 如果设置为 `True`，则 Pull Request 将在流程结束时合并。默认为 `True`。

+   `num_threads` (`int`, *optional*) — 上传文件的并发线程数。默认为 5。

+   `verbose` (`bool`) — 如果设置为 `True`，则流程将以详细模式运行，即打印有关正在进行的任务的信息。默认为 `False`。

返回

`str`

创建的 PR 的 URL。

引发

`MultiCommitException`

+   `MultiCommitException` — 如果在流程中发生意外问题：空提交，PR 中的意外提交，意外的 PR 描述等。

将更改推送到 Hub 中的多个提交。

提交将推送到草稿 PR 分支。如果上传失败或中断，可以恢复。进度将在 PR 描述中跟踪。在流程结束时，PR 被设置为打开，并且标题将更新以匹配初始提交消息。如果传递了 `merge_pr=True`，则 PR 将自动合并。

所有删除提交将首先推送，然后是添加提交。由于我们可能会在将来实现并行提交，因此提交的顺序不能保证。请确保您不会多次更新同一文件。

`create_commits_on_pr` 是实验性的。其 API 和行为可能会在未来发生变化，恕不另行通知。

`create_commits_on_pr` 假设仓库已经存在于 Hub 上。如果收到客户端错误 404，请确保已经进行了身份验证，并且 `repo_id` 和 `repo_type` 设置正确。如果仓库不存在，请先使用 [create_repo()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_repo) 创建它。

示例：

```py
>>> from huggingface_hub import HfApi, plan_multi_commits
>>> addition_commits, deletion_commits = plan_multi_commits(
...     operations=[
...          CommitOperationAdd(...),
...          CommitOperationAdd(...),
...          CommitOperationDelete(...),
...          CommitOperationDelete(...),
...          CommitOperationAdd(...),
...     ],
... )
>>> HfApi().create_commits_on_pr(
...     repo_id="my-cool-model",
...     addition_commits=addition_commits,
...     deletion_commits=deletion_commits,
...     (...)
...     verbose=True,
... )
```

#### `create_discussion`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L5805)

```py
( repo_id: str title: str token: Optional[str] = None description: Optional[str] = None repo_type: Optional[str] = None pull_request: bool = False )
```

参数

+   `repo_id` (`str`) — 一个由 `/` 分隔的命名空间（用户或组织）和仓库名称。

+   `title` (`str`) — 讨论的标题。最多可以有 200 个字符，并且必须至少有 3 个字符。前导和尾随空格将被删除。

+   `token` (`str`, *optional*) — 认证令牌（参见 [https://huggingface.co/settings/token](https://huggingface.co/settings/token)）

+   `description` (`str`, *optional*) — 一个可选的描述，用于 Pull Request。默认为 `"Discussion opened with the huggingface_hub Python library"`

+   `pull_request` (`bool`, *optional*) — 是否创建 Pull Request 或讨论。如果为 `True`，则创建 Pull Request。如果为 `False`，则创建讨论。默认为 `False`。

+   `repo_type` (`str`, *optional*) — 如果上传到数据集或空间，则设置为 `"dataset"` 或 `"space"`，如果上传到模型，则为 `None` 或 `"model"`。默认为 `None`。

创建一个讨论或 Pull Request。

通过编程方式创建的 Pull Request 将处于 `"draft"` 状态。

使用 [HfApi.create_commit()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_commit) 可以一次性创建带有更改的 Pull Request。

返回：[DiscussionWithDetails](/docs/huggingface_hub/v0.20.3/en/package_reference/community#huggingface_hub.DiscussionWithDetails)

引发以下错误：

+   如果 HuggingFace API 返回错误，则为 [`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError)

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果某个参数值无效

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果要下载的仓库找不到。这可能是因为它不存在，或者因为它被设置为 `private`，而您没有访问权限。

#### `create_inference_endpoint`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6929)

```py
( name: str repository: str framework: str accelerator: str instance_size: str instance_type: str region: str vendor: str account_id: Optional[str] = None min_replica: int = 0 max_replica: int = 1 revision: Optional[str] = None task: Optional[str] = None custom_image: Optional[Dict] = None type: InferenceEndpointType = <InferenceEndpointType.PROTECTED: 'protected'> namespace: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';InferenceEndpoint
```

参数

+   `name` (`str`) — 新推理端点的唯一名称。

+   `repository` (`str`) — 与推理端点关联的模型存储库的名称（例如 `"gpt2"`）。

+   `framework` (`str`) — 用于模型的机器学习框架（例如 `"custom"`）。

+   `accelerator` (`str`) — 用于推理的硬件加速器（例如 `"cpu"`）。

+   `instance_size` (`str`) — 用于托管模型的实例的大小或类型（例如 `"large"`）。

+   `instance_type` (`str`) — 将部署推理端点的云实例类型（例如 `"c6i"`）。

+   `region` (`str`) — 将创建推理端点的云区域（例如 `"us-east-1"`）。

+   `vendor` (`str`) — 将托管推理端点的云提供商或供应商（例如 `"aws"`）。

+   `account_id` (`str`, *optional*) — 用于将VPC链接到私有推理端点的帐户ID（如果适用）。

+   `min_replica` (`int`, *optional*) — 保持运行的最小副本（实例）数量。默认为0。

+   `max_replica` (`int`, *optional*) — 要扩展到的最大副本（实例）数量。默认为1。

+   `revision` (`str`, *optional*) — 部署在推理端点上的特定模型修订版（例如 `"6c0e6080953db56375760c0471a8c5f2929baf11"`）。

+   `task` (`str`, *optional*) — 部署模型的任务（例如 `"text-classification"`）。

+   `custom_image` (`Dict`, *optional*) — 用于推理端点的自定义Docker镜像。如果要部署在`text-generation-inference`（TGI）框架上运行的推理端点，则这很有用（请参阅示例）。

+   `type` ([`InferenceEndpointType]`, *optional*) -- 推理端点的类型，可以是`“protected”`（默认）、`“public”`或`“private”`。

+   `namespace` (`str`, *optional*) — 推理端点将被创建的命名空间。默认为当前用户的命名空间。

+   `token` (`str`, *optional*) — 认证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

返回

[InferenceEndpoint](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_endpoints#huggingface_hub.InferenceEndpoint)

有关更新的推理端点的信息。

创建一个新的推理端点。

示例：

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> create_inference_endpoint(
...     "my-endpoint-name",
...     repository="gpt2",
...     framework="pytorch",
...     task="text-generation",
...     accelerator="cpu",
...     vendor="aws",
...     region="us-east-1",
...     type="protected",
...     instance_size="medium",
...     instance_type="c6i",
... )
>>> endpoint
InferenceEndpoint(name='my-endpoint-name', status="pending",...)

# Run inference on the endpoint
>>> endpoint.client.text_generation(...)
"..."
```

```py
# Start an Inference Endpoint running Zephyr-7b-beta on TGI
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> create_inference_endpoint(
...     "aws-zephyr-7b-beta-0486",
...     repository="HuggingFaceH4/zephyr-7b-beta",
...     framework="pytorch",
...     task="text-generation",
...     accelerator="gpu",
...     vendor="aws",
...     region="us-east-1",
...     type="protected",
...     instance_size="medium",
...     instance_type="g5.2xlarge",
...     custom_image={
...         "health_route": "/health",
...         "env": {
...             "MAX_BATCH_PREFILL_TOKENS": "2048",
...             "MAX_INPUT_LENGTH": "1024",
...             "MAX_TOTAL_TOKENS": "1512",
...             "MODEL_ID": "/repository"
...         },
...         "url": "ghcr.io/huggingface/text-generation-inference:1.1.0",
...     },
... )

```

#### `create_pull_request`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L5894)

```py
( repo_id: str title: str token: Optional[str] = None description: Optional[str] = None repo_type: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 由斜杠分隔的命名空间（用户或组织）和存储库名称。

+   `title` (`str`) — 讨论的标题。最多可以有200个字符，并且必须至少有3个字符。前导和尾随空格将被剥离。

+   `token` (`str`, *optional*) — 认证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）

+   `description` (`str`, *optional*) — 拉取请求的可选描述。默认为 `"Discussion opened with the huggingface_hub Python library"`。

+   `repo_type` (`str`, *optional*) — 如果上传到数据集或空间，则设置为`"dataset"`或`"space"`，如果上传到模型，则为`None`或`"model"`。默认为`None`。

创建拉取请求。以编程方式创建的拉取请求将处于`"draft"`状态。

还可以使用[HfApi.create_commit()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_commit)一次性创建带有更改的拉取请求；

这是[HfApi.create_discussion()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_discussion)的包装器。

返回：[DiscussionWithDetails](/docs/huggingface_hub/v0.20.3/en/package_reference/community#huggingface_hub.DiscussionWithDetails)

引发以下错误：

+   如果HuggingFace API返回错误，则引发[`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError)

+   如果某个参数值无效，则引发[`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果要下载的存储库找不到。这可能是因为它不存在，或者因为它被设置为`private`而您无法访问。

#### `create_repo`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L3063)

```py
( repo_id: str token: Optional[str] = None private: bool = False repo_type: Optional[str] = None exist_ok: bool = False space_sdk: Optional[str] = None space_hardware: Optional[SpaceHardware] = None space_storage: Optional[SpaceStorage] = None space_sleep_time: Optional[int] = None space_secrets: Optional[List[Dict[str, str]]] = None space_variables: Optional[List[Dict[str, str]]] = None ) → export const metadata = 'undefined';RepoUrl
```

参数

+   `repo_id` (`str`) — 由斜杠分隔的命名空间（用户或组织）和存储库名称。

+   `token` (`str`, *optional*) — 身份验证令牌（请参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）

+   `private` (`bool`, *optional*, 默认为`False`) — 模型存储库是否应为私有。

+   `repo_type` (`str`, *optional*) — 如果上传到数据集或空间，则设置为`"dataset"`或`"space"`，如果上传到模型，则为`None`或`"model"`。默认为`None`。

+   `exist_ok` (`bool`, *optional*, 默认为`False`) — 如果为`True`，则如果存储库已经存在，则不会引发错误。

+   `space_sdk` (`str`, *optional*) — 如果`repo_type`为“space”，则要使用的SDK选择。可以是“streamlit”、“gradio”、“docker”或“static”。

+   `space_hardware` (`SpaceHardware`或`str`, *optional*) — 如果`repo_type`为“space”，则硬件选择。有关完整列表，请参见[SpaceHardware](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceHardware)。

+   `space_storage` (`SpaceStorage`或`str`, *optional*) — 持久性存储层的选择。示例："small"。有关完整列表，请参见[SpaceStorage](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceStorage)。

+   `space_sleep_time` (`int`, *optional`) — 在将Space置于休眠状态之前等待的不活动秒数。如果不希望Space休眠，则设置为`-1`（升级硬件的默认行为）。对于免费硬件，您无法配置休眠时间（值固定为48小时的不活动时间）。有关更多详细信息，请参见[https://huggingface.co/docs/hub/spaces-gpus#sleep-time](https://huggingface.co/docs/hub/spaces-gpus#sleep-time)。

+   `space_secrets` (`List[Dict[str, str]]`, *optional*) — 要在您的Space中设置的秘密密钥列表。每个项目的形式为`{"key": ..., "value": ..., "description": ...}`，其中描述是可选的。有关更多详细信息，请参见[https://huggingface.co/docs/hub/spaces-overview#managing-secrets](https://huggingface.co/docs/hub/spaces-overview#managing-secrets)。

+   `space_variables` (`List[Dict[str, str]]`, *optional*) — 要在您的Space中设置的公共环境变量列表。每个项目的形式为`{"key": ..., "value": ..., "description": ...}`，其中描述是可选的。有关更多详细信息，请参见[https://huggingface.co/docs/hub/spaces-overview#managing-secrets-and-environment-variables](https://huggingface.co/docs/hub/spaces-overview#managing-secrets-and-environment-variables)。

返回

[RepoUrl](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.RepoUrl)

新创建的存储库的URL。值是包含`endpoint`、`repo_type`和`repo_id`等属性的`str`子类。

在HuggingFace Hub上创建一个空的存储库。

#### `create_tag`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L5470)

```py
( repo_id: str tag: str tag_message: Optional[str] = None revision: Optional[str] = None token: Optional[str] = None repo_type: Optional[str] = None exist_ok: bool = False )
```

参数

+   `repo_id` (`str`) — 将要标记的存储库。示例："user/my-cool-model"。

+   `tag` (`str`) — 要创建的标签名称。

+   `tag_message` (`str`, *optional*) — 要创建的标签的描述。

+   `revision` (`str`, *optional*) — 要标记的git修订版。可以是分支名称或提交的OID/SHA，以十六进制字符串表示。也支持缩写（前7个字符）。默认为`"main"`分支的头部。

+   `token` (`str`, *可选*) — 认证令牌。将默认为存储的令牌。

+   `repo_type` (`str`, *可选*) — 如果在数据集或空间上打标签，则设置为`"dataset"`或`"space"`，如果在模型上打标签，则设置为`None`或`"model"`。默认为`None`。

+   `exist_ok` (`bool`, *可选*, 默认为`False`) — 如果为`True`，则如果标签已经存在，则不会引发错误。

引发

[RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 或 [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) 或 [HfHubHTTPError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HfHubHTTPError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果找不到存储库（错误404）：错误的repo_id/repo_type，私有但未经身份验证或存储库不存在。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) — 如果在存储库中找不到修订版（错误404）。

+   [HfHubHTTPError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HfHubHTTPError) — 如果存储库中已经存在该分支（错误409），并且`exist_ok`设置为`False`。

在Hub上标记存储库的给定提交。

#### `dataset_info`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2089)

```py
( repo_id: str revision: Optional[str] = None timeout: Optional[float] = None files_metadata: bool = False token: Optional[Union[bool, str]] = None ) → export const metadata = 'undefined';hf_api.DatasetInfo
```

参数

+   `repo_id` (`str`) — 由斜杠分隔的命名空间（用户或组织）和存储库名称。

+   `revision` (`str`, *可选*) — 要获取信息的数据集存储库的修订版。

+   `timeout` (`float`, *可选*) — 是否为请求到Hub设置超时。

+   `files_metadata` (`bool`, *可选*) — 是否检索存储库中文件的元数据（大小、LFS元数据等）。默认为`False`。

+   `token` (`bool` 或 `str`, *可选*) — 有效的认证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则不会在请求头中发送令牌。

返回

[hf_api.DatasetInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.DatasetInfo)

数据集存储库信息。

获取huggingface.co上一个特定数据集的信息。

如果传递一个可接受的令牌，数据集可以是私有的。

引发以下错误：

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果无法找到要下载的存储库。这可能是因为它不存在，或者因为它设置为`private`而您没有访问权限。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) 如果无法找到要下载的修订版。

#### `delete_branch`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L5421)

```py
( repo_id: str branch: str token: Optional[str] = None repo_type: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 将删除分支的存储库。示例：`"user/my-cool-model"`。

+   `branch` (`str`) — 要删除的分支的名称。

+   `token` (`str`, *可选*) — 认证令牌。将默认为存储的令牌。

+   `repo_type` (`str`, *可选*) — 如果在数据集或空间上创建分支，则设置为`"dataset"`或`"space"`，如果在模型上打标签，则设置为`None`或`"model"`。默认为`None`。

引发

[RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 或 [HfHubHTTPError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HfHubHTTPError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果找不到仓库（错误 404）：错误的 repo_id/repo_type，私有但未经身份验证或仓库不存在。

+   [HfHubHTTPError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HfHubHTTPError) — 尝试删除受保护的分支时。例如：`main` 无法删除。

+   [HfHubHTTPError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HfHubHTTPError) — 尝试删除不存在的分支时。

从 Hub 上的仓库中删除一个分支。

#### `delete_collection`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7569)

```py
( collection_slug: str missing_ok: bool = False token: Optional[str] = None )
```

参数

+   `collection_slug` (`str`) — 要删除的集合的 slug。示例：`"TheBloke/recent-models-64f9a55bb3115b4f513ec026"`。

+   `missing_ok` (`bool`, *optional*) — 如果为 `True`，则如果集合不存在则不会引发错误。

+   `token` (`str`, *optional*) — Hugging Face 令牌。如果未提供，将默认为本地保存的令牌。

在 Hub 上删除一个集合。

示例：

```py
>>> from huggingface_hub import delete_collection
>>> collection = delete_collection("username/useless-collection-64f9a55bb3115b4f513ec026", missing_ok=True)
```

这是一个不可逆的操作。删除的集合无法恢复。

#### `delete_collection_item`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7727)

```py
( collection_slug: str item_object_id: str missing_ok: bool = False token: Optional[str] = None )
```

参数

+   `collection_slug` (`str`) — 要更新的集合的 slug。示例：`"TheBloke/recent-models-64f9a55bb3115b4f513ec026"`。

+   `item_object_id` (`str`) — 集合中项目的 ID。这不是 Hub 上项目的 id（repo_id 或 paper id）。必须从 [CollectionItem](/docs/huggingface_hub/v0.20.3/en/package_reference/collections#huggingface_hub.CollectionItem) 对象中检索。示例：`collection.items[0]._id`。

+   `missing_ok` (`bool`, *optional*) — 如果为 `True`，则如果项目不存在则不会引发错误。

+   `token` (`str`, *optional*) — Hugging Face 令牌。如果未提供，将默认为本地保存的令牌。

从集合中删除一个项目。

示例：

```py
>>> from huggingface_hub import get_collection, delete_collection_item

# Get collection first
>>> collection = get_collection("TheBloke/recent-models-64f9a55bb3115b4f513ec026")

# Delete item based on its ID
>>> delete_collection_item(
...     collection_slug="TheBloke/recent-models-64f9a55bb3115b4f513ec026",
...     item_object_id=collection.items[-1].item_object_id,
... )
```

#### `delete_file`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L4628)

```py
( path_in_repo: str repo_id: str token: Optional[str] = None repo_type: Optional[str] = None revision: Optional[str] = None commit_message: Optional[str] = None commit_description: Optional[str] = None create_pr: Optional[bool] = None parent_commit: Optional[str] = None )
```

参数

+   `path_in_repo` (`str`) — 仓库中的相对文件路径，例如：`"checkpoints/1fec34a/weights.bin"`

+   `repo_id` (`str`) — 将删除文件的仓库，例如：`"username/custom_transformers"`

+   `token` (`str`, *optional*) — 认证令牌，使用 `HfApi.login` 方法获取。将默认为存储的令牌。

+   `repo_type` (`str`, *optional*) — 如果文件在数据集或空间中，则设置为 `"dataset"` 或 `"space"`，如果在模型中，则为 `None` 或 `"model"`。默认为 `None`。

+   `revision` (`str`, *optional*) — 提交的 git 版本。默认为 `"main"` 分支的头部。

+   `commit_message` (`str`, *optional*) — 生成提交的摘要 / 标题 / 第一行。默认为 `f"Delete {path_in_repo} with huggingface_hub"`。

+   `commit_description` (`str` *optional*) — 生成提交的描述

+   `create_pr` (`boolean`, *optional*) — 是否创建一个包含该提交的 Pull Request。默认为 `False`。如果未设置 `revision`，PR 将针对 `"main"` 分支打开。如果设置了 `revision` 并且是一个分支，PR 将针对该分支打开。如果设置了 `revision` 并且不是分支名称（例如：提交 oid），服务器将返回 `RevisionNotFoundError`。

+   `parent_commit` (`str`, *可选*) — 父提交的OID / SHA，以十六进制字符串表示。也支持缩写（前7个字符）。如果指定并且`create_pr`为`False`，则如果`revision`不指向`parent_commit`，提交将失败。如果指定并且`create_pr`为`True`，将从`parent_commit`创建拉取请求。指定`parent_commit`可确保在提交更改之前存储库未更改，并且如果存储库同时更新/提交，则尤其有用。

删除给定存储库中的文件。

引发以下错误：

+   [HTTPError](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError) 如果HuggingFace API返回错误

+   [ValueError](https://docs.python.org/3/library/exceptions.html#ValueError) 如果某个参数值无效

+   [RepositoryNotFoundError] 如果无法找到要下载的存储库。这可能是因为它不存在，或者因为它被设置为`private`，而您没有访问权限。

+   [RevisionNotFoundError] 如果无法找到要下载的修订版本。

+   [EntryNotFoundError] 如果无法找到要下载的文件。

#### `delete_folder`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L4716)

```py
( path_in_repo: str repo_id: str token: Optional[str] = None repo_type: Optional[str] = None revision: Optional[str] = None commit_message: Optional[str] = None commit_description: Optional[str] = None create_pr: Optional[bool] = None parent_commit: Optional[str] = None )
```

参数

+   `path_in_repo` (`str`) — 存储库中的相对文件夹路径，例如：`"checkpoints/1fec34a"`。

+   `repo_id` (`str`) — 将删除文件夹的存储库，例如：`"username/custom_transformers"`

+   `token` (`str`, *可选*) — 认证令牌，使用`HfApi.login`方法获取。将默认为存储的令牌。

+   `repo_type` (`str`, *可选*) — 如果文件夹在数据集或空间中，则设置为`"dataset"`或`"space"`，如果在模型中则为`None`或`"model"`。默认为`None`。

+   `revision` (`str`, *可选*) — 要提交的git修订版本。默认为`"main"`分支的头部。

+   `commit_message` (`str`, *可选*) — 生成提交的摘要/标题/第一行。默认为`f"Delete folder {path_in_repo} with huggingface_hub"`。

+   `commit_description` (`str` *可选*) — 生成提交的描述。

+   `create_pr` (`boolean`, *可选*) — 是否创建具有该提交的拉取请求。默认为`False`。如果未设置`revision`，则PR将针对`"main"`分支打开。如果设置了`revision`并且是一个分支，则PR将针对此分支打开。如果设置了`revision`并且不是分支名称（例如：一个提交oid），服务器将返回`RevisionNotFoundError`。

+   `parent_commit` (`str`, *可选*) — 父提交的OID / SHA，以十六进制字符串表示。也支持缩写（前7个字符）。如果指定并且`create_pr`为`False`，则如果`revision`不指向`parent_commit`，提交将失败。如果指定并且`create_pr`为`True`，将从`parent_commit`创建拉取请求。指定`parent_commit`可确保在提交更改之前存储库未更改，并且如果存储库同时更新/提交，则尤其有用。

删除给定存储库中的文件夹。

对[create_commit()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_commit)方法的简单包装。

#### `delete_inference_endpoint`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7216)

```py
( name: str namespace: Optional[str] = None token: Optional[str] = None )
```

参数

+   `name` (`str`) — 要删除的推断端点的名称。

+   `namespace` (`str`, *可选*) — 推断端点所在的命名空间。默认为当前用户。

+   `token` (`str`, *可选*) — 认证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

删除一个推理端点。

此操作是不可逆的。如果您不想为推理端点付费，最好使用[pause_inference_endpoint()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.pause_inference_endpoint)暂停它或使用[scale_to_zero_inference_endpoint()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.scale_to_zero_inference_endpoint)将其缩减为零。

为了方便起见，您也可以使用[InferenceEndpoint.delete()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_endpoints#huggingface_hub.InferenceEndpoint.delete)删除推理端点。

#### `delete_repo`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L3198)

```py
( repo_id: str token: Optional[str] = None repo_type: Optional[str] = None missing_ok: bool = False )
```

参数

+   `repo_id` (`str`) — 由命名空间（用户或组织）和由`/`分隔的repo名称。

+   `token` (`str`, *可选*) — 认证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）

+   `repo_type` (`str`, *可选*) — 如果上传到数据集或空间，则设置为`"dataset"`或`"space"`，如果上传到模型，则设置为`None`或`"model"`。

+   `missing_ok` (`bool`, *可选*, 默认为`False`) — 如果为`True`，则如果repo不存在，则不会引发错误。

Raises

+   +   — [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果要删除的存储库找不到且`missing_ok`设置为False（默认）。

从HuggingFace Hub中删除一个repo。注意：这是不可逆的。

#### `delete_space_secret`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6398)

```py
( repo_id: str key: str token: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 要更新的repo的ID。示例："bigcode/in-the-stack"。

+   `key` (`str`) — 秘密键。示例："GITHUB_API_KEY"。

+   `token` (`str`, *可选*) — Hugging Face令牌。如果未提供，将默认使用本地保存的令牌。

从空间中删除一个秘密。

秘密允许在不硬编码的情况下将秘密键或令牌设置到空间中。有关更多详细信息，请参阅[https://huggingface.co/docs/hub/spaces-overview#managing-secrets](https://huggingface.co/docs/hub/spaces-overview#managing-secrets)。

#### `delete_space_storage`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6842)

```py
( repo_id: str token: Optional[str] = None ) → export const metadata = 'undefined';SpaceRuntime
```

参数

+   `repo_id` (`str`) — 要更新的空间的ID。示例："HuggingFaceH4/open_llm_leaderboard"。

+   `token` (`str`, *可选*) — Hugging Face令牌。如果未提供，将默认使用本地保存的令牌。

返回

[SpaceRuntime](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceRuntime)

关于空间的运行时信息，包括空间阶段和硬件。

Raises

`BadRequestError`

+   `BadRequestError` — 如果空间没有持久存储。

删除空间的持久存储。

#### `delete_space_variable`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6472)

```py
( repo_id: str key: str token: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 要更新的repo的ID。示例："bigcode/in-the-stack"。

+   `key` (`str`) — 变量键。示例："MODEL_REPO_ID"

+   `token` (`str`, *可选*) — Hugging Face令牌。如果未提供，将默认使用本地保存的令牌。

从空间中删除一个变量。

变量允许在不硬编码的情况下将环境变量设置到空间中。有关更多详细信息，请参阅[https://huggingface.co/docs/hub/spaces-overview#managing-secrets-and-environment-variables](https://huggingface.co/docs/hub/spaces-overview#managing-secrets-and-environment-variables)

#### `delete_tag`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L5541)

```py
( repo_id: str tag: str token: Optional[str] = None repo_type: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 将删除标签的仓库。示例："user/my-cool-model"。

+   `tag` (`str`) — 要删除的标签名称。

+   `token` (`str`, *optional*) — 认证令牌。将默认使用存储的令牌。

+   `repo_type` (`str`, *optional*) — 如果标记数据集或空间，则设置为 `"dataset"` 或 `"space"`，如果标记模型，则设置为 `None` 或 `"model"`。默认为 `None`。

引发

[RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 或 [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果未找到仓库（错误 404）：错误的 repo_id/repo_type、私有但未经身份验证或仓库不存在。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) — 如果未找到标签。

从 Hub 上的仓库中删除一个标签。

#### `duplicate_space`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6691)

```py
( from_id: str to_id: Optional[str] = None private: Optional[bool] = None token: Optional[str] = None exist_ok: bool = False hardware: Optional[SpaceHardware] = None storage: Optional[SpaceStorage] = None sleep_time: Optional[int] = None secrets: Optional[List[Dict[str, str]]] = None variables: Optional[List[Dict[str, str]]] = None ) → export const metadata = 'undefined';RepoUrl
```

参数

+   `from_id` (`str`) — 要复制的空间的 ID。示例："pharma/CLIP-Interrogator"。

+   `to_id` (`str`, *optional*) — 新空间的 ID。示例："dog/CLIP-Interrogator"。如果未提供，则新空间将具有与原始空间相同的名称，但在您的帐户中。

+   `private` (`bool`, *optional*) — 新空间是否应为私有。默认与原始空间的隐私设置相同。

+   `token` (`str`, *optional*) — Hugging Face token. 如果未提供，将默认使用本地保存的令牌。

+   `exist_ok` (`bool`, *optional*, 默认为 `False`) — 如果为 `True`，则如果仓库已经存在，则不会引发错误。

+   `hardware` (`SpaceHardware` 或 `str`, *optional*) — 硬件选择。示例："t4-medium"。查看 [SpaceHardware](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceHardware) 获取完整列表。

+   `storage` (`SpaceStorage` 或 `str`, *optional*) — 持久存储层的选择。示例："small"。查看 [SpaceStorage](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceStorage) 获取完整列表。

+   `sleep_time` (`int`, *optional*) — 在将空间置于休眠状态之前等待的不活动秒数。如果不希望您的空间休眠（升级后硬件的默认行为），请设置为 `-1`。对于免费硬件，您无法配置休眠时间（值固定为 48 小时的不活动时间）。有关更多详细信息，请参见 [https://huggingface.co/docs/hub/spaces-gpus#sleep-time](https://huggingface.co/docs/hub/spaces-gpus#sleep-time)。

+   `secrets` (`List[Dict[str, str]]`, *optional*) — 要在您的空间中设置的秘密密钥列表。每个项目的形式为 `{"key": ..., "value": ..., "description": ...}`，其中描述是可选的。有关更多详细信息，请参见 [https://huggingface.co/docs/hub/spaces-overview#managing-secrets](https://huggingface.co/docs/hub/spaces-overview#managing-secrets)。

+   `variables` (`List[Dict[str, str]]`, *optional*) — 一个要在您的空间中设置的公共环境变量列表。每个项目的形式为 `{"key": ..., "value": ..., "description": ...}`，其中描述是可选的。有关更多详细信息，请参见 [https://huggingface.co/docs/hub/spaces-overview#managing-secrets-and-environment-variables](https://huggingface.co/docs/hub/spaces-overview#managing-secrets-and-environment-variables)。

返回值

[RepoUrl](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.RepoUrl)

新创建的仓库的 URL。值是 `str` 的子类，包含 `endpoint`、`repo_type` 和 `repo_id` 等属性。

引发

+   +   — 如果 HuggingFace API 返回错误，则为 [`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError)

+   +   — [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果`from_id`或`to_id`中的一个无法找到。这可能是因为它不存在，或者因为它设置为`private`且您无权访问。

复制一个空间。

以编程方式复制一个空间。新空间将在您的帐户中创建，并且将处于与原始空间相同的状态（运行或暂停）。无论空间的当前状态如何，都可以复制空间。

示例：

```py
>>> from huggingface_hub import duplicate_space

# Duplicate a Space to your account
>>> duplicate_space("multimodalart/dreambooth-training")
RepoUrl('https://huggingface.co/spaces/nateraw/dreambooth-training',...)

# Can set custom destination id and visibility flag.
>>> duplicate_space("multimodalart/dreambooth-training", to_id="my-dreambooth", private=True)
RepoUrl('https://huggingface.co/spaces/nateraw/my-dreambooth',...)
```

#### `edit_discussion_comment`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6249)

```py
( repo_id: str discussion_num: int comment_id: str new_content: str token: Optional[str] = None repo_type: Optional[str] = None ) → export const metadata = 'undefined';DiscussionComment
```

参数

+   `repo_id` (`str`) — 由斜杠分隔的命名空间（用户或组织）和存储库名称。

+   `discussion_num` (`int`) — 讨论或拉取请求的编号。必须是严格正整数。

+   `comment_id` (`str`) — 要编辑的评论的ID。

+   `new_content` (`str`) — 评论的新内容。评论支持Markdown格式。

+   `repo_type` (`str`, *可选*) — 如果要上传到数据集或空间，则设置为`"dataset"`或`"space"`，如果要上传到模型，则设置为`None`或`"model"`。默认为`None`。

+   `token` (`str`, *可选*) — 身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）

返回

[DiscussionComment](/docs/huggingface_hub/v0.20.3/en/package_reference/community#huggingface_hub.DiscussionComment)

编辑后的评论

编辑讨论/拉取请求上的评论。

引发以下错误：

+   [`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError) 如果HuggingFace API返回错误

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果某个参数值无效

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果无法找到要下载的存储库。这可能是因为它不存在，或者因为它设置为`private`且您无权访问。

#### `file_exists`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2331)

```py
( repo_id: str filename: str repo_type: Optional[str] = None revision: Optional[str] = None token: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 由斜杠分隔的命名空间（用户或组织）和存储库名称。

+   `filename` (`str`) — 要检查的文件名，例如："config.json"

+   `repo_type` (`str`, *可选*) — 如果从数据集或空间获取存储库信息，则设置为`"dataset"`或`"space"`，如果从模型获取存储库信息，则设置为`None`或`"model"`。默认为`None`。

+   `revision` (`str`, *可选*) — 要获取信息的存储库的修订版本。默认为`"main"`分支。

+   `token` (`bool`或`str`, *可选*) — 有效的身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则令牌不会发送到请求头中。

检查Hugging Face Hub上存储库中是否存在文件。

示例：

```py
>>> from huggingface_hub import file_exists
>>> file_exists("bigcode/starcoder", "config.json")
True
>>> file_exists("bigcode/starcoder", "not-a-file")
False
>>> file_exists("bigcode/not-a-repo", "config.json")
False
```

#### `get_collection`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7403)

```py
( collection_slug: str token: Optional[str] = None )
```

参数

+   `collection_slug` (`str`) — Hub集合的slug。示例："TheBloke/recent-models-64f9a55bb3115b4f513ec026"。

+   `token` (`str`, *可选*) — Hugging Face 令牌。如果未提供，将默认使用本地保存的令牌。

获取有关Hub上集合的信息。

返回：[Collection](/docs/huggingface_hub/v0.20.3/en/package_reference/collections#huggingface_hub.Collection)

示例：

```py
>>> from huggingface_hub import get_collection
>>> collection = get_collection("TheBloke/recent-models-64f9a55bb3115b4f513ec026")
>>> collection.title
'Recent models'
>>> len(collection.items)
37
>>> collection.items[0]
CollectionItem(
    item_object_id='651446103cd773a050bf64c2',
    item_id='TheBloke/U-Amethyst-20B-AWQ',
    item_type='model',
    position=88,
    note=None
)
```

#### `get_dataset_tags`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1349)

```py
( )
```

列出所有有效的数据集标签作为嵌套命名空间对象。

#### `get_discussion_details`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L5729)

```py
( repo_id: str discussion_num: int repo_type: Optional[str] = None token: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 由命名空间（用户或组织）和由`/`分隔的存储库名称。

+   `discussion_num` (`int`) — 讨论或拉取请求的编号。必须是严格的正整数。

+   `repo_type` (`str`, *optional*) — 如果上传到数据集或空间，则设置为`"dataset"`或`"space"`，如果上传到模型，则为`None`或`"model"`。默认为`None`。

+   `token` (`str`, *optional*) — 身份验证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）

从Hub获取讨论/拉取请求的详细信息。

返回：[DiscussionWithDetails](/docs/huggingface_hub/v0.20.3/en/package_reference/community#huggingface_hub.DiscussionWithDetails)

引发以下错误：

+   如果HuggingFace API返回错误，则返回[`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError)。

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError)如果某个参数值无效

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError)如果无法找到要下载的存储库。这可能是因为它不存在，或者因为它设置为`private`并且您无法访问。

#### `get_full_repo_name`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L5587)

```py
( model_id: str organization: Optional[str] = None token: Optional[Union[bool, str]] = None ) → export const metadata = 'undefined';str
```

参数

+   `model_id` (`str`) — 模型的名称。

+   `organization` (`str`, *optional*) — 如果传递，则存储库名称将位于组织命名空间而不是用户命名空间中。

+   `token` (`bool`或`str`, *optional*) — 有效的身份验证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则不会在请求标头中发送令牌。

返回

`str`

如果未传递组织，则在用户命名空间中的存储库名称（{用户名}/{模型ID}），否则在组织命名空间中（{组织}/{模型ID}）。

给定模型ID和可选组织的存储库名称。

#### `get_hf_file_metadata`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L4781)

```py
( url: str token: Union[bool, str, None] = None proxies: Optional[Dict] = None timeout: Optional[float] = 10 )
```

参数

+   `url` (`str`) — 文件URL，例如由[hf_hub_url()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.hf_hub_url)返回。

+   `token` (`str`或`bool`, *optional*) — 用于下载的令牌。

    +   如果为`True`，则从HuggingFace配置文件夹中读取令牌。

    +   如果为`False`或`None`，则不提供令牌。

    +   如果是字符串，则用作身份验证令牌。

+   `proxies` (`dict`, *optional*) — 将协议映射到传递给 `requests.request` 的代理的URL的字典。

+   `timeout` (`float`, *optional*, 默认为10) — 等待服务器发送元数据的秒数，超过后放弃。

获取在Hub上版本化的文件的元数据。

#### `get_inference_endpoint`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7083)

```py
( name: str namespace: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';InferenceEndpoint
```

参数

+   `name` (`str`) — 要检索信息的推理端点的名称。

+   `namespace` (`str`, *optional*) — 推理端点所在的命名空间。默认为当前用户。

+   `token` (`str`, *optional*) — 身份验证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

返回

[InferenceEndpoint](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_endpoints#huggingface_hub.InferenceEndpoint)

有关请求的推理端点的信息。

获取有关推理端点的信息。

示例：

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> endpoint = api.get_inference_endpoint("my-text-to-image")
>>> endpoint
InferenceEndpoint(name='my-text-to-image', ...)

# Get status
>>> endpoint.status
'running'
>>> endpoint.url
'https://my-text-to-image.region.vendor.endpoints.huggingface.cloud'

# Run inference
>>> endpoint.client.text_to_image(...)
```

#### `get_model_tags`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1340)

```py
( )
```

列出所有有效的模型标签作为嵌套命名空间对象

#### `get_paths_info`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2902)

```py
( repo_id: str paths: Union[List[str], str] expand: bool = False revision: Optional[str] = None repo_type: Optional[str] = None token: Optional[Union[bool, str]] = None ) → export const metadata = 'undefined';List[Union[RepoFile, RepoFolder]]
```

参数

+   `repo_id` (`str`) — 由斜杠分隔的命名空间（用户或组织）和仓库名称。

+   `paths` (`Union[List[str], str]`，*可选*) — 要获取信息的路径。如果路径不存在，则会被忽略而不会引发异常。

+   `expand` (`bool`，*可选*，默认为 `False`) — 是否获取有关路径的更多信息（例如最后一次提交和文件的安全扫描结果）。对于服务器来说，这个操作更昂贵，因此每页只返回 50 个结果（而不是 1000 个）。由于在 `huggingface_hub` 中实现了分页，这对您来说是透明的，除了获取结果所需的时间。

+   `revision` (`str`，*可选*) — 从中获取信息的仓库的修订版本。默认为 `"main"` 分支。

+   `repo_type` (`str`，*可选*) — 从中获取信息的仓库类型（`"model"`、`"dataset"` 或 `"space"`）。默认为 `"model"`。

+   `token` (`bool` 或 `str`，*可选*) — 有效的身份验证令牌（参见 [https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为 `None` 或 `True` 并且机器已登录（通过 `huggingface-cli login` 或 [login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），令牌将从缓存中检索。如果为 `False`，则令牌不会在请求头中发送。

返回

`List[Union[RepoFile, RepoFolder]]`

作为 `RepoFile` 和 `RepoFolder` 对象列表的路径信息。

引发

[RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 或 [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果找不到仓库（错误 404）：错误的 repo_id/repo_type，私有但未经身份验证或仓库不存在。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) — 如果在仓库中找不到修订版本（错误 404）。

获取有关仓库路径的信息。

示例：

```py
>>> from huggingface_hub import get_paths_info
>>> paths_info = get_paths_info("allenai/c4", ["README.md", "en"], repo_type="dataset")
>>> paths_info
[
    RepoFile(path='README.md', size=2379, blob_id='f84cb4c97182890fc1dbdeaf1a6a468fd27b4fff', lfs=None, last_commit=None, security=None),
    RepoFolder(path='en', tree_id='dc943c4c40f53d02b31ced1defa7e5f438d5862e', last_commit=None)
]
```

#### `get_repo_discussions`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L5625)

```py
( repo_id: str author: Optional[str] = None discussion_type: Optional[DiscussionTypeFilter] = None discussion_status: Optional[DiscussionStatusFilter] = None repo_type: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';Iterator[Discussion]
```

参数

+   `repo_id` (`str`) — 由斜杠分隔的命名空间（用户或组织）和仓库名称。

+   `author` (`str`，*可选*) — 传递一个值以按讨论作者筛选。`None` 表示无筛选。默认为 `None`。

+   `discussion_type` (`str`，*可选*) — 设置为 `"pull_request"` 以仅获取拉取请求，设置为 `"discussion"` 以仅获取讨论。设置为 `"all"` 或 `None` 以获取全部。默认为 `None`。

+   `discussion_status` (`str`，*可选*) — 设置为 `"open"`（分别为 `"closed"`）以仅获取开放的（分别为已关闭的）讨论。设置为 `"all"` 或 `None` 以获取全部。默认为 `None`。

+   `repo_type` (`str`，*可选*) — 如果从数据集或空间获取，则设置为 `"dataset"` 或 `"space"`，如果从模型获取，则设置为 `None` 或 `"model"`。默认为 `None`。

+   `token` (`str`，*可选*) — 身份验证令牌（参见 [https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

返回

`Iterator[Discussion]`

[Discussion](/docs/huggingface_hub/v0.20.3/en/package_reference/community#huggingface_hub.Discussion) 对象的迭代器。

获取给定仓库的讨论和拉取请求。

示例：

在列表中收集仓库的所有讨论：

```py
>>> from huggingface_hub import get_repo_discussions
>>> discussions_list = list(get_repo_discussions(repo_id="bert-base-uncased"))
```

遍历仓库讨论的示例：

```py
>>> from huggingface_hub import get_repo_discussions
>>> for discussion in get_repo_discussions(repo_id="bert-base-uncased"):
...     print(discussion.num, discussion.title)
```

#### `get_safetensors_metadata`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L5133)

```py
( repo_id: str repo_type: Optional[str] = None revision: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';SafetensorsRepoMetadata
```

参数

+   `repo_id`（`str`）—由用户或组织名称和由`/`分隔的仓库名称。

+   `filename`（`str`）—仓库中的文件名。

+   `repo_type`（`str`，*可选*）—如果文件在数据集或空间中，则设置为`"dataset"`或`"space"`，如果在模型中则为`None`或`"model"`。默认为`None`。

+   `revision`（`str`，*可选*）—要从中提取文件的git修订版。可以是分支名称、标签或提交哈希。默认为`"main"`分支的头部。

+   `token`（`bool`或`str`，*可选*）—有效的身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则令牌不会发送到请求头中。

返回

`SafetensorsRepoMetadata`

与safetensors仓库相关的信息。

引发

+   +   — `NotASafetensorsRepoError`：如果仓库不是safetensors仓库，即没有`model.safetensors`或`model.safetensors.index.json`文件。

+   +   — `SafetensorsParsingError`：如果无法正确解析safetensors文件头。

解析Hub上safetensors仓库的元数据。

我们首先检查仓库是否具有单个safetensors文件或分片的safetensors仓库。如果是单个safetensors文件，则从该文件中解析元数据。如果是分片的safetensors仓库，则从索引文件中解析元数据，然后从每个分片中解析元数据。

要从单个safetensors文件中解析元数据，请使用[get_safetensors_metadata()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.get_safetensors_metadata)。

有关safetensors格式的更多详细信息，请查看[https://huggingface.co/docs/safetensors/index#format](https://huggingface.co/docs/safetensors/index#format)。

示例：

```py
# Parse repo with single weights file
>>> metadata = get_safetensors_metadata("bigscience/bloomz-560m")
>>> metadata
SafetensorsRepoMetadata(
    metadata=None,
    sharded=False,
    weight_map={'h.0.input_layernorm.bias': 'model.safetensors', ...},
    files_metadata={'model.safetensors': SafetensorsFileMetadata(...)}
)
>>> metadata.files_metadata["model.safetensors"].metadata
{'format': 'pt'}

# Parse repo with sharded model
>>> metadata = get_safetensors_metadata("bigscience/bloom")
Parse safetensors files: 100%|██████████████████████████████████████████| 72/72 [00:12<00:00,  5.78it/s]
>>> metadata
SafetensorsRepoMetadata(metadata={'total_size': 352494542848}, sharded=True, weight_map={...}, files_metadata={...})
>>> len(metadata.files_metadata)
72  # All safetensors files have been fetched

# Parse repo with sharded model
>>> get_safetensors_metadata("runwayml/stable-diffusion-v1-5")
NotASafetensorsRepoError: 'runwayml/stable-diffusion-v1-5' is not a safetensors repo. Couldn't find 'model.safetensors.index.json' or 'model.safetensors' files.
```

#### `get_space_runtime`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6497)

```py
( repo_id: str token: Optional[str] = None ) → export const metadata = 'undefined';SpaceRuntime
```

参数

+   `repo_id`（`str`）—要更新的仓库的ID。示例：`"bigcode/in-the-stack"`。

+   `token`（`str`，*可选*）—Hugging Face令牌。如果未提供，则将默认为本地保存的令牌。

返回

[SpaceRuntime](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceRuntime)

有关空间的运行时信息，包括空间阶段和硬件。

获取有关空间的运行时信息。

#### `get_space_variables`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6420)

```py
( repo_id: str token: Optional[str] = None )
```

参数

+   `repo_id`（`str`）—要查询的仓库的ID。示例：`"bigcode/in-the-stack"`。

+   `token`（`str`，*可选*）—Hugging Face令牌。如果未提供，则将默认为本地保存的令牌。

获取空间中的所有变量。

变量允许将环境变量设置为一个空间，而无需将它们硬编码。有关更多详细信息，请参阅[https://huggingface.co/docs/hub/spaces-overview#managing-secrets-and-environment-variables](https://huggingface.co/docs/hub/spaces-overview#managing-secrets-and-environment-variables)。

#### `get_token_permission`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1321)

```py
( token: Optional[str] = None ) → export const metadata = 'undefined';Literal["read", "write", None]
```

参数

+   `token`（`str`，*可选*）—要检查有效性的令牌。默认为本地保存的令牌。

返回

`Literal["read", "write", None]`

令牌授予的权限（“读取”或“写入”）。如果没有传递令牌或令牌无效，则返回`None`。

检查给定的`token`是否有效并返回其权限。

有关令牌的更多详细信息，请参阅[https://huggingface.co/docs/hub/security-tokens#what-are-user-access-tokens](https://huggingface.co/docs/hub/security-tokens#what-are-user-access-tokens)。

#### `grant_access`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L8124)

```py
( repo_id: str user: str repo_type: Optional[str] = None token: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 要授予访问权限的存储库的 ID。

+   `user` (`str`) — 要授予访问权限的用户的用户名。

+   `repo_type` (`str`，*可选*) — 要授予访问权限的存储库类型。必须是`model`、`dataset`或`space`之一。默认为`model`。

+   `token` (`str`，*可选*) — 有效的身份验证令牌（请参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

引发

`HTTPError`

+   `HTTPError` — 如果存储库没有受限，则为 HTTP 400。

+   `HTTPError` — 如果用户已经对存储库具有访问权限，则为 HTTP 400。

+   `HTTPError` — 如果您只有对存储库的只读访问权限，则为 HTTP 403。如果您在存储库所属的组织中没有`write`或`admin`角色，或者传递了一个`read`令牌，就可能出现这种情况。

+   `HTTPError` — 如果用户在 Hub 上不存在，则为 HTTP 404。

为给定的受限存储库授予用户访问权限。

授予访问权限不需要用户自己发送访问请求。用户会自动添加到接受列表中，这意味着他们可以下载文件。您可以随时使用[cancel_access_request()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.cancel_access_request)或[reject_access_request()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.reject_access_request)来撤销授予的访问权限。

有关受限存储库的更多信息，请参见[https://huggingface.co/docs/hub/models-gated](https://huggingface.co/docs/hub/models-gated)。

#### `hf_hub_download`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L4823)

```py
( repo_id: str filename: str subfolder: Optional[str] = None repo_type: Optional[str] = None revision: Optional[str] = None cache_dir: Union[str, Path, None] = None local_dir: Union[str, Path, None] = None local_dir_use_symlinks: Union[bool, Literal['auto']] = 'auto' force_download: bool = False force_filename: Optional[str] = None proxies: Optional[Dict] = None etag_timeout: float = 10 resume_download: bool = False token: Optional[Union[str, bool]] = None local_files_only: bool = False legacy_cache_layout: bool = False )
```

参数

+   `repo_id` (`str`) — 由用户或组织名称和存储库名称以`/`分隔组成。

+   `filename` (`str`) — 存储库中文件的名称。

+   `subfolder` (`str`，*可选*) — 与模型存储库中的文件夹对应的可选值。

+   `repo_type` (`str`，*可选*) — 如果从数据集或空间下载，则设置为`"dataset"`或`"space"`，如果从模型下载，则为`None`或`"model"`。默认为`None`。

+   `revision` (`str`，*可选*) — 可选的 Git 修订 ID，可以是分支名称、标签或提交哈希。

+   `cache_dir` (`str`，`Path`，*可选*) — 缓存文件存储的文件夹路径。

+   `local_dir` (`str`或`Path`，*可选*) — 如果提供，下载的文件将放在此目录下，可以是符号链接（默认）或常规文件（有关更多详细信息，请参见描述）。

+   `local_dir_use_symlinks` (`"auto"`或`bool`，默认为`"auto"`) — 与`local_dir`一起使用。如果设置为`"auto"`，将使用缓存目录，并根据文件大小将文件复制或创建符号链接到本地目录。如果设置为`True`，将创建符号链接，无论文件大小如何。如果设置为`False`，文件将从缓存中复制（如果已存在）或从 Hub 下载并不缓存。有关更多详细信息，请参见描述。

+   `force_download` (`bool`，*可选*，默认为`False`) — 如果文件已经存在于本地缓存中，是否应下载该文件。

+   `proxies` (`dict`，*可选*) — 将协议映射到传递给`requests.request`的代理的 URL 的字典。

+   `etag_timeout` (`float`，*可选*，默认为`10`) — 在获取 ETag 时，等待服务器发送数据的秒数，然后放弃，这将传递给`requests.request`。

+   `resume_download` (`bool`，*可选*，默认为`False`) — 如果为`True`，则恢复先前中断的下载。

+   `token`（`bool`或`str`，*可选*）— 有效的身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则不会在请求标头中发送令牌。

+   `local_files_only`（`bool`，*可选*，默认为`False`）— 如果为`True`，则避免下载文件，并返回本地缓存文件的路径（如果存在）。

+   `legacy_cache_layout`（`bool`，*可选*，默认为`False`）— 如果为`True`，则使用传统的文件缓存布局，即只需调用[hf_hub_url()](/docs/huggingface_hub/v0.20.3/en/package_reference/file_download#huggingface_hub.hf_hub_url)然后`cached_download`。由于新的缓存布局更强大，因此此方法已被弃用。

如果本地缓存中不存在给定文件，则下载该文件。

新的缓存文件布局如下：

+   缓存目录包含每个repo_id的一个子文件夹（由repo类型命名空间化）

+   在每个存储库文件夹内：

    +   refs是最新已知的修订 => 提交哈希对列表

    +   blobs包含实际的文件blob（通过它们的git-sha或sha256标识，取决于它们是否是LFS文件）

    +   快照包含每个提交的一个子文件夹，每个“提交”包含在该特定提交中已解析的文件子集。每个文件名都是指向该特定提交中的blob的符号链接。

如果提供了`local_dir`，则将在此位置复制来自存储库的文件结构。您可以配置如何移动这些文件：

+   如果`local_dir_use_symlinks="auto"`（默认），文件将被下载并存储在缓存目录中作为blob文件。小文件（<5MB）将在`local_dir`中复制，而对于较大的文件将创建符号链接。目标是能够手动编辑和保存小文件而不会破坏缓存，同时为二进制文件节省磁盘空间。可以使用`HF_HUB_LOCAL_DIR_AUTO_SYMLINK_THRESHOLD`环境变量配置5MB阈值。

+   如果`local_dir_use_symlinks=True`，文件将被下载，存储在缓存目录中，并在`local_dir`中创建符号链接。这在磁盘使用方面是最佳的，但文件不能手动编辑。

+   如果`local_dir_use_symlinks=False`并且blob文件存在于缓存目录中，则会在本地目录中复制这些文件。这意味着磁盘使用量没有优化。

+   最后，如果`local_dir_use_symlinks=False`并且blob文件不存在于缓存目录中，则文件将被下载并直接放在`local_dir`下。这意味着如果以后需要重新下载它们，它们将被完全重新下载。

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

+   [`OSError`](https://docs.python.org/3/library/exceptions.html#OSError) 如果无法确定ETag。

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果某个参数值无效

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果无法找到要下载的存储库。这可能是因为它不存在，或者因为它被设置为`private`并且您无法访问。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) 如果无法找到要下载的修订版本。

+   [EntryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.EntryNotFoundError) 如果无法找到要下载的文件。

+   [LocalEntryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.LocalEntryNotFoundError) 如果网络已禁用或不可用，并且在缓存中找不到文件。

#### `hide_discussion_comment`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6306)

```py
( repo_id: str discussion_num: int comment_id: str token: Optional[str] = None repo_type: Optional[str] = None ) → export const metadata = 'undefined';DiscussionComment
```

参数

+   `repo_id` (`str`) — 由`/`分隔的命名空间（用户或组织）和仓库名称。

+   `discussion_num` (`int`) — 讨论或拉取请求的编号。必须是严格正整数。

+   `comment_id` (`str`) — 要编辑的评论的ID。

+   `repo_type` (`str`, *可选*) — 如果上传到数据集或空间，则设置为`"dataset"`或`"space"`，如果上传到模型，则为`None`或`"model"`。默认为`None`。

+   `token` (`str`, *可选*) — 认证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）

返回

[DiscussionComment](/docs/huggingface_hub/v0.20.3/en/package_reference/community#huggingface_hub.DiscussionComment)

隐藏的评论

隐藏讨论/拉取请求中的评论。

无法再检索隐藏评论的内容。隐藏评论是不可逆的。

引发以下错误：

+   [`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError) 如果HuggingFace API返回错误

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果某个参数值无效

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果无法找到要下载的仓库。这可能是因为它不存在，或者因为它被设置为`private`而您没有访问权限。

#### `like`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1801)

```py
( repo_id: str token: Optional[str] = None repo_type: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 要点赞的仓库。示例："user/my-cool-model"。

+   `token` (`str`, *可选*) — 认证令牌。将默认使用存储的令牌。

+   `repo_type` (`str`, *可选*) — 如果点赞数据集或空间，则设置为`"dataset"`或`"space"`，如果点赞模型，则为`None`或`"model"`。默认为`None`。

Raises

[RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果找不到仓库（错误404）：错误的repo_id/repo_type，私有但未经身份验证或仓库不存在。

在Hub上喜欢给定的仓库（例如设置为收藏）。

另请参阅[unlike()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.unlike)和[list_liked_repos()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_liked_repos)。

示例：

```py
>>> from huggingface_hub import like, list_liked_repos, unlike
>>> like("gpt2")
>>> "gpt2" in list_liked_repos().models
True
>>> unlike("gpt2")
>>> "gpt2" in list_liked_repos().models
False
```

#### `list_accepted_access_requests`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7841)

```py
( repo_id: str repo_type: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';List[AccessRequest]
```

参数

+   `repo_id` (`str`) — 要获取访问请求的仓库的ID。

+   `repo_type` (`str`, *可选*) — 要获取访问请求的仓库类型。必须是`model`、`dataset`或`space`之一。默认为`model`。

+   `token` (`str`, *可选*) — 有效的认证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

返回

`List[AccessRequest]`

一个包含`AccessRequest`对象的列表。每个时间包含一个`username`、`email`、`status`和`timestamp`属性。如果受限制的仓库有自定义表单，`fields`属性将被填充为用户的答案。

Raises

`HTTPError`

+   `HTTPError` — 如果仓库没有受限制，则返回HTTP 400。

+   `HTTPError` — 如果您只有对仓库的只读访问权限，则会返回HTTP 403。如果您在所属组织中没有`write`或`admin`角色，或者如果您传递了一个`read`令牌，这可能是情况。

获取给定受限制仓库的已接受访问请求。

接受的请求意味着用户已请求访问存储库，并且请求已被接受。用户可以下载存储库的任何文件。如果批准模式是自动的，默认情况下此列表应包含所有请求。接受的请求可以随时使用[cancel_access_request()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.cancel_access_request)和[reject_access_request()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.reject_access_request)进行取消或拒绝。取消的请求将返回到待处理列表，而拒绝的请求将进入拒绝列表。在这两种情况下，用户将失去对存储库的访问权限。

有关受控存储库的更多信息，请参见[https://huggingface.co/docs/hub/models-gated](https://huggingface.co/docs/hub/models-gated)。

示例：

```py
>>> from huggingface_hub import list_accepted_access_requests

>>> requests = list_accepted_access_requests("meta-llama/Llama-2-7b")
>>> len(requests)
411
>>> requests[0]
[
    AccessRequest(
        username='clem',
        fullname='Clem 🤗',
        email='***',
        timestamp=datetime.datetime(2023, 11, 23, 18, 4, 53, 828000, tzinfo=datetime.timezone.utc),
        status='accepted',
        fields=None,
    ),
    ...
]
```

#### `list_collections`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7347)

```py
( owner: Union[List[str], str, None] = None item: Union[List[str], str, None] = None sort: Optional[Literal['lastModified', 'trending', 'upvotes']] = None limit: Optional[int] = None token: Optional[Union[bool, str]] = None ) → export const metadata = 'undefined';Iterable[Collection]
```

参数

+   `owner` (`List[str]`或`str`，*可选*) — 按所有者的用户名进行过滤。

+   `item` (`List[str]`或`str`，*可选*) — 过滤包含特定项的集合。例如："models/teknium/OpenHermes-2.5-Mistral-7B"，"datasets/squad"或"papers/2311.12983"。

+   `sort` (`Literal["lastModified", "trending", "upvotes"]`，*可选*) — 按最后修改、趋势或点赞排序集合。

+   `limit` (`int`，*可选*) — 要返回的集合的最大数量。

+   `token` (`bool`或`str`，*可选*) — 身份验证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

返回值

`Iterable[Collection]`

一个[Collection](/docs/huggingface_hub/v0.20.3/en/package_reference/collections#huggingface_hub.Collection)对象的可迭代。

给定一些过滤器，在Huggingface Hub上列出集合。

在列出集合时，每个集合的项列表被截断为最多4个项。要检索集合中的所有项，必须使用[get_collection()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.get_collection)。

#### `list_datasets`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1544)

```py
( filter: Union[DatasetFilter, str, Iterable[str], None] = None author: Optional[str] = None search: Optional[str] = None sort: Union[Literal['last_modified'], str, None] = None direction: Optional[Literal[-1]] = None limit: Optional[int] = None full: Optional[bool] = None token: Optional[str] = None ) → export const metadata = 'undefined';Iterable[DatasetInfo]
```

参数

+   `filter` ([DatasetFilter](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.DatasetFilter)或`str`或`Iterable`，*可选*) — 用于在hub上识别数据集的字符串或[DatasetFilter](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.DatasetFilter)。

+   `author` (`str`，*可选*) — 用于标识返回数据集的作者的字符串。

+   `search` (`str`，*可选*) — 包含在返回数据集中的字符串。

+   `sort` (`Literal["last_modified"]`或`str`，*可选*) — 用于对结果数据集进行排序的键。可能的值是[huggingface_hub.hf_api.DatasetInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.DatasetInfo)类的属性。

+   `direction` (`Literal[-1]`或`int`，*可选*) — 排序方向。值`-1`按降序排序，而所有其他值按升序排序。

+   `limit` (`int`，*可选*) — 获取数据集的数量限制。将此选项留空将获取所有数据集。

+   `full` (`bool`，*可选*) — 是否获取所有数据集数据，包括`last_modified`，`card_data`和文件。可以包含有用的信息，如PapersWithCode ID。

+   `token` (`bool`或`str`，*可选*) — 有效的身份验证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则不会在请求头中发送令牌。

返回值

`Iterable[DatasetInfo]`

[huggingface_hub.hf_api.DatasetInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.DatasetInfo)对象的可迭代对象。

给定一些过滤器，列出托管在Huggingface Hub上的数据集。

带有`filter`参数的示例用法：

```py
>>> from huggingface_hub import HfApi

>>> api = HfApi()

>>> # List all datasets
>>> api.list_datasets()

>>> # List only the text classification datasets
>>> api.list_datasets(filter="task_categories:text-classification")
>>> # Using the `DatasetFilter`
>>> filt = DatasetFilter(task_categories="text-classification")

>>> # List only the datasets in russian for language modeling
>>> api.list_datasets(
...     filter=("language:ru", "task_ids:language-modeling")
... )
>>> # Using the `DatasetFilter`
>>> filt = DatasetFilter(language="ru", task_ids="language-modeling")

>>> api.list_datasets(filter=filt)
```

带有`search`参数的示例用法：

```py
>>> from huggingface_hub import HfApi

>>> api = HfApi()

>>> # List all datasets with "text" in their name
>>> api.list_datasets(search="text")

>>> # List all datasets with "text" in their name made by google
>>> api.list_datasets(search="text", author="google")
```

#### `list_files_info`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2393)

```py
( repo_id: str paths: Union[List[str], str, None] = None expand: bool = False revision: Optional[str] = None repo_type: Optional[str] = None token: Optional[Union[bool, str]] = None ) → export const metadata = 'undefined';Iterable[RepoFile]
```

参数

+   `repo_id`（`str`）—由`/`分隔的命名空间（用户或组织）和存储库名称。

+   `paths`（`Union[List[str], str, None]`，*可选*）—要获取信息的路径。文件路径直接解析。文件夹路径将递归解析，这意味着将返回有关文件夹及其子文件夹中的所有文件的信息。如果为`None`，则返回所有文件（默认）。如果路径不存在，则会忽略而不会引发异常。

+   `expand`（`bool`，*可选*，默认为`False`）—是否获取有关文件的更多信息（例如最后一次提交和安全扫描结果）。对于服务器来说，这个操作更昂贵，因此每页只返回50个结果（而不是1000个）。由于`huggingface_hub`中实现了分页，因此对于您来说，这是透明的，除了获取结果所需的时间。

+   `revision`（`str`，*可选*）—要获取信息的存储库的修订版。默认为`"main"`分支。

+   `repo_type`（`str`，*可选*）—要获取信息的存储库的类型（`"model"`，`"dataset"`或`"space"`。默认为`"model"`。

+   `token`（`bool`或`str`，*可选*）—有效的身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则令牌不会在请求标头中发送。

返回

`Iterable[RepoFile]`

有关文件的信息，作为`RepoFile`对象的可迭代对象。文件的顺序不保证。

引发

[RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError)或[RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果找不到存储库（错误404）：错误的`repo_id/repo_type`，私有但未经身份验证或存储库不存在。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) — 如果在存储库上找不到修订版（错误404）。

列出存储库中的文件并获取有关它们的信息。

接受路径列表作为输入。这些路径可以是文件或文件夹。调用两个服务器端点：

1.  POST “/paths-info”以获取有关提供的路径的信息。仅调用一次。

1.  GET “/tree?recursive=True”以对输入文件夹进行分页。仅在提供文件夹路径作为输入时调用。将被多次调用以跟随分页。如果未提供路径作为输入，则忽略步骤1，并列出存储库中的所有文件。

示例：

获取有关存储库中文件的信息。

```py
>>> from huggingface_hub import list_files_info
>>> files_info = list_files_info("lysandre/arxiv-nlp", ["README.md", "config.json"])
>>> files_info
<generator object HfApi.list_files_info at 0x7f93b848e730>
>>> list(files_info)
[
    RepoFile(path='README.md', size=391, blob_id='43bd404b159de6fba7c2f4d3264347668d43af25', lfs=None, last_commit=None, security=None),
    RepoFile(path='config.json', size=554, blob_id='2f9618c3a19b9a61add74f70bfb121335aeef666', lfs=None, last_commit=None, security=None)
]
```

获取有关存储库中文件的更多信息（最后一次提交和安全扫描结果）

```py
>>> from huggingface_hub import list_files_info
>>> files_info = list_files_info("prompthero/openjourney-v4", expand=True)
>>> list(files_info)
[
    RepoFile(
        path='safety_checker/pytorch_model.bin',
        size=1216064769,
        blob_id='c8835557a0d3af583cb06c7c154b7e54a069c41d',
        lfs={
            'size': 1216064769,
            'sha256': '16d28f2b37109f222cdc33620fdd262102ac32112be0352a7f77e9614b35a394',
            'pointer_size': 135
        },
        last_commit={
            'oid': '47b62b20b20e06b9de610e840282b7e6c3d51190',
            'title': 'Upload diffusers weights (#48)',
            'date': datetime.datetime(2023, 3, 21, 10, 5, 27, tzinfo=datetime.timezone.utc)
        },
        security={
            'safe': True,
            'av_scan': {
                'virusFound': False,
                'virusNames': None
            },
            'pickle_import_scan': {
                'highestSafetyLevel': 'innocuous',
                'imports': [
                    {'module': 'torch', 'name': 'FloatStorage', 'safety': 'innocuous'},
                    {'module': 'collections', 'name': 'OrderedDict', 'safety': 'innocuous'},
                    {'module': 'torch', 'name': 'LongStorage', 'safety': 'innocuous'},
                    {'module': 'torch._utils', 'name': '_rebuild_tensor_v2', 'safety': 'innocuous'}
                ]
            }
        }
    ),
    RepoFile(
        path='scheduler/scheduler_config.json',
        size=465,
        blob_id='70d55e3e9679f41cbc66222831b38d5abce683dd',
        lfs=None,
        last_commit={
            'oid': '47b62b20b20e06b9de610e840282b7e6c3d51190',
            'title': 'Upload diffusers weights (#48)',
            'date': datetime.datetime(2023, 3, 21, 10, 5, 27, tzinfo=datetime.timezone.utc)},
            security={
                'safe': True,
                'av_scan': {
                    'virusFound': False,
                    'virusNames': None
                },
                'pickle_import_scan': None
            }
    ),
    ...
]
```

从“stabilityai/stable-diffusion-2”存储库中的“vae/”文件夹中列出LFS文件。

```py
>>> from huggingface_hub import list_files_info
>>> [info.path for info in list_files_info("stabilityai/stable-diffusion-2", "vae") if info.lfs is not None]
['vae/diffusion_pytorch_model.bin', 'vae/diffusion_pytorch_model.safetensors']
```

列出存储库中的所有文件。

```py
>>> from huggingface_hub import list_files_info
>>> [info.path for info in list_files_info("glue", repo_type="dataset")]
['.gitattributes', 'README.md', 'dataset_infos.json', 'glue.py']
```

#### `list_inference_endpoints`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6874)

```py
( namespace: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';ListInferenceEndpoint
```

参数

+   `namespace`（`str`，*可选*）—要列出端点的命名空间。默认为当前用户。设置为`"*"`以列出所有命名空间的所有端点（即个人命名空间和用户所属的所有组织）。

+   `token`（`str`，*可选*）— 身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

返回

List[InferenceEndpoint](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_endpoints#huggingface_hub.InferenceEndpoint)

给定命名空间的所有推理端点列表。

列出给定命名空间的所有推理端点。

示例：

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> api.list_inference_endpoints()
[InferenceEndpoint(name='my-endpoint', ...), ...]
```

#### `list_liked_repos`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1896)

```py
( user: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';UserLikes
```

参数

+   `user`（`str`，*可选*）— 要获取喜欢的用户的名称。

+   `token`（`str`，*可选*）— 有效的身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。仅在未传递`user`以隐式确定当前用户名时使用。

返回

[UserLikes](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.UserLikes)

包含用户名称和3个存储库ID列表的对象（1个用于模型，1个用于数据集，1个用于Spaces）。

引发

`ValueError`

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) — 如果未传递`user`且未找到令牌（从参数或机器中）。

列出用户在huggingface.co上喜欢的所有公共存储库。

此列表是公开的，因此令牌是可选的。如果未传递`user`，则默认为已登录用户。

另请参阅[like()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.like)和[unlike()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.unlike)。

示例：

```py
>>> from huggingface_hub import list_liked_repos

>>> likes = list_liked_repos("julien-c")

>>> likes.user
"julien-c"

>>> likes.models
["osanseviero/streamlit_1.15", "Xhaheen/ChatGPT_HF", ...]
```

#### `list_metrics`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1701)

```py
( ) → export const metadata = 'undefined';List[MetricInfo]
```

返回

`List[MetricInfo]`

一个包含`MetricInfo`对象的列表。

获取huggingface.co上所有指标的公共列表

#### `list_models`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1358)

```py
( filter: Union[ModelFilter, str, Iterable[str], None] = None author: Optional[str] = None search: Optional[str] = None emissions_thresholds: Optional[Tuple[float, float]] = None sort: Union[Literal['last_modified'], str, None] = None direction: Optional[Literal[-1]] = None limit: Optional[int] = None full: Optional[bool] = None cardData: bool = False fetch_config: bool = False token: Optional[Union[bool, str]] = None ) → export const metadata = 'undefined';Iterable[ModelInfo]
```

参数

+   `filter`（[ModelFilter](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.ModelFilter)或`str`或`Iterable`，*可选*）— 一个字符串或[ModelFilter](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.ModelFilter)，可用于在Hub上识别模型。

+   `author`（`str`，*可选*）— 用于标识返回模型的作者（用户或组织）的字符串

+   `search`（`str`，*可选*）— 将包含在返回的模型ID中的字符串。

+   `emissions_thresholds`（`Tuple`，*可选*）— 一个由两个整数或浮点数组成的元组，表示筛选结果模型的最小和最大碳足迹（以克为单位）。

+   `sort`（`Literal["last_modified"]`或`str`，*可选*）— 用于对结果模型进行排序的键。可能的值是[huggingface_hub.hf_api.ModelInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.ModelInfo)类的属性。

+   `direction`（`Literal[-1]`或`int`，*可选*）— 要排序的方向。值`-1`按降序排序，而所有其他值按升序排序。

+   `limit`（`int`，*可选*）— 获取的模型数量限制。将此选项保留为`None`将获取所有模型。

+   `full`（`bool`，*可选*）— 是否获取所有模型数据，包括`last_modified`、`sha`、文件和`tags`。在使用过滤器时，默认设置为`True`。

+   `cardData`（`bool`，*可选*）— 是否也获取模型的元数据。可以包含有用的信息，如碳排放、指标和训练数据集。

+   `fetch_config`（`bool`，*可选*）— 是否也获取模型配置。由于其大小，这不包含在`full`中。

+   `token` (`bool`或`str`, *可选*) — 有效的身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则不会在请求标头中发送令牌。

返回

`Iterable[ModelInfo]`

[huggingface_hub.hf_api.ModelInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.ModelInfo)对象的可迭代对象。

列出托管在Huggingface Hub上的模型，给定一些过滤条件。

带有`filter`参数的示例用法：

```py
>>> from huggingface_hub import HfApi

>>> api = HfApi()

>>> # List all models
>>> api.list_models()

>>> # List only the text classification models
>>> api.list_models(filter="text-classification")
>>> # Using the `ModelFilter`
>>> filt = ModelFilter(task="text-classification")

>>> # List only models from the AllenNLP library
>>> api.list_models(filter="allennlp")
```

带有`search`参数的示例用法：

```py
>>> from huggingface_hub import HfApi

>>> api = HfApi()

>>> # List all models with "bert" in their name
>>> api.list_models(search="bert")

>>> # List all models with "bert" in their name made by google
>>> api.list_models(search="bert", author="google")
```

#### `list_pending_access_requests`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7780)

```py
( repo_id: str repo_type: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';List[AccessRequest]
```

参数

+   `repo_id` (`str`) — 获取访问请求的存储库的ID。

+   `repo_type` (`str`, *可选*) — 要获取访问请求的存储库类型。必须是`model`、`dataset`或`space`之一。默认为`model`。

+   `token` (`str`, *可选*) — 有效的身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

返回

`List[AccessRequest]`

`AccessRequest`对象的列表。每次包含`username`、`email`、`status`和`timestamp`属性。如果受限存储库有自定义表单，则`fields`属性将填充用户的答案。

引发

`HTTPError`

+   `HTTPError` — 如果存储库未受限，则为HTTP 400。

+   `HTTPError` — 如果您只有对存储库的只读访问权限，则为HTTP 403。如果您在存储库所属的组织中没有`write`或`admin`角色，或者传递了一个`read`令牌，这可能是情况。

获取给定受限存储库的待处理访问请求。

待处理请求意味着用户已请求访问存储库，但请求尚未处理。如果批准模式是自动的，则此列表应为空。可以使用[accept_access_request()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.accept_access_request)和[reject_access_request()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.reject_access_request)接受或拒绝待处理请求。

有关受限存储库的更多信息，请参阅[https://huggingface.co/docs/hub/models-gated](https://huggingface.co/docs/hub/models-gated)。

示例：

```py
>>> from huggingface_hub import list_pending_access_requests, accept_access_request

# List pending requests
>>> requests = list_pending_access_requests("meta-llama/Llama-2-7b")
>>> len(requests)
411
>>> requests[0]
[
    AccessRequest(
        username='clem',
        fullname='Clem 🤗',
        email='***',
        timestamp=datetime.datetime(2023, 11, 23, 18, 4, 53, 828000, tzinfo=datetime.timezone.utc),
        status='pending',
        fields=None,
    ),
    ...
]

# Accept Clem's request
>>> accept_access_request("meta-llama/Llama-2-7b", "clem")
```

#### `list_rejected_access_requests`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7900)

```py
( repo_id: str repo_type: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';List[AccessRequest]
```

参数

+   `repo_id` (`str`) — 获取访问请求的存储库的ID。

+   `repo_type` (`str`, *可选*) — 要获取访问请求的存储库类型。必须是`model`、`dataset`或`space`之一。默认为`model`。

+   `token` (`str`, *可选*) — 有效的身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

返回

`List[AccessRequest]`

`AccessRequest`对象的列表。每次包含`username`、`email`、`status`和`timestamp`属性。如果受限存储库有自定义表单，则`fields`属性将填充用户的答案。

引发

`HTTPError`

+   `HTTPError` — 如果存储库未受限，则为HTTP 400。

+   `HTTPError` — 如果您只有对存储库的只读访问权限，则为HTTP 403。如果您在存储库所属的组织中没有`write`或`admin`角色，或者传递了一个`read`令牌，这可能是情况。

获取给定受限存储库的被拒绝访问请求。

拒绝的请求意味着用户已请求访问存储库，并且该请求已被存储库所有者明确拒绝（您或您组织的另一个用户）。用户无法下载存储库的任何文件。拒绝的请求可以随时使用[accept_access_request()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.accept_access_request)和[cancel_access_request()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.cancel_access_request)接受或取消。取消的请求将返回到待处理列表，而接受的请求将进入已接受列表。

有关受限存储库的更多信息，请参见[https://huggingface.co/docs/hub/models-gated](https://huggingface.co/docs/hub/models-gated)。

示例：

```py
>>> from huggingface_hub import list_rejected_access_requests

>>> requests = list_rejected_access_requests("meta-llama/Llama-2-7b")
>>> len(requests)
411
>>> requests[0]
[
    AccessRequest(
        username='clem',
        fullname='Clem 🤗',
        email='***',
        timestamp=datetime.datetime(2023, 11, 23, 18, 4, 53, 828000, tzinfo=datetime.timezone.utc),
        status='rejected',
        fields=None,
    ),
    ...
]
```

#### `list_repo_commits`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2816)

```py
( repo_id: str repo_type: Optional[str] = None token: Optional[Union[bool, str]] = None revision: Optional[str] = None formatted: bool = False ) → export const metadata = 'undefined';List[GitCommitInfo]
```

参数

+   `repo_id` (`str`) — 由命名空间（用户或组织）和存储库名称以`/`分隔组成。

+   `repo_type` (`str`, *可选*) — 如果从数据集或空间列出提交，请设置为`"dataset"`或`"space"`，如果从模型列出，请设置为`None`或`"model"`。默认为`None`。

+   `token` (`bool`或`str`, *可选*) — 有效的身份验证令牌（请参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则令牌不会发送到请求头中。

+   `revision` (`str`, *可选*) — 要提交的git修订版本。默认为`"main"`分支的头部。

+   `formatted` (`bool`) — 是否返回提交的HTML格式化标题和描述。默认为False。

返回

List[[GitCommitInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.GitCommitInfo)]

包含有关Hub上存储库提交的信息的对象列表。

引发

[RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError)或[RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果找不到存储库（错误404）：错误的repo_id/repo_type，私有但未经身份验证或存储库不存在。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) — 如果在存储库上找不到修订版本（错误404）。

获取Hub上给定修订版本的存储库的提交列表。

提交按日期排序（最新提交在前）。

示例：

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()

# Commits are sorted by date (last commit first)
>>> initial_commit = api.list_repo_commits("gpt2")[-1]

# Initial commit is always a system commit containing the `.gitattributes` file.
>>> initial_commit
GitCommitInfo(
    commit_id='9b865efde13a30c13e0a33e536cf3e4a5a9d71d8',
    authors=['system'],
    created_at=datetime.datetime(2019, 2, 18, 10, 36, 15, tzinfo=datetime.timezone.utc),
    title='initial commit',
    message='',
    formatted_title=None,
    formatted_message=None
)

# Create an empty branch by deriving from initial commit
>>> api.create_branch("gpt2", "new_empty_branch", revision=initial_commit.commit_id)
```

#### `list_repo_files`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2577)

```py
( repo_id: str revision: Optional[str] = None repo_type: Optional[str] = None token: Optional[Union[bool, str]] = None ) → export const metadata = 'undefined';List[str]
```

参数

+   `repo_id` (`str`) — 由命名空间（用户或组织）和存储库名称以`/`分隔组成。

+   `revision` (`str`, *可选*) — 要获取信息的模型存储库的修订版本。

+   `repo_type` (`str`, *可选*) — 如果上传到数据集或空间，请设置为`"dataset"`或`"space"`，如果上传到模型，则设置为`None`或`"model"`。默认为`None`。

+   `token` (`bool`或`str`, *可选*) — 有效的身份验证令牌（请参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则令牌不会发送到请求头中。

返回

`List[str]`

给定存储库中的文件列表。

获取给定存储库中的文件列表。

#### `list_repo_likers`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1971)

```py
( repo_id: str repo_type: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';List[User]
```

参数

+   `repo_id` (`str`) — 要检索的存储库。示例: `"user/my-cool-model"`.

+   `token` (`str`, *可选*) — 身份验证令牌。将默认为存储的令牌。

+   `repo_type` (`str`, *可选*) — 如果上传到数据集或空间，则设置为 `"dataset"` 或 `"space"`, 如果上传到模型，则为 `None` 或 `"model"`。默认为 `None`。

返回

`List[User]`

[User](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.User) 对象的列表。

列出在 Hugging Face Hub 上喜欢给定存储库的所有用户。

另请参阅 [like()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.like) 和 [list_liked_repos()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_liked_repos)。

#### `list_repo_refs`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2744)

```py
( repo_id: str repo_type: Optional[str] = None include_pull_requests: bool = False token: Optional[Union[bool, str]] = None ) → export const metadata = 'undefined';GitRefs
```

参数

+   `repo_id` (`str`) — 由 `/` 分隔的命名空间 (用户或组织) 和存储库名称。

+   `repo_type` (`str`, *可选*) — 如果从数据集或空间列出引用，则设置为 `"dataset"` 或 `"space"，如果从模型列出，则为 `None` 或 `"model"`。默认为 `None`。

+   `include_pull_requests` (`bool`, *可选*) — 是否在列表中包含来自拉取请求的引用。默认为 `False`。

+   `token` (`bool` 或 `str`, *可选*) — 有效的身份验证令牌 (参见 [https://huggingface.co/settings/token](https://huggingface.co/settings/token))。如果为 `None` 或 `True` 并且机器已登录 (通过 `huggingface-cli login` 或 [login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)), 令牌将从缓存中检索。如果为 `False`，则令牌不会在请求头中发送。

返回

[GitRefs](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.GitRefs)

包含有关存储库在 Hub 上的所有分支和标签的信息的对象。

获取给定存储库的引用列表 (标签和分支)。

示例:

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> api.list_repo_refs("gpt2")
GitRefs(branches=[GitRefInfo(name='main', ref='refs/heads/main', target_commit='e7da7f221d5bf496a48136c0cd264e630fe9fcc8')], converts=[], tags=[])

>>> api.list_repo_refs("bigcode/the-stack", repo_type='dataset')
GitRefs(
    branches=[
        GitRefInfo(name='main', ref='refs/heads/main', target_commit='18edc1591d9ce72aa82f56c4431b3c969b210ae3'),
        GitRefInfo(name='v1.1.a1', ref='refs/heads/v1.1.a1', target_commit='f9826b862d1567f3822d3d25649b0d6d22ace714')
    ],
    converts=[],
    tags=[
        GitRefInfo(name='v1.0', ref='refs/tags/v1.0', target_commit='c37a8cd1e382064d8aced5e05543c5f7753834da')
    ]
)
```

#### `list_repo_tree`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2613)

```py
( repo_id: str path_in_repo: Optional[str] = None recursive: bool = False expand: bool = False revision: Optional[str] = None repo_type: Optional[str] = None token: Optional[Union[bool, str]] = None ) → export const metadata = 'undefined';Iterable[Union[RepoFile, RepoFolder]]
```

参数

+   `repo_id` (`str`) — 由 `/` 分隔的命名空间 (用户或组织) 和存储库名称。

+   `path_in_repo` (`str`, *可选*) — 存储库中树 (文件夹) 的相对路径，例如: `"checkpoints/1fec34a/results"`。将默认为存储库的根树 (文件夹)。

+   `recursive` (`bool`, *可选*, 默认为 `False`) — 是否递归列出树的文件和文件夹。

+   `expand` (`bool`, *可选*, 默认为 `False`) — 是否获取有关树的文件和文件夹的更多信息 (例如最后提交和文件的安全扫描结果)。对于服务器来说，这个操作更昂贵，因此每页只返回 50 个结果 (而不是 1000)。由于 `huggingface_hub` 中实现了分页，因此对于您来说，这是透明的，除了获取结果所需的时间。

+   `revision` (`str`, *可选*) — 从中获取树的存储库的修订版本。默认为 `"main"` 分支。

+   `repo_type` (`str`, *可选*) — 从中获取树的存储库的类型 (`"model"`, `"dataset"` 或 `"space"`. 默认为 `"model"`。

+   `token` (`bool` 或 `str`, *可选*) — 有效的身份验证令牌 (参见 [https://huggingface.co/settings/token](https://huggingface.co/settings/token))。如果为 `None` 或 `True` 并且机器已登录 (通过 `huggingface-cli login` 或 [login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)), 令牌将从缓存中检索。如果为 `False`，则令牌不会在请求头中发送。

返回

`Iterable[Union[RepoFile, RepoFolder]]`

树的文件和文件夹的信息，作为 `RepoFile` 和 `RepoFolder` 对象的可迭代对象。文件和文件夹的顺序不保证。

引发

[RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 或 [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) 或 [EntryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.EntryNotFoundError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果找不到仓库（错误404）：错误的repo_id/repo_type、私有但未经身份验证或仓库不存在。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) — 如果在仓库上找不到修订版（错误404）。

+   [EntryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.EntryNotFoundError) — 如果仓库上的树（文件夹）不存在（错误404）。

列出仓库树的文件和文件夹，并获取有关它们的信息。

示例：

获取有关仓库树的信息。

```py
>>> from huggingface_hub import list_repo_tree
>>> repo_tree = list_repo_tree("lysandre/arxiv-nlp")
>>> repo_tree
<generator object HfApi.list_repo_tree at 0x7fa4088e1ac0>
>>> list(repo_tree)
[
    RepoFile(path='.gitattributes', size=391, blob_id='ae8c63daedbd4206d7d40126955d4e6ab1c80f8f', lfs=None, last_commit=None, security=None),
    RepoFile(path='README.md', size=391, blob_id='43bd404b159de6fba7c2f4d3264347668d43af25', lfs=None, last_commit=None, security=None),
    RepoFile(path='config.json', size=554, blob_id='2f9618c3a19b9a61add74f70bfb121335aeef666', lfs=None, last_commit=None, security=None),
    RepoFile(
        path='flax_model.msgpack', size=497764107, blob_id='8095a62ccb4d806da7666fcda07467e2d150218e',
        lfs={'size': 497764107, 'sha256': 'd88b0d6a6ff9c3f8151f9d3228f57092aaea997f09af009eefd7373a77b5abb9', 'pointer_size': 134}, last_commit=None, security=None
    ),
    RepoFile(path='merges.txt', size=456318, blob_id='226b0752cac7789c48f0cb3ec53eda48b7be36cc', lfs=None, last_commit=None, security=None),
    RepoFile(
        path='pytorch_model.bin', size=548123560, blob_id='64eaa9c526867e404b68f2c5d66fd78e27026523',
        lfs={'size': 548123560, 'sha256': '9be78edb5b928eba33aa88f431551348f7466ba9f5ef3daf1d552398722a5436', 'pointer_size': 134}, last_commit=None, security=None
    ),
    RepoFile(path='vocab.json', size=898669, blob_id='b00361fece0387ca34b4b8b8539ed830d644dbeb', lfs=None, last_commit=None, security=None)]
]
```

获取有关仓库树的更多信息（最后一次提交和文件的安全扫描结果）

```py
>>> from huggingface_hub import list_repo_tree
>>> repo_tree = list_repo_tree("prompthero/openjourney-v4", expand=True)
>>> list(repo_tree)
[
    RepoFolder(
        path='feature_extractor',
        tree_id='aa536c4ea18073388b5b0bc791057a7296a00398',
        last_commit={
            'oid': '47b62b20b20e06b9de610e840282b7e6c3d51190',
            'title': 'Upload diffusers weights (#48)',
            'date': datetime.datetime(2023, 3, 21, 9, 5, 27, tzinfo=datetime.timezone.utc)
        }
    ),
    RepoFolder(
        path='safety_checker',
        tree_id='65aef9d787e5557373fdf714d6c34d4fcdd70440',
        last_commit={
            'oid': '47b62b20b20e06b9de610e840282b7e6c3d51190',
            'title': 'Upload diffusers weights (#48)',
            'date': datetime.datetime(2023, 3, 21, 9, 5, 27, tzinfo=datetime.timezone.utc)
        }
    ),
    RepoFile(
        path='model_index.json',
        size=582,
        blob_id='d3d7c1e8c3e78eeb1640b8e2041ee256e24c9ee1',
        lfs=None,
        last_commit={
            'oid': 'b195ed2d503f3eb29637050a886d77bd81d35f0e',
            'title': 'Fix deprecation warning by changing `CLIPFeatureExtractor` to `CLIPImageProcessor`. (#54)',
            'date': datetime.datetime(2023, 5, 15, 21, 41, 59, tzinfo=datetime.timezone.utc)
        },
        security={
            'safe': True,
            'av_scan': {'virusFound': False, 'virusNames': None},
            'pickle_import_scan': None
        }
    )
    ...
]
```

#### `list_spaces`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1714)

```py
( filter: Union[str, Iterable[str], None] = None author: Optional[str] = None search: Optional[str] = None sort: Union[Literal['last_modified'], str, None] = None direction: Optional[Literal[-1]] = None limit: Optional[int] = None datasets: Union[str, Iterable[str], None] = None models: Union[str, Iterable[str], None] = None linked: bool = False full: Optional[bool] = None token: Optional[str] = None ) → export const metadata = 'undefined';Iterable[SpaceInfo]
```

参数

+   `filter` (`str` 或 `Iterable`, *可选*) — 可用于识别Hub上的空间的字符串标签或标签列表。

+   `author` (`str`, *可选*) — 用于标识返回的空间的作者的字符串。

+   `search` (`str`, *可选*) — 将包含在返回的空间中的字符串。

+   `sort` (`Literal["last_modified"]` 或 `str`, *可选*) — 用于对结果空间进行排序的键。可能的值是[huggingface_hub.hf_api.SpaceInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.SpaceInfo)`类的属性。

+   `direction` (`Literal[-1]` 或 `int`, *可选*) — 要排序的方向。值`-1`按降序排序，而所有其他值按升序排序。

+   `limit` (`int`, *可选*) — 获取的空间数量限制。将此选项保留为`None`将获取所有空间。

+   `datasets` (`str` 或 `Iterable`, *可选*) — 是否返回使用数据集的空间。可以传递特定数据集的名称作为字符串。

+   `models` (`str` 或 `Iterable`, *可选*) — 是否返回使用模型的空间。可以传递特定模型的名称作为字符串。

+   `linked` (`bool`, *可选*) — 是否返回使用模型或数据集的空间。

+   `full` (`bool`, *可选*) — 是否获取所有空间数据，包括`last_modified`、`siblings`和`card_data`字段。

+   `token` (`bool` 或 `str`, *可选*) — 有效的身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则令牌不会发送到请求头中。

返回

`Iterable[SpaceInfo]`

[huggingface_hub.hf_api.SpaceInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.SpaceInfo)对象的可迭代。

列出托管在Huggingface Hub上的空间，给定一些过滤条件。

#### `merge_pull_request`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6196)

```py
( repo_id: str discussion_num: int token: Optional[str] = None comment: Optional[str] = None repo_type: Optional[str] = None ) → export const metadata = 'undefined';DiscussionStatusChange
```

参数

+   `repo_id` (`str`) — 由一个命名空间（用户或组织）和由`/`分隔的仓库名称。

+   `discussion_num` (`int`) — 讨论或拉取请求的编号。必须是严格的正整数。

+   `comment` (`str`, *可选*) — 与状态更改一起发布的可选评论。

+   `repo_type` (`str`, *可选*) — 如果上传到数据集或空间，则设置为 `"dataset"` 或 `"space"`，如果上传到模型，则为 `None` 或 `"model"`。默认为 `None`。

+   `token` (`str`, *可选*) — 身份验证令牌（参见 [https://huggingface.co/settings/token](https://huggingface.co/settings/token)）

返回

[DiscussionStatusChange](/docs/huggingface_hub/v0.20.3/en/package_reference/community#huggingface_hub.DiscussionStatusChange)

状态更改事件

合并拉取请求。

引发以下错误：

+   [`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError) 如果 HuggingFace API 返回错误

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果某个参数值无效

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果无法找到要下载的仓库。这可能是因为它不存在，或者因为它设置为 `private`，而您没有访问权限。

#### `model_info`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2021)

```py
( repo_id: str revision: Optional[str] = None timeout: Optional[float] = None securityStatus: Optional[bool] = None files_metadata: bool = False token: Optional[Union[bool, str]] = None ) → export const metadata = 'undefined';huggingface_hub.hf_api.ModelInfo
```

参数

+   `repo_id` (`str`) — 由 `/` 分隔的命名空间（用户或组织）和仓库名称。

+   `revision` (`str`, *可选*) — 要获取信息的模型仓库的修订版本。

+   `timeout` (`float`, *可选*) — 是否为请求到 Hub 设置超时。

+   `securityStatus` (`bool`, *可选*) — 是否从模型仓库中检索安全状态。

+   `files_metadata` (`bool`, *可选*) — 是否检索仓库中文件的元数据（大小、LFS 元数据等）。默认为 `False`。

+   `token` (`bool` 或 `str`, *可选*) — 有效的身份验证令牌（参见 [https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为 `None` 或 `True` 并且机器已登录（通过 `huggingface-cli login` 或 [login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为 `False`，则令牌不会发送到请求头中。

返回

[huggingface_hub.hf_api.ModelInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.ModelInfo)

模型仓库信息。

在 huggingface.co 上获取有关特定模型的信息

如果传递可接受的令牌或已登录，则模型可以是私有的。

引发以下错误：

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果无法找到要下载的仓库。这可能是因为它不存在，或者因为它设置为 `private`，而您没有访问权限。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) 如果无法找到要下载的修订版本。

#### `move_repo`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L3305)

```py
( from_id: str to_id: str repo_type: Optional[str] = None token: Optional[str] = None )
```

参数

+   `from_id` (`str`) — 由 `/` 分隔的命名空间（用户或组织）和仓库名称。原始仓库标识符。

+   `to_id` (`str`) — 由 `/` 分隔的命名空间（用户或组织）和仓库名称。最终仓库标识符。

+   `repo_type` (`str`, *可选*) — 如果上传到数据集或空间，则设置为 `"dataset"` 或 `"space"`，如果上传到模型，则为 `None` 或 `"model"`。默认为 `None`。

+   `token` (`str`, *可选*) — 身份验证令牌（参见 [https://huggingface.co/settings/token](https://huggingface.co/settings/token)）

将仓库从 namespace1/repo_name1 移动到 namespace2/repo_name2

请注意有一些限制。有关移动仓库的更多信息，请参阅 [https://hf.co/docs/hub/repositories-settings#renaming-or-transferring-a-repo](https://hf.co/docs/hub/repositories-settings#renaming-or-transferring-a-repo)。

引发以下错误：

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果无法找到要下载的存储库。这可能是因为它不存在，或者因为它设置为 `private`，您无法访问。

#### `parse_safetensors_file_metadata`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L5254)

```py
( repo_id: str filename: str repo_type: Optional[str] = None revision: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';SafetensorsFileMetadata
```

参数

+   `repo_id` (`str`) — 由用户或组织名称和由 `/` 分隔的存储库名称。

+   `filename` (`str`) — 存储库中文件的名称。

+   `repo_type` (`str`，*可选*) — 如果文件在数据集或空间中，则设置为 `"dataset"` 或 `"space"`，如果在模型中，则为 `None` 或 `"model"`。默认为 `None`。

+   `revision` (`str`，*可选*) — 从中提取文件的 git 修订版。可以是分支名称、标签或提交哈希。默认为 `"main"` 分支的头部。

+   `token` (`bool` 或 `str`，*可选*) — 有效的身份验证令牌（参见 [https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为 `None` 或 `True` 并且机器已登录（通过 `huggingface-cli login` 或 [login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为 `False`，则令牌不会在请求头中发送。

返回

`SafetensorsFileMetadata`

与 safetensors 文件相关的信息。

引发

+   +   — `NotASafetensorsRepoError`：如果存储库不是 safetensors 存储库，即没有 `model.safetensors` 或 `model.safetensors.index.json` 文件。

+   +   — `SafetensorsParsingError`：如果无法正确解析 safetensors 文件头。

从 Hub 上的 safetensors 文件中解析元数据。

要一次从存储库中解析所有 safetensors 文件的元数据，请使用 [get_safetensors_metadata()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.get_safetensors_metadata)。

有关 safetensors 格式的更多详细信息，请查看 [https://huggingface.co/docs/safetensors/index#format](https://huggingface.co/docs/safetensors/index#format)。

#### `pause_inference_endpoint`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7241)

```py
( name: str namespace: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';InferenceEndpoint
```

参数

+   `name` (`str`) — 要暂停的推理端点的名称。

+   `namespace` (`str`，*可选*) — 推理端点所在的命名空间。默认为当前用户。

+   `token` (`str`，*可选*) — 身份验证令牌（参见 [https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

返回

[InferenceEndpoint](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_endpoints#huggingface_hub.InferenceEndpoint)

有关暂停推理端点的信息。

暂停推理端点。

暂停的推理端点不会收费。可以随时使用 [resume_inference_endpoint()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.resume_inference_endpoint) 恢复。这与使用 [scale_to_zero_inference_endpoint()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.scale_to_zero_inference_endpoint) 将推理端点缩减到零不同，后者在请求时会自动重新启动。

为方便起见，您也可以使用 [pause_inference_endpoint()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.pause_inference_endpoint) 暂停推理端点。

#### `pause_space`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6612)

```py
( repo_id: str token: Optional[str] = None ) → export const metadata = 'undefined';SpaceRuntime
```

参数

+   `repo_id` (`str`) — 要暂停的空间的 ID。示例："Salesforce/BLIP2"。

+   `token` (`str`，*可选*) — Hugging Face 令牌。如果未提供，将默认使用本地保存的令牌。

返回

[SpaceRuntime](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceRuntime)

有关您的空间的运行时信息，包括`stage=PAUSED`和请求的硬件。

引发

[RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError)或[HfHubHTTPError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HfHubHTTPError)或[BadRequestError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.BadRequestError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果找不到您的空间（错误404）。很可能是错误的repo_id或您的空间是私有的，但您未经过身份验证。

+   [HfHubHTTPError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HfHubHTTPError) — 403 Forbidden：只有空间所有者才能暂停空间。如果要管理您不拥有的空间，请通过打开讨论向所有者请求或复制空间。

+   [BadRequestError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.BadRequestError) — 如果您的空间是静态空间。静态空间始终运行且不计费。如果要隐藏静态空间，可以将其设置为私有。

暂停您的空间。

暂停的空间会停止执行，直到其所有者手动重新启动。这与免费空间在48小时不活动后进入的睡眠状态不同。暂停时间不会计入您的账户，无论您选择的硬件如何。要重新启动您的空间，请使用[restart_space()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.restart_space)并转到您的空间设置页面。

有关更多详细信息，请访问[文档](https://huggingface.co/docs/hub/spaces-gpus#pause)。

#### `preupload_lfs_files`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L3925)

```py
( repo_id: str additions: Iterable[CommitOperationAdd] token: Optional[str] = None repo_type: Optional[str] = None revision: Optional[str] = None create_pr: Optional[bool] = None num_threads: int = 5 free_memory: bool = True gitignore_content: Optional[str] = None )
```

参数

+   `repo_id`（`str`） — 您将提交文件的存储库，例如：`"username/custom_transformers"`。

+   `operations`（[CommitOperationAdd](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitOperationAdd)的`Iterable`） — 要上传的文件列表。警告：此列表中的对象将被修改以包含与上传相关的信息。不要重复使用相同的对象进行多次提交。

+   `token`（`str`，*可选*） — 认证令牌。将默认为存储的令牌。

+   `repo_type`（`str`，*可选*） — 要上传到的存储库类型（例如`"model"`-默认-，`"dataset"`或`"space"`）。

+   `revision`（`str`，*可选*） — 要从中提交的git修订版本。默认为`"main"`分支的头部。

+   `create_pr`（`boolean`，*可选*） — 您是否计划使用该提交创建拉取请求。默认为`False`。

+   `num_threads`（`int`，*可选*） — 用于上传文件的并发线程数。默认为5。将其设置为2意味着最多同时上传2个文件。

+   `gitignore_content`（`str`，*可选*） — `.gitignore`文件的内容，用于了解应忽略哪些文件。优先级顺序是首先检查是否传递了`gitignore_content`，然后检查文件提交列表中是否存在`.gitignore`文件，最后默认使用Hub上已托管的`.gitignore`文件（如果有）。

在未来提交之前，将LFS文件预先上传到S3。

如果您正在动态生成要上传的文件，并且不希望在一次性上传之前将它们存储在内存中，则此方法很有用。

这是一个高级用户方法。您不应该直接调用它来进行正常提交。而是直接使用[create_commit()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_commit)。

在过程中，提交操作将被改变。特别是，在上传后，附加的`path_or_fileobj`将被删除以节省内存（并替换为一个空的`bytes`对象）。除了将它们传递给[create_commit()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_commit)之外，不要重复使用相同的对象。如果您不想从提交操作对象中删除附加的内容，请传递`free_memory=False`。

Example:

```py
>>> from huggingface_hub import CommitOperationAdd, preupload_lfs_files, create_commit, create_repo

>>> repo_id = create_repo("test_preupload").repo_id

# Generate and preupload LFS files one by one
>>> operations = [] # List of all `CommitOperationAdd` objects that will be generated
>>> for i in range(5):
...     content = ... # generate binary content
...     addition = CommitOperationAdd(path_in_repo=f"shard_{i}_of_5.bin", path_or_fileobj=content)
...     preupload_lfs_files(repo_id, additions=[addition]) # upload + free memory
...     operations.append(addition)

# Create commit
>>> create_commit(repo_id, operations=operations, commit_message="Commit all shards")
```

#### `reject_access_request`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L8064)

```py
( repo_id: str user: str repo_type: Optional[str] = None token: Optional[str] = None )
```

Parameters

+   `repo_id` (`str`) — 要拒绝访问请求的存储库的ID。

+   `user` (`str`) — 应拒绝访问请求的用户的用户名。

+   `repo_type` (`str`, *optional*) — 要拒绝访问请求的存储库类型。必须是`model`、`dataset`或`space`之一。默认为`model`。

+   `token` (`str`, *optional*) — 有效的身份验证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

Raises

`HTTPError`

+   `HTTPError` — 如果存储库没有受限，则为HTTP 400。

+   `HTTPError` — 如果您只有对存储库的只读访问权限，则为HTTP 403。如果您在存储库所属的组织中没有`write`或`admin`角色，或者如果您传递了一个`read`令牌，则可能会出现这种情况。

+   `HTTPError` — 如果Hub上不存在用户，则为HTTP 404。

+   `HTTPError` — 如果找不到用户访问请求，则为HTTP 404。

+   `HTTPError` — 如果用户访问请求已在被拒绝列表中，则为HTTP 404。

拒绝用户对给定受限存储库的访问请求。

被拒绝的请求将进入被拒绝列表。用户无法下载存储库的任何文件。可以随时使用[accept_access_request()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.accept_access_request)和[cancel_access_request()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.cancel_access_request)接受或取消被拒绝的请求。取消的请求将返回到待处理列表，而接受的请求将进入已接受列表。

有关受限存储库的更多信息，请参见[https://huggingface.co/docs/hub/models-gated](https://huggingface.co/docs/hub/models-gated)。

#### `rename_discussion`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6054)

```py
( repo_id: str discussion_num: int new_title: str token: Optional[str] = None repo_type: Optional[str] = None ) → export const metadata = 'undefined';DiscussionTitleChange
```

Parameters

+   `repo_id` (`str`) — 由`/`分隔的命名空间（用户或组织）和存储库名称。

+   `discussion_num` (`int`) — 讨论或拉取请求的编号。必须是严格正整数。

+   `new_title` (`str`) — 讨论的新标题

+   `repo_type` (`str`, *optional*) — 如果上传到数据集或空间，则设置为`"dataset"`或`"space"`，如果上传到模型，则设置为`None`或`"model"`。默认为`None`。

+   `token` (`str`, *optional*) — 身份验证令牌（请参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token))。

Returns

[DiscussionTitleChange](/docs/huggingface_hub/v0.20.3/en/package_reference/community#huggingface_hub.DiscussionTitleChange)

标题更改事件

重命名讨论。

Examples:

```py
>>> new_title = "New title, fixing a typo"
>>> HfApi().rename_discussion(
...     repo_id="username/repo_name",
...     discussion_num=34
...     new_title=new_title
... )
# DiscussionTitleChange(id='deadbeef0000000', type='title-change', ...)

```

引发以下错误：

+   [`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError) 如果HuggingFace API返回错误

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果某个参数值无效

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果无法找到要下载的存储库。这可能是因为它不存在，或者因为它被设置为`private`，而您没有访问权限。

#### `repo_exists`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2283)

```py
( repo_id: str repo_type: Optional[str] = None token: Optional[str] = None )
```

Parameters

+   `repo_id` (`str`) — 由`/`分隔的命名空间（用户或组织）和存储库名称。

+   `repo_type` (`str`, *可选*) — 如果从数据集或空间获取存储库信息，则设置为`"dataset"`或`"space"`，如果从模型获取存储库信息，则设置为`None`或`"model"`。默认为`None`。

+   `token` (`bool`或`str`, *可选*) — 有效的身份验证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则令牌不会发送到请求头中。

检查Hugging Face Hub上是否存在存储库。

示例：

```py
>>> from huggingface_hub import repo_exists
>>> repo_exists("huggingface/transformers")
True
>>> repo_exists("huggingface/not-a-repo")
False
```

#### `repo_info`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2215)

```py
( repo_id: str revision: Optional[str] = None repo_type: Optional[str] = None timeout: Optional[float] = None files_metadata: bool = False token: Optional[Union[bool, str]] = None ) → export const metadata = 'undefined';Union[SpaceInfo, DatasetInfo, ModelInfo]
```

参数

+   `repo_id` (`str`) — 由命名空间（用户或组织）和存储库名称以`/`分隔组成。

+   `revision` (`str`, *可选*) — 要获取信息的存储库的修订版本。

+   `repo_type` (`str`, *可选*) — 如果从数据集或空间获取存储库信息，则设置为`"dataset"`或`"space"`，如果从模型获取存储库信息，则设置为`None`或`"model"`。默认为`None`。

+   `timeout` (`float`, *可选*) — 是否为Hub请求设置超时。

+   `files_metadata` (`bool`, *可选*) — 是否检索存储库中文件的元数据（大小，LFS元数据等）。默认为`False`。

+   `token` (`bool`或`str`, *可选*) — 有效的身份验证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为`None`或`True`且机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为`False`，则令牌不会发送到请求头中。

返回

`Union[SpaceInfo, DatasetInfo, ModelInfo]`

存储库信息，作为[huggingface_hub.hf_api.DatasetInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.DatasetInfo)、[huggingface_hub.hf_api.ModelInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.ModelInfo)或[huggingface_hub.hf_api.SpaceInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.SpaceInfo)对象。

获取给定类型的给定存储库的信息对象。

引发以下错误：

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果无法找到要下载的存储库。这可能是因为它不存在，或者因为它被设置为`private`而您无法访问。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) 如果无法找到要下载的修订版本。

#### `request_space_hardware`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6516)

```py
( repo_id: str hardware: SpaceHardware token: Optional[str] = None sleep_time: Optional[int] = None ) → export const metadata = 'undefined';SpaceRuntime
```

参数

+   `repo_id` (`str`) — 要更新的存储库的ID。示例："bigcode/in-the-stack"。

+   `hardware` (`str`或[SpaceHardware](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceHardware)) — 在哪种硬件上运行空间。示例："t4-medium"。

+   `token` (`str`, *可选*) — Hugging Face令牌。如果未提供，将默认使用本地保存的令牌。

+   `sleep_time` (`int`, *可选*) — 在将空间置于休眠状态之前等待的不活动秒数。如果不希望空间休眠（升级硬件的默认行为），请将其设置为`-1`。对于免费硬件，无法配置休眠时间（值固定为48小时的不活动时间）。有关更多详细信息，请参见[https://huggingface.co/docs/hub/spaces-gpus#sleep-time](https://huggingface.co/docs/hub/spaces-gpus#sleep-time)。

返回

[SpaceRuntime](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceRuntime)

关于空间的运行时信息，包括空间阶段和硬件。

为空间请求新的硬件。

在创建空间存储库时也可以直接请求硬件！有关详细信息，请参阅[create_repo()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_repo)。

#### `request_space_storage`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6806)

```py
( repo_id: str storage: SpaceStorage token: Optional[str] = None ) → export const metadata = 'undefined';SpaceRuntime
```

参数

+   `repo_id` (`str`) — 要更新的空间的ID。示例："HuggingFaceH4/open_llm_leaderboard"。

+   `storage` (`str` 或 [SpaceStorage](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceStorage)) — 存储层。可以是‘small’、‘medium’或‘large’。

+   `token` (`str`, *可选*) — Hugging Face token。如果未提供，将默认使用本地保存的令牌。

返回

[SpaceRuntime](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceRuntime)

关于空间的运行时信息，包括空间阶段和硬件。

为空间请求持久存储。

一旦授予持久存储，就无法减少持久存储。要这样做，必须通过[delete_space_storage()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.delete_space_storage)删除它。

#### `restart_space`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6648)

```py
( repo_id: str token: Optional[str] = None factory_reboot: bool = False ) → export const metadata = 'undefined';SpaceRuntime
```

参数

+   `repo_id` (`str`) — 要重新启动的空间的ID。示例："Salesforce/BLIP2"。

+   `token` (`str`, *可选*) — Hugging Face token。如果未提供，将默认使用本地保存的令牌。

+   `factory_reboot` (`bool`, *可选*) — 如果为`True`，空间将从头开始重建，不缓存任何要求。

返回

[SpaceRuntime](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceRuntime)

关于您的空间的运行时信息。

引发

[RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 或 [HfHubHTTPError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HfHubHTTPError) 或 [BadRequestError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.BadRequestError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果未找到您的空间（错误404）。很可能是错误的repo_id或您的空间是私有的，但您未经过身份验证。

+   [HfHubHTTPError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HfHubHTTPError) — 403 Forbidden: 只有空间的所有者才能重新启动它。如果要重新启动您不拥有的空间，请向所有者提出请求，或复制该空间。

+   [BadRequestError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.BadRequestError) — 如果您的空间是静态空间。静态空间始终运行且不收费。如果要隐藏静态空间，可以将其设置为私有。

重新启动您的空间。

这是唯一一种以编程方式重新启动空间的方法，如果您将其暂停（请参阅[pause_space()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.pause_space)）。您必须是空间的所有者才能重新启动它。如果您正在使用升级后的硬件，一旦重新启动空间，您的帐户将被收费。无论空间的当前状态如何，都可以触发重新启动。

有关更多详细信息，请访问[文档](https://huggingface.co/docs/hub/spaces-gpus#pause)。

#### `resume_inference_endpoint`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7273)

```py
( name: str namespace: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';InferenceEndpoint
```

参数

+   `name` (`str`) — 要恢复的推理端点的名称。

+   `namespace` (`str`, *可选*) — 推理端点所在的命名空间。默认为当前用户。

+   `token` (`str`, *可选*) — 认证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

返回

[InferenceEndpoint](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_endpoints#huggingface_hub.InferenceEndpoint)

有关已恢复的推理端点的信息。

恢复推理端点。

为方便起见，您还可以使用[InferenceEndpoint.resume()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_endpoints#huggingface_hub.InferenceEndpoint.resume)来恢复推理端点。

#### `run_as_future`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1251)

```py
( fn: Callable[..., R] *args **kwargs ) → export const metadata = 'undefined';Future
```

参数

+   `fn` (`Callable`) — 要在后台运行的方法。

+   `*args,` **kwargs — 要调用方法的参数。

返回

`Future`

一个[Future](https://docs.python.org/3/library/concurrent.futures.html#future-objects)实例，用于获取任务的结果。

在后台运行方法并返回Future实例。

主要目标是在不阻塞主线程的情况下运行方法（例如在训练期间推送数据）。后台作业被排队以保持顺序，但不会并行运行。如果您需要通过并行化大量调用API来加快脚本的速度，您必须设置并使用自己的[ThreadPoolExecutor](https://docs.python.org/3/library/concurrent.futures.html#threadpoolexecutor)。

注意：像[upload_file()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.upload_file)、[upload_folder()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.upload_folder)和[create_commit()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_commit)这样的常用方法有一个`run_as_future: bool`参数，可以直接在后台调用它们。这等同于在它们上调用`api.run_as_future(...)`，但更简洁。

示例：

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> future = api.run_as_future(api.whoami) # instant
>>> future.done()
False
>>> future.result() # wait until complete and return result
(...)
>>> future.done()
True
```

#### `scale_to_zero_inference_endpoint`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7301)

```py
( name: str namespace: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';InferenceEndpoint
```

参数

+   `name` (`str`) — 要缩减到零的推理端点的名称。

+   `namespace` (`str`, *可选*) — 推理端点所在的命名空间。默认为当前用户。

+   `token` (`str`, *可选*) — 认证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

返回

[InferenceEndpoint](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_endpoints#huggingface_hub.InferenceEndpoint)

有关缩减到零的推理端点的信息。

将推理端点缩减到零。

缩减到零的推理端点不会收费。它将在下一次请求时恢复，但会有冷启动延迟。这与使用[pause_inference_endpoint()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.pause_inference_endpoint)暂停推理端点不同，后者需要使用[resume_inference_endpoint()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.resume_inference_endpoint)手动恢复。

为方便起见，您还可以使用[InferenceEndpoint.scale_to_zero()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_endpoints#huggingface_hub.InferenceEndpoint.scale_to_zero)将推理端点缩减到零。

#### `set_space_sleep_time`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L6566)

```py
( repo_id: str sleep_time: int token: Optional[str] = None ) → export const metadata = 'undefined';SpaceRuntime
```

参数

+   `repo_id` (`str`) — 要更新的存储库的ID。示例："bigcode/in-the-stack"。

+   `sleep_time` (`int`, *optional*) — 在将 Space 设置为休眠之前等待的不活动秒数。如果不想让 Space 暂停（升级硬件的默认行为），请将其设置为 `-1`。对于免费硬件，无法配置休眠时间（值固定为48小时的不活动时间）。有关更多详细信息，请参阅[https://huggingface.co/docs/hub/spaces-gpus#sleep-time](https://huggingface.co/docs/hub/spaces-gpus#sleep-time)。

+   `token` (`str`, *optional*) — Hugging Face 令牌。如果未提供，将默认使用本地保存的令牌。

返回

[SpaceRuntime](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceRuntime)

有关 Space 的运行时信息，包括 Space 阶段和硬件。

为在升级的硬件上运行的 Space 设置自定义休眠时间。

您的 Space 在 X 秒的不活动后会进入休眠状态。当您的 Space 处于“休眠”模式时，不会收费。如果新访客登陆您的 Space，它将“唤醒”它。只有升级的硬件才能具有可配置的休眠时间。要了解有关休眠阶段的更多信息，请参阅[https://huggingface.co/docs/hub/spaces-gpus#sleep-time](https://huggingface.co/docs/hub/spaces-gpus#sleep-time)。

在请求硬件时，也可以设置自定义休眠时间，使用 [request_space_hardware()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.request_space_hardware)。

#### `snapshot_download`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L4991)

```py
( repo_id: str repo_type: Optional[str] = None revision: Optional[str] = None cache_dir: Union[str, Path, None] = None local_dir: Union[str, Path, None] = None local_dir_use_symlinks: Union[bool, Literal['auto']] = 'auto' proxies: Optional[Dict] = None etag_timeout: float = 10 resume_download: bool = False force_download: bool = False token: Optional[Union[str, bool]] = None local_files_only: bool = False allow_patterns: Optional[Union[List[str], str]] = None ignore_patterns: Optional[Union[List[str], str]] = None max_workers: int = 8 tqdm_class: Optional[base_tqdm] = None )
```

参数

+   `repo_id` (`str`) — 由用户或组织名称和仓库名称以 `/` 分隔的字符串。

+   `repo_type` (`str`, *optional*) — 如果从数据集或空间下载，则设置为 `"dataset"` 或 `"space"`，如果从模型下载，则设置为 `None` 或 `"model"`。默认为 `None`。

+   `revision` (`str`, *optional*) — 可选的 Git 修订 ID，可以是分支名称、标签或提交哈希。

+   `cache_dir` (`str`, `Path`, *optional*) — 存储缓存文件的文件夹路径。

+   `local_dir` (`str` or `Path`, *optional*) — 如果提供，下载的文件将放置在此目录下，可以是符号链接（默认）或常规文件（有关更多详细信息，请参见描述）。

+   `local_dir_use_symlinks` (`"auto"` or `bool`, 默认为 `"auto"`) — 与 `local_dir` 一起使用。如果设置为 `"auto"`，将使用缓存目录，并根据文件大小将文件复制或创建符号链接到本地目录。如果设置为 `True`，将创建符号链接，无论文件大小如何。如果设置为 `False`，文件将从缓存中复制（如果已存在）或从 Hub 下载并不缓存。有关更多详细信息，请参见描述。

+   `proxies` (`dict`, *optional*) — 将协议映射到传递给 `requests.request` 的代理的URL的字典。

+   `etag_timeout` (`float`, *optional*, 默认为 `10`) — 在获取 ETag 时，等待服务器发送数据的秒数，超过这个时间将放弃，这个时间会传递给 `requests.request`。

+   `resume_download` (`bool`, *optional*, 默认为 `False) -- 如果为` True`，恢复先前中断的下载。

+   `force_download` (`bool`, *optional*, 默认为 `False`) — 是否应下载文件，即使它已经存在于本地缓存中。

+   `token` (`bool` or `str`, *optional*) — 有效的身份验证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果为 `None` 或 `True` 并且机器已登录（通过 `huggingface-cli login` 或 [login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为 `False`，则请求标头中不会发送令牌。

+   `local_files_only` (`bool`, *optional*, 默认为 `False`) — 如果为 `True`，避免下载文件，并返回本地缓存文件的路径（如果存在）。

+   `allow_patterns` (`List[str]` or `str`, *optional*) — 如果提供，只有与至少一个模式匹配的文件才会被下载。

+   `ignore_patterns` (`List[str]` 或 `str`, *可选*) — 如果提供，则不会下载与任何模式匹配的文件。

+   `max_workers` (`int`, *可选*) — 下载文件的并发线程数（1 线程 = 1 个文件下载）。默认为 8。

+   `tqdm_class` (`tqdm`, *可选*) — 如果提供，将覆盖进度条的默认行为。传递的参数必须继承自 `tqdm.auto.tqdm` 或至少模仿其行为。请注意，`tqdm_class` 不会传递给每个单独的下载。默认为自定义 HF 进度条，可以通过设置 `HF_HUB_DISABLE_PROGRESS_BARS` 环境变量来禁用。

下载存储库文件。

下载指定修订版本的存储库文件的完整快照。当您想要从存储库获取所有文件时很有用，因为您事先不知道需要哪些文件。所有文件都嵌套在一个文件夹中，以保持它们相对于该文件夹的实际文件名。您还可以使用 `allow_patterns` 和 `ignore_patterns` 进行筛选要下载的文件。

如果提供了 `local_dir`，则将在此位置复制来自存储库的文件结构。您可以配置如何移动这些文件：

+   如果 `local_dir_use_symlinks="auto"`（默认值），文件将被下载并存储在缓存目录中作为 blob 文件。小文件（<5MB）将在 `local_dir` 中复制，而对于较大的文件则创建符号链接。目标是能够手动编辑和保存小文件而不会破坏缓存，同时为二进制文件节省磁盘空间。5MB 阈值可以通过 `HF_HUB_LOCAL_DIR_AUTO_SYMLINK_THRESHOLD` 环境变量进行配置。

+   如果 `local_dir_use_symlinks=True`，文件将被下载，存储在缓存目录中，并在 `local_dir` 中创建符号链接。这在磁盘使用方面是最佳的，但文件不能手动编辑。

+   如果 `local_dir_use_symlinks=False` 并且 blob 文件存在于缓存目录中，则会在本地目录中复制它们。这意味着磁盘使用未经优化。

+   最后，如果 `local_dir_use_symlinks=False` 并且 blob 文件不存在于缓存目录中，则文件将被下载并直接放在 `local_dir` 下。这意味着如果以后需要重新下载它们，它们将被完全重新下载。

另一种方法是克隆存储库，但这需要安装和正确配置 git 和 git-lfs。在使用 git 克隆存储库时，也无法筛选要下载的文件。

引发以下错误：

+   [`EnvironmentError`](https://docs.python.org/3/library/exceptions.html#EnvironmentError) 如果 `token=True` 且找不到令牌。

+   [`OSError`](https://docs.python.org/3/library/exceptions.html#OSError) 如果无法确定 ETag。

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果某个参数值无效

#### `space_info`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2152)

```py
( repo_id: str revision: Optional[str] = None timeout: Optional[float] = None files_metadata: bool = False token: Optional[Union[bool, str]] = None ) → export const metadata = 'undefined';SpaceInfo
```

参数

+   `repo_id` (`str`) — 由斜杠分隔的命名空间（用户或组织）和存储库名称。

+   `revision` (`str`, *可选*) — 要获取信息的空间存储库的修订版本。

+   `timeout` (`float`, *可选*) — 是否为请求到 Hub 设置超时。

+   `files_metadata` (`bool`, *可选*) — 是否检索存储库中文件的元数据（大小、LFS 元数据等）。默认为 `False`。

+   `token` (`bool` 或 `str`, *可选*) — 有效的身份验证令牌（参见 [https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果 `None` 或 `True` 并且机器已登录（通过 `huggingface-cli login` 或 [login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌将从缓存中检索。如果为 `False`，则请求头中不会发送令牌。

返回

[SpaceInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.SpaceInfo)

空间存储库信息。

获取有关huggingface.co上一个特定空间的信息。

如果传递一个可接受的令牌，空间可以是私有的。

引发以下错误：

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果找不到要下载的存储库。这可能是因为它不存在，或者因为它被设置为`private`而您无法访问。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) 如果找不到要下载的修订版本。

#### `super_squash_history`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L2978)

```py
( repo_id: str branch: Optional[str] = None commit_message: Optional[str] = None repo_type: Optional[str] = None token: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 由`/`分隔的命名空间（用户或组织）和存储库名称。

+   `branch` (`str`, *optional*) — 要合并的分支。默认为`"main"`分支的头部。

+   `commit_message` (`str`, *optional*) — 用于合并提交的提交消息。

+   `repo_type` (`str`, *optional*) — 如果从数据集或空间列出提交，则设置为`"dataset"`或`"space"`，如果从模型列出，则设置为`None`或`"model"`。默认为`None`。

+   `token` (`str`, *optional*) — 有效的身份验证令牌（参见[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。如果机器已登录（通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)），则令牌可以自动从缓存中检索。

引发

[RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 或 [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) 或 [BadRequestError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.BadRequestError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果找不到存储库（错误404）：错误的repo_id/repo_type，私有但未经身份验证或存储库不存在。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) — 如果找不到要合并的分支。

+   [BadRequestError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.BadRequestError) — 如果分支的引用无效。您不能在标签上合并历史记录。

在Hub上为存储库的分支合并提交历史。

当您知道将要进行数百次提交并且不想混乱历史记录时，合并存储库历史非常有用。只能从分支的头部执行合并提交。

一旦合并，提交历史将无法检索。这是一个不可逆转的操作。

一旦分支的历史被合并，就不可能将其合并回另一个分支，因为它们的历史将会分歧。

示例：

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()

# Create repo
>>> repo_id = api.create_repo("test-squash").repo_id

# Make a lot of commits.
>>> api.upload_file(repo_id=repo_id, path_in_repo="file.txt", path_or_fileobj=b"content")
>>> api.upload_file(repo_id=repo_id, path_in_repo="lfs.bin", path_or_fileobj=b"content")
>>> api.upload_file(repo_id=repo_id, path_in_repo="file.txt", path_or_fileobj=b"another_content")

# Squash history
>>> api.super_squash_history(repo_id=repo_id)
```

#### `unlike`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1849)

```py
( repo_id: str token: Optional[str] = None repo_type: Optional[str] = None )
```

参数

+   `repo_id` (`str`) — 要取消关注的存储库。示例："user/my-cool-model"。

+   `token` (`str`, *optional*) — 身份验证令牌。将默认使用存储的令牌。

+   `repo_type` (`str`, *optional*) — 如果取消关注数据集或空间，则设置为`"dataset"`或`"space"`，如果取消关注模型，则设置为`None`或`"model"`。默认为`None`。

引发

[RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError)

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) — 如果找不到存储库（错误404）：错误的repo_id/repo_type，私有但未经身份验证或存储库不存在。

取消关注Hub上的给定存储库（例如，从收藏列表中删除）。

另请参阅[like()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.like)和[list_liked_repos()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_liked_repos)。

示例：

```py
>>> from huggingface_hub import like, list_liked_repos, unlike
>>> like("gpt2")
>>> "gpt2" in list_liked_repos().models
True
>>> unlike("gpt2")
>>> "gpt2" in list_liked_repos().models
False
```

#### `update_collection_item`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7677)

```py
( collection_slug: str item_object_id: str note: Optional[str] = None position: Optional[int] = None token: Optional[str] = None )
```

参数

+   `collection_slug` (`str`) — 要更新的集合的slug。示例："TheBloke/recent-models-64f9a55bb3115b4f513ec026"。

+   `item_object_id` (`str`) — 集合中项目的ID。这不是Hub上项目的ID（repo_id或paper id）。必须从[CollectionItem](/docs/huggingface_hub/v0.20.3/en/package_reference/collections#huggingface_hub.CollectionItem)对象中检索。示例：`collection.items[0].item_object_id`。

+   `note` (`str`, *optional*) — 要附加到集合中项目的注释。注释的最大长度为500个字符。

+   `position` (`int`, *optional*) — 项目在集合中的新位置。

+   `token` (`str`, *optional*) — Hugging Face令牌。如果未提供，将默认为本地保存的令牌。

更新集合中的项目。

示例：

```py
>>> from huggingface_hub import get_collection, update_collection_item

# Get collection first
>>> collection = get_collection("TheBloke/recent-models-64f9a55bb3115b4f513ec026")

# Update item based on its ID (add note + update position)
>>> update_collection_item(
...     collection_slug="TheBloke/recent-models-64f9a55bb3115b4f513ec026",
...     item_object_id=collection.items[-1].item_object_id,
...     note="Newly updated model!"
...     position=0,
... )
```

#### `update_collection_metadata`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7504)

```py
( collection_slug: str title: Optional[str] = None description: Optional[str] = None position: Optional[int] = None private: Optional[bool] = None theme: Optional[str] = None token: Optional[str] = None )
```

参数

+   `collection_slug` (`str`) — 要更新的集合的slug。示例："TheBloke/recent-models-64f9a55bb3115b4f513ec026"。

+   `title` (`str`) — 要更新的集合的标题。

+   `description` (`str`, *optional*) — 要更新的集合的描述。

+   `position` (`int`, *optional*) — 用户集合列表中集合的新位置。

+   `private` (`bool`, *optional*) — 集合是否应为私有。

+   `theme` (`str`, *optional*) — Hub上集合的主题。

+   `token` (`str`, *optional*) — Hugging Face令牌。如果未提供，将默认为本地保存的令牌。

更新Hub上集合的元数据。

所有参数都是可选的。只会更新提供的元数据。

返回：[Collection](/docs/huggingface_hub/v0.20.3/en/package_reference/collections#huggingface_hub.Collection)

示例：

```py
>>> from huggingface_hub import update_collection_metadata
>>> collection = update_collection_metadata(
...     collection_slug="username/iccv-2023-64f9a55bb3115b4f513ec026",
...     title="ICCV Oct. 2023"
...     description="Portfolio of models, datasets, papers and demos I presented at ICCV Oct. 2023",
...     private=False,
...     theme="pink",
... )
>>> collection.slug
"username/iccv-oct-2023-64f9a55bb3115b4f513ec026"
# ^collection slug got updated but not the trailing ID
```

#### `update_inference_endpoint`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L7127)

```py
( name: str accelerator: Optional[str] = None instance_size: Optional[str] = None instance_type: Optional[str] = None min_replica: Optional[int] = None max_replica: Optional[int] = None repository: Optional[str] = None framework: Optional[str] = None revision: Optional[str] = None task: Optional[str] = None namespace: Optional[str] = None token: Optional[str] = None ) → export const metadata = 'undefined';InferenceEndpoint
```

参数

+   `name` (`str`) — 要更新的推理端点的名称。

+   `accelerator` (`str`, *optional*) — 用于推理的硬件加速器（例如 `"cpu"`）。

+   `instance_size` (`str`, *optional*) — 用于托管模型的实例的大小或类型（例如 `"large"`）。

+   `instance_type` (`str`, *optional*) — 将部署推理端点的云实例类型（例如 `"c6i"`）。

+   `min_replica` (`int`, *optional*) — 要保持运行的最小副本（实例）数量。

+   `max_replica` (`int`, *optional*) — 要扩展到的最大副本（实例）数量。

+   `repository` (`str`, *optional*) — 与推理端点关联的模型存储库的名称（例如 `"gpt2"`）。

+   `framework` (`str`, *optional*) — 用于模型的机器学习框架（例如 `"custom"`）。

+   `revision` (`str`, *optional*) — 要在推理端点上部署的特定模型修订版（例如 `"6c0e6080953db56375760c0471a8c5f2929baf11"`）。

+   `task` (`str`, *optional*) — 要部署模型的任务（例如 `"text-classification"`）。

+   `namespace` (`str`, *optional*) — 要更新推理端点的命名空间。默认为当前用户的命名空间。

+   `token` (`str`, *optional*) — 身份验证令牌（请参阅[https://huggingface.co/settings/token](https://huggingface.co/settings/token)）。

返回

[InferenceEndpoint](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_endpoints#huggingface_hub.InferenceEndpoint)

有关更新后的推理端点的信息。

更新推理端点。

此方法允许更新计算配置、部署模型或两者。所有参数都是可选的，但至少必须提供一个。

为方便起见，您还可以使用 [InferenceEndpoint.update()](/docs/huggingface_hub/v0.20.3/en/package_reference/inference_endpoints#huggingface_hub.InferenceEndpoint.update) 更新推理端点。

#### `update_repo_visibility`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L3245)

```py
( repo_id: str private: bool = False token: Optional[str] = None organization: Optional[str] = None repo_type: Optional[str] = None name: Optional[str] = None )
```

参数

+   `repo_id` (`str`, *可选*) — 由命名空间（用户或组织）和仓库名称以 `/` 分隔组成。

+   `private` (`bool`, *可选*, 默认为 `False`) — 模型仓库是否应该是私有的。

+   `token` (`str`, *可选*) — 认证令牌（请参阅 [https://huggingface.co/settings/token](https://huggingface.co/settings/token)）

+   `repo_type` (`str`, *可选*) — 如果上传到数据集或空间，则设置为 `"dataset"` 或 `"space"`, 如果上传到模型则设置为 `None` 或 `"model"`。默认为 `None`。

更新仓库的可见性设置。

引发以下错误：

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果无法找到要下载的仓库。这可能是因为它不存在，或者因为它被设置为 `private` 且您无法访问。

#### `upload_file`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L4111)

```py
( path_or_fileobj: Union[str, Path, bytes, BinaryIO] path_in_repo: str repo_id: str token: Optional[str] = None repo_type: Optional[str] = None revision: Optional[str] = None commit_message: Optional[str] = None commit_description: Optional[str] = None create_pr: Optional[bool] = None parent_commit: Optional[str] = None run_as_future: bool = False ) → export const metadata = 'undefined';CommitInfo or Future
```

参数

+   `path_or_fileobj` (`str`, `Path`, `bytes` 或 `IO`) — 本地计算机上文件的路径或二进制数据流 / 文件对象 / 缓冲区。

+   `path_in_repo` (`str`) — 仓库中的相对文件路径，例如：`"checkpoints/1fec34a/weights.bin"`

+   `repo_id` (`str`) — 将文件上传到的仓库，例如：`"username/custom_transformers"`

+   `token` (`str`, *可选*) — 认证令牌，使用 `HfApi.login` 方法获取。将默认使用存储的令牌。

+   `repo_type` (`str`, *可选*) — 如果上传到数据集或空间，则设置为 `"dataset"` 或 `"space"`, 如果上传到模型则设置为 `None` 或 `"model"`。默认为 `None`。

+   `revision` (`str`, *可选*) — 要提交的 git 版本。默认为 `"main"` 分支的头。

+   `commit_message` (`str`, *可选*) — 生成提交的摘要 / 标题 / 第一行

+   `commit_description` (`str` *可选*) — 生成提交的描述

+   `create_pr` (`boolean`, *可选*) — 是否创建一个带有该提交的拉取请求。默认为 `False`。如果未设置 `revision`，则 PR 将针对 `"main"` 分支打开。如果设置了 `revision` 并且是一个分支，则 PR 将针对此分支打开。如果设置了 `revision` 并且不是分支名称（例如：提交 oid），服务器将返回 `RevisionNotFoundError`。

+   `parent_commit` (`str`, *可选*) — 父提交的 OID / SHA，以十六进制字符串表示。也支持缩写（前7个字符）。如果指定并且 `create_pr` 为 `False`，则如果 `revision` 没有指向 `parent_commit`，提交将失败。如果指定并且 `create_pr` 为 `True`，将从 `parent_commit` 创建拉取请求。指定 `parent_commit` 可确保在提交更改之前仓库未更改，并且如果仓库同时更新 / 提交，则尤其有用。

+   `run_as_future` (`bool`, *可选*) — 是否在后台运行此方法。后台作业按顺序运行，不会阻塞主线程。传递 `run_as_future=True` 将返回一个 [Future](https://docs.python.org/3/library/concurrent.futures.html#future-objects) 对象。默认为 `False`。

返回

[CommitInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitInfo) 或 `Future`

包含有关新创建提交的信息的[CommitInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitInfo)实例（提交哈希、提交URL、PR URL、提交消息等）。如果传递了`run_as_future=True`，则返回一个Future对象，该对象在执行时将包含结果。

将本地文件（最多50 GB）上传到给定的存储库。上传通过HTTP post请求完成，不需要安装git或git-lfs。

引发以下错误：

+   [`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError) 如果HuggingFace API返回错误

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果某个参数值无效

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果无法找到要下载的存储库。这可能是因为它不存在，或者因为它设置为`private`并且您无权访问。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) 如果无法找到要下载的修订版本。

`upload_file` 假定存储库已经存在于Hub上。如果收到客户端错误404，请确保已经进行了身份验证，并且`repo_id`和`repo_type`已正确设置。如果存储库不存在，请先使用[create_repo()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_repo)创建它。

示例：

```py
>>> from huggingface_hub import upload_file

>>> with open("./local/filepath", "rb") as fobj:
...     upload_file(
...         path_or_fileobj=fileobj,
...         path_in_repo="remote/file/path.h5",
...         repo_id="username/my-dataset",
...         repo_type="dataset",
...         token="my_token",
...     )
"https://huggingface.co/datasets/username/my-dataset/blob/main/remote/file/path.h5"

>>> upload_file(
...     path_or_fileobj=".\\local\\file\\path",
...     path_in_repo="remote/file/path.h5",
...     repo_id="username/my-model",
...     token="my_token",
... )
"https://huggingface.co/username/my-model/blob/main/remote/file/path.h5"

>>> upload_file(
...     path_or_fileobj=".\\local\\file\\path",
...     path_in_repo="remote/file/path.h5",
...     repo_id="username/my-model",
...     token="my_token",
...     create_pr=True,
... )
"https://huggingface.co/username/my-model/blob/refs%2Fpr%2F1/remote/file/path.h5"
```

#### `upload_folder`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L4369)

```py
( repo_id: str folder_path: Union[str, Path] path_in_repo: Optional[str] = None commit_message: Optional[str] = None commit_description: Optional[str] = None token: Optional[str] = None repo_type: Optional[str] = None revision: Optional[str] = None create_pr: Optional[bool] = None parent_commit: Optional[str] = None allow_patterns: Optional[Union[List[str], str]] = None ignore_patterns: Optional[Union[List[str], str]] = None delete_patterns: Optional[Union[List[str], str]] = None multi_commits: bool = False multi_commits_verbose: bool = False run_as_future: bool = False ) → export const metadata = 'undefined';CommitInfo or Future
```

参数

+   `repo_id` (`str`) — 将上传文件的存储库，例如：`"username/custom_transformers"`

+   `folder_path` (`str` or `Path`) — 要上传到本地文件系统上的文件夹路径

+   `path_in_repo` (`str`, *optional*) — 存储库中目录的相对路径，例如：`"checkpoints/1fec34a/results"`。将默认为存储库的根文件夹。

+   `token` (`str`, *optional*) — 认证令牌，使用`HfApi.login`方法获取。将默认为存储的令牌。

+   `repo_type` (`str`, *optional*) — 如果上传到数据集或空间，则设置为`"dataset"`或`"space"`，如果上传到模型，则为`None`或`"model"`。默认为`None`。

+   `revision` (`str`, *optional*) — 要提交的git修订版本。默认为`"main"`分支的头部。

+   `commit_message` (`str`, *optional*) — 生成提交的摘要/标题/第一行。默认为：`f"Upload {path_in_repo} with huggingface_hub"`

+   `commit_description` (`str` *optional*) — 生成的提交描述

+   `create_pr` (`boolean`, *optional*) — 是否创建具有该提交的拉取请求。默认为`False`。如果未设置`revision`，则PR将针对`"main"`分支打开。如果设置了`revision`并且是一个分支，则PR将针对此分支打开。如果设置了`revision`并且不是分支名称（例如：提交oid），服务器将返回`RevisionNotFoundError`。如果`multi_commits`和`create_pr`都为True，则在多次提交过程中创建的PR将保持打开状态。

+   `parent_commit` (`str`, *optional*) — 父提交的OID / SHA，以十六进制字符串表示。也支持缩写（前7个字符）。如果指定并且`create_pr`为`False`，则如果`revision`不指向`parent_commit`，提交将失败。如果指定并且`create_pr`为`True`，将从`parent_commit`创建拉取请求。指定`parent_commit`可确保在提交更改之前存储库未更改，并且如果存储库同时更新/提交，这可能特别有用。

+   `allow_patterns` (`List[str]` or `str`, *optional*) — 如果提供，只有至少匹配一个模式的文件才会被上传。

+   `ignore_patterns` (`List[str]` or `str`, *optional*) — 如果提供，与任何模式匹配的文件不会被上传。

+   `delete_patterns`（`List[str]`或`str`，*可选*）— 如果提供，与任何模式匹配的远程文件将在提交新文件时从存储库中删除。如果您不知道已经上传了哪些文件，这将很有用。注意：为了避免不一致，即使匹配模式，也不会删除`.gitattributes`文件。

+   `multi_commits`（`bool`）— 如果为True，则使用多次提交过程将更改推送到PR。默认为`False`。

+   `multi_commits_verbose`（`bool`）— 如果为True并且使用了`multi_commits`，将向用户显示更多信息。

+   `run_as_future`（`bool`，*可选*）— 是否在后台运行此方法。后台作业按顺序运行，不会阻塞主线程。传递`run_as_future=True`将返回一个[Future](https://docs.python.org/3/library/concurrent.futures.html#future-objects)对象。默认为`False`。

返回

[CommitInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitInfo)或`Future`

包含有关新创建提交的信息（提交哈希、提交URL、PR URL、提交消息等）的[CommitInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitInfo)实例。如果传递了`run_as_future=True`，则返回一个Future对象，该对象在执行时将包含结果。`str`或`Future`：如果`multi_commits=True`，则返回创建用于推送更改的PR的URL。如果传递了`run_as_future=True`，则返回一个Future对象，该对象在执行时将包含结果。

将本地文件夹上传到给定的存储库。上传通过HTTP请求完成，不需要安装git或git-lfs。

文件夹的结构将被保留。已经存在于存储库中的同名文件将被覆盖。其他文件将保持不变。

使用`allow_patterns`和`ignore_patterns`参数指定要上传的文件。这些参数接受单个模式或模式列表。模式是标准通配符（globbing patterns），如[此处](https://tldp.org/LDP/GNU-Linux-Tools-Summary/html/x11655.htm)所述。如果同时提供`allow_patterns`和`ignore_patterns`，则两个约束都适用。默认情况下，将上传文件夹中的所有文件。

使用`delete_patterns`参数指定要删除的远程文件。输入类型与`allow_patterns`相同（见上文）。如果还提供了`path_in_repo`，则模式将与相对于此文件夹的路径匹配。例如，`upload_folder(..., path_in_repo="experiment", delete_patterns="logs/*")`将删除`./experiment/logs/`下的任何远程文件。请注意，即使匹配模式，也不会删除`.gitattributes`文件。

任何子目录中存在的任何`.git/`文件夹将被忽略。但是，请注意，`.gitignore`文件不会被考虑在内。

在内部使用`HfApi.create_commit`。

引发以下错误：

+   [`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError)如果HuggingFace API返回错误

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError)如果某个参数值无效

`upload_folder`假定存储库已经存在于Hub上。如果收到客户端错误404，请确保已经进行了身份验证，并且`repo_id`和`repo_type`已正确设置。如果存储库不存在，请先使用[create_repo()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_repo)创建它。

`multi_commits`是实验性的。其API和行为可能会在未来发生变化，而不事先通知。

示例：

```py
# Upload checkpoints folder except the log files
>>> upload_folder(
...     folder_path="local/checkpoints",
...     path_in_repo="remote/experiment/checkpoints",
...     repo_id="username/my-dataset",
...     repo_type="datasets",
...     token="my_token",
...     ignore_patterns="**/logs/*.txt",
... )
# "https://huggingface.co/datasets/username/my-dataset/tree/main/remote/experiment/checkpoints"

# Upload checkpoints folder including logs while deleting existing logs from the repo
# Useful if you don't know exactly which log files have already being pushed
>>> upload_folder(
...     folder_path="local/checkpoints",
...     path_in_repo="remote/experiment/checkpoints",
...     repo_id="username/my-dataset",
...     repo_type="datasets",
...     token="my_token",
...     delete_patterns="**/logs/*.txt",
... )
"https://huggingface.co/datasets/username/my-dataset/tree/main/remote/experiment/checkpoints"

# Upload checkpoints folder while creating a PR
>>> upload_folder(
...     folder_path="local/checkpoints",
...     path_in_repo="remote/experiment/checkpoints",
...     repo_id="username/my-dataset",
...     repo_type="datasets",
...     token="my_token",
...     create_pr=True,
... )
"https://huggingface.co/datasets/username/my-dataset/tree/refs%2Fpr%2F1/remote/experiment/checkpoints"

```

#### `whoami`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1291)

```py
( token: Optional[str] = None )
```

参数

+   `token`（`str`，*可选*）— Hugging Face令牌。如果未提供，将默认使用本地保存的令牌。

调用HF API以了解“whoami”。

#### `huggingface_hub.plan_multi_commits`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_multi_commits.py#L77)

```py
( operations: Iterable max_operations_per_commit: int = 50 max_upload_size_per_commit: int = 2147483648 ) → export const metadata = 'undefined';Tuple[List[List[CommitOperationAdd]], List[List[CommitOperationDelete]]]
```

参数

+   `operations` (`List` of `CommitOperation()`) — 要拆分为提交的操作列表。

+   `max_operations_per_commit` (`int`) — 单个提交中的最大操作数。默认为50。

+   `max_upload_size_per_commit` (`int`) — 单个提交中要上传的最大大小（以字节为单位）。默认为2GB。超过此限制的文件将以每个提交1个的方式上传。

返回

`Tuple[List[List[CommitOperationAdd]], List[List[CommitOperationDelete]]]`

一个元组。第一个项目是一个列表的列表，表示要推送的添加提交的[CommitOperationAdd](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitOperationAdd)。第二个项目是一个列表的列表，表示删除提交的[CommitOperationDelete](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitOperationDelete)。

将操作列表拆分为要执行的提交列表。

实现遵循一个次优化（但简单）的算法：

1.  删除操作按照每个提交最大`max_operations_per_commits`操作分组在一起。

1.  所有超过`max_upload_size_per_commit`的添加操作都将逐个提交。

1.  所有剩余的添加操作都将分组在一起，并在达到`max_operations_per_commit`或`max_upload_size_per_commit`限制时拆分。

我们不尝试优化拆分以获得最少数量的提交，因为这是一个NP难题（参见[装箱问题](https://en.wikipedia.org/wiki/Bin_packing_problem)）。对于我们的用例，使用次优化解决方案并不成问题，因此我们选择了一个易于解释的实现。

`plan_multi_commits`是实验性的。其API和行为可能会在未来发生变化，恕不另行通知。

示例：

```py
>>> from huggingface_hub import HfApi, plan_multi_commits
>>> addition_commits, deletion_commits = plan_multi_commits(
...     operations=[
...          CommitOperationAdd(...),
...          CommitOperationAdd(...),
...          CommitOperationDelete(...),
...          CommitOperationDelete(...),
...          CommitOperationAdd(...),
...     ],
... )
>>> HfApi().create_commits_on_pr(
...     repo_id="my-cool-model",
...     addition_commits=addition_commits,
...     deletion_commits=deletion_commits,
...     (...)
...     verbose=True,
... )
```

操作的初始顺序不能保证！所有删除操作将在添加操作之前执行。如果您不是多次更新同一文件，则没问题。

## API数据类

### AccessRequest

### `class huggingface_hub.hf_api.AccessRequest`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L342)

```py
( username: str fullname: str email: str timestamp: datetime status: Literal['pending', 'accepted', 'rejected'] fields: Optional[Dict[str, Any]] = None )
```

参数

+   `username` (`str`) — 请求访问的用户的用户名。

+   `fullname` (`str`) — 请求访问的用户的全名。

+   `email` (`str`) — 请求访问的用户的电子邮件。

+   `timestamp` (`datetime`) — 请求的时间戳。

+   `status` (`Literal["pending", "accepted", "rejected"]`) — 请求的状态。可以是`["pending", "accepted", "rejected"]`中的一个。

+   `fields` (`Dict[str, Any]`, *可选*) — 用户在门户表单中填写的附加字段。

包含有关用户访问请求信息的数据结构。

### CommitInfo

### `class huggingface_hub.CommitInfo`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L272)

```py
( *args commit_url: str _url: Optional[str] = None **kwargs )
```

参数

+   `commit_url` (`str`) — 提交的URL。

+   `commit_message` (`str`) — 创建的提交的摘要（第一行）。

+   `commit_description` (`str`) — 创建的提交的描述。可以为空。

+   `oid` (`str`) — 提交哈希ID。示例：`"91c54ad1727ee830252e457677f467be0bfd8a57"`。

+   `pr_url` (`str`, *可选*) — 如果有的话，创建的PR的URL。当传递`create_pr=True`时填充。

+   `pr_revision` (`str`, *可选*) — 如果有的话，创建的PR的修订版本。当传递`create_pr=True`时填充。示例：`"refs/pr/1"`。

+   `pr_num` (`int`, *可选*) — 如果有的话，创建的PR讨论的编号。当传递`create_pr=True`时填充。可以在[get_discussion_details()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.get_discussion_details)中作为`discussion_num`传递。示例：`1`。

+   `_url` (`str`, *可选*) — 用于`str`兼容性的旧URL。可以是Hub上上传文件的URL（如果由[upload_file()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.upload_file)返回），Hub上上传文件夹的URL（如果由[upload_folder()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.upload_folder)返回）或Hub上提交的URL（如果由[create_commit()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_commit)返回）。默认为`commit_url`。使用此属性已被弃用。请改用`commit_url`。

包含有关新创建提交的信息的数据结构。

由在Hub上创建提交的任何方法返回：[create_commit()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_commit), [upload_file()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.upload_file), [upload_folder()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.upload_folder), [delete_file()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.delete_file), [delete_folder()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.delete_folder)。它继承自`str`以实现向后兼容，但使用特定于`str`的方法已被弃用。

### DatasetInfo

### `class huggingface_hub.hf_api.DatasetInfo`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L697)

```py
( **kwargs )
```

参数

+   `id` (`str`) — 数据集的ID。

+   `author` (`str`) — 数据集的作者。

+   `sha` (`str`) — 此特定修订版的仓库SHA。

+   `created_at` (`datetime`, *可选*) — 仓库在Hub上创建的日期。请注意，最低值为`2022-03-02T23:29:04.000Z`，对应于我们开始存储创建日期的日期。

+   `last_modified` (`datetime`, *可选*) — 对仓库的最后一次提交日期。

+   `private` (`bool`) — 仓库是否为私有。

+   `disabled` (`bool`, *可选*) — 仓库是否已禁用。

+   `gated` (`Literal["auto", "manual", False]`, *可选*) — 仓库是否受限。如果是，是否有手动或自动批准。

+   `downloads` (`int`) — 数据集的下载次数。

+   `likes` (`int`) — 数据集的点赞数。

+   `tags` (`List[str]`) — 数据集的标签列表。

+   `card_data` (`DatasetCardData`, *可选*) — 模型卡片元数据，作为[huggingface_hub.repocard_data.DatasetCardData](/docs/huggingface_hub/v0.20.3/en/package_reference/cards#huggingface_hub.DatasetCardData)对象。

+   `siblings` (`List[RepoSibling]`) — 构成数据集的[huggingface_hub.hf_api.RepoSibling](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.RepoSibling)对象列表。

包含有关Hub上数据集的信息。

此类的大多数属性都是可选的。这是因为Hub返回的数据取决于所做的查询。一般来说，查询越具体，返回的信息就越多。相反，当使用[list_datasets()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_datasets)列出数据集时，只返回属性的子集。

### GitRefInfo

### `class huggingface_hub.GitRefInfo`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1055)

```py
( name: str ref: str target_commit: str )
```

参数

+   `name` (`str`) — 引用的名称（例如标签名称或分支名称）。

+   `ref` (`str`) — Hub上的完整git引用（例如 `"refs/heads/main"` 或 `"refs/tags/v1.0"`）。

+   `target_commit` (`str`) — 引用的目标提交的OID（例如 `"e7da7f221d5bf496a48136c0cd264e630fe9fcc8"`）

包含有关Hub上仓库的git引用的信息。

### GitCommitInfo

### `class huggingface_hub.GitCommitInfo`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1100)

```py
( commit_id: str authors: List[str] created_at: datetime title: str message: str formatted_title: Optional[str] formatted_message: Optional[str] )
```

参数

+   `commit_id` (`str`) — 提交的OID（例如`"e7da7f221d5bf496a48136c0cd264e630fe9fcc8"`）

+   `authors` (`List[str]`) — 提交的作者列表。

+   `created_at` (`datetime`) — 提交创建的日期时间。

+   `title` (`str`) — 提交的标题。这是作者输入的自由文本值。

+   `message` (`str`) — 提交的描述。这是作者输入的自由文本值。

+   `formatted_title` (`str`) — 提交的标题格式化为HTML。仅在设置`formatted=True`时返回。

+   `formatted_message` (`str`) — 提交的描述格式化为HTML。仅在设置`formatted=True`时返回。

包含有关Hub上存储库的git提交的信息。查看[list_repo_commits()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_repo_commits)获取更多详细信息。

### GitRefs

### `class huggingface_hub.GitRefs`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1074)

```py
( branches: List[GitRefInfo] converts: List[GitRefInfo] tags: List[GitRefInfo] pull_requests: Optional[List[GitRefInfo]] )
```

参数

+   `branches` (`List[GitRefInfo]`) — 包含有关存储库上分支的信息的[GitRefInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.GitRefInfo)列表。

+   `converts` (`List[GitRefInfo]`) — 包含有关存储库上“convert”引用的信息的[GitRefInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.GitRefInfo)列表。转换是用于在数据集存储库中推送预处理数据的引用（内部使用）。

+   `tags` (`List[GitRefInfo]`) — 包含有关存储库上标签的信息的[GitRefInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.GitRefInfo)列表。

+   `pull_requests` (`List[GitRefInfo]`, *optional*) — 包含有关存储库上拉取请求的信息的[GitRefInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.GitRefInfo)列表。仅在设置`include_prs=True`时返回。

包含有关Hub上存储库的所有git引用的信息。

对象由[list_repo_refs()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_repo_refs)返回。

### ModelInfo

### `class huggingface_hub.hf_api.ModelInfo`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L551)

```py
( **kwargs )
```

参数

+   `id` (`str`) — 模型的ID。

+   `author` (`str`, *optional*) — 模型的作者。

+   `sha` (`str`, *optional*) — 此特定修订版的存储库SHA。

+   `created_at` (`datetime`, *optional*) — 存储库在Hub上创建的日期。请注意，最低值为`2022-03-02T23:29:04.000Z`，对应于我们开始存储创建日期的日期。

+   `last_modified` (`datetime`, *optional*) — 存储库上最后一次提交的日期。

+   `private` (`bool`) — 存储库是否为私有。

+   `disabled` (`bool`, *optional*) — 存储库是否已禁用。

+   `gated` (`Literal["auto", "manual", False]`, *optional*) — 存储库是否受限制。如果是，是手动还是自动批准。

+   `downloads` (`int`) — 模型的下载次数。

+   `likes` (`int`) — 模型的点赞数。

+   `library_name` (`str`, *optional*) — 与模型关联的库。

+   `tags` (`List[str]`) — 模型的标签列表。与`card_data.tags`相比，包含Hub计算的额外标签（例如支持的库，模型的arXiv）。

+   `pipeline_tag` (`str`, *optional*) — 与模型关联的管道标记。

+   `mask_token` (`str`, *optional*) — 模型使用的掩码标记。

+   `widget_data` (`Any`, *optional*) — 与模型关联的小部件数据。

+   `model_index` (`Dict`, *optional*) — 评估模型的模型索引。

+   `config` (`Dict`, *optional*) — 模型配置。

+   `transformers_info` (`TransformersInfo`, *optional*) — 与模型关联的Transformers特定信息（自动类，处理器等）。

+   `card_data` (`ModelCardData`, *optional*) — 模型卡片元数据，作为[huggingface_hub.repocard_data.ModelCardData](/docs/huggingface_hub/v0.20.3/en/package_reference/cards#huggingface_hub.ModelCardData)对象。

+   `siblings` (`List[RepoSibling]`) — 构成模型的[huggingface_hub.hf_api.RepoSibling](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.RepoSibling)对象列表。

+   `spaces` (`List[str]`, *可选*) — 使用该模型的空间列表。

+   `safetensors` (`SafeTensorsInfo`, *可选*) — 模型的safetensors信息。

包含有关Hub上模型的信息。

这个类的大多数属性都是可选的。这是因为Hub返回的数据取决于所做的查询。一般来说，查询越具体，返回的信息就越多。相反，当使用[list_models()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_models)列出模型时，只返回部分属性。

### RepoSibling

### `class huggingface_hub.hf_api.RepoSibling`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L431)

```py
( rfilename: str size: Optional[int] = None blob_id: Optional[str] = None lfs: Optional[BlobLfsInfo] = None )
```

参数

+   `rfilename` (str) — 相对于仓库根目录的文件名。

+   `size` (`int`, *可选*) — 文件的大小，以字节为单位。当[repo_info()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.repo_info)的`files_metadata`参数设置为`True`时，定义此属性。否则为`None`。

+   `blob_id` (`str`, *可选*) — 文件的git OID。当[repo_info()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.repo_info)的`files_metadata`参数设置为`True`时，定义此属性。否则为`None`。

+   `lfs` (`BlobLfsInfo`, *可选*) — 文件的LFS元数据。当[repo_info()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.repo_info)的`files_metadata`参数设置为`True`且文件存储在Git LFS中时，定义此属性。否则为`None`。

包含有关Hub上仓库内的一个仓库文件的基本信息。

这个类的所有属性都是可选的，除了`rfilename`。这是因为在Hub上列出仓库时（使用[list_models()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_models)、[list_datasets()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_datasets)或[list_spaces()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_spaces)），只返回文件名。如果需要更多信息，如文件大小、blob id或lfs详细信息，则必须从一个仓库中单独请求它们（使用[model_info()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.model_info)、[dataset_info()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.dataset_info)或[space_info()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.space_info)），因为这会增加后端服务器检索这些信息的约束。

### RepoFile

### `class huggingface_hub.hf_api.RepoFile`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L466)

```py
( **kwargs )
```

参数

+   `path` (str) — 相对于仓库根目录的文件路径。

+   `size` (`int`) — 文件的大小，以字节为单位。

+   `blob_id` (`str`) — 文件的git OID。

+   `lfs` (`BlobLfsInfo`) — 文件的LFS元数据。

+   `last_commit` (`LastCommitInfo`, *可选*) — 文件的最后提交元数据。仅当使用`expand=True`调用[list_files_info()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_files_info)、[list_repo_tree()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_repo_tree)和[get_paths_info()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.get_paths_info)时才定义。

+   `security` (`BlobSecurityInfo`, *可选*) — 文件的安全扫描元数据。仅在调用 [list_files_info()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_files_info)、[list_repo_tree()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_repo_tree) 和 [get_paths_info()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.get_paths_info) 时定义为 `expand=True`。

包含有关 Hub 上文件的信息。

### RepoUrl

### `class huggingface_hub.RepoUrl`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L371)

```py
( url: Any endpoint: Optional[str] = None )
```

参数

+   `url` (`Any`) — repo URL 的字符串值。

+   `endpoint` (`str`, *可选*) — Hub 的端点。默认为 [https://huggingface.co](https://huggingface.co)。

引发

+   +   — [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果无法解析 URL。

+   +   — [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果 `repo_type` 是未知的。

描述 Hub 上的 repo URL 的 `str` 的子类。

`RepoUrl` 是由 `HfApi.create_repo` 返回的。它继承自 `str` 以实现向后兼容。在初始化时，URL 被解析以填充属性：

+   endpoint (`str`)

+   namespace (`Optional[str]`)

+   repo_name (`str`)

+   repo_id (`str`)

+   repo_type (`Literal["model", "dataset", "space"]`)

+   url (`str`)

示例：

```py
>>> RepoUrl('https://huggingface.co/gpt2')
RepoUrl('https://huggingface.co/gpt2', endpoint='https://huggingface.co', repo_type='model', repo_id='gpt2')

>>> RepoUrl('https://hub-ci.huggingface.co/datasets/dummy_user/dummy_dataset', endpoint='https://hub-ci.huggingface.co')
RepoUrl('https://hub-ci.huggingface.co/datasets/dummy_user/dummy_dataset', endpoint='https://hub-ci.huggingface.co', repo_type='dataset', repo_id='dummy_user/dummy_dataset')

>>> RepoUrl('hf://datasets/my-user/my-dataset')
RepoUrl('hf://datasets/my-user/my-dataset', endpoint='https://huggingface.co', repo_type='dataset', repo_id='user/dataset')

>>> HfApi.create_repo("dummy_model")
RepoUrl('https://huggingface.co/Wauplin/dummy_model', endpoint='https://huggingface.co', repo_type='model', repo_id='Wauplin/dummy_model')
```

### SafetensorsRepoMetadata

### `class huggingface_hub.utils.SafetensorsRepoMetadata`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_safetensors.py#L86)

```py
( metadata: Optional sharded: bool weight_map: Dict files_metadata: Dict )
```

参数

+   `metadata` (`Dict`, *可选*) — 如果存在，则包含在 ‘model.safetensors.index.json’ 文件中的元数据。仅对分片模型填充。

+   `sharded` (`bool`) — repo 是否包含分片模型。

+   `weight_map` (`Dict[str, str]`) — 所有权重的映射。键是张量名称，值是包含张量的文件的文件名。

+   `files_metadata` (`Dict[str, SafetensorsFileMetadata]`) — 所有文件元数据的映射。键是文件名，值是相应文件的元数据，作为 `SafetensorsFileMetadata` 对象。

+   `parameter_count` (`Dict[str, int]`) — 每种数据类型的参数数量的映射。键是数据类型，值是该数据类型的参数数量。

Safetensors repo 的元数据。

如果 repo 在其根目录包含 ‘model.safetensors’ 权重文件（非共享模型）或 ‘model.safetensors.index.json’ 索引文件（分片模型），则认为该 repo 是 Safetensors repo。

此类由 [get_safetensors_metadata()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.get_safetensors_metadata) 返回。

有关 safetensors 格式的更多详细信息，请查看 [https://huggingface.co/docs/safetensors/index#format](https://huggingface.co/docs/safetensors/index#format)。

### SafetensorsFileMetadata

### `class huggingface_hub.utils.SafetensorsFileMetadata`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_safetensors.py#L56)

```py
( metadata: Dict tensors: Dict )
```

参数

+   `metadata` (`Dict`) — 文件中包含的元数据。

+   `tensors` (`Dict[str, TensorInfo]`) — 所有张量的映射。键是张量名称，值是关于相应张量的信息，作为 `TensorInfo` 对象。

+   `parameter_count` (`Dict[str, int]`) — 每种数据类型的参数数量的映射。键是数据类型，值是该数据类型的参数数量。

Hub 上托管的 Safetensors 文件的元数据。

此类由 [parse_safetensors_file_metadata()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.parse_safetensors_file_metadata) 返回。

有关 safetensors 格式的更多详细信息，请查看 [https://huggingface.co/docs/safetensors/index#format](https://huggingface.co/docs/safetensors/index#format)。

### SpaceInfo

### `class huggingface_hub.hf_api.SpaceInfo`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L804)

```py
( **kwargs )
```

参数

+   `id` (`str`) — 空间的ID。

+   `author` (`str`, *可选*) — 空间的作者。

+   `sha` (`str`, *可选*) — 此特定修订版的仓库SHA。

+   `created_at` (`datetime`, *可选*) — 在Hub上创建仓库的日期。请注意，最低值为`2022-03-02T23:29:04.000Z`，对应于我们开始存储创建日期的日期。

+   `last_modified` (`datetime`, *可选*) — 仓库的最后提交日期。

+   `private` (`bool`) — 仓库是否私有。

+   `gated` (`Literal["auto", "manual", False]`, *可选*) — 仓库是否受限。如果是，是手动还是自动批准。

+   `disabled` (`bool`, *可选*) — 空间是否已禁用。

+   `host` (`str`, *可选*) — 空间的主机URL。

+   `subdomain` (`str`, *可选*) — 空间的子域。

+   `likes` (`int`) — 空间的喜欢数量。

+   `tags` (`List[str]`) — 空间的标签列表。

+   `siblings` (`List[RepoSibling]`) — 构成空间的[huggingface_hub.hf_api.RepoSibling](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.hf_api.RepoSibling)对象列表。

+   `card_data` (`SpaceCardData`, *可选*) — 空间卡片元数据，作为[huggingface_hub.repocard_data.SpaceCardData](/docs/huggingface_hub/v0.20.3/en/package_reference/cards#huggingface_hub.SpaceCardData)对象。

+   `runtime` (`SpaceRuntime`, *可选*) — 空间运行时信息，作为[huggingface_hub.hf_api.SpaceRuntime](/docs/huggingface_hub/v0.20.3/en/package_reference/space_runtime#huggingface_hub.SpaceRuntime)对象。

+   `sdk` (`str`, *可选*) — 空间使用的SDK。

+   `models` (`List[str]`, *可选*) — 空间使用的模型列表。

+   `datasets` (`List[str]`, *可选*) — 空间使用的数据集列表。

包含有关Hub上空间的信息。

此类的大多数属性都是可选的。这是因为Hub返回的数据取决于所做的查询。一般来说，查询越具体，返回的信息就越多。相反，当使用[list_spaces()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_spaces)列出空间时，只返回部分属性。

### TensorInfo

### `class huggingface_hub.utils.TensorInfo`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_safetensors.py#L26)

```py
( dtype: Literal shape: List data_offsets: Tuple )
```

参数

+   `dtype` (`str`) — 张量的数据类型（“F64”，“F32”，“F16”，“BF16”，“I64”，“I32”，“I16”，“I8”，“U8”，“BOOL”）。

+   `shape` (`List[int]`) — 张量的形状。

+   `data_offsets` (`Tuple[int, int]`) — 文件中数据的偏移量，表示为元组`[BEGIN, END]`。

+   `parameter_count` (`int`) — 张量中的参数数量。

有关张量的信息。

有关safetensors格式的更多详细信息，请查看[https://huggingface.co/docs/safetensors/index#format](https://huggingface.co/docs/safetensors/index#format)。

### 用户

### `class huggingface_hub.User`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1161)

```py
( avatar_url: str username: str fullname: str )
```

参数

+   `avatar_url` (`str`) — 用户头像的URL。

+   `username` (`str`) — Hub上用户的名称（唯一）。

+   `fullname` (`str`) — 用户的全名。

包含有关Hub上空间的信息。

### UserLikes

### `class huggingface_hub.UserLikes`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L1133)

```py
( user: str total: int datasets: List[str] models: List[str] spaces: List[str] )
```

参数

+   `user` (`str`) — 获取喜欢的用户的名称。

+   `total` (`int`) — 喜欢的总数。

+   `datasets` (`List[str]`) — 用户喜欢的数据集列表（作为repo_ids）。

+   `models` (`List[str]`) — 用户喜欢的模型列表（作为repo_ids）。

+   `spaces` (`List[str]`) — 用户喜欢的空间列表（作为repo_ids）。

包含有关Hub上用户喜欢的信息。

## CommitOperation

以下是`CommitOperation()`支持的值：

### `class huggingface_hub.CommitOperationAdd`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_commit_api.py#L110)

```py
( path_in_repo: str path_or_fileobj: Union )
```

参数

+   `path_in_repo`（`str`）—存储库中的相对文件路径，例如：`"checkpoints/1fec34a/weights.bin"`

+   `path_or_fileobj`（`str`，`Path`，`bytes`或`BinaryIO`）—要么：

    +   要上传的本地文件的路径（作为`str`或`pathlib.Path`）

    +   一个包含要上传文件内容的字节缓冲区（`bytes`）

    +   一个“文件对象”（`io.BufferedIOBase`的子类），通常使用`open(path, "rb")`获得。它必须支持`seek()`和`tell()`方法。

引发

`ValueError`

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) —如果`path_or_fileobj`不是`str`，`Path`，`bytes`或`io.BufferedIOBase`之一。

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) —如果`path_or_fileobj`是`str`或`Path`，但不是指向现有文件的路径。

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) —如果`path_or_fileobj`是`io.BufferedIOBase`，但它不支持`seek()`和`tell()`两种方法。

保存在Hub上的存储库中上传文件所需信息的数据结构。

#### `as_file`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_commit_api.py#L189)

```py
( with_tqdm: bool = False )
```

参数

+   `with_tqdm`（`bool`，*可选*，默认为`False`）—如果为True，则在文件对象上迭代将显示进度条。仅在文件类似对象是文件路径时有效。纯字节和缓冲区不受支持。

一个上下文管理器，产生一个类似文件的对象，允许读取`path_or_fileobj`背后的基础数据。

示例：

```py
>>> operation = CommitOperationAdd(
...        path_in_repo="remote/dir/weights.h5",
...        path_or_fileobj="./local/weights.h5",
... )
CommitOperationAdd(path_in_repo='remote/dir/weights.h5', path_or_fileobj='./local/weights.h5')

>>> with operation.as_file() as file:
...     content = file.read()

>>> with operation.as_file(with_tqdm=True) as file:
...     while True:
...         data = file.read(1024)
...         if not data:
...              break
config.json: 100%|█████████████████████████| 8.19k/8.19k [00:02<00:00, 3.72kB/s]

>>> with operation.as_file(with_tqdm=True) as file:
...     requests.put(..., data=file)
config.json: 100%|█████████████████████████| 8.19k/8.19k [00:02<00:00, 3.72kB/s]
```

#### `b64content`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_commit_api.py#L239)

```py
( )
```

`path_or_fileobj`的base64编码内容

返回：`bytes`

### `class huggingface_hub.CommitOperationDelete`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_commit_api.py#L49)

```py
( path_in_repo: str is_folder: Union = 'auto' )
```

参数

+   `path_in_repo`（`str`）—存储库中的相对文件路径，例如：对于文件为`"checkpoints/1fec34a/weights.bin"`，对于文件夹为`"checkpoints/1fec34a/"`。

+   `is_folder`（`bool`或`Literal["auto"]`，*可选*）—删除操作是否适用于文件夹。如果为“auto”，则通过查看路径是否以“/”（文件夹）结尾来自动猜测路径类型（文件或文件夹）。要明确设置路径类型，可以设置`is_folder=True`或`is_folder=False`。

保存从Hub上的存储库中删除文件或文件夹所需信息的数据结构。

### `class huggingface_hub.CommitOperationCopy`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_commit_api.py#L80)

```py
( src_path_in_repo: str path_in_repo: str src_revision: Optional = None )
```

参数

+   `src_path_in_repo`（`str`）—要复制的文件在存储库中的相对文件路径，例如`"checkpoints/1fec34a/weights.bin"`。

+   `path_in_repo`（`str`）—要复制文件的存储库中的相对文件路径，例如`"checkpoints/1fec34a/weights_copy.bin"`。

+   `src_revision`（`str`，*可选*）—要复制的文件的git修订版。可以是任何有效的git修订版。默认为目标提交修订版。

保存在Hub上的存储库中复制文件所需信息的数据结构。

限制：

+   只有LFS文件才能被复制。要复制常规文件，您需要将其下载到本地，然后重新上传

+   不支持跨存储库复制。

注意：您可以将[CommitOperationCopy](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitOperationCopy)和[CommitOperationDelete](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitOperationDelete)组合在一起，以在Hub上重命名LFS文件。

## CommitScheduler

### `class huggingface_hub.CommitScheduler`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_commit_scheduler.py#L29)

```py
( repo_id: str folder_path: Union every: Union = 5 path_in_repo: Optional = None repo_type: Optional = None revision: Optional = None private: bool = False token: Optional = None allow_patterns: Union = None ignore_patterns: Union = None squash_history: bool = False hf_api: Optional = None )
```

参数

+   `repo_id`（`str`）—要提交的存储库的id。

+   `folder_path` (`str` 或 `Path`) — 要定期上传的本地文件夹的路径。

+   `every` (`int` 或 `float`, *可选*) — 每次提交之间的分钟数。默认为5分钟。

+   `path_in_repo` (`str`, *可选*) — 存储库中目录的相对路径，例如：`"checkpoints/"`。默认为存储库的根文件夹。

+   `repo_type` (`str`, *可选*) — 要提交的存储库类型。默认为 `model`。

+   `revision` (`str`, *可选*) — 要提交的存储库的修订版本。默认为 `main`。

+   `private` (`bool`, *可选*) — 是否将存储库设为私有。默认为 `False`。如果存储库已存在，则此值将被忽略。

+   `token` (`str`, *可选*) — 用于提交到存储库的令牌。默认为机器上保存的令牌。

+   `allow_patterns` (`List[str]` 或 `str`, *可选*) — 如果提供，只有至少匹配一个模式的文件才会被上传。

+   `ignore_patterns` (`List[str]` 或 `str`, *可选*) — 如果提供，匹配任何模式的文件将不会被上传。

+   `squash_history` (`bool`, *可选*) — 每次提交后是否压缩存储库的历史记录。默认为 `False`。在存储库变得过大时，压缩提交对避免性能下降很有用。

+   `hf_api` (`HfApi`, *可选*) — 用于提交到Hub的[HfApi](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi)客户端。可以使用自定义设置（用户代理，令牌等）进行设置。

调度器以固定时间间隔（例如每5分钟）将本地文件夹上传到Hub。

调度器在实例化时启动并无限期运行。在脚本结束时，会触发最后一次提交。查看[上传指南](https://huggingface.co/docs/huggingface_hub/guides/upload#scheduled-uploads)以了解如何使用它。

示例：

```py
>>> from pathlib import Path
>>> from huggingface_hub import CommitScheduler

# Scheduler uploads every 10 minutes
>>> csv_path = Path("watched_folder/data.csv")
>>> CommitScheduler(repo_id="test_scheduler", repo_type="dataset", folder_path=csv_path.parent, every=10)

>>> with csv_path.open("a") as f:
...     f.write("first line")

# Some time later (...)
>>> with csv_path.open("a") as f:
...     f.write("second line")
```

#### `push_to_hub`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_commit_scheduler.py#L178)

```py
( )
```

将文件夹推送到Hub并返回提交信息。

此方法不应直接调用。它由调度器在后台运行，遵循队列机制以避免并发提交。直接调用该方法可能导致并发问题。

`push_to_hub`的默认行为是假定为追加文件夹。它列出文件夹中的所有文件并仅上传更改的文件。如果未发现更改，则该方法返回而不进行提交。如果要更改此行为，可以继承自[CommitScheduler](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.CommitScheduler)并覆盖此方法。例如，在提交之前将数据压缩到单个文件中可能很有用。有关更多详细信息和示例，请查看我们的[集成指南](https://huggingface.co/docs/huggingface_hub/main/en/guides/upload#scheduled-uploads)。

#### `stop`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_commit_scheduler.py#L140)

```py
( )
```

停止调度器。

停止的调度器无法重新启动。主要用于测试目的。

#### `trigger`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_commit_scheduler.py#L155)

```py
( )
```

触发`push_to_hub`并返回一个未来。

此方法每隔`every`分钟自动调用一次。您也可以手动调用它以立即触发提交，而无需等待下一个计划的提交。

## 搜索助手

一些用于在Hub上过滤存储库的助手可在`huggingface_hub`包中找到。

### 数据集过滤器

### `class huggingface_hub.DatasetFilter`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/endpoint_helpers.py#L69)

```py
( author: Optional = None benchmark: Union = None dataset_name: Optional = None language_creators: Union = None language: Union = None multilinguality: Union = None size_categories: Union = None task_categories: Union = None task_ids: Union = None )
```

参数

+   `author` (`str`, *可选*) — 用于通过原始上传者（作者或组织）在Hub上识别数据集的字符串或字符串列表，例如 `facebook` 或 `huggingface`。

+   `benchmark` (`str` 或 `List`, *可选*) — 一个字符串或字符串列表，可用于通过官方基准来识别Hub上的数据集。

+   `dataset_name` (`str`, *可选*) — 一个字符串或字符串列表，可用于通过名称来识别Hub上的数据集，例如 `SQAC` 或 `wikineural`。

+   `language_creators` (`str` 或 `List`, *可选*) — 一个字符串或字符串列表，可用于通过数据的策划方式来识别Hub上的数据集，例如 `crowdsourced` 或 `machine_generated`。

+   `language` (`str` 或 `List`, *可选*) — 一个字符串或字符串列表，表示用于在Hub上过滤数据集的两字符语言。

+   `multilinguality` (`str` 或 `List`, *可选*) — 一个字符串或字符串列表，表示包含多种语言的数据集的过滤器。

+   `size_categories` (`str` 或 `List`, *可选*) — 一个字符串或字符串列表，可用于通过数据集的大小来识别Hub上的数据集，例如 `100K<n<1M` 或 `1M<n<10M`。

+   `task_categories` (`str` 或 `List`, *可选*) — 一个字符串或字符串列表，可用于通过设计任务来识别Hub上的数据集，例如 `audio_classification` 或 `named_entity_recognition`。

+   `task_ids` (`str` 或 `List`, *可选*) — 一个字符串或字符串列表，可用于通过特定任务来识别Hub上的数据集，例如 `speech_emotion_recognition` 或 `paraphrase`。

一个将人类可读的数据集搜索参数转换为与REST API兼容的参数的类。对于所有参数，大小写不重要。

示例：

```py
>>> from huggingface_hub import DatasetFilter

>>> # Using author
>>> new_filter = DatasetFilter(author="facebook")

>>> # Using benchmark
>>> new_filter = DatasetFilter(benchmark="raft")

>>> # Using dataset_name
>>> new_filter = DatasetFilter(dataset_name="wikineural")

>>> # Using language_creator
>>> new_filter = DatasetFilter(language_creator="crowdsourced")

>>> # Using language
>>> new_filter = DatasetFilter(language="en")

>>> # Using multilinguality
>>> new_filter = DatasetFilter(multilinguality="multilingual")

>>> # Using size_categories
>>> new_filter = DatasetFilter(size_categories="100K<n<1M")

>>> # Using task_categories
>>> new_filter = DatasetFilter(task_categories="audio_classification")

>>> # Using task_ids
>>> new_filter = DatasetFilter(task_ids="paraphrase")
```

### ModelFilter

### `class huggingface_hub.ModelFilter`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/endpoint_helpers.py#L155)

```py
( author: Optional = None library: Union = None language: Union = None model_name: Optional = None task: Union = None trained_dataset: Union = None tags: Union = None )
```

参数

+   `author` (`str`, *可选*) — 一个字符串，可用于通过原始上传者（作者或组织）来识别Hub上的模型，例如 `facebook` 或 `huggingface`。

+   `library` (`str` 或 `List`, *可选*) — 一个字符串或字符串列表，表示模型最初训练所使用的基础库，例如 pytorch、tensorflow 或 allennlp。

+   `language` (`str` 或 `List`, *可选*) — 一个字符串或字符串列表，表示语言，包括名称和国家代码，例如 “en” 或 “English”

+   `model_name` (`str`, *可选*) — 一个包含模型在Hub上完整或部分名称的字符串，例如 “bert” 或 “bert-base-cased”

+   `task` (`str` 或 `List`, *可选*) — 一个字符串或字符串列表，表示模型设计用于的任务，例如: “fill-mask” 或 “automatic-speech-recognition”

+   `tags` (`str` 或 `List`, *可选*) — 一个字符串标签或标签列表，用于通过标签来过滤Hub上的模型，例如 `text-generation` 或 `spacy`。

+   `trained_dataset` (`str` 或 `List`, *可选*) — 一个模型在Hub上训练数据集的字符串标签或字符串标签列表。

一个将人类可读的模型搜索参数转换为与REST API兼容的参数的类。对于所有参数，大小写不重要。

```py
>>> from huggingface_hub import ModelFilter

>>> # For the author_or_organization
>>> new_filter = ModelFilter(author_or_organization="facebook")

>>> # For the library
>>> new_filter = ModelFilter(library="pytorch")

>>> # For the language
>>> new_filter = ModelFilter(language="french")

>>> # For the model_name
>>> new_filter = ModelFilter(model_name="bert")

>>> # For the task
>>> new_filter = ModelFilter(task="text-classification")

>>> from huggingface_hub import HfApi

>>> api = HfApi()
# To list model tags

>>> new_filter = ModelFilter(tags="benchmark:raft")

>>> # Related to the dataset
>>> new_filter = ModelFilter(trained_dataset="common_voice")
```
