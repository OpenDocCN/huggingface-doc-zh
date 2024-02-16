# 基于模型的强化学习（MBRL）

> 原文链接：[`huggingface.co/learn/deep-rl-course/unitbonus3/model-based`](https://huggingface.co/learn/deep-rl-course/unitbonus3/model-based)

基于模型的强化学习只在学习*动力学模型*方面与其无模型的对应物有所不同，但这对决策的下游影响很大。

动力学模型通常模拟环境转换动态，但是像逆动力学模型（从状态到动作的映射）或奖励模型（预测奖励）可以在这个框架中使用。

## 简单定义

+   有一个代理不断尝试解决问题，**积累状态和动作数据**。

+   有了这些数据，代理创建了一个结构化学习工具，*动力学模型*，用于推理世界。

+   有了动力学模型，代理**通过预测未来来决定如何行动**。

+   通过这些动作，**代理收集更多数据，改进所述模型，并希望改进未来的动作**。

## 学术定义

基于模型的强化学习（MBRL）遵循代理与环境交互，**学习所述环境模型**，然后**利用模型进行控制（做出决策）的框架。

具体来说，代理在由转换函数<math><semantics><mrow><msub><mi>s</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub><mo>=</mo><mi>f</mi><mo stretchy="false">(</mo><msub><mi>s</mi><mi>t</mi></msub><mo separator="true">,</mo><msub><mi>a</mi><mi>t</mi></msub><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">s_{t+1} = f (s_t , a_t)</annotation></semantics></math> st+1​=f(st​,at​) 控制的马尔可夫决策过程（MDP）中行动，并在每一步返回奖励<math><semantics><mrow><mi>r</mi><mo stretchy="false">(</mo><msub><mi>s</mi><mi>t</mi></msub><mo separator="true">,</mo><msub><mi>a</mi><mi>t</mi></msub><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">r(s_t, a_t)</annotation></semantics></math> r(st​,at​)。通过收集的数据集<math><semantics><mrow><mi>D</mi><mo>:</mo><mo>=</mo><mrow><msub><mi>s</mi><mi>i</mi></msub><mo separator="true">,</mo><msub><mi>a</mi><mi>i</mi></msub><mo separator="true">,</mo><msub><mi>s</mi><mrow><mi>i</mi><mo>+</mo><mn>1</mn></mrow></msub><mo separator="true">,</mo><msub><mi>r</mi><mi>i</mi></msub></mrow></mrow> <annotation encoding="application/x-tex">D :={ s_i, a_i, s_{i+1}, r_i}</annotation></semantics></math> D:=si​,ai​,si+1​,ri​，代理学习一个模型<math><semantics><mrow><msub><mi>s</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub><mo>=</mo><msub><mi>f</mi><mi>θ</mi></msub><mo stretchy="false">(</mo><msub><mi>s</mi><mi>t</mi></msub><mo separator="true">,</mo><msub><mi>a</mi><mi>t</mi></msub><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">s_{t+1} = f_\theta (s_t , a_t)</annotation></semantics></math> st+1​=fθ​(st​,at​> **以最小化转换的负对数似然**。

我们使用基于样本的模型预测控制（MPC），利用学习的动力学模型优化在有限的、递归预测的时间范围内的预期奖励，从一个从均匀分布中采样的动作集合 U(a)中进行优化（参见[论文](https://arxiv.org/pdf/2002.04523)或[论文](https://arxiv.org/pdf/2012.09156.pdf)或[论文](https://arxiv.org/pdf/2009.01221.pdf)）。

## 进一步阅读

有关 MBRL 的更多信息，我们建议您查看以下资源：

+   一篇关于调试 MBRL 的[博客文章](https://www.natolambert.com/writing/debugging-mbrl)。

+   一篇关于 MBRL 的[最新综述论文](https://arxiv.org/abs/2006.16712)，

## 作者

这部分由[Nathan Lambert](https://twitter.com/natolambert)撰写
