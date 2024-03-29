# 一个 Q-Learning 示例

> 原文链接：[`huggingface.co/learn/deep-rl-course/unit2/q-learning-example`](https://huggingface.co/learn/deep-rl-course/unit2/q-learning-example)

为了更好地理解 Q-Learning，让我们来看一个简单的例子：

![迷宫示例](img/7bdcf448ce834a314efde42768c303f7.png)

+   你是这个小迷宫中的一只老鼠。你总是**从同一个起点开始。**

+   目标是**在右下角吃掉大堆奶酪**并避免毒药。毕竟，谁不喜欢奶酪呢？

+   如果我们吃了毒药、**吃了大堆奶酪**或者走了超过五步，情节就会结束。

+   学习率为 0.1

+   折扣率（gamma）为 0.99

![迷宫示例](img/c17db319504a73cec7f8f89c466d8000.png)

奖励函数如下：

+   **+0：** 去一个没有奶酪的状态。

+   **+1：** 去一个有一小块奶酪的状态。

+   **+10：** 去一个有大堆奶酪的状态。

+   **-10：** 去一个有毒药的状态，从而死亡。

+   **+0** 如果我们走了超过五步。

![迷宫示例](img/f8fc089c4b1640dfdcc484c4f5916025.png)

为了训练我们的智能体具有最优策略（即向右、向右、向下的策略），**我们将使用 Q-Learning 算法**。

## 步骤 1：初始化 Q 表

![迷宫示例](img/36ec86ed2fe8ef5a648fca96e67b8e6a.png)

因此，**目前我们的 Q 表是无用的**；我们需要**使用 Q-Learning 算法训练我们的 Q 函数。**

让我们进行 2 个训练时间步：

训练时间步 1：

## 步骤 2：使用 Epsilon Greedy 策略选择动作

因为 epsilon 很大（= 1.0），我采取了一个随机动作。在这种情况下，我向右走。

![迷宫示例](img/3b703676d561e8e0bcd1fb87c2b3aef2.png)

## 步骤 3：执行动作 At，获取 Rt+1 和 St+1

向右走，我得到了一小块奶酪，所以 Rt+1 = 1，我进入了一个新的状态。

![迷宫示例](img/59cd579ddd1d870f7a3df5e80ebc2b03.png)

## 步骤 4：更新 Q(St, At)

我们现在可以使用我们的公式更新 Q(St, At)。

![迷宫示例](img/039d07cc30eaeda29cabcae129111e00.png) ![迷宫示例](img/a6cf10447e88cd0d903730da115cb468.png)

训练时间步 2：

## 步骤 2：使用 Epsilon Greedy 策略选择动作

**我再次采取随机动作，因为 epsilon=0.99 很大**。（请注意，我们稍微降低 epsilon，因为随着训练的进行，我们希望探索越来越少）。

我选择了动作“向下”。**这不是一个好的动作，因为它导致我吃到了毒药。**

![迷宫示例](img/8a555fe2a851cda0cd8432c383e56d7f.png)

## 步骤 3：执行动作 At，获取 Rt+1 和 St+1

因为我吃了毒药，**我得到 Rt+1 = -10，然后我死了。**

![迷宫示例](img/44ccf2ebfc7c5cc6d2383ac1d60d8f4a.png)

## 步骤 4：更新 Q(St, At)

![迷宫示例](img/b633bdc1c80d53eef7dad7daab4e8e1e.png)

因为我们已经死了，我们开始一个新的情节。但我们在这里看到的是，**通过两次探索步骤，我的智能体变得更聪明了。**

随着我们继续探索和利用环境，并使用 TD 目标更新 Q 值，**Q 表将给我们一个越来越好的近似值。在训练结束时，我们将得到最优 Q 函数的估计。**
