# 管理本地和在线存储库

> 原文链接：[`huggingface.co/docs/huggingface_hub/package_reference/repository`](https://huggingface.co/docs/huggingface_hub/package_reference/repository)

`Repository` 类是一个辅助类，包装了 `git` 和 `git-lfs` 命令。它提供了适用于管理可能非常庞大的存储库的工具。

每当涉及任何 `git` 操作或合作将成为重点时，这是推荐的工具。

## 存储库类

### `class huggingface_hub.Repository`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L425)

```py
( local_dir: Union clone_from: Optional = None repo_type: Optional = None token: Union = True git_user: Optional = None git_email: Optional = None revision: Optional = None skip_lfs_files: bool = False client: Optional = None )
```

包装 git 和 git-lfs 命令的辅助类。

旨在简化与 huggingface.co 托管的模型或数据集存储库的交互，尽管这里（如果有的话）实际上与 huggingface.co 没有太多特定的内容。

Repository 已被弃用，推荐使用 HfApi 中实现的基于 http 的替代方案。鉴于它在旧代码中的广泛应用，完全移除 Repository 将在版本 `v1.0` 中才会发生。更多详情，请阅读 [`huggingface.co/docs/huggingface_hub/concepts/git_vs_http`](https://huggingface.co/docs/huggingface_hub/concepts/git_vs_http)。

#### `__init__`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L445)

```py
( local_dir: Union clone_from: Optional = None repo_type: Optional = None token: Union = True git_user: Optional = None git_email: Optional = None revision: Optional = None skip_lfs_files: bool = False client: Optional = None )
```

参数

+   `local_dir` (`str` 或 `Path`) — 本地目录的路径（例如 `'my_trained_model/'`），在那里将初始化 `Repository`。

+   `clone_from` (`str`, *可选*) — 要么是存储库 URL，要么是 `repo_id`。示例：

    +   `"https://huggingface.co/philschmid/playground-tests"`

    +   `"philschmid/playground-tests"`

+   `repo_type` (`str`, *可选*) — 从 repo_id 克隆存储库时设置。默认为 model。

+   `token` (`bool` 或 `str`, *可选*) — 一个有效的身份验证令牌（参见 [`huggingface.co/settings/token`](https://huggingface.co/settings/token)）。如果为 `None` 或 `True` 并且机器已登录（通过 `huggingface-cli login` 或 login()），则令牌将从缓存中检索。如果为 `False`，则请求头中不会发送令牌。

+   `git_user` (`str`, *可选*) — 将覆盖用于提交和推送文件到中心的 `git config user.name`。

+   `git_email` (`str`, *可选*) — 将覆盖用于提交和推送文件到中心的 `git config user.email`。

+   `revision` (`str`, *可选*) — 初始化存储库后要检出的修订版。如果修订版不存在，则将从默认分支的当前 HEAD 创建一个具有该修订版名称的分支。

+   `skip_lfs_files` (`bool`, *可选*, 默认为 `False`) — 是否跳过 git-LFS 文件。

+   `client` (`HfApi`, *可选*) — 在调用 HF Hub API 时要使用的 HfApi 实例。如果将其留空，则将创建一个新实例。

引发

+   +   — 如果在 `clone_from` 中设置的远程存储库不存在，则引发 [`EnvironmentError`](https://docs.python.org/3/library/exceptions.html#EnvironmentError)。

实例化一个 git 存储库的本地克隆。

如果设置了 `clone_from`，则将从现有远程存储库克隆存储库。如果远程存储库不存在，则将引发 `EnvironmentError` 异常。请先使用 create_repo() 创建远程存储库。

`Repository` 默认使用本地 git 凭据。如果明确设置，则将使用 `token` 或 `git_user`/`git_email` 对。

#### `current_branch`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L562)

```py
( ) → export const metadata = 'undefined';str
```

返回

`str`

当前检出的分支。

返回当前检出的分支。

#### `add_tag`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1246)

```py
( tag_name: str message: Optional = None remote: Optional = None )
```

参数

+   `tag_name` (`str`) — 要添加的标签的名称。

+   `message` (`str`, *可选*) — 附带标签的消息。如果传递了消息，则标签将变为注释标签。

+   `remote` (`str`, *可选*) — 要添加标签的远程。

在当前头部添加标签并推送它

如果远程为 None，则只会在本地更新

如果未提供消息，则标签将是轻量级的。如果提供了消息，则标签将是注释的。

#### `auto_track_binary_files`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L888)

```py
( pattern: str = '.' ) → export const metadata = 'undefined';List[str]
```

参数

+   `pattern` (`str`, *可选*, 默认为”.“) — 用于跟踪二进制文件的模式。

返回

`List[str]`

由于是二进制文件而现在被跟踪的文件名列表

自动使用 git-lfs 跟踪二进制文件。

#### `auto_track_large_files`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L931)

```py
( pattern: str = '.' ) → export const metadata = 'undefined';List[str]
```

参数

+   `pattern` (`str`, *可选*, 默认为”.“) — 用于跟踪大小超过 10MB 的文件的模式。

返回

`List[str]`

由于其大小而现在被跟踪的文件名列表。

自动使用 git-lfs 跟踪大文件（大于 10MB 的文件）。

#### `check_git_versions`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L577)

```py
( )
```

引发

+   +   — [`EnvironmentError`](https://docs.python.org/3/library/exceptions.html#EnvironmentError) 如果未安装`git`或`git-lfs`。

检查`git`和`git-lfs`是否可以运行。

#### `clone_from`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L600)

```py
( repo_url: str token: Union = None )
```

参数

+   `repo_url` (`str`) — 从中克隆存储库的 URL

+   `token` (`Union[str, bool]`, *可选*) — 是否使用身份验证令牌。可以是：

    +   一个字符串，即令牌本身

    +   `False`，不使用身份验证令牌

    +   `True`，将从本地文件夹获取身份验证令牌并使用它（您必须登录才能使用）。

    +   `None`，这将检索`self.huggingface_token`的值。

从远程克隆。如果文件夹已经存在，将尝试在其中克隆存储库。

如果此文件夹是具有链接历史的 git 存储库，则将尝试更新存储库。

引发以下错误：

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 如果传递了组织令牌（以“api_org”开头）。必须使用您自己的个人访问令牌（请参阅[`hf.co/settings/tokens`](https://hf.co/settings/tokens)）。

+   [`EnvironmentError`](https://docs.python.org/3/library/exceptions.html#EnvironmentError) 如果您尝试在非空文件夹中克隆存储库，或者如果`git`操作引发错误。

#### `commit`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1330)

```py
( commit_message: str branch: Optional = None track_large_files: bool = True blocking: bool = True auto_lfs_prune: bool = False )
```

参数

+   `commit_message` (`str`) — 用于提交的消息。

+   `branch` (`str`, *可选*) — 提交将出现在的分支。在任何操作之前，将检出此分支。

+   `track_large_files` (`bool`, *可选*, 默认为`True`) — 是否自动跟踪大文件。默认情况下会这样做。

+   `blocking` (`bool`, *可选*, 默认为`True`) — 函数是否应该仅在`git push`完成时返回。

+   `auto_lfs_prune` (`bool`, 默认为`True`) — 是否在文件被推送到远程后自动修剪文件。

处理提交到存储库的上下文管理器实用程序。这将自动跟踪大文件（>10Mb）使用 git-lfs。如果要忽略该行为，请将 `track_large_files` 参数设置为 `False`。

示例：

```py
>>> with Repository(
...     "text-files",
...     clone_from="<user>/text-files",
...     token=True,
>>> ).commit("My first file :)"):
...     with open("file.txt", "w+") as f:
...         f.write(json.dumps({"hey": 8}))

>>> import torch

>>> model = torch.nn.Transformer()
>>> with Repository(
...     "torch-model",
...     clone_from="<user>/torch-model",
...     token=True,
>>> ).commit("My cool model :)"):
...     torch.save(model.state_dict(), "model.pt")
```

#### `delete_tag`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1209)

```py
( tag_name: str remote: Optional = None ) → export const metadata = 'undefined';bool
```

参数

+   `tag_name` (`str`) — 要删除的标签名称。

+   `remote` (`str`, *optional*) — 要删除标签的远程。

返回

`bool`

如果已删除，则返回 `True`，如果标签不存在，则返回 `False`。如果未传递远程，则仅在本地更新

删除标签，无论是本地还是远程，如果存在。

#### `git_add`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1006)

```py
( pattern: str = '.' auto_lfs_track: bool = False )
```

参数

+   `pattern` (`str`, *optional*, 默认为 ”.“) — 用于将文件添加到暂存区的模式。

+   `auto_lfs_track` (`bool`, *optional*, 默认为 `False`) — 是否自动跟踪大型和二进制文件使用 git-lfs。任何大小超过 10MB 或为二进制格式的文件将自动跟踪。

git add

将 `auto_lfs_track` 参数设置为 `True` 将自动跟踪大于 10MB 的文件使用 `git-lfs`。

`git_checkout`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1147)

```py
( revision: str create_branch_ok: bool = False )
```

参数

+   `revision` (`str`) — 要检出的修订版本。

+   `create_branch_ok` (`str`, *optional*, 默认为 `False`) — 如果允许创建以当前检出引用处传递的 `revision` 命名的分支，而 `revision` 不是现有的修订版本。

git checkout 给定的修订版本

将 `create_branch_ok` 指定为 `True` 将在当前检出的引用处创建到给定修订版本的分支（如果该修订版本不存在）。

#### `git_commit`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1040)

```py
( commit_message: str = 'commit files to HF hub' )
```

参数

+   `commit_message` (`str`, *optional*, 默认为 “commit files to HF hub”) — 提交的消息。

git commit

#### `git_config_username_and_email`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L728)

```py
( git_user: Optional = None git_email: Optional = None )
```

参数

+   `git_user` (`str`, *optional*) — 通过 `git` 注册的用户名。

+   `git_email` (`str`, *optional*) — 通过 `git` 注册的电子邮件。

设置 git 用户名和电子邮件（仅在当前存储库中）。

#### `git_credential_helper_store`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L747)

```py
( )
```

将 git 凭据助手设置为 `store`

#### `git_head_commit_url`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L784)

```py
( ) → export const metadata = 'undefined';str
```

返回

`str`

当前检出的提交的 URL。

获取 HEAD 上最后一次提交的 URL。我们假设已经推送，并且 URL 方案与 GitHub 或 HuggingFace 的相同。

#### `git_head_hash`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L756)

```py
( ) → export const metadata = 'undefined';str
```

返回

`str`

当前检出的提交 SHA。

获取 HEAD 顶部的提交 sha。

#### `git_pull`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L982)

```py
( rebase: bool = False lfs: bool = False )
```

参数

+   `rebase` (`bool`, *optional*, 默认为 `False`) — 在获取后是否将当前分支重新设置到上游分支之上。

+   `lfs` (`bool`, *optional*, 默认为 `False`) — 是否也获取 LFS 文件。此选项仅在克隆存储库时未获取 LFS 文件时更改行为；调用 `repo.git_pull(lfs=True)` 将从远程存储库获取 LFS 文件。

git pull

#### `git_push`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1057)

```py
( upstream: Optional = None blocking: bool = True auto_lfs_prune: bool = False )
```

参数

+   `upstream` (`str`, *optional*) — 要推送到的上游。如果未指定，将推送到最后定义的上游或默认上游（`origin main`）。

+   `blocking` (`bool`, *可选*, 默认为 `True`) — 函数是否应该仅在推送完成时返回。将此设置为`False`将返回一个`CommandInProgress`对象，该对象具有`is_done`属性。当推送完成时，此属性将设置为`True`。

+   `auto_lfs_prune` (`bool`, *可选*, 默认为 `False`) — 是否在文件被推送到远程后自动修剪文件。

git push

如果不设置`blocking`使用，将返回远程存储库上提交的 URL。如果使用`blocking=True`，将返回一个包含提交 URL 和关于过程信息的命令对象的元组。

#### `git_remote_url`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L769)

```py
( ) → export const metadata = 'undefined';str
```

返回

`str`

`origin`远程的 URL。

获取到原始远程的 URL。

#### `is_repo_clean`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1280)

```py
( ) → export const metadata = 'undefined';bool
```

返回

`bool`

如果 git 状态干净，则为`True`，否则为`False`。

返回 git 状态是否干净或不干净

#### `lfs_enable_largefiles`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L874)

```py
( )
```

HF 特定。这使得支持上传大于 5GB 的文件。

#### `lfs_prune`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L964)

```py
( recent = False )
```

参数

+   `recent` (`bool`, *可选*, 默认为 `False`) — 是否修剪文件，即使它们被最近的提交引用。有关更多信息，请参阅以下[链接](https://github.com/git-lfs/git-lfs/blob/f3d43f0428a84fc4f1e5405b76b5a73ec2437e65/docs/man/git-lfs-prune.1.ronn#recent-files)。

git lfs prune

#### `lfs_track`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L833)

```py
( patterns: Union filename: bool = False )
```

参数

+   `patterns` (`Union[str, List[str]]`) — 要使用 git-lfs 跟踪的模式或模式列表。

+   `filename` (`bool`, *可选*, 默认为 `False`) — 是否将模式视为字面文件名。

告诉 git-lfs 根据模式跟踪文件。

将`filename`参数设置为`True`将把参数视为字面文件名，而不是模式。写入`.gitattributes`文件时，文件名中的任何特殊通配符将被转义。

#### `lfs_untrack`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L858)

```py
( patterns: Union )
```

参数

+   `patterns` (`Union[str, List[str]]`) — 要使用 git-lfs 取消跟踪的模式或模式列表。

告诉 git-lfs 取消跟踪这些文件。

#### `list_deleted_files`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L798)

```py
( ) → export const metadata = 'undefined';List[str]
```

返回

`List[str]`

在工作目录或索引中已删除的文件列表。

返回一个在工作目录或索引中已删除的文件列表。

#### `push_to_hub`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1294)

```py
( commit_message: str = 'commit files to HF hub' blocking: bool = True clean_ok: bool = True auto_lfs_prune: bool = False )
```

参数

+   `commit_message` (`str`) — 用于提交的消息。

+   `blocking` (`bool`, *可选*, 默认为 `True`) — 函数是否应该仅在`git push`完成时返回。

+   `clean_ok` (`bool`, *可选*, 默认为 `True`) — 如果为 True，则如果存储库未更改，此函数将返回 None。默认行为是失败，因为 git 命令失败。

+   `auto_lfs_prune` (`bool`, *可选*, 默认为 `False`) — 是否在文件被推送到远程后自动修剪文件。

帮助将文件添加、提交和推送到 HuggingFace Hub 上的远程存储库。将自动跟踪大文件（>10MB）。

#### `tag_exists`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1179)

```py
( tag_name: str remote: Optional = None ) → export const metadata = 'undefined';bool
```

参数

+   `tag_name` (`str`) — 要检查的标签名称。

+   `remote` (`str`, *可选*) — 是否要检查远程上是否存在标签。此参数应为远程的标识符。

返回

`bool`

标签是否存在。

检查标签是否存在。

#### `wait_for_commands`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1458)

```py
( )
```

阻塞方法：阻止所有后续执行，直到所有命令都已处理。

## 辅助方法

#### `huggingface_hub.repository.is_git_repo`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L114)

```py
( folder: Union ) → export const metadata = 'undefined';bool
```

参数

+   `folder` (`str`) — 要运行命令的文件夹。

返回

`bool`

如果存储库是存储库的一部分，则为 `True`，否则为 `False`。

检查文件夹是否是根目录或 git 存储库的一部分

#### `huggingface_hub.repository.is_local_clone`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L131)

```py
( folder: Union remote_url: str ) → export const metadata = 'undefined';bool
```

参数

+   `folder` (`str` 或 `Path`) — 要运行命令的文件夹。

+   `remote_url` (`str`) — git 存储库的 url。

返回

`bool`

如果存储库是指定的远程存储库的本地克隆，则为 `True`，否则为 `False`。

检查文件夹是否是远程 url 的本地克隆

#### `huggingface_hub.repository.is_tracked_with_lfs`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L156)

```py
( filename: Union ) → export const metadata = 'undefined';bool
```

参数

+   `filename` (`str` 或 `Path`) — 要检查的文件名。

返回

`bool`

如果传递的文件被 git-lfs 跟踪，则为 `True`，否则为 `False`。

检查传递的文件是否由 git-lfs 跟踪。

#### `huggingface_hub.repository.is_git_ignored`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L193)

```py
( filename: Union ) → export const metadata = 'undefined';bool
```

参数

+   `filename` (`str` 或 `Path`) — 要检查的文件名。

返回

`bool`

如果传递的文件被 `git` 忽略，则为 `True`，否则为 `False`。

检查文件是否被 git 忽略。支持嵌套的 .gitignore 文件。

#### `huggingface_hub.repository.files_to_be_staged`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L241)

```py
( pattern: str = '.' folder: Union = None ) → export const metadata = 'undefined';List[str]
```

参数

+   `pattern` (`str` 或 `Path`) — 要检查的文件名模式。放置 `.` 以获取所有文件。

+   `folder` (`str` 或 `Path`) — 要运行命令的文件夹。

返回

`List[str]`

要暂存的文件列表。

返回要暂存的文件名列表。

#### `huggingface_hub.repository.is_tracked_upstream`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L266)

```py
( folder: Union ) → export const metadata = 'undefined';bool
```

参数

+   `folder` (`str` 或 `Path`) — 要运行命令的文件夹。

返回

`bool`

如果当前签出的分支被上游跟踪，则为 `True`，否则为 `False`。

检查当前签出的分支是否被追踪到上游。

#### `huggingface_hub.repository.commits_to_push`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L288)

```py
( folder: Union upstream: Optional = None ) → export const metadata = 'undefined';int
```

参数

+   `folder` (`str` 或 `Path`) — 要运行命令的文件夹。

+   `upstream` (`str`, *可选*) —

返回

`int`

如果继续 `git push`，将推送到上游的提交数。

检查将被推送到上游的提交数

应进行比较的上游存储库的名称。

## 以下异步命令

`Repository` 实用程序提供了几种可以异步启动的方法：

+   `git_push`

+   `git_pull`

+   `push_to_hub`

+   `commit` 上下文管理器

查看以下用于管理此类异步方法的实用程序。

### `class huggingface_hub.Repository`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L425)

```py
( local_dir: Union clone_from: Optional = None repo_type: Optional = None token: Union = True git_user: Optional = None git_email: Optional = None revision: Optional = None skip_lfs_files: bool = False client: Optional = None )
```

包装 git 和 git-lfs 命令的辅助类。

目的是简化与 huggingface.co 托管的模型或数据集存储库的交互，尽管这里（如果有的话）实际上与 huggingface.co 没有太多特定的内容。

Repository 已被弃用，推荐使用基于 http 的替代方案，该替代方案已在 HfApi 中实现。鉴于其在旧代码中的广泛应用，完全移除 Repository 将仅在版本`v1.0`中发生。有关更多详细信息，请阅读[`huggingface.co/docs/huggingface_hub/concepts/git_vs_http`](https://huggingface.co/docs/huggingface_hub/concepts/git_vs_http)。

#### `commands_failed`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1444)

```py
( )
```

返回失败的异步命令。

#### `commands_in_progress`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1451)

```py
( )
```

返回当前正在进行的异步命令。

#### `wait_for_commands`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L1458)

```py
( )
```

阻塞方法：阻止所有后续执行，直到所有命令都已处理。

### `class huggingface_hub.repository.CommandInProgress`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repository.py#L31)

```py
( title: str is_done_method: Callable status_method: Callable process: Popen post_method: Optional = None )
```

用于跟踪异步启动的命令的实用程序。
