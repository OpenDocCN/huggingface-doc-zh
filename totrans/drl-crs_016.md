# 测验

> 原始文本：[https://huggingface.co/learn/deep-rl-course/unit1/quiz](https://huggingface.co/learn/deep-rl-course/unit1/quiz)

学习和[避免能力错觉](https://www.coursera.org/lecture/learning-how-to-learn/illusions-of-competence-BuFzf)的最佳方法是测试自己。这将帮助你找到需要加强知识的地方。

### Q1：什么是强化学习？

<details data-svelte-h="svelte-hwjwin"><summary>解决方案</summary>

强化学习是通过与环境进行试错交互并接收奖励（积极或消极）作为独特反馈来解决控制任务（也称为决策问题）的框架。</details>

### Q2：定义RL循环

![练习RL循环](../Images/7dd50ea3f5814cad587739a6db3ce516.png)

在每一步：

+   我们的Agent从环境中接收__。

+   基于那个__，Agent采取一个__。

+   我们的Agent将向右移动

+   环境进入一个__。

+   环境给Agent一个__。

### Q3：状态和观察之间有什么区别？

### Q4：任务是强化学习问题的一个实例。有哪两种类型的任务？

### Q5：什么是探索/开发权衡？

<details data-svelte-h="svelte-lhagbu"><summary>解决方案</summary>

在强化学习中，我们需要平衡探索环境和利用我们对环境的了解的程度。

+   *探索*是通过尝试随机动作来探索环境，以便更多地了解环境。

+   *开发*是利用已知信息来最大化奖励。

![探索开发权衡](../Images/3a59b593e994b9d356515c58b0fa6a24.png)</details>

### Q6：什么是策略？

<details data-svelte-h="svelte-su9zbv"><summary>解决方案</summary>

+   策略π是我们Agent的大脑。它是告诉我们在特定状态下应该采取什么行动的函数。因此，它定义了Agent在特定时间的行为。

![策略](../Images/83518e23a957f171ab1fe3fa7a6bbe35.png)</details>

### Q7：什么是基于价值的方法？

<details data-svelte-h="svelte-g9n3n1"><summary>解决方案</summary>

+   基于价值的方法是解决RL问题的主要方法之一。

+   在基于价值的方法中，我们不是训练一个策略函数，而是训练一个价值函数，将一个状态映射到在该状态的预期价值。</details>

### Q8：什么是基于策略的方法？

<details data-svelte-h="svelte-aae5od"><summary>解决方案</summary>

+   在*基于策略的方法*中，我们直接学习一个策略函数。

+   这个策略函数将从每个状态映射到该状态的最佳对应动作。或者是该状态可能动作集合上的概率分布。</details>

恭喜你完成了这个测验🥳，如果你错过了一些元素，请花时间再次阅读章节，以加强（😏）你的知识，但不要担心：在课程中，我们将再次讨论这些概念，并且你将通过实践加强你的理论知识。
