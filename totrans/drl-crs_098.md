# 引入剪辑替代目标函数

> 原文链接：[https://huggingface.co/learn/deep-rl-course/unit8/clipped-surrogate-objective](https://huggingface.co/learn/deep-rl-course/unit8/clipped-surrogate-objective)

## 回顾：策略目标函数

让我们记住Reinforce中要优化的目标：

![Reinforce](../Images/3ea9a018889b4f5f31f0194f43f0595c.png)

想法是通过在这个函数上进行梯度上升步骤（等同于对这个函数的负梯度进行梯度下降），我们将**推动我们的代理人采取导致更高奖励并避免有害动作的行动。**

然而，问题出在步长：

+   太小，训练过程太慢

+   太高，训练中的变化太大

使用PPO，想法是通过一个名为*Clipped surrogate objective function*的新目标函数来约束我们的策略更新，**使用剪辑将策略变化限制在一个小范围内。**

这个新功能旨在避免权重更新过大破坏性地：

![PPO替代函数](../Images/bd42a48549cf230c618200f1b3512d6e.png)

让我们研究每个部分，以了解它是如何工作的。

## 比率函数

![比率](../Images/10170b8c578faa2b06caa171526c06aa.png)

这个比率计算如下：

![比率](../Images/ec4562b6e26c0b68f44d7fa0c50254c8.png)

这是在当前策略中采取动作<math><semantics><mrow><msub><mi>a</mi><mi>t</mi></msub></mrow> <annotation encoding="application/x-tex">a_t</annotation></semantics></math> 在状态<math><semantics><mrow><msub><mi>s</mi><mi>t</mi></msub></mrow> <annotation encoding="application/x-tex">s_t</annotation></semantics></math> 中的概率，除以先前策略相同的概率。

正如我们所看到的，<math><semantics><mrow><msub><mi>r</mi><mi>t</mi></msub><mo stretchy="false">(</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">r_t(\theta)</annotation></semantics></math> 表示当前和旧策略之间的概率比率：

+   如果<math><semantics><mrow><msub><mi>r</mi><mi>t</mi></msub><mo stretchy="false">(</mo><mi>θ</mi><mo stretchy="false">)</mo><mo>></mo><mn>1</mn></mrow> <annotation encoding="application/x-tex">r_t(\theta) > 1</annotation></semantics></math>，**在当前策略中，动作<math><semantics><mrow><msub><mi>a</mi><mi>t</mi></msub></mrow> <annotation encoding="application/x-tex">a_t</annotation></semantics></math> 在状态<math><semantics><mrow><msub><mi>s</mi><mi>t</mi></msub></mrow> <annotation encoding="application/x-tex">s_t</annotation></semantics></math>中比旧策略更有可能。**

+   如果<math><semantics><mrow><msub><mi>r</mi><mi>t</mi></msub><mo stretchy="false">(</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">r_t(\theta)</annotation></semantics></math> 在0和1之间，**动作在当前策略中的可能性比旧策略中的可能性小。**

因此，这个概率比率是**估计旧策略和当前策略之间的差异的一种简单方法。**

## 剪辑替代目标函数的未剪辑部分

![PPO](../Images/00c0cdc7880db7501dbb213c1490b249.png)

这个比率**可以替代我们在策略目标函数中使用的对数概率**。这给我们新目标函数的左部分：将比率乘以优势。

![PPO](../Images/f43eb6cfc9c7ae8d1931c818bfc24184.png)

[近端策略优化算法](https://arxiv.org/pdf/1707.06347.pdf)

然而，如果没有约束，如果当前策略中采取的动作比以前策略中更有可能，**这将导致显著的策略梯度步骤**，因此，**过度的策略更新。**

## 剪辑替代目标函数的剪辑部分

![PPO](../Images/581016b29c8a3fc730cba20795499167.png)

因此，我们需要通过惩罚导致远离1的比率的变化来约束这个目标函数（在论文中，比率只能在0.8到1.2之间变化）。

**通过修剪比率，我们确保不会有太大的策略更新，因为当前策略不能与旧策略有太大的不同。**

为此，我们有两种解决方案：

+   *TRPO（Trust Region Policy Optimization）*在目标函数之外使用KL散度约束来约束策略更新。但这种方法**实现起来复杂，需要更多的计算时间。**

+   *PPO*在其**修剪的替代目标函数**中直接修剪概率比率。

![PPO](../Images/581016b29c8a3fc730cba20795499167.png)

这个修剪部分是一个版本，其中<math><semantics><mrow><msub><mi>r</mi><mi>t</mi></msub><mo stretchy="false">(</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">r_t(\theta)</annotation></semantics></math> rt​(θ)在<math><semantics><mrow><mo stretchy="false">[</mo><mn>1</mn><mo>−</mo><mi>ϵ</mi><mo separator="true">,</mo><mn>1</mn><mo>+</mo><mi>ϵ</mi><mo stretchy="false">]</mo></mrow> <annotation encoding="application/x-tex">[1 - \epsilon, 1 + \epsilon]</annotation></semantics></math> [1−ϵ,1+ϵ]之间修剪。

使用修剪的替代目标函数，我们有两个概率比率，一个是非修剪的，一个是在<math><semantics><mrow><mo stretchy="false">[</mo><mn>1</mn><mo>−</mo><mi>ϵ</mi><mo separator="true">,</mo><mn>1</mn><mo>+</mo><mi>ϵ</mi><mo stretchy="false">]</mo></mrow> <annotation encoding="application/x-tex">[1 - \epsilon, 1 + \epsilon]</annotation></semantics></math> [1−ϵ,1+ϵ]范围内修剪的，epsilon是一个超参数，帮助我们定义这个修剪范围（在论文中<math><semantics><mrow><mi>ϵ</mi><mo>=</mo><mn>0.2</mn></mrow> <annotation encoding="application/x-tex">\epsilon = 0.2</annotation></semantics></math> ϵ=0.2）。

然后，我们取修剪和非修剪目标的最小值，**因此最终目标是未修剪目标的下界（悲观下界）。**

取修剪和非修剪目标的最小值意味着**我们将根据比率和优势情况选择修剪或非修剪目标**。
