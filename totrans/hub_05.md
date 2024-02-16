# Pull请求和讨论

> 原文：[https://huggingface.co/docs/hub/repositories-pull-requests-discussions](https://huggingface.co/docs/hub/repositories-pull-requests-discussions)

Hub Pull请求和讨论允许用户对存储库进行社区贡献。对于所有存储库类型，Pull请求和讨论的工作方式相同。

总体而言，目标是构建其他git主机（如GitHub）PR和Issues的简化版本：

+   不涉及分叉：贡献者直接将内容推送到源存储库上的特殊`ref`分支。

+   讨论和PR之间没有明确的区别：它们本质上是相同的，因此它们显示在相同的列表中。

+   它们专为ML（即模型/数据集/空间存储库）而设计，而不是任意存储库。

*注意，可以从[存储库设置](./repositories-settings#disabling-discussions-pull-requests)启用或禁用Pull Requests和讨论*

## 列表

通过转到任何存储库中的社区选项卡，您可以查看所有讨论和Pull请求。您还可以过滤以仅查看已打开的讨论。

![](../Images/af4962317415dbaaba3787c741d9797c.png) ![](../Images/e380b1fd6798945dae39cb9180a004ff.png)

## 查看

讨论页面允许您查看不同用户的评论。如果是Pull Request，您可以通过转到“更改的文件”选项卡查看所有更改。

![](../Images/ead31aa3a680d63f01b6a9b27191b3a7.png) ![](../Images/dd1589c4912a5773ff01f28b45b79dc0.png)

## 编辑讨论/PR标题

如果您打开了PR或讨论，是存储库的作者，或者具有对其的写入访问权限，则可以单击铅笔按钮编辑讨论标题。

![](../Images/f8f31e5e017ae97f12e939d6cc43025e.png) ![](../Images/7cec56e1cbe5320bd0037afb664a8195.png)

## 固定讨论/PR

如果您对存储库具有写入访问权限，则可以固定讨论和Pull请求。固定的讨论将显示在所有讨论的顶部。

![](../Images/ba9d4204081817d93555b3dfa3d20eba.png) ![](../Images/e7a6fe8d74e9dc66568de1ee6cc98f70.png)![](../Images/a2558a3be993c98c5d4b1698d9fab2e1.png) ![](../Images/7419ebcd6f1486199ccbcb4c19acdbaf.png)

## 锁定讨论/PR

如果您对存储库具有写入访问权限，则可以锁定讨论或Pull请求。一旦讨论被锁定，以前的评论仍然可见，用户将无法添加新评论。

![](../Images/4bfd104f018b0f392509ae3c978608f2.png) ![](../Images/b3bc2497b4111bc1b8116b556bed8276.png)![](../Images/c9cdbdf4e8669dc56df4caf858f71729.png) ![](../Images/1a5086121e28d40ca232d87b1aac3d6c.png)

## 评论编辑和管理

如果您撰写了评论或对存储库具有写入访问权限，则可以通过单击评论框右上角的上下文菜单编辑评论内容。

![](../Images/03d346a375673847d17674fd57b25ea9.png) ![](../Images/7db656f4e973109e5466b617d1eb1929.png)![](../Images/5dd5b3d2c9a200b192a31eb43ed0bb7b.png) ![](../Images/d87ec03f7176d18e0ee72124a8578dcd.png)

一旦评论被编辑，新链接将出现在评论上方。此链接显示编辑历史记录。

![](../Images/fc2a56abadf8b10e5cc406a094c2bc9c.png) ![](../Images/0e3502b6fde899b4327bfd6233c14a21.png)![](../Images/e28be5ae56e9637e73250a9810473cce.png) ![](../Images/0fdfbf0bba1308b22daf61e922b9ab4e.png)

您还可以隐藏评论。隐藏评论是不可逆的，没有人将能够看到其内容或再次编辑它。

![](../Images/bcff45f21bd577ebb90642a1a5ad303c.png) ![](../Images/2f667f124241821b863c30a881663f9e.png)

还要阅读[moderation](./moderation)以了解如何报告滥用评论。

## 我可以在我的评论和讨论中使用Markdown和LaTeX吗？

是的！您可以使用Markdown为您的评论添加格式。此外，您可以利用LaTeX进行数学排版，您的公式将在Markdown中解析之前使用[KaTeX](https://katex.org/)进行呈现。

对于LaTeX方程式，您必须使用以下定界符：

+   `$$ ... $$`用于显示模式

+   `\\(...\\)` 用于内联模式（斜杠和括号之间没有空格）。

## 我如何在本地管理拉取请求？

假设您的PR编号是42。

```py
git fetch origin refs/pr/42:pr/42
git checkout pr/42
# Do your changes
git add .
git commit -m "Add your change"
git push origin pr/42:refs/pr/42
```

### 草稿模式

在“高级模式”中从头开始打开新拉取请求时，草稿模式是默认状态。有了这个状态，其他贡献者就知道您的拉取请求正在进行中，不能合并。当您的分支准备就绪时，只需点击“发布”按钮将拉取请求的状态更改为“打开”。请注意，一旦发布，您就无法返回到草稿模式。

## 拉取请求的高级用法

### 更改存储在git存储库中的位置在哪里？

我们的拉取请求不使用分叉和分支，而是使用自定义的“引用”（refs），直接存储在源存储库上。

[Git References](https://git-scm.com/book/en/v2/Git-Internals-Git-References) 是git的内部机制，已经存储了标签和分支。

使用自定义引用（例如`refs/pr/42`）而不是分支的优势在于，它们不会被默认克隆存储库的人（包括存储库“所有者”）获取，但仍然可以按需获取。

### 获取所有拉取请求：对于git魔术师🧙‍♀️

您可以调整本地的**refspec**来获取所有拉取请求：

1.  获取

```py
git fetch origin refs/pr/*:refs/remotes/origin/pr/*
```

1.  创建一个本地分支来跟踪引用

```py
git checkout pr/{PR_NUMBER}
# for example: git checkout pr/42
```

1.  如果您进行了本地更改，要推送到PR引用：

```py
git push origin pr/{PR_NUMBER}:refs/pr/{PR_NUMBER}
# for example: git push origin pr/42:refs/pr/42
```
