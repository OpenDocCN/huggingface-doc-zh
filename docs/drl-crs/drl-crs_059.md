# 什么是基于策略的方法？

> 原文链接：[`huggingface.co/learn/deep-rl-course/unit4/what-are-policy-based-methods`](https://huggingface.co/learn/deep-rl-course/unit4/what-are-policy-based-methods)

强化学习的主要目标是找到将最大化预期累积奖励的最优策略π∗。因为强化学习基于“奖励假设”：所有目标都可以描述为最大化预期累积奖励。

例如，在足球比赛中（您将在两个单位中训练代理人），目标是赢得比赛。我们可以将这个目标描述为强化学习中最大化进球数（当球越过球门线时）到对手的足球门中，并最小化自己的足球门中的进球数。

![足球](img/a9c2200aa04bae4394f998a72fe3492d.png)

## 基于值、基于策略和演员-评论家方法

在第一个单元中，我们看到了两种找到（或者大多数情况下，近似）最优策略π∗的方法。

+   在基于值的方法中，我们学习一个值函数。

    +   这个想法是，一个最优值函数会导致一个最优策略π∗。

    +   我们的目标是最小化预测值和目标值之间的损失，以逼近真实的动作值函数。

    +   我们有一个政策，但它是隐含的，因为它直接从值函数生成。例如，在 Q-Learning 中，我们使用了一个（epsilon-）贪婪策略。

+   另一方面，在基于策略的方法中，我们直接学习近似π∗，而无需学习值函数。

    +   这个想法是对策略进行参数化。例如，使用神经网络πθ，这个策略将输出动作的概率分布（随机策略）。

    +   ![随机策略](img/9123df7ebedfa0c5bc669c2d2531968f.png)

    +   我们的目标是通过梯度上升来最大化参数化策略的性能。

    +   为了做到这一点，我们控制将影响状态上动作分布的参数θ。

![基于策略](img/7b4b24746a62f4244cc0e64f74bdaef3.png)

+   下次，我们将学习演员-评论家方法，这是基于值和基于策略方法的结合。

因此，借助基于策略的方法，我们可以直接优化我们的策略<math><semantics><mrow><msub><mi>π</mi><mi>θ</mi></msub></mrow><annotation encoding="application/x-tex">\pi_\theta</annotation></semantics></math>πθ​ 以输出一个动作的概率分布<math><semantics><mrow><msub><mi>π</mi><mi>θ</mi></msub><mo stretchy="false">(</mo><mi>a</mi><mi mathvariant="normal">∣</mi><mi>s</mi><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">\pi_\theta(a|s)</annotation></semantics></math>πθ​(a∣s)，从而获得最佳的累积回报。为此，我们定义一个目标函数<math><semantics><mrow><mi>J</mi><mo stretchy="false">(</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">J(\theta)</annotation></semantics></math>J(θ)，即预期的累积奖励，并且**希望找到最大化这个目标函数的值<math><semantics><mrow><mi>θ</mi></mrow><annotation encoding="application/x-tex">\theta</annotation></semantics></math>θ**。

## 基于策略和策略梯度方法之间的区别

策略梯度方法，也就是我们将在本单元中学习的方法，是基于策略的方法的一个子类。在基于策略的方法中，优化大部分时间都是*on-policy*的，因为对于每次更新，我们只使用由我们最新版本的<math><semantics><mrow><msub><mi>π</mi><mi>θ</mi></msub></mrow><annotation encoding="application/x-tex">\pi_\theta</annotation></semantics></math>πθ​ 收集的数据（轨迹）。

这两种方法之间的区别**在于我们如何优化参数**<math><semantics><mrow><mi>θ</mi></mrow><annotation encoding="application/x-tex">\theta</annotation></semantics></math>θ：

+   在*基于策略的方法*中，我们直接寻找最优策略。我们可以通过像爬山、模拟退火或进化策略等技术来最大化目标函数的局部近似来**间接**优化参数<math><semantics><mrow><mi>θ</mi></mrow><annotation encoding="application/x-tex">\theta</annotation></semantics></math>θ。

+   在*策略梯度方法*中，因为它是基于策略的方法的一个子类，我们直接寻找最优策略。但我们通过在目标函数<math><semantics><mrow><mi>J</mi><mo stretchy="false">(</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">J(\theta)</annotation></semantics></math>J(θ)的性能上执行梯度上升来**直接**优化参数<math><semantics><mrow><mi>θ</mi></mrow><annotation encoding="application/x-tex">\theta</annotation></semantics></math>θ。

在更深入研究策略梯度方法的工作原理（目标函数、策略梯度定理、梯度上升等）之前，让我们先研究基于策略的方法的优缺点。
