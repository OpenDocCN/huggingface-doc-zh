# 贝尔曼方程：简化我们的值估计

> 原文链接：[https://huggingface.co/learn/deep-rl-course/unit2/bellman-equation](https://huggingface.co/learn/deep-rl-course/unit2/bellman-equation)

贝尔曼方程**简化了我们的状态值或状态-动作值计算。**

![贝尔曼方程](../Images/2bf6b05b2e64c9c62e78603164e99c30.png)

根据我们目前学到的知识，我们知道如果计算<math><semantics><mrow><mi>V</mi><mo stretchy="false">(</mo><msub><mi>S</mi><mi>t</mi></msub><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">V(S_t)</annotation></semantics></math>V(St​)（状态的值），我们需要计算从该状态开始的回报，然后永远遵循策略。（我们在下面的示例中定义的策略是贪婪策略；为简化起见，我们不打折奖励）。

因此，要计算<math><semantics><mrow><mi>V</mi><mo stretchy="false">(</mo><msub><mi>S</mi><mi>t</mi></msub><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">V(S_t)</annotation></semantics></math>V(St​)，我们需要计算预期奖励的总和。因此：

![贝尔曼方程](../Images/f3af14f7f97044c041b7bab60378775a.png)

要计算状态1的值：如果代理从该状态开始，然后按照贪婪策略（采取导致最佳状态值的动作）进行所有时间步骤的奖励总和。

然后，要计算<math><semantics><mrow><mi>V</mi><mo stretchy="false">(</mo><msub><mi>S</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">V(S_{t+1})</annotation></semantics></math>V(St+1​)，我们需要计算从该状态<math><semantics><mrow><msub><mi>S</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub></mrow><annotation encoding="application/x-tex">S_{t+1}</annotation></semantics></math>St+1​开始的回报。

![贝尔曼方程](../Images/168b16071cc51b3d9fd633df82adf056.png)

要计算状态2的值：如果代理从该状态开始，然后按照**所有时间步骤的策略**进行奖励的总和。

所以你可能已经注意到，我们在计算不同状态的值时重复了计算，如果你需要为每个状态值或状态-动作值都这样做，这可能会很繁琐。

不是为每个状态或每个状态-动作对计算预期回报，**我们可以使用贝尔曼方程。**（提示：如果你知道什么是动态规划，这非常相似！如果你不知道是什么，不用担心！）

贝尔曼方程是一个递归方程，工作原理如下：不是从每个状态的开始计算回报，而是将任何状态的值视为：

**即时奖励<math><semantics><mrow><msub><mi>R</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub></mrow><annotation encoding="application/x-tex">R_{t+1}</annotation></semantics></math>Rt+1​ + 后续状态的折现值（<math><semantics><mrow><mi>g</mi><mi>a</mi><mi>m</mi><mi>m</mi><mi>a</mi><mo>∗</mo><mi>V</mi><mo stretchy="false">(</mo><msub><mi>S</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">gamma * V(S_{t+1})</annotation></semantics></math> gamma∗V(St+1​) )。**

![贝尔曼方程](../Images/a9dac28cddf8b65584ecce42d2ee7bb0.png)

如果我们回到我们的例子，我们可以说状态1的值等于如果我们从该状态开始的预期累积回报。

![贝尔曼方程](../Images/f3af14f7f97044c041b7bab60378775a.png)

要计算状态1的值：如果代理从该状态1开始，然后按照**所有时间步骤的策略**进行奖励的总和。

这等同于 <math><semantics><mrow><mi>V</mi><mo stretchy="false">(</mo><msub><mi>S</mi><mi>t</mi></msub><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">V(S_{t})</annotation></semantics></math>V(St​) = 立即奖励 <math><semantics><mrow><msub><mi>R</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub></mrow><annotation encoding="application/x-tex">R_{t+1}</annotation></semantics></math>Rt+1​ + 下一个状态的折现价值 <math><semantics><mrow><mi>γ</mi><mo>∗</mo><mi>V</mi><mo stretchy="false">(</mo><msub><mi>S</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">\gamma * V(S_{t+1})</annotation></semantics></math>γ∗V(St+1​)

![贝尔曼方程](../Images/41ddd09d9f3e231dd44290e310a72771.png)

为简化起见，这里我们不打折，所以γ = 1。

为简单起见，这里我们不打折，所以γ = 1。但在本单元的Q学习部分，您将学习一个γ = 0.99的示例。

+   <math><semantics><mrow><mi>V</mi><mo stretchy="false">(</mo><msub><mi>S</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">V(S_{t+1})</annotation></semantics></math> V(St+1​) = 立即奖励 <math><semantics><mrow><msub><mi>R</mi><mrow><mi>t</mi><mo>+</mo><mn>2</mn></mrow></msub></mrow><annotation encoding="application/x-tex">R_{t+2}</annotation></semantics></math>Rt+2​ + 下一个状态的折现价值 (<math><semantics><mrow><mi>γ</mi><mi>a</mi><mi>m</mi><mi>m</mi><mi>a</mi><mo>∗</mo><mi>V</mi><mo stretchy="false">(</mo><msub><mi>S</mi><mrow><mi>t</mi><mo>+</mo><mn>2</mn></mrow></msub><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">gamma * V(S_{t+2})</annotation></semantics></math>gamma∗V(St+2​) ).

+   等等。

总之，贝尔曼方程的思想是，**不是将每个值计算为预期回报的总和，这是一个漫长的过程**，而是将值计算为**立即奖励的总和 + 随后状态的折现价值**。

在进入下一节之前，请考虑贝尔曼方程中γ的作用。如果γ的值非常低（例如0.1甚至为0），会发生什么？如果值为1会发生什么？如果值非常高，比如一百万会发生什么？
