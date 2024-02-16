# 深度Q-Learning

> 原文链接：[https://huggingface.co/learn/deep-rl-course/unit3/introduction](https://huggingface.co/learn/deep-rl-course/unit3/introduction)

![第3单元缩略图](../Images/e8420d1d9f22aa4095ae8b961c412a91.png)

在上一个单元中，我们学习了我们的第一个强化学习算法：Q-Learning，**从头开始实现**，并在两个环境中进行了训练，FrozenLake-v1 ☃️和Taxi-v3 🚕。

我们用这个简单的算法取得了出色的结果，但这些环境相对简单，因为**状态空间是离散且较小的**（FrozenLake-v1有16个不同的状态，Taxi-v3有500个）。相比之下，Atari游戏中的状态空间可以**包含<math><semantics><mrow><mn>1</mn><msup><mn>0</mn><mn>9</mn></msup></mrow><annotation encoding="application/x-tex">10^{9}</annotation></semantics></math>109到<math><semantics><mrow><mn>1</mn><msup><mn>0</mn><mn>11</mn></msup></mrow><annotation encoding="application/x-tex">10^{11}</annotation></semantics></math>1011个状态**。

但正如我们将看到的，**在大状态空间环境中生成和更新Q表可能变得无效**。

因此，在本单元中，**我们将学习我们的第一个深度强化学习代理**：深度Q-Learning。深度Q-Learning不使用Q表，而是使用一个神经网络，该神经网络接受一个状态，并根据该状态近似计算每个动作的Q值。

**我们将使用[RL-Zoo](https://github.com/DLR-RM/rl-baselines3-zoo)**来训练它玩太空侵略者和其他Atari环境，RL-Zoo是一个使用Stable-Baselines的RL训练框架，提供了用于训练、评估代理、调整超参数、绘制结果和录制视频的脚本。

![环境](../Images/bf441b005cda192d0dc86eb42475aeb3.png)

让我们开始吧！🚀
