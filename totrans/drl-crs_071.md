# SnowballTarget环境

> 原始文本：[https://huggingface.co/learn/deep-rl-course/unit5/snowball-target](https://huggingface.co/learn/deep-rl-course/unit5/snowball-target)

![SnowballTarget](../Images/fb625fa1ee3e280912baaaa56548960f.png)

SnowballTarget是我们在Hugging Face创建的一个环境，使用了[Kay Lousberg](https://kaylousberg.com/)的资源。如果您想学习如何使用Unity创建自己的环境，我们在本单元末尾有一个可选部分。

## 代理的目标

您将要训练的第一个代理叫做Julien the bear 🐻。Julien被训练**用雪球击中目标**。

在这个环境中的目标是，Julien **在有限的时间内击中尽可能多的目标**（1000个时间步）。它需要**正确地定位自己与目标的关系并射击**来实现这一目标。

此外，为了避免“雪球垃圾邮件”（即每个时间步都射出一个雪球），**Julien有一个“冷却”系统**（需要等待0.5秒后才能再次射击）。

![Cool Off System](../Images/c50f4eb3112c190598a995d006aece23.png)

代理需要等待0.5秒才能再次射出雪球

## 奖励函数和奖励工程问题

奖励函数很简单。**每当代理的雪球击中目标时，环境会给予+1的奖励**。因为代理的目标是最大化预期的累积奖励，**它将尽可能击中更多的目标**。

![Reward system](../Images/97bb6395a1762bfb94d1754c058aa292.png)

我们可以有一个更复杂的奖励函数（例如，对代理施加惩罚以促使其更快行动）。但是当设计环境时，您需要避免*奖励工程问题*，即使用过于复杂的奖励函数来强制代理按您希望的方式行事。为什么？因为这样做，**您可能会错过代理将通过更简单的奖励函数找到的有趣策略**。

在代码方面，它看起来像这样：

![Reward](../Images/7b73d2b4241bd0c68e89efd1fe743505.png)

## 观察空间

关于观察，我们不使用正常的视觉（帧），而是**使用射线投射**。

将射线投射想象成激光，它们将检测是否穿过物体。

![Raycasts](../Images/93b379dd693a76d5472608228f47e6f3.png)

来源：[ML-Agents文档](https://github.com/Unity-Technologies/ml-agents)

在这个环境中，我们的代理有多组射线投射：

![Raycasts](../Images/04047cdc3e12f4d8b6fc04d527b49c3e.png)

除了射线投射外，代理还会得到一个“我能射击吗”的布尔值作为观察。

![Obs](../Images/781b3f0cb3bdaeaea583e42caa651ef3.png)

## 动作空间

动作空间是离散的：

![Action Space](../Images/1a9d30bdfb0e6cf6ea4e7c6189d64c1f.png)
