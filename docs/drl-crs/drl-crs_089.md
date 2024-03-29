# 设计多智能体系统

> 原文链接：[`huggingface.co/learn/deep-rl-course/unit7/multi-agent-setting`](https://huggingface.co/learn/deep-rl-course/unit7/multi-agent-setting)

在这一部分，您将观看由[Brian Douglas](https://www.youtube.com/channel/UCq0imsn84ShAe9PBOFnoIrg)制作的关于多智能体的优秀介绍。

[`www.youtube-nocookie.com/embed/qgb0gyrpiGk`](https://www.youtube-nocookie.com/embed/qgb0gyrpiGk)

在这个视频中，Brian 谈到了如何设计多智能体系统。他特别提到了一个多智能体吸尘器系统，并问：**它们如何可以相互合作**？

我们有两种解决方案来设计这个多智能体强化学习系统（MARL）。

## 分散式系统

![Decentralized](img/d82bc536a16b8e15791ba574b6f9f35f.png)

来源：[多智能体强化学习介绍](https://www.youtube.com/watch?v=qgb0gyrpiGk)

在分散式学习中，**每个智能体都是独立训练的**。在给定的示例中，每个吸尘器都学习尽可能多地清洁地方**而不关心其他吸尘器（智能体）在做什么**。

好处在于**由于智能体之间没有共享信息，这些吸尘器可以像我们训练单个智能体一样设计和训练**。

这里的想法是**我们的训练智能体将把其他智能体视为环境动态的一部分**。而不是智能体。

然而，这种技术的一个重大缺点是，它会**使环境变得非静止**，因为随着其他智能体也在环境中交互，潜在的马尔可夫决策过程会随时间变化。这对许多强化学习算法来说是有问题的，**因为它们无法在非静止环境中达到全局最优**。

## 集中式方法

![Centralized](img/5eb69229013368a2d3d27ad287a597e8.png)

来源：[多智能体强化学习介绍](https://www.youtube.com/watch?v=qgb0gyrpiGk)

在这种架构中，**我们有一个收集智能体经验的高级过程**：经验缓冲区。我们将使用这些经验**来学习一个共同的策略**。

例如，在吸尘器示例中，观察将是：

+   吸尘器的覆盖地图。

+   所有吸尘器的位置。

我们利用这种集体经验**来训练一个策略，以最有利的方式移动所有三个机器人作为一个整体**。因此，每个机器人都从他们的共同经验中学习。现在我们有一个静止的环境，因为所有智能体都被视为一个更大的实体，并且他们知道其他智能体策略的变化（因为它与他们的相同）。

如果我们回顾一下：

+   在*分散式方法*中，我们**独立地处理所有智能体，而不考虑其他智能体的存在**。

    +   在这种情况下，所有智能体**将其他智能体视为环境的一部分**。

    +   **这是一个非静止环境条件**，所以没有收敛的保证。

+   在*集中式方法*中：

    +   **从所有智能体中学习单一策略**。

    +   将环境的当前状态和策略输出联合动作作为输入。

    +   奖励是全局的。
