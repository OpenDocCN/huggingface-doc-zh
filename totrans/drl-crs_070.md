# Unity ML-Agents如何工作？

> 原文：[https://huggingface.co/learn/deep-rl-course/unit5/how-mlagents-works](https://huggingface.co/learn/deep-rl-course/unit5/how-mlagents-works)

在训练我们的代理之前，我们需要了解**ML-Agents是什么以及它是如何工作的**。

## 什么是Unity ML-Agents？

[Unity ML-Agents](https://github.com/Unity-Technologies/ml-agents)是Unity游戏引擎的工具包，**允许我们使用Unity创建环境或使用预先制作的环境来训练我们的代理**。

它由[Unity Technologies](https://unity.com/)开发，是Unity的开发者之一，Unity是最著名的游戏引擎之一，被Firewatch、Cuphead和Cities: Skylines的创作者使用。

![Firewatch](../Images/56b87aa0405b4b90defdc70caa6f9571.png)

Firewatch是用Unity制作的

## 六个组件

使用Unity ML-Agents，您有六个基本组件：

![MLAgents](../Images/f1083a6f889f83b0d7ba8f82fe1c00da.png)

来源：[Unity ML-Agents文档](https://unity-technologies.github.io/ml-agents/)

+   第一个是*学习环境*，其中包含**Unity场景（环境）和环境元素**（游戏角色）。

+   第二个是*Python低级API*，其中包含**用于与环境交互和操作的低级Python接口**。这是我们用来启动训练的API。

+   然后，我们有*外部通信器*，它**连接学习环境（用C#制作）与低级Python API（Python）**。

+   *Python训练器*：使用PyTorch制作的**强化学习算法（PPO、SAC等）**。

+   *Gym包装器*：将RL环境封装在gym包装器中。

+   *PettingZoo包装器*：PettingZoo是gym包装器的多代理版本。

## 在学习组件内部

在学习组件内部，我们有**两个重要元素**：

+   第一个是*代理组件*，场景的执行者。我们将通过优化其策略来**训练代理**（这将告诉我们在每个状态下应该采取什么行动）。策略被称为*Brain*。

+   最后，有*学院*。这个组件**协调代理和它们的决策过程**。将这个学院想象成一个处理Python API请求的老师。

为了更好地理解其作用，让我们回顾一下RL过程。这可以被建模为一个循环，工作方式如下：

![RL过程](../Images/018079078cf4ad9c782cc74fc0ce7a20.png)

RL过程：一个状态、动作、奖励和下一个状态的循环

来源：[强化学习：一种介绍，Richard Sutton和Andrew G. Barto](http://incompleteideas.net/book/RLbook2020.pdf)

现在，让我们想象一个代理学习玩平台游戏。RL过程看起来像这样：

![RL过程](../Images/79d6e90ecca40e7412a5ae37c07bf478.png)

+   我们的代理从**环境**接收**状态<math><semantics><mrow><msub><mi>S</mi><mn>0</mn></msub></mrow><annotation encoding="application/x-tex">S_0</annotation></semantics></math>S0** — 我们接收游戏的第一帧（环境）。

+   基于那个**状态<math><semantics><mrow><msub><mi>S</mi><mn>0</mn></msub></mrow><annotation encoding="application/x-tex">S_0</annotation></semantics></math>S0**，代理会采取**动作<math><semantics><mrow><msub><mi>A</mi><mn>0</mn></msub></mrow><annotation encoding="application/x-tex">A_0</annotation></semantics></math>A0** — 我们的代理会向右移动。

+   环境进入一个**新的**状态<math><semantics><mrow><msub><mi>S</mi><mn>1</mn></msub></mrow><annotation encoding="application/x-tex">S_1</annotation></semantics></math>S1** — 新的帧。

+   环境给代理一些**奖励<math><semantics><mrow><msub><mi>R</mi><mn>1</mn></msub></mrow><annotation encoding="application/x-tex">R_1</annotation></semantics></math>R1** — 我们还没有死亡（正奖励+1）。

这个RL循环输出一个序列**状态、动作、奖励和下一个状态**。代理的目标是**最大化预期的累积奖励**。

学院将会**向我们的智能体发送指令，并确保智能体同步**：

+   收集观察数据

+   使用你的策略选择动作

+   执行动作

+   如果你达到了最大步数或者已经完成了，就重置。

![MLAgents学院](../Images/ce5e350e3e33e7b48aee5a99e9c2666d.png)

现在我们了解了ML-Agents的工作原理，**我们准备好训练我们的智能体了。**
