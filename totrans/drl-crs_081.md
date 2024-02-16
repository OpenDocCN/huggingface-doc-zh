# 优势演员评论家（A2C）

> 原文链接：[https://huggingface.co/learn/deep-rl-course/unit6/advantage-actor-critic](https://huggingface.co/learn/deep-rl-course/unit6/advantage-actor-critic)

## 用演员评论家方法减少方差

减少Reinforce算法的方差并更快更好地训练我们的代理的解决方案是使用基于策略和基于价值的方法的组合：*演员评论家方法*。

要理解演员评论家，想象一下你在玩一个视频游戏。你可以和一个朋友一起玩，他会给你一些反馈。你是演员，你的朋友是评论家。

![演员评论家](../Images/042bd39f9e4b369ec6cfbc9ae67d3829.png)

在开始时你不知道如何玩，**所以你随机尝试一些动作**。评论家观察你的动作并**提供反馈**。

从这个反馈中学习，**你将更新你的策略并在玩游戏时变得更好**。

另一方面，你的朋友（评论家）也会更新他们的方式来提供反馈，以便下次更好。

这就是演员评论家背后的想法。我们学习两个函数近似：

+   *一个控制我们的代理如何行动的策略*：<math><semantics><mrow><msub><mi>π</mi><mi>θ</mi></msub><mo stretchy="false">(</mo><mi>s</mi><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">\pi_{\theta}(s)</annotation></semantics></math> πθ​(s)

+   *一个价值函数*，通过衡量所采取的行动有多好来辅助策略更新：<math><semantics><mrow><msub><mover accent="true"><mi>q</mi><mo>^</mo></mover><mi>w</mi></msub><mo stretchy="false">(</mo><mi>s</mi><mo separator="true">,</mo><mi>a</mi><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">\hat{q}_{w}(s,a)</annotation></semantics></math> q^​w​(s,a)

## 演员评论家过程

现在我们已经看到了演员评论家的大局，让我们深入了解在训练过程中演员和评论家如何一起改进。

正如我们所看到的，使用演员评论家方法，有两个函数近似（两个神经网络）：

+   *演员*，一个由theta参数化的**策略函数**：<math><semantics><mrow><msub><mi>π</mi><mi>θ</mi></msub><mo stretchy="false">(</mo><mi>s</mi><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">\pi_{\theta}(s)</annotation></semantics></math> πθ​(s)

+   *评论家*，一个由w参数化的**价值函数**：<math><semantics><mrow><msub><mover accent="true"><mi>q</mi><mo>^</mo></mover><mi>w</mi></msub><mo stretchy="false">(</mo><mi>s</mi><mo separator="true">,</mo><mi>a</mi><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">\hat{q}_{w}(s,a)</annotation></semantics></math> q^​w​(s,a)

让我们看看训练过程，以了解演员和评论家如何被优化：

+   在每个时间步t，我们从环境中得到当前状态<math><semantics><mrow><msub><mi>S</mi><mi>t</mi></msub></mrow> <annotation encoding="application/x-tex">S_t</annotation></semantics></math>St​，**通过我们的演员和评论家作为输入传递**。

+   我们的策略接受状态并**输出一个动作**<math><semantics><mrow><msub><mi>A</mi><mi>t</mi></msub></mrow> <annotation encoding="application/x-tex">A_t</annotation></semantics></math> At​。

![步骤1 演员评论家](../Images/5633968bb872e917c968f590412b0f7b.png)

+   评论家也将该行动作为输入，并使用<math><semantics><mrow><msub><mi>S</mi><mi>t</mi></msub></mrow> <annotation encoding="application/x-tex">S_t</annotation></semantics></math>St​和<math><semantics><mrow><msub><mi>A</mi><mi>t</mi></msub></mrow> <annotation encoding="application/x-tex">A_t</annotation></semantics></math> At​，**计算在该状态下采取该行动的价值：Q值**。

![步骤2 演员评论家](../Images/2240186d96dc80dba4057389b4c239ab.png)

+   在环境中执行的动作<math><semantics><mrow><msub><mi>A</mi><mi>t</mi></msub></mrow> <annotation encoding="application/x-tex">A_t</annotation></semantics></math>在t+1时刻产生一个新状态<math><semantics><mrow><msub><mi>S</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub></mrow> <annotation encoding="application/x-tex">S_{t+1}</annotation></semantics></math>和一个奖励<math><semantics><mrow><msub><mi>R</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub></mrow> <annotation encoding="application/x-tex">R_{t+1}</annotation></semantics></math>。

![步骤3 Actor Critic](../Images/aca327aae99a5abb4493fc07abd2dd69.png)

+   演员使用Q值更新其策略参数。

![步骤4 Actor Critic](../Images/cf429723f7f14d366206f1e0c83f52ed.png)

+   由于更新的参数，演员产生下一个要在<math><semantics><mrow><msub><mi>A</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub></mrow> <annotation encoding="application/x-tex">A_{t+1}</annotation></semantics></math>处采取的动作，给定新状态<math><semantics><mrow><msub><mi>S</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub></mrow> <annotation encoding="application/x-tex">S_{t+1}</annotation></semantics></math>。

+   然后评论家更新其价值参数。

![步骤5 Actor Critic](../Images/ec927ac9ae5bbb27566b494db46568aa.png)

## 在Actor-Critic（A2C）中添加优势

我们可以通过**使用优势函数作为评论家而不是动作值函数**来进一步稳定学习。

这个想法是，优势函数计算动作相对于在状态可能的其他动作的优势：**在该状态采取该动作相对于该状态的平均值更好的程度**。它从状态动作对中减去了状态的平均值：

![优势函数](../Images/d178c6579c3c840bd90abfd5839b83af.png)

换句话说，该函数计算**如果我们在该状态采取此动作与我们在该状态获得的平均奖励相比，我们获得的额外奖励**。

额外的奖励是超出该状态预期值的部分。

+   如果A（s，a）>0：我们的梯度会**被推向那个方向**。

+   如果A（s，a）<0（我们的动作比该状态的平均值表现更差），**我们的梯度会被推向相反的方向**。

实现这个优势函数的问题在于它需要两个价值函数 — <math><semantics><mrow><mi>Q</mi><mo stretchy="false">(</mo><mi>s</mi><mo separator="true">,</mo><mi>a</mi><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">Q(s,a)</annotation></semantics></math>和 <math><semantics><mrow><mi>V</mi><mo stretchy="false">(</mo><mi>s</mi><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">V(s)</annotation></semantics></math>。幸运的是，**我们可以使用TD误差作为优势函数的良好估计器**。

![优势函数](../Images/f524503c61319621c198656df4b56427.png)
