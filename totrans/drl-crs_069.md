# Unity ML-Agents简介

> 原文：[https://huggingface.co/learn/deep-rl-course/unit5/introduction](https://huggingface.co/learn/deep-rl-course/unit5/introduction)

![缩略图](../Images/de76e57f9c05ee846deb2b4e602f2f0b.png)

在强化学习中的一个挑战是**创建环境**。幸运的是，我们可以利用游戏引擎来实现这一点。这些引擎，如[Unity](https://unity.com/)、[Godot](https://godotengine.org/)或[虚幻引擎](https://www.unrealengine.com/)，是用来创建视频游戏的程序。它们非常适合创建环境：它们提供物理系统、2D/3D渲染等功能。

其中一个，[Unity](https://unity.com/)，创建了[Unity ML-Agents工具包](https://github.com/Unity-Technologies/ml-agents)，这是一个基于游戏引擎Unity的插件，允许我们**使用Unity游戏引擎作为环境构建器来训练代理**。在第一个奖励单元中，这就是我们用来训练Huggy抓住棍子的方法！

![MLAgents环境](../Images/d46c22cdfa0b477a79175309ef8a1d53.png)

来源：[ML-Agents文档](https://github.com/Unity-Technologies/ml-agents)

Unity ML-Agents工具包提供了许多出色的预制环境，从踢足球、学习行走到跳过高墙。

在本单元中，我们将学习如何使用ML-Agents，但是**如果您不知道如何使用Unity游戏引擎也不用担心**：您不需要使用它来训练您的代理。

所以，今天，我们将训练两个代理：

+   第一个将学会**向生成的目标发射雪球**。

+   第二个需要**按下一个按钮生成一个金字塔，然后导航到金字塔，将其推倒，然后移动到顶部的金砖**。为了做到这一点，它将需要探索环境，这将使用一种称为好奇心的技术来完成。

![环境](../Images/a536e3342e9e887aef01941b6e7dc2d3.png)

然后，在训练之后，**您将把训练好的代理推送到Hugging Face Hub**，您将能够**直接在浏览器上可视化它们的游戏，而无需使用Unity编辑器**。

完成这个单元将**为您准备好下一个挑战：AI对抗AI，在那里您将在多代理环境中训练代理并与您的同学代理竞争**。

听起来很激动人心吗？让我们开始吧！
