# 决策 Transformer

> 原始文本：[`huggingface.co/learn/deep-rl-course/unitbonus3/decision-transformers`](https://huggingface.co/learn/deep-rl-course/unitbonus3/decision-transformers)

决策 Transformer 模型由[Chen L.等人提出的“决策 Transformer：通过序列建模进行强化学习”](https://arxiv.org/abs/2106.01345)引入。它将强化学习抽象为一个条件序列建模问题。

主要思想是，**我们使用一个序列建模算法（Transformer），根据期望的回报、过去的状态和行动，生成未来的行动以实现期望的回报**，而不是使用 RL 方法训练策略，比如拟合值函数，告诉我们应该采取什么行动来最大化回报（累积奖励）。这是一个自回归模型，根据期望的回报、过去的状态和行动来生成未来的行动，以实现期望的回报。

这是对强化学习范式的完全转变，因为我们使用生成轨迹建模（建模状态、行动和奖励序列的联合分布）来替代传统的 RL 算法。这意味着在决策 Transformer 中，我们不是最大化回报，而是生成一系列未来的行动，以实现期望的回报。

🤗 Transformers 团队将决策 Transformer（一种离线强化学习方法）集成到库中，以及 Hugging Face Hub。

## 了解决策 Transformer

要了解更多关于决策 Transformer 的信息，您应该阅读我们撰写的博文[Introducing Decision Transformers on Hugging Face](https://huggingface.co/blog/decision-transformers)

## 训练您的第一个决策 Transformer

现在您已经了解了决策 Transformer 的工作原理，感谢[Introducing Decision Transformers on Hugging Face](https://huggingface.co/blog/decision-transformers)，您已经准备好学习如何从头开始训练您的第一个离线决策 Transformer 模型，让半猎豹奔跑。

从这里开始教程👉[`huggingface.co/blog/train-decision-transformers`](https://huggingface.co/blog/train-decision-transformers)

## 进一步阅读

要了解更多信息，建议查看以下资源：

+   [决策 Transformer：通过序列建模进行强化学习](https://arxiv.org/abs/2106.01345)

+   [在线决策 Transformer](https://arxiv.org/abs/2202.05607)

## 作者

这一部分由[Edward Beeching](https://twitter.com/edwardbeeching)撰写
