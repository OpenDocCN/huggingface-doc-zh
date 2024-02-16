# PPO 的直觉

> 原文：[`huggingface.co/learn/deep-rl-course/unit8/intuition-behind-ppo`](https://huggingface.co/learn/deep-rl-course/unit8/intuition-behind-ppo)

使用 Proximal Policy Optimization（PPO）的想法是，我们希望通过限制每个训练时期对策略所做的更改来改善策略的训练稳定性：**我们希望避免进行过大的策略更新。**

有两个原因：

+   我们凭经验知道，在训练过程中较小的策略更新**更有可能收敛到最优解。**

+   在策略更新中迈出太大的一步可能导致“掉下悬崖”（得到一个糟糕的策略）**并且需要很长时间甚至没有恢复的可能性。**

![策略更新悬崖](img/8fa9c88b0d8ad6450f392bf8060c51c3.png)

采取较小的策略更新以改善训练稳定性

改编自 RL - Proximal Policy Optimization（PPO）[由 Jonathan Hui 解释](https://jonathan-hui.medium.com/rl-proximal-policy-optimization-ppo-explained-77f014ec3f12)

**因此，使用 PPO，我们保守地更新策略**。为此，我们需要测量当前策略相对于以前策略的变化量，使用当前和以前策略之间的比率计算。我们将这个比率剪切在一个范围<math><semantics><mrow><mo stretchy="false">[</mo><mn>1</mn><mo>−</mo><mi>ϵ</mi><mo separator="true">,</mo><mn>1</mn><mo>+</mo><mi>ϵ</mi><mo stretchy="false">]</mo></mrow> <annotation encoding="application/x-tex">[1 - \epsilon, 1 + \epsilon]</annotation></semantics></math> [1−ϵ,1+ϵ]，这意味着我们**消除了当前策略远离旧策略的动机（因此有了近端策略术语）。**
