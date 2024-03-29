# 竞赛空间

> 原文链接：[`huggingface.co/docs/competitions/competition_space`](https://huggingface.co/docs/competitions/competition_space)

竞赛空间是一个 Hugging Face 空间，实际的竞赛活动在这里进行。在这个空间里，你可以提交你的模型并获得评分。竞争者也可以在这个空间看到排行榜，讨论并提交作品。

查看下面的一个示例竞赛空间：

![竞赛空间](img/42975e4386aad470b0b36aa2c34096f3.png)

竞赛空间包括以下内容：

+   竞赛描述（内容从私人竞赛存储库中获取）

+   数据集描述（内容从私人竞赛存储库中获取）

+   排行榜（内容从私人竞赛存储库中获取）

    +   公共（对每个人都是开放的，随时可用）

    +   私人（对每个人都是开放的，但只能在竞赛结束后）

+   提交

    +   提交指南（内容从私人竞赛存储库中获取）

    +   我的提交（用户可以看到自己的提交）

    +   新提交（用户可以进行新的提交）

+   讨论（可通过社区选项卡访问）

### 秘密

竞赛空间需要两个秘密：

+   `HF_TOKEN`：这是创建竞赛空间的用户的 Hugging Face 写入令牌。这个令牌必须在竞赛期间保持活动状态。如果令牌过期，竞赛空间将停止工作。如果您更改/刷新/删除此令牌，您将需要更新此秘密。

+   `COMPETITION_ID`：这是私人竞赛存储库的路径。例如 `abhishek/private-competition-data`。如果您更改私人竞赛存储库的名称，您将需要更新此秘密。

注意：上述两个秘密对于竞赛空间的运行至关重要！

### 公共和私人竞赛空间

一个竞赛空间可以是公共的或私人的。公共竞赛空间对每个人都是开放的，随时可用。私人竞赛空间只对创建该竞赛空间的组织成员开放。

您可以随时将竞赛公开。

通常，我们建议在将竞赛空间公开之前，在私人竞赛空间中测试竞赛空间的每个方面。
