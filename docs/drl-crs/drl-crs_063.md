# 术语表

> 原文链接：[`huggingface.co/learn/deep-rl-course/unit4/glossary`](https://huggingface.co/learn/deep-rl-course/unit4/glossary)

这是一个由社区创建的术语表。欢迎贡献！

+   **深度 Q 学习：** 一种基于值的深度强化学习算法，使用深度神经网络来逼近给定状态下的动作的 Q 值。深度 Q 学习的目标是通过学习动作值来找到最优策略，从而最大化期望的累积奖励。

+   基于价值的方法：估计值函数作为找到最优策略的中间步骤的强化学习方法。

+   基于策略的方法：直接学习逼近最优策略而不学习值函数的强化学习方法。在实践中，它们输出动作的概率分布。

    使用策略梯度方法而不是基于价值的方法的好处包括：

    +   集成的简单性：无需存储动作值；

    +   学习随机策略的能力：代理探索状态空间而不总是采取相同的轨迹，并避免感知混淆的问题；

    +   在高维和连续动作空间中的有效性；以及

    +   改进的收敛性质。

+   **策略梯度：** 策略梯度方法的一个子集，其目标是通过梯度上升最大化参数化策略的性能。策略梯度的目标是通过调整策略来控制动作的概率分布，使得未来更频繁地采样好的动作（最大化回报）。

+   **蒙特卡洛强化学习：** 一种策略梯度算法，使用整个回合的估计回报来更新策略参数。

如果您想改进课程，可以[提交拉取请求。](https://github.com/huggingface/deep-rl-class/pulls)

这个术语表得以实现，感谢：

+   [Diego Carpintero](https://github.com/dcarpintero)
