# 强化学习中的“深度”

> 原文链接：[https://huggingface.co/learn/deep-rl-course/unit1/deep-rl](https://huggingface.co/learn/deep-rl-course/unit1/deep-rl)

到目前为止我们讨论的是强化学习。但是“深度”是如何发挥作用的呢？

深度强化学习引入了**深度神经网络来解决强化学习问题**，因此得名“深度”。

例如，在下一个单元中，我们将学习两种基于价值的算法：Q学习（经典强化学习）和深度Q学习。

你会看到区别在于，在第一种方法中，**我们使用传统算法**来创建一个Q表，帮助我们找到每个状态下应该采取的动作。

在第二种方法中，**我们将使用神经网络**（来近似Q值）。

![基于价值的强化学习](../Images/1e4c6acfdf811be054c82941f53e5853.png)

灵感来自Udacity的Q学习笔记本

如果您对深度学习不熟悉，一定要观看[FastAI实用深度学习课程](https://course.fast.ai)（免费）。
