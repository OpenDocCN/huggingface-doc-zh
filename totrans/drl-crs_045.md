# 从Q-Learning到深度Q-Learning

> 原文链接：[https://huggingface.co/learn/deep-rl-course/unit3/from-q-to-dqn](https://huggingface.co/learn/deep-rl-course/unit3/from-q-to-dqn)

我们学到了**Q-Learning是一种用来训练我们的Q函数的算法**，这是一个**动作值函数**，用来确定在特定状态下采取特定动作的价值。

![Q函数](../Images/d98598351e812f60049067f862f79c69.png)

**Q来自于“在那个状态下那个动作的质量”。**

在内部，我们的Q函数由**Q表编码，一个表格，其中每个单元格对应于一个状态-动作对值。**将这个Q表想象成**我们Q函数的记忆或备忘录。**

问题在于Q-Learning是一种*表格方法*。如果状态和动作空间**不足以通过数组和表格有效表示**，这就成为了问题。换句话说：它**不可扩展**。Q-Learning在小状态空间环境中效果很好，比如：

+   FrozenLake，我们有16个状态。

+   Taxi-v3，我们有500个状态。

但想想今天我们要做什么：我们将训练一个代理学习玩太空侵略者，一个更复杂的游戏，使用帧作为输入。

正如**[Nikita Melkozerov提到的](https://twitter.com/meln1k)，Atari环境**的观察空间形状为(210, 160, 3)*，包含值从0到255，这给了我们<math><semantics><mrow><mn>25</mn><msup><mn>6</mn><mrow><mn>210</mn><mo>×</mo><mn>160</mn><mo>×</mo><mn>3</mn></mrow></msup><mo>=</mo><mn>25</mn><msup><mn>6</mn><mn>100800</mn></msup></mrow><annotation encoding="application/x-tex">256^{210 \times 160 \times 3} = 256^{100800}</annotation></semantics></math>256210×160×3=256100800个可能的观察（作为比较，我们大约有<math><semantics><mrow><mn>1</mn><msup><mn>0</mn><mn>80</mn></msup></mrow><annotation encoding="application/x-tex">10^{80}</annotation></semantics></math>1080个原子在可观察宇宙中）。

+   Atari中的单帧由一个210x160像素的图像组成。考虑到图像是彩色的（RGB），有3个通道。这就是形状为(210, 160, 3)的原因。对于每个像素，值可以从0到255。

![Atari状态空间](../Images/51e288086cae4a16b8a0ca90888a8264.png)

因此，状态空间是巨大的；因此，在该环境中创建和更新Q表将不是有效的。在这种情况下，最好的想法是使用参数化的Q函数来近似Q值<math><semantics><mrow><msub><mi>Q</mi><mi>θ</mi></msub><mo stretchy="false">(</mo><mi>s</mi><mo separator="true">,</mo><mi>a</mi><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">Q_{\theta}(s,a)</annotation></semantics></math>Qθ​(s,a)。

这个神经网络将近似，给定一个状态，该状态下每个可能动作的不同Q值。这正是深度Q学习所做的。

![深度Q学习](../Images/1e4c6acfdf811be054c82941f53e5853.png)

现在我们了解了深度Q学习，让我们深入了解深度Q网络。
