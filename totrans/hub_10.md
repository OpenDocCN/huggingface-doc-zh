# 下一步

> 原文链接：[https://huggingface.co/docs/hub/repositories-next-steps](https://huggingface.co/docs/hub/repositories-next-steps)

接下来的部分突出显示了您可能发现有用的功能和额外信息，以充分利用Hugging Face Hub上的Git存储库。

## 如何以编程方式管理存储库

Hugging Face支持通过[`huggingface_hub`库](https://huggingface.co/docs/huggingface_hub/index)使用Python访问存储库。我们已经探讨过的操作，如下载存储库和上传文件，都可以通过该库进行，还有其他有用的功能！

如果您更喜欢直接使用git，请阅读下面的部分。

## 了解更多关于Git的知识

如果您想继续学习Git，可以访问[这个Git教程](https://learngitbranching.js.org/)。要了解更多关于Git的背景知识，您可以查看[GitHub的Git指南](https://github.com/git-guides)。

## 如何使用分支

为了有效地协作使用Git存储库，并在发布之前的代码上工作，您可以使用**分支**。分支允许您将“正在进行中”的代码与“准备投入生产”的代码分开，还可以让多人在项目上工作而不经常与彼此的贡献发生冲突。您可以使用分支将实验隔离在自己的分支中，甚至[采用团队范例来管理分支](https://ericmjl.github.io/essays-on-data-science/workflow/gitflow/)。

要了解Git分支，请尝试[Learn Git Branching交互式教程](https://learngitbranching.js.org/)。

## 使用标签

Git允许您*标记*提交，以便您可以轻松地在项目中记录里程碑。因此，您可以使用标签在Hub存储库中标记提交！要了解如何使用标签，您可以访问[这篇DevConnected文章](https://devconnected.com/how-to-create-git-tags/)。

除了帮助您在存储库历史中识别重要提交外，使用Git标签还可以让您进行A/B测试，[在特定标签处克隆存储库](https://www.techiedelight.com/clone-specific-tag-with-git/)等！`huggingface_hub`库还支持与标签一起工作，例如[从特定标记的提交中下载文件](https://huggingface.co/docs/huggingface_hub/main/en/how-to-downstream#hfhuburl)。

## 如何复制或分叉一个存储库（包括LFS指针）

如果您想要复制一个存储库，取决于您是否想要保留Git历史，有两种选项。

### 无Git历史复制

在许多情况下，如果您想要自己的特定代码库副本，您可能不关心以前的Git历史。在这种情况下，您可以快速使用方便的[Repo Duplicator](https://huggingface.co/spaces/huggingface-projects/repo_duplicator)复制存储库！您将需要创建一个用户访问令牌，您可以在[安全文档](./security-tokens)中了解更多。

### 使用Git历史进行复制（分叉）

保留提交历史的存储库副本称为*分叉*。您可以选择分叉自己的存储库，但如果您想要对其他人的项目进行调整，也常见分叉其他人的项目。

**请注意，您需要[安装Git LFS](https://git-lfs.github.com/)和[`huggingface_hub` CLI](https://huggingface.co/docs/huggingface_hub/index)才能按照此过程操作**。当您想要分叉或[变基](https://git-scm.com/docs/git-rebase)一个包含LFS文件的存储库时，您不能使用您熟悉的常规Git方法，因为您需要小心不要破坏LFS指针。分叉可能需要时间，取决于您的带宽，因为您将不得不获取和重新上传您的分叉中的所有LFS文件。

例如，假设您有一个上游存储库，**upstream**，并且您刚刚在Hub上创建了自己的存储库，本例中为**myfork**。

1.  在[https://huggingface.co](https://huggingface.co)中创建一个目标存储库（例如**myfork**）

1.  克隆您的分支存储库：

```py
git clone git@hf.co:me/myfork
```

1.  获取非LFS文件：

```py
cd myfork
git lfs install --skip-smudge --local # affects only this clone
git remote add upstream git@hf.co:friend/upstream
git fetch upstream
```

1.  获取大文件。这可能需要一些时间，取决于您的下载带宽：

```py
git lfs fetch --all upstream # this can take time depending on your download bandwidth
```

4.a. 如果您想完全覆盖分支历史记录（应该只有一个初始提交），运行：

```py
git reset --hard upstream/main
```

4.b. 如果您想要重新定位而不是覆盖，请运行以下命令并解决任何冲突：

```py
git rebase upstream/main
```

1.  准备推送您的LFS文件：

```py
git lfs install --force --local # this reinstalls the LFS hooks
huggingface-cli lfs-enable-largefiles . # needed if some files are bigger than 5GB
```

1.  最后推送：

```py
git push --force origin main # this can take time depending on your upload bandwidth
```

现在您在Hub中有自己的分支或重新定位的存储库！
