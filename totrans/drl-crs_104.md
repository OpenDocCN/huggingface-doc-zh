# 使用Sample-Factory介绍PPO

> 原文链接：[https://huggingface.co/learn/deep-rl-course/unit8/introduction-sf](https://huggingface.co/learn/deep-rl-course/unit8/introduction-sf)

![缩略图](../Images/1636e0752d93a1e41c70f4a1147a2563.png)

在第8单元的第二部分中，我们将通过使用[Sample-Factory](https://samplefactory.dev/)深入了解PPO优化，这是PPO算法的**异步实现**，用于训练我们的代理玩[vizdoom](https://vizdoom.cs.put.edu.pl/)（Doom的开源版本）。

在笔记本中，**您将训练您的代理玩健康收集关卡**，代理必须收集健康包以避免死亡。之后，您可以**训练您的代理玩更复杂的关卡，比如死斗**。

![环境](../Images/3244d92e568ba653445e92e451579990.png)

听起来很激动人心吗？让我们开始吧！🚀

这个实践是由[Hugging Face的机器学习研究科学家Edward Beeching](https://twitter.com/edwardbeeching)制作的。他曾在Godot强化学习代理上工作，这是一个用于在Godot游戏引擎中开发环境和代理的开源接口。
