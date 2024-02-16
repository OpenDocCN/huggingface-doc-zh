# 与讨论和拉取请求交互

> [`huggingface.co/docs/huggingface_hub/guides/community`](https://huggingface.co/docs/huggingface_hub/guides/community)

`huggingface_hub`库提供了一个 Python 接口，用于与 Hub 上的拉取请求和讨论进行交互。访问[专门的文档页面](https://huggingface.co/docs/hub/repositories-pull-requests-discussions)以深入了解 Hub 上的讨论和拉取请求是什么，以及它们在幕后是如何工作的。

## 从 Hub 检索讨论和拉取请求

`HfApi`类允许您检索给定存储库上的讨论和拉取请求：

```py
>>> from huggingface_hub import get_repo_discussions
>>> for discussion in get_repo_discussions(repo_id="bigscience/bloom"):
...     print(f"{discussion.num} - {discussion.title}, pr: {discussion.is_pull_request}")

# 11 - Add Flax weights, pr: True
# 10 - Update README.md, pr: True
# 9 - Training languages in the model card, pr: True
# 8 - Update tokenizer_config.json, pr: True
# 7 - Slurm training script, pr: False
[...]
```

`HfApi.get_repo_discussions`支持按作者、类型（拉取请求或讨论）和状态（`open`或`closed`）进行过滤：

```py
>>> from huggingface_hub import get_repo_discussions
>>> for discussion in get_repo_discussions(
...    repo_id="bigscience/bloom",
...    author="ArthurZ",
...    discussion_type="pull_request",
...    discussion_status="open",
... ):
...     print(f"{discussion.num} - {discussion.title} by {discussion.author}, pr: {discussion.is_pull_request}")

# 19 - Add Flax weights by ArthurZ, pr: True
```

`HfApi.get_repo_discussions`返回一个[生成器](https://docs.python.org/3.7/howto/functional.html#generators)，它产生 Discussion 对象。要获取单个列表中的所有讨论，请运行：

```py
>>> from huggingface_hub import get_repo_discussions
>>> discussions_list = list(get_repo_discussions(repo_id="bert-base-uncased"))
```

HfApi.get_repo_discussions()返回的 Discussion 对象包含了讨论或拉取请求的高级概述。您还可以使用 HfApi.get_discussion_details()获取更详细的信息：

```py
>>> from huggingface_hub import get_discussion_details

>>> get_discussion_details(
...     repo_id="bigscience/bloom-1b3",
...     discussion_num=2
... )
DiscussionWithDetails(
    num=2,
    author='cakiki',
    title='Update VRAM memory for the V100s',
    status='open',
    is_pull_request=True,
    events=[
        DiscussionComment(type='comment', author='cakiki', ...),
        DiscussionCommit(type='commit', author='cakiki', summary='Update VRAM memory for the V100s', oid='1256f9d9a33fa8887e1c1bf0e09b4713da96773a', ...),
    ],
    conflicting_files=[],
    target_branch='refs/heads/main',
    merge_commit_oid=None,
    diff='diff --git a/README.md b/README.md\nindex a6ae3b9294edf8d0eda0d67c7780a10241242a7e..3a1814f212bc3f0d3cc8f74bdbd316de4ae7b9e3 100644\n--- a/README.md\n+++ b/README.md\n@@ -132,7 +132,7 [...]',
)
```

HfApi.get_discussion_details()返回一个 DiscussionWithDetails 对象，它是 Discussion 的子类，提供有关讨论或拉取请求的更详细信息。信息包括所有评论、状态更改和通过`DiscussionWithDetails.events`重命名的讨论。

在拉取请求的情况下，您可以使用`DiscussionWithDetails.diff`检索原始 git 差异。拉取请求的所有提交都列在`DiscussionWithDetails.events`中。

## 通过编程方式创建和编辑讨论或拉取请求

HfApi 类还提供了创建和编辑讨论和拉取请求的方法。您需要一个[访问令牌](https://huggingface.co/docs/hub/security-tokens)来创建和编辑讨论或拉取请求。

在 Hub 上为存储库提出更改的最简单方法是通过 create_commit() API：只需将`create_pr`参数设置为`True`。此参数也适用于其他包装 create_commit()的方法：

+   upload_file()

+   upload_folder()

+   delete_file()

+   delete_folder()

+   metadata_update()

```py
>>> from huggingface_hub import metadata_update

>>> metadata_update(
...     repo_id="username/repo_name",
...     metadata={"tags": ["computer-vision", "awesome-model"]},
...     create_pr=True,
... )
```

您还可以使用 HfApi.create_discussion()（分别使用 HfApi.create_pull_request()）在存储库上创建讨论（分别创建拉取请求）。以这种方式打开拉取请求可能很有用，如果您需要在本地工作进行更改。以这种方式打开的拉取请求将处于“草稿”模式。

```py
>>> from huggingface_hub import create_discussion, create_pull_request

>>> create_discussion(
...     repo_id="username/repo-name",
...     title="Hi from the huggingface_hub library!",
...     token="<insert your access token here>",
... )
DiscussionWithDetails(...)

>>> create_pull_request(
...     repo_id="username/repo-name",
...     title="Hi from the huggingface_hub library!",
...     token="<insert your access token here>",
... )
DiscussionWithDetails(..., is_pull_request=True)
```

管理拉取请求和讨论可以完全通过 HfApi 类来完成。例如：

+   使用 comment_discussion()来添加评论

+   使用 edit_discussion_comment()来编辑评论

+   使用 rename_discussion()来重命名讨论或拉取请求

+   使用 change_discussion_status()来打开或关闭讨论/拉取请求

+   使用 merge_pull_request()来合并拉取请求

访问 HfApi 文档页面，查看所有可用方法的详尽参考。

## 推送更改到拉取请求

*即将推出！*

## 另请参阅

有关更详细的参考，请访问讨论和拉取请求以及 hf_api 文档页面。
