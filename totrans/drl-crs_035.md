# 引入Q学习

> 原文链接：[https://huggingface.co/learn/deep-rl-course/unit2/q-learning](https://huggingface.co/learn/deep-rl-course/unit2/q-learning)

## 什么是Q学习？

Q学习是一种离策略基于价值的方法，它使用TD方法来训练其动作价值函数：

+   离策略：我们将在本单元末讨论这个问题。

+   基于价值的方法：通过训练一个值或动作价值函数间接找到最优策略，该函数将告诉我们每个状态或每个状态-动作对的价值。

+   TD方法：在每一步更新其动作价值函数，而不是在每一集的结束时。

Q学习是我们用来训练我们的Q函数的算法，一个动作价值函数，确定在特定状态下采取特定动作的价值。

![Q函数](../Images/d98598351e812f60049067f862f79c69.png)

给定一个状态和动作，我们的Q函数输出一个状态-动作值（也称为Q值）

Q来自于“那个状态的那个动作的质量”（价值）。

让我们回顾一下价值和奖励之间的区别：

+   一个状态的价值，或者一个状态-动作对的价值，是我们的代理根据其策略在这个状态（或状态-动作对）开始并相应地行动时获得的预期累积奖励。

+   奖励是在状态执行动作后从环境中获得的反馈。

在内部，我们的Q函数由一个Q表编码，一个表格，其中每个单元格对应一个状态-动作对值。将这个Q表看作是我们Q函数的记忆或备忘录。

让我们通过一个迷宫的例子来说明。

![迷宫示例](../Images/444a72a9a003265b612877410c530a95.png)

Q表被初始化。这就是为什么所有值都等于0。这个表包含每个状态和动作的相应状态-动作值。对于这个简单的例子，状态仅由鼠标的位置定义。因此，我们的Q表中有2*3行，每一行对应鼠标的每个可能位置。在更复杂的情况下，状态可能包含比演员位置更多的信息。

![迷宫示例](../Images/cd71daf8ad310d839422ce684bcb5c10.png)

在这里我们看到，初始状态和向上的状态-动作值为0：

![迷宫示例](../Images/6b2018a29d1825bf6cbb14397a596eae.png)

因此：Q函数使用一个Q表，其中包含每个状态-动作对的价值。给定一个状态和动作，我们的Q函数将在其Q表中搜索以输出该值。

![Q函数](../Images/c6f51357ba01781edc9f3041b33e5be4.png)

如果我们回顾一下，Q学习是强化学习算法，它是：

+   训练一个Q函数（一个动作价值函数），它内部是一个包含所有状态-动作对值的Q表。

+   给定一个状态和动作，我们的Q函数将在其Q表中搜索相应的值。

+   当训练完成时，我们有一个最优Q函数，这意味着我们有一个最优Q表。

+   如果我们有一个最优Q函数，我们就有一个最优策略，因为我们知道在每个状态下采取的最佳动作。

![链接价值策略](../Images/06e7785cc764e6109bfc6c89005a4d92.png)

一开始，我们的Q表是无用的，因为它为每个状态-动作对给出任意值（大多数情况下，我们将Q表初始化为0）。随着代理探索环境并更新Q表，它将给出对最优策略的越来越好的近似。

![Q-learning](../Images/5eeae1d543b1ae0da4b6f9afe9fe07c9.png)

我们在这里看到，通过训练，我们的Q表更好了，因为有了它，我们可以知道每个状态-动作对的价值。

现在我们了解了Q学习、Q函数和Q表是什么，让我们深入了解Q学习算法。

## Q学习算法

这是Q-Learning伪代码；让我们在实现之前研究每个部分，**看看在实现之前如何使用一个简单的例子。**不要被吓到，它比看起来简单！我们将逐步讲解每个步骤。

![Q-learning](../Images/e98aadd735672374a66857c170d3b2ce.png)

### 步骤1：我们初始化Q表

![Q-learning](../Images/01250a85fb5041af0c4b2aaf8c987543.png)

我们需要为每个状态-动作对初始化Q表。**大多数情况下，我们使用值为0进行初始化。**

### 步骤2：使用ε-贪心策略选择一个动作

![Q-learning](../Images/30b0aba4490af7f85f0594dc198e9c03.png)

ε-贪心策略是一种处理探索/利用权衡的策略。

这个想法是，初始值为ɛ=1.0时：

+   *概率为1-ε*：我们进行**利用**（即我们的代理选择具有最高状态-动作对值的动作）。

+   以概率ɛ：**我们进行探索**（尝试随机动作）。

在训练开始时，**由于ɛ非常高，进行探索的概率将非常大，因此大部分时间我们将进行探索。**但随着训练的进行，因此我们的**Q表在估计方面变得越来越好，我们逐渐减少ε值**，因为我们将需要越来越少的探索和更多的利用。

![Q-learning](../Images/4d0d8f523643ebe543960e9ea3a2a4b7.png)

### 步骤3：执行动作At，获得奖励Rt+1和下一个状态St+1

![Q-learning](../Images/f834496430b9ed9ec65c64061d432454.png)

### 步骤4：更新Q(St, At)

请记住，在TD学习中，我们在一步交互之后更新我们的策略或值函数（取决于我们选择的RL方法）。 

为了生成我们的TD目标，**我们使用了即时奖励<math><semantics><mrow><msub><mi>R</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub></mrow><annotation encoding="application/x-tex">R_{t+1}</annotation></semantics></math>Rt+1​加上下一个状态的折现值**，通过找到最大化下一个状态当前Q函数的动作来计算。（我们称之为自举）。

![Q-learning](../Images/bdb95512c529cc741a6379b8a07de2b0.png)

因此，我们的<math><semantics><mrow><mi>Q</mi><mo stretchy="false">(</mo><msub><mi>S</mi><mi>t</mi></msub><mo separator="true">,</mo><msub><mi>A</mi><mi>t</mi></msub><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">Q(S_t, A_t)</annotation></semantics></math>Q(St​,At​) **更新公式如下：**

![Q-learning](../Images/0bfe74186cd45c67a2935f61d93e0937.png)

这意味着为了更新我们的<math><semantics><mrow><mi>Q</mi><mo stretchy="false">(</mo><msub><mi>S</mi><mi>t</mi></msub><mo separator="true">,</mo><msub><mi>A</mi><mi>t</mi></msub><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">Q(S_t, A_t)</annotation></semantics></math>Q(St​,At​)：

+   我们需要<math><semantics><mrow><msub><mi>S</mi><mi>t</mi></msub><mo separator="true">,</mo><msub><mi>A</mi><mi>t</mi></msub><mo separator="true">,</mo><msub><mi>R</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub><mo separator="true">,</mo><msub><mi>S</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub></mrow><annotation encoding="application/x-tex">S_t, A_t, R_{t+1}, S_{t+1}</annotation></semantics></math>St​,At​,Rt+1​,St+1​。

+   为了更新给定状态-动作对的Q值，我们使用TD目标。

我们如何形成TD目标？

1.  我们在执行动作<math><semantics><mrow><msub><mi>A</mi><mi>t</mi></msub></mrow><annotation encoding="application/x-tex">A_t</annotation></semantics></math>At后获得奖励<math><semantics><mrow><msub><mi>R</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub></mrow><annotation encoding="application/x-tex">R_{t+1}</annotation></semantics></math>Rt+1​。

1.  为了获得下一个状态的**最佳状态-动作对值**，我们使用贪心策略选择下一个最佳动作。请注意，这不是ε-贪心策略，这将始终选择具有最高状态-动作值的动作。

然后当这个Q值更新完成后，我们从一个新状态开始，并再次使用ε-贪心策略选择我们的动作。

**这就是为什么我们说Q学习是一种离线策略算法。**

## 离线策略 vs 在线策略

这种差异微妙：

+   *离线策略*：在行动（推理）和更新（训练）中使用**不同的策略。**

例如，对于Q学习，ε-贪心策略（行动策略）与贪心策略不同，后者**用于选择最佳的下一个状态动作值来更新我们的Q值（更新策略）。**

![离线策略](../Images/962ba1bb9bae9ace4837824a79c98010.png)

行动策略

与我们在训练过程中使用的策略不同：

![离线策略](../Images/e14d304b020044952e6f8de20e71e925.png)

更新策略

+   *在线策略*：在行动和更新中使用**相同的策略。**

例如，对于Sarsa，另一个基于值的算法，**ε-贪心策略选择下一个状态-动作对，而不是贪心策略。**

![离线策略](../Images/b57fef603ca195b7a4707f530c6082f7.png)

Sarsa

![离线策略](../Images/ce691ce98ae89b58669eb975be3f446c.png)
