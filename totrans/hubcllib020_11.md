# 创建和管理存储库

> 原始文本：[https://huggingface.co/docs/huggingface_hub/guides/repository](https://huggingface.co/docs/huggingface_hub/guides/repository)

Hugging Face Hub是一组git存储库。[Git](https://git-scm.com/)是软件开发中广泛使用的工具，可在协作工作时轻松对项目进行版本控制。本指南将向您展示如何与Hub上的存储库进行交互，特别是：

+   创建和删除存储库。

+   管理分支和标签。

+   重命名您的存储库。

+   更新您的存储库可见性。

+   管理您的存储库的本地副本。

如果您习惯于使用GitLab/GitHub/Bitbucket等平台，您的第一反应可能是使用`git` CLI克隆您的存储库（`git clone`），提交更改（`git add, git commit`）并推送它们（`git push`）。在使用Hugging Face Hub时，这是有效的。然而，软件工程和机器学习并不具有相同的要求和工作流程。模型存储库可能会维护不同框架和工具的大型模型权重文件，因此克隆存储库可能导致您维护大型本地文件夹并具有巨大大小。因此，使用我们的自定义HTTP方法可能更有效。您可以阅读我们的[Git vs HTTP范式](../concepts/git_vs_http)解释页面以获取更多详细信息。

如果您想在Hub上创建和管理存储库，您的计算机必须已登录。如果没有，请参考[此部分](../quick-start#authentication)。在本指南的其余部分，我们将假设您的计算机已登录。

## 存储库创建和删除

第一步是了解如何创建和删除存储库。您只能管理您拥有的存储库（在您的用户名命名空间下）或您具有写权限的组织的存储库。

### 创建一个存储库

使用[create_repo()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_repo)创建一个空存储库，并使用`repo_id`参数为其命名。`repo_id`是您的命名空间，后跟存储库名称：`username_or_org/repo_name`。

```py
>>> from huggingface_hub import create_repo
>>> create_repo("lysandre/test-model")
'https://huggingface.co/lysandre/test-model'
```

默认情况下，[create_repo()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_repo) 创建一个模型存储库。但是您可以使用`repo_type`参数来指定另一种存储库类型。例如，如果您想创建一个数据集存储库：

```py
>>> from huggingface_hub import create_repo
>>> create_repo("lysandre/test-dataset", repo_type="dataset")
'https://huggingface.co/datasets/lysandre/test-dataset'
```

创建存储库时，您可以使用`private`参数设置存储库可见性。

```py
>>> from huggingface_hub import create_repo
>>> create_repo("lysandre/test-private", private=True)
```

如果您希望以后更改存储库的可见性，可以使用[update_repo_visibility()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.update_repo_visibility)函数。

### 删除存储库

使用[delete_repo()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.delete_repo)删除存储库。请确保您要删除存储库，因为这是一个不可逆的过程！

指定要删除的存储库的`repo_id`：

```py
>>> delete_repo(repo_id="lysandre/my-corrupted-dataset", repo_type="dataset")
```

### 复制存储库（仅适用于Spaces）

在某些情况下，您可能希望复制他人的存储库以适应您的用例。对于Spaces，可以使用[duplicate_space()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.duplicate_space)方法实现。它将复制整个存储库。您仍需要配置自己的设置（硬件、睡眠时间、存储、变量和秘密）。查看我们的[管理您的Space](./manage-spaces)指南以获取更多详细信息。

```py
>>> from huggingface_hub import duplicate_space
>>> duplicate_space("multimodalart/dreambooth-training", private=False)
RepoUrl('https://huggingface.co/spaces/nateraw/dreambooth-training',...)
```

## 上传和下载文件

现在您已经创建了您的存储库，您有兴趣将更改推送到它并从中下载文件。

这两个主题值得拥有自己的指南。请参考[上传](./upload)和[下载](./download)指南，了解如何使用您的存储库。

## 分支和标签

Git 存储库通常使用分支来存储同一存储库的不同版本。标签也可以用于标记存储库的特定状态，例如，在发布版本时。一般来说，分支和标签被称为[git 引用](https://git-scm.com/book/en/v2/Git-Internals-Git-References)。

### 创建分支和标签

您可以使用 [create_branch()](/docs/huggingface_hub/v0.20.3/zh/package_reference/hf_api#huggingface_hub.HfApi.create_branch) 和 [create_tag()](/docs/huggingface_hub/v0.20.3/zh/package_reference/hf_api#huggingface_hub.HfApi.create_tag) 创建新的分支和标签：

```py
>>> from huggingface_hub import create_branch, create_tag

# Create a branch on a Space repo from `main` branch
>>> create_branch("Matthijs/speecht5-tts-demo", repo_type="space", branch="handle-dog-speaker")

# Create a tag on a Dataset repo from `v0.1-release` branch
>>> create_branch("bigcode/the-stack", repo_type="dataset", revision="v0.1-release", tag="v0.1.1", tag_message="Bump release version.")
```

您可以使用 [delete_branch()](/docs/huggingface_hub/v0.20.3/zh/package_reference/hf_api#huggingface_hub.HfApi.delete_branch) 和 [delete_tag()](/docs/huggingface_hub/v0.20.3/zh/package_reference/hf_api#huggingface_hub.HfApi.delete_tag) 函数以相同的方式删除分支或标签。

### 列出所有分支和标签

您还可以使用 [list_repo_refs()](/docs/huggingface_hub/v0.20.3/zh/package_reference/hf_api#huggingface_hub.HfApi.list_repo_refs) 列出存储库中现有的 git 引用：

```py
>>> from huggingface_hub import list_repo_refs
>>> list_repo_refs("bigcode/the-stack", repo_type="dataset")
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

## 更改存储库设置

存储库带有一些您可以配置的设置。大多数情况下，您将希望在浏览器中的存储库设置页面中手动进行配置。您必须对存储库具有写入访问权限才能对其进行配置（拥有它或是组织的一部分）。在本节中，我们将看到您还可以使用 `huggingface_hub` 以编程方式配置的设置。

一些设置特定于 Spaces（硬件、环境变量等）。要配置这些设置，请参考我们的[管理您的 Spaces](../guides/manage-spaces)指南。

### 更新可见性

存储库可以是公共的或私有的。私有存储库只对您或存储库所在组织的成员可见。将存储库更改为私有的示例如下：

```py
>>> from huggingface_hub import update_repo_visibility
>>> update_repo_visibility(repo_id=repo_id, private=True)
```

### 重命名您的存储库

您可以使用 [move_repo()](/docs/huggingface_hub/v0.20.3/zh/package_reference/hf_api#huggingface_hub.HfApi.move_repo) 在 Hub 上重命名您的存储库。使用此方法，您还可以将存储库从用户移动到组织。在这样做时，有一些[限制](https://hf.co/docs/hub/repositories-settings#renaming-or-transferring-a-repo)您应该注意。例如，您无法将存储库转移到另一个用户。

```py
>>> from huggingface_hub import move_repo
>>> move_repo(from_id="Wauplin/cool-model", to_id="huggingface/cool-model")
```

## 管理存储库的本地副本

上述所有操作都可以使用 HTTP 请求完成。但是，在某些情况下，您可能有兴趣拥有存储库的本地副本，并使用您熟悉的 Git 命令与其交互。

[Repository](/docs/huggingface_hub/v0.20.3/zh/package_reference/repository#huggingface_hub.Repository) 类允许您与 Hub 上的文件和存储库进行交互，其功能类似于 Git 命令。它是对 Git 和 Git-LFS 方法的封装，以便您可以使用您已经熟悉和喜爱的 Git 命令。在开始之前，请确保您已安装了 Git-LFS（请参阅[此处](https://git-lfs.github.com/)获取安装说明）。

[Repository](/docs/huggingface_hub/v0.20.3/zh/package_reference/repository#huggingface_hub.Repository) 已被弃用，推荐使用 [HfApi](/docs/huggingface_hub/v0.20.3/zh/package_reference/hf_api#huggingface_hub.HfApi) 中实现的基于 http 的替代方法。鉴于其在传统代码中的广泛应用，[Repository](/docs/huggingface_hub/v0.20.3/zh/package_reference/repository#huggingface_hub.Repository) 的完全移除将仅在版本 `v1.0` 中发生。有关更多详细信息，请阅读[此解释页面](./concepts/git_vs_http)。

### 使用本地存储库

使用指向本地存储库的路径实例化一个[Repository](/docs/huggingface_hub/v0.20.3/zh/package_reference/repository#huggingface_hub.Repository)对象：

```py
>>> from huggingface_hub import Repository
>>> repo = Repository(local_dir="<path>/<to>/<folder>")
```

### 克隆

`clone_from` 参数将存储库从 Hugging Face 存储库 ID 克隆到由 `local_dir` 参数指定的本地目录：

```py
>>> from huggingface_hub import Repository
>>> repo = Repository(local_dir="w2v2", clone_from="facebook/wav2vec2-large-960h-lv60")
```

`clone_from`也可以使用URL克隆存储库：

```py
>>> repo = Repository(local_dir="huggingface-hub", clone_from="https://huggingface.co/facebook/wav2vec2-large-960h-lv60")
```

您可以将`clone_from`参数与[create_repo()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_repo)结合使用来创建和克隆存储库：

```py
>>> repo_url = create_repo(repo_id="repo_name")
>>> repo = Repository(local_dir="repo_local_path", clone_from=repo_url)
```

您还可以在克隆存储库时通过指定`git_user`和`git_email`参数来配置Git用户名和电子邮件。当用户提交到该存储库时，Git将知道提交作者。

```py
>>> repo = Repository(
...   "my-dataset",
...   clone_from="<user>/<dataset_id>",
...   token=True,
...   repo_type="dataset",
...   git_user="MyName",
...   git_email="me@cool.mail"
... )
```

### 分支

分支对于协作和实验非常重要，而不会影响您当前的文件和代码。使用[git_checkout()](/docs/huggingface_hub/v0.20.3/en/package_reference/repository#huggingface_hub.Repository.git_checkout)在分支之间切换。例如，如果您想从`branch1`切换到`branch2`：

```py
>>> from huggingface_hub import Repository
>>> repo = Repository(local_dir="huggingface-hub", clone_from="<user>/<dataset_id>", revision='branch1')
>>> repo.git_checkout("branch2")
```

### 拉取

[git_pull()](/docs/huggingface_hub/v0.20.3/en/package_reference/repository#huggingface_hub.Repository.git_pull)允许您从远程存储库更新当前本地分支的更改：

```py
>>> from huggingface_hub import Repository
>>> repo.git_pull()
```

如果您希望本地提交发生在您的分支更新为来自远程的新提交之后，请设置`rebase=True`：

```py
>>> repo.git_pull(rebase=True)
```
