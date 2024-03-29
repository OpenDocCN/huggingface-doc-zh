# 离线与在线强化学习

> 原文链接：[`huggingface.co/learn/deep-rl-course/unitbonus3/offline-online`](https://huggingface.co/learn/deep-rl-course/unitbonus3/offline-online)

深度强化学习（RL）是一个**构建决策代理的框架**。这些代理通过**试错交互并接收奖励作为唯一反馈**来学习最佳行为（策略）。

代理的目标**是最大化其累积奖励**，称为回报。因为强化学习基于*奖励假设*：所有目标都可以描述为**最大化预期累积奖励**。

深度强化学习代理**通过经验批次学习**。问题是，它们如何收集经验？：

![单元奖励 3 缩略图](img/88ecc42f5b64e1f4b4d04c4ba4639ae7.png)

强化学习在在线和离线环境中的比较，图表取自[这篇文章](https://offline-rl.github.io/)

+   在*在线强化学习*中，这是我们在本课程中学到的，代理**直接收集数据**：它通过**与环境交互**收集一批经验。然后，它立即使用这些经验（或通过一些重放缓冲区）来学习（更新其策略）。

但这意味着你要么**直接在现实世界中训练你的代理，要么有一个模拟器**。如果你没有一个，你需要构建一个，这可能非常复杂（如何在环境中反映现实世界的复杂现实？），昂贵，并且不安全（如果模拟器存在缺陷可能提供竞争优势，代理将利用它们）。

+   另一方面，在*离线强化学习*中，代理只**使用从其他代理或人类演示收集的数据**。它**不与环境交互**。

流程如下：

+   使用一个或多个策略和/或人类互动**创建一个数据集**。

+   在这个数据集上运行**离线强化学习**以学习一个策略

这种方法有一个缺点：*反事实查询问题*。如果我们的代理**决定做一些我们没有数据的事情**怎么办？例如，在十字路口右转，但我们没有这个轨迹。

关于这个主题存在一些解决方案，但如果你想了解更多关于离线强化学习的内容，你可以[观看这个视频](https://www.youtube.com/watch?v=k08N5a0gG0A)

## 进一步阅读

有关更多信息，我们建议您查看以下资源：

+   [离线强化学习，Sergei Levine 的演讲](https://www.youtube.com/watch?v=qgZPZREor5I)

+   [离线强化学习：教程，评论和对开放问题的展望](https://arxiv.org/abs/2005.01643)

## 作者

本节由[Thomas Simonini](https://twitter.com/ThomasSimonini)撰写
