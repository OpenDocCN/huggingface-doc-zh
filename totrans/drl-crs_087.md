# 介绍

> 原文：[https://huggingface.co/learn/deep-rl-course/unit7/introduction](https://huggingface.co/learn/deep-rl-course/unit7/introduction)

![缩略图](../Images/191cf822590d58b820b7b1a8cc193fb2.png)

自课程开始以来，我们学习了如何在*单代理系统*中训练代理程序，其中我们的代理程序独自在其环境中：它**不与其他代理程序合作或协作**。

这种方法非常有效，单代理系统对许多应用程序都有用。

![拼贴](../Images/0bfa9643ea17c46664be61fcfb3e7a48.png)

自课程开始以来，您已经训练了代理程序的所有环境的拼贴

但是，作为人类，**我们生活在一个多代理世界中**。我们的智能来自与其他代理的互动。因此，我们的**目标是创建可以与其他人类和其他代理互动的代理程序**。

因此，我们必须研究如何在*多代理系统*中训练深度强化学习代理程序，以构建能够适应、合作或竞争的强大代理程序。

因此，今天我们将**学习多代理强化学习（MARL）这一迷人主题的基础知识**。

最令人兴奋的部分是，在这个单元中，您将在一个多代理系统中训练您的第一个代理程序：**一个2对2的足球队，需要击败对手队**。

您将参加**AI对AI挑战**，在这个挑战中，您训练的代理程序将每天与其他同学的代理程序竞争，并在[新排行榜](https://huggingface.co/spaces/huggingface-projects/AIvsAI-SoccerTwos)上排名。

![SoccerTwos](../Images/b8d7d800c316a50a5f64472742088b73.png)

这个环境是由[Unity MLAgents团队](https://github.com/Unity-Technologies/ml-agents)制作的

所以让我们开始吧！
