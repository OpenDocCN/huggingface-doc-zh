# 与讨论和拉取请求交互

> 原始文本：[`huggingface.co/docs/huggingface_hub/package_reference/community`](https://huggingface.co/docs/huggingface_hub/package_reference/community)

查看 HfApi 文档页面，了解与 Hub 上的拉取请求和讨论交互的方法的参考。

+   get_repo_discussions()

+   get_discussion_details()

+   create_discussion()

+   create_pull_request()

+   rename_discussion()

+   comment_discussion()

+   edit_discussion_comment()

+   change_discussion_status()

+   merge_pull_request()

## 数据结构

### `class huggingface_hub.Discussion`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/community.py#L18)

```py
( title: str status: Literal num: int repo_id: str repo_type: str author: str is_pull_request: bool created_at: datetime endpoint: str )
```

参数

+   `title`（`str`）— 讨论/拉取请求的标题

+   `status`（`str`）— 讨论/拉取请求的状态。必须是以下之一：

    +   “打开”

    +   `"closed"`

    +   “合并”（仅适用于拉取请求）

    +   “草稿”（仅适用于拉取请求）

+   `num`（`int`）— 讨论/拉取请求的编号。

+   `repo_id`（`str`）— 讨论/拉取请求所在仓库的 id（`"{namespace}/{repo_name}"`）。

+   `repo_type`（`str`）— 讨论/拉取请求所在仓库的类型。可能的值为：`"model"`、`"dataset"`、`"space"`。

+   `author`（`str`）— 讨论/拉取请求作者的用户名。如果用户已被删除，则可以是`"deleted"`。

+   `is_pull_request`（`bool`）— 是否为拉取请求。

+   `created_at`（`datetime`）— 讨论/拉取请求创建的`datetime`。

+   `endpoint`（`str`）— Hub 的端点。默认为[`huggingface.co`](https://huggingface.co)。

+   `git_reference`（`str`，*可选*）—（属性）如果这是一个拉取请求，可以推送更改的 Git 引用，否则为`None`。

+   `url`（`str`）—（属性）Hub 上讨论的 URL。

Hub 上的讨论或拉取请求。

此数据类不打算直接实例化。

### `class huggingface_hub.DiscussionWithDetails`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/community.py#L86)

```py
( title: str status: Literal num: int repo_id: str repo_type: str author: str is_pull_request: bool created_at: datetime endpoint: str events: List conflicting_files: Union target_branch: Optional merge_commit_oid: Optional diff: Optional )
```

参数

+   `title`（`str`）— 讨论/拉取请求的标题

+   `status`（`str`）— 讨论/拉取请求的状态。可以是以下之一：

    +   “打开”

    +   `"closed"`

    +   “合并”（仅适用于拉取请求）

    +   “草稿”（仅适用于拉取请求）

+   `num`（`int`）— 讨论/拉取请求的编号。

+   `repo_id`（`str`）— 讨论/拉取请求所在仓库的 id（`"{namespace}/{repo_name}"`）。

+   `repo_type`（`str`）— 讨论/拉取请求所在仓库的类型。可能的值为：`"model"`、`"dataset"`、`"space"`。

+   `author`（`str`）— 讨论/拉取请求作者的用户名。如果用户已被删除，则可以是`"deleted"`。

+   `is_pull_request`（`bool`）— 是否为拉取请求。

+   `created_at` (`datetime`) — 讨论/拉取请求创建的日期时间。

+   `events` (`list` of DiscussionEvent) — 此讨论或拉取请求中的`DiscussionEvents`列表。

+   `conflicting_files` (`Union[List[str], bool, None]`, *可选*) — 如果这是一个拉取请求，则是冲突文件的列表。如果`self.is_pull_request`为`False`，则为`None`。如果存在冲突文件但无法检索列表，则为`True`。

+   `target_branch` (`str`, *可选*) — 如果这是一个拉取请求，表示要将更改合并到的分支。如果`self.is_pull_request`为`False`，则为`None`。

+   `merge_commit_oid` (`str`, *可选*) — 如果这是一个已合并的拉取请求，这将设置为合并提交的 OID/SHA，否则为`None`。

+   `diff` (`str`, *可选*) — 如果这是一个拉取请求，则是 git diff，否则为`None`。

+   `endpoint` (`str`) — Hub 的端点。默认为[`huggingface.co`](https://huggingface.co)。

+   `git_reference` (`str`, *可选*) — (属性) 如果这是一个拉取请求，则表示可以推送更改的 git 引用，否则为`None`。

+   `url` (`str`) — (属性) Hub 上讨论的 URL。

Discussion 的子类。

### `class huggingface_hub.DiscussionEvent`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/community.py#L145)

```py
( id: str type: str created_at: datetime author: str _event: dict )
```

参数

+   `id` (`str`) — 事件的 ID。一个十六进制字符串。

+   `type` (`str`) — 事件的类型。

+   `created_at` (`datetime`) — 一个[`datetime`](https://docs.python.org/3/library/datetime.html?highlight=datetime#datetime.datetime)对象，保存事件的创建时间戳。

+   `author` (`str`) — 讨论/拉取请求作者的用户名。如果用户已被删除，则可以是`"deleted"`。

讨论或拉取请求中的事件。

使用具体的类：

+   DiscussionComment

+   DiscussionStatusChange

+   DiscussionCommit

+   DiscussionTitleChange

### `class huggingface_hub.DiscussionComment`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/community.py#L178)

```py
( id: str type: str created_at: datetime author: str _event: dict content: str edited: bool hidden: bool )
```

参数

+   `id` (`str`) — 事件的 ID。一个十六进制字符串。

+   `type` (`str`) — 事件的类型。

+   `created_at` (`datetime`) — 一个[`datetime`](https://docs.python.org/3/library/datetime.html?highlight=datetime#datetime.datetime)对象，保存事件的创建时间戳。

+   `author` (`str`) — 讨论/拉取请求作者的用户名。如果用户已被删除，则可以是`"deleted"`。

+   `content` (`str`) — 评论的原始 markdown 内容。提及、链接和图片不会被渲染。

+   `edited` (`bool`) — 评论是否已被编辑。

+   `hidden` (`bool`) — 评论是否已被隐藏。

讨论/拉取请求中的评论。

DiscussionEvent 的子类。

### `class huggingface_hub.DiscussionStatusChange`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/community.py#L233)

```py
( id: str type: str created_at: datetime author: str _event: dict new_status: str )
```

参数

+   `id` (`str`) — 事件的 ID。一个十六进制字符串。

+   `type` (`str`) — 事件的类型。

+   `created_at` (`datetime`) — 一个[`datetime`](https://docs.python.org/3/library/datetime.html?highlight=datetime#datetime.datetime)对象，保存事件的创建时间戳。

+   `author`（`str`）— 讨论/拉取请求作者的用户名。如果用户已被删除，则可以是`"deleted"`。

+   `new_status`（`str`）— 更改后的讨论/拉取请求状态。可以是以下之一：

    +   “打开”

    +   “关闭”

    +   “合并”（仅适用于拉取请求）

讨论/拉取请求中状态的更改。

DiscussionEvent 的子类。

### `class huggingface_hub.DiscussionCommit`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/community.py#L261)

```py
( id: str type: str created_at: datetime author: str _event: dict summary: str oid: str )
```

参数

+   `id`（`str`）— 事件的 ID。一个十六进制字符串。

+   `type`（`str`）— 事件类型。

+   `created_at`（`datetime`）— 一个包含事件创建时间戳的[`datetime`](https://docs.python.org/3/library/datetime.html?highlight=datetime#datetime.datetime)对象。

+   `author`（`str`）— 讨论/拉取请求作者的用户名。如果用户已被删除，则可以是`"deleted"`。

+   `summary`（`str`）— 提交的摘要。

+   `oid`（`str`）— 提交的 OID/SHA，作为十六进制字符串。

拉取请求中的提交。

DiscussionEvent 的子类。

### `class huggingface_hub.DiscussionTitleChange`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/community.py#L288)

```py
( id: str type: str created_at: datetime author: str _event: dict old_title: str new_title: str )
```

参数

+   `id`（`str`）— 事件的 ID。一个十六进制字符串。

+   `type`（`str`）— 事件类型。

+   `created_at`（`datetime`）— 一个包含事件创建时间戳的[`datetime`](https://docs.python.org/3/library/datetime.html?highlight=datetime#datetime.datetime)对象。

+   `author`（`str`）— 讨论/拉取请求作者的用户名。如果用户已被删除，则可以是`"deleted"`。

+   `old_title`（`str`）— 讨论/拉取请求的先前标题。

+   `new_title`（`str`）— 新标题。

讨论/拉取请求中的重命名事件。

DiscussionEvent 的子类。
