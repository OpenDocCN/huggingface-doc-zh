# Huggy的工作方式

> 原始文本：[https://huggingface.co/learn/deep-rl-course/unitbonus1/how-huggy-works](https://huggingface.co/learn/deep-rl-course/unitbonus1/how-huggy-works)

Huggy是由Hugging Face制作的深度强化学习环境，基于[Unity MLAgents团队的项目Puppo the Corgi](https://blog.unity.com/technology/puppo-the-corgi-cuteness-overload-with-the-unity-ml-agents-toolkit)。这个环境是使用[Unity游戏引擎](https://unity.com/)和[MLAgents](https://github.com/Unity-Technologies/ml-agents)创建的。ML-Agents是Unity游戏引擎的一个工具包，允许我们**使用Unity创建环境或使用预先制作的环境来训练我们的代理**。

![Huggy](../Images/57b557f6c646b773c9c9c0173a5adf4f.png)

在这个环境中，我们的目标是训练Huggy**去接我们扔的棍子。这意味着他需要正确地朝着棍子移动**。

## 状态空间，Huggy所感知的。

Huggy并不“看到”他的环境。相反，我们为他提供了关于环境的信息：

+   目标（棍子）位置

+   他自己和目标之间的相对位置

+   他的腿的方向。

考虑到所有这些信息，Huggy可以**使用他的策略来确定下一步该采取哪个动作来实现他的目标**。

## 动作空间，Huggy可以执行的动作

![Huggy动作](../Images/df930c385e7a4439c314cb4356dc2db4.png)

**关节电机驱动Huggy的腿**。这意味着为了到达目标，Huggy需要**学会正确旋转他每条腿的关节电机，以便他可以移动**。

## 奖励函数

奖励函数设计得**让Huggy实现他的目标**：接棍子。

请记住，强化学习的基础之一是*奖励假设*：一个目标可以描述为**最大化预期累积奖励**。

在这里，我们的目标是让Huggy**朝着棍子走，但不要旋转太多**。因此，我们的奖励函数必须体现这个目标。

我们的奖励函数：

![Huggy奖励函数](../Images/02ce46ce021fe61f090b79e963e5fc0e.png)

+   *方向奖励*：我们**奖励他靠近目标**。

+   *时间惩罚*：在每个动作中给予固定时间惩罚，以**迫使他尽快到达棍子**。

+   *旋转惩罚*：如果**他旋转太多并且转动太快**，我们会惩罚Huggy。

+   *到达目标奖励*：我们奖励Huggy**到达目标**。

如果您想看看这个奖励函数在数学上是什么样子，请查看[Puppo the Corgi演示](https://blog.unity.com/technology/puppo-the-corgi-cuteness-overload-with-the-unity-ml-agents-toolkit)。

## 训练Huggy

Huggy的目标是**学会正确快速地朝着目标奔跑**。为了做到这一点，在每一步骤中，根据环境观察，他需要决定如何旋转他的腿的每个关节电机，以正确地移动（不要旋转太多）并朝着目标移动。

训练循环如下：

![Huggy循环](../Images/0086bd47ee63acbf18ec6dafecaadb0d.png)

训练环境如下：

![Huggy训练环境](../Images/2835c90e4b4250494efe778ec6a42cfd.png)

这是一个**棍子随机生成的地方**。当Huggy到达时，棍子会在其他地方生成。我们为训练构建了**多个环境的副本**。这有助于通过提供更多不同的体验来加快训练速度。

现在您已经了解了环境的整体情况，您可以开始训练Huggy去接棍子了。

为了做到这一点，我们将使用[MLAgents](https://github.com/Unity-Technologies/ml-agents)。如果你以前从未使用过，不用担心。在本单元中，我们将使用Google Colab来训练Huggy，然后您将能够加载您训练过的Huggy并直接在浏览器中与他互动。

在未来的单元中，我们将更深入地研究MLAgents并了解它的工作原理。但现在，我们只需使用提供的实现来保持简单。
