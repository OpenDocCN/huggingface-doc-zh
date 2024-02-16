# 测验

> 原始文本：[https://huggingface.co/learn/deep-rl-course/unit3/quiz](https://huggingface.co/learn/deep-rl-course/unit3/quiz)

学习和[避免自以为是的错觉](https://www.coursera.org/lecture/learning-how-to-learn/illusions-of-competence-BuFzf)的最佳方法**是测试自己**。这将帮助您找到**需要加强知识的地方**。

### Q1: 我们提到 Q 学习是一种表格方法。什么是表格方法？

<details data-svelte-h="svelte-gsebop"><summary>解决方案</summary>

*表格方法*是一种问题类型，其中状态和动作空间足够小，可以将值函数近似为**数组和表**。例如，**Q 学习是一种表格方法**，因为我们使用表来表示状态和动作值对。</details>

### Q2: 为什么我们不能使用经典的 Q 学习来解决 Atari 游戏？

### Q3: 当我们在深度 Q 学习中将四个帧堆叠在一起时，为什么我们要这样做？

<details data-svelte-h="svelte-nzbq54"><summary>解决方案</summary>

我们将帧堆叠在一起，因为这有助于我们**处理时间限制的问题**：一个帧不足以捕捉时间信息。例如，在乒乓球中，如果我们只有一个帧，我们的代理**将无法知道球的方向**。

![时间限制](../Images/916225d18ad696514245f8c4e88a5a56.png) ![时间限制](../Images/e35a3e3cfeefe6f7a16b681ab91dfa7b.png)</details>

### Q4: 深度 Q 学习的两个阶段是什么？

### Q5: 为什么我们在深度 Q 学习中创建重放内存？

<details data-svelte-h="svelte-10rxkt3"><summary>解决方案</summary>

**1\. 在训练期间更有效地利用经验**

通常，在在线强化学习中，代理与环境互动，获得经验（状态、动作、奖励和下一个状态），从中学习（更新神经网络），然后丢弃它们。这并不高效。但是，通过经验重放，**我们创建一个保存经验样本的重放缓冲区，可以在训练期间重复使用**。

**2\. 避免忘记先前的经验并减少经验之间的相关性**

如果我们将经验的顺序样本提供给我们的神经网络，问题就会出现，因为它**倾向于忘记先前的经验，因为它会覆盖新的经验**。例如，如果我们在第一关然后是不同的第二关，我们的代理可能会忘记如何在第一关中行为和玩耍。</details>

### Q6: 我们如何使用双重深度 Q 学习？

<details data-svelte-h="svelte-1u03gxl"><summary>解决方案</summary>

当我们计算 Q 目标时，我们使用两个网络来解耦动作选择和目标 Q 值生成。我们：

+   使用我们的*DQN 网络*来**选择下一个状态要采取的最佳行动**（具有最高 Q 值的行动）。

+   使用我们的*目标网络*来计算**在下一个状态采取该动作的目标 Q 值**。</details>

恭喜您完成了这个测验🥳，如果您错过了一些元素，请花时间再次阅读章节，以加强（😏）您的知识。
