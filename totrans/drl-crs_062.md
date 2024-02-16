# （可选）策略梯度定理

> 原始文本：[https://huggingface.co/learn/deep-rl-course/unit4/pg-theorem](https://huggingface.co/learn/deep-rl-course/unit4/pg-theorem)

在这个可选部分中，我们将**研究如何对我们将用来近似策略梯度的目标函数进行微分**。

让我们首先回顾一下我们的不同公式：

1.  目标函数

![Return](../Images/91e5d228c6d4b4dfdbd4a60c7ae8fa63.png)

1.  轨迹的概率（假设动作来自<math><semantics><mrow><msub><mi>π</mi><mi>θ</mi></msub></mrow><annotation encoding="application/x-tex">\pi_\theta</annotation></semantics></math>πθ​）：

![Probability](../Images/4b4baebaf1b080cb2852e25cd8686dec.png)

所以我们有：<math><semantics><mrow><msub><mi mathvariant="normal">∇</mi><mi>θ</mi></msub><mi>J</mi><mo stretchy="false">(</mo><mi>θ</mi><mo stretchy="false">)</mo><mo>=</mo><msub><mi mathvariant="normal">∇</mi><mi>θ</mi></msub><msub><mo>∑</mo><mi>τ</mi></msub><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo><mi>R</mi><mo stretchy="false">(</mo><mi>τ</mi><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">\nabla_\theta J(\theta) = \nabla_\theta \sum_{\tau}P(\tau;\theta)R(\tau)</annotation></semantics></math>∇θ​J(θ)=∇θ​∑τ​P(τ;θ)R(τ)

我们可以将总和的梯度重写为梯度的总和：<math><semantics><mrow><mo>=</mo><msub><mo>∑</mo><mi>τ</mi></msub><msub><mi mathvariant="normal">∇</mi><mi>θ</mi></msub><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo><mi>R</mi><mo stretchy="false">(</mo><mi>τ</mi><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">= \sum_{\tau} \nabla_\theta P(\tau;\theta)R(\tau)</annotation></semantics></math> =∑τ​∇θ​P(τ;θ)R(τ)

然后我们将总和中的每一项乘以<math><semantics><mrow><mfrac><mrow><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow><mrow><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow></mfrac></mrow><annotation encoding="application/x-tex">\frac{P(\tau;\theta)}{P(\tau;\theta)}</annotation></semantics></math>P(τ;θ)P(τ;θ)​（因为它等于1）<math><semantics><mrow><mo>=</mo><msub><mo>∑</mo><mi>τ</mi></msub><mfrac><mrow><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow><mrow><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow></mfrac><msub><mi mathvariant="normal">∇</mi><mi>θ</mi></msub><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo><mi>R</mi><mo stretchy="false">(</mo><mi>τ</mi><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">= \sum_{\tau} \frac{P(\tau;\theta)}{P(\tau;\theta)}\nabla_\theta P(\tau;\theta)R(\tau)</annotation></semantics></math> =∑τ​P(τ;θ)P(τ;θ)​∇θ​P(τ;θ)R(τ)

我们可以进一步简化这个表达式，因为<math><semantics><mrow><mfrac><mrow><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow><mrow><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow></mfrac><msub><mi mathvariant="normal">∇</mi><mi>θ</mi></msub><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo><mo>=</mo><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo><mfrac><mrow><msub><mi mathvariant="normal">∇</mi><mi>θ</mi></msub><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow><mrow><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow></mfrac></mrow> <annotation encoding="application/x-tex">\frac{P(\tau;\theta)}{P(\tau;\theta)}\nabla_\theta P(\tau;\theta) = P(\tau;\theta)\frac{\nabla_\theta P(\tau;\theta)}{P(\tau;\theta)}</annotation></semantics></math> P(τ;θ)P(τ;θ)​∇θ​P(τ;θ)=P(τ;θ)P(τ;θ)∇θ​P(τ;θ)​ <math><semantics><mrow><mo>=</mo><msub><mo>∑</mo><mi>τ</mi></msub><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo><mfrac><mrow><msub><mi mathvariant="normal">∇</mi><mi>θ</mi></msub><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow><mrow><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow></mfrac><mi>R</mi><mo stretchy="false">(</mo><mi>τ</mi><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">= \sum_{\tau} P(\tau;\theta) \frac{\nabla_\theta P(\tau;\theta)}{P(\tau;\theta)}R(\tau)</annotation></semantics></math> =∑τ​P(τ;θ)P(τ;θ)∇θ​P(τ;θ)​R(τ)

然后我们可以使用*导数对数技巧*（也称为*似然比技巧*或*REINFORCE技巧*），这是微积分中的一个简单规则，意味着<math><semantics><mrow><msub><mi mathvariant="normal">∇</mi><mi>x</mi></msub><mi>l</mi><mi>o</mi><mi>g</mi><mi>f</mi><mo stretchy="false">(</mo><mi>x</mi><mo stretchy="false">)</mo><mo>=</mo><mfrac><mrow><msub><mi mathvariant="normal">∇</mi><mi>x</mi></msub><mi>f</mi><mo stretchy="false">(</mo><mi>x</mi><mo stretchy="false">)</mo></mrow><mrow><mi>f</mi><mo stretchy="false">(</mo><mi>x</mi><mo stretchy="false">)</mo></mrow></mfrac></mrow> <annotation encoding="application/x-tex">\nabla_x log f(x) = \frac{\nabla_x f(x)}{f(x)}</annotation></semantics></math> ∇x​logf(x)=f(x)∇x​f(x)​

因此，鉴于我们有<math><semantics><mrow><mfrac><mrow><msub><mi mathvariant="normal">∇</mi><mi>θ</mi></msub><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow><mrow><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow></mfrac></mrow><annotation encoding="application/x-tex">\frac{\nabla_\theta P(\tau;\theta)}{P(\tau;\theta)}</annotation></semantics></math> P(τ;θ)∇θ​P(τ;θ)​，我们将其转换为<math><semantics><mrow><msub><mi mathvariant="normal">∇</mi><mi>θ</mi></msub><mi>l</mi><mi>o</mi><mi>g</mi><mi>P</mi><mo stretchy="false">(</mo><mi>τ</mi><mi mathvariant="normal">∣</mi><mi>θ</mi><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">\nabla_\theta log P(\tau|\theta)</annotation></semantics></math> ∇θ​logP(τ∣θ)

所以这是我们的似然策略梯度：∇θ​J(θ)=∑τ​P(τ;θ)∇θ​logP(τ;θ)R(τ)

谢谢这个新公式，我们可以使用轨迹样本来估计梯度（如果您愿意，我们可以用基于样本的估计来近似似然比策略梯度）。其中每个τ(i)是一个采样轨迹。

但是我们仍然有一些数学工作要做：我们需要简化∇θ​logP(τ∣θ)

我们知道：<math><semantics><mrow><msub><mi mathvariant="normal">∇</mi><mi>θ</mi></msub><mi>l</mi><mi>o</mi><mi>g</mi><mi>P</mi><mo stretchy="false">(</mo><msup><mi>τ</mi><mrow><mo stretchy="false">(</mo><mi>i</mi><mo stretchy="false">)</mo></mrow></msup><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo><mo>=</mo><msub><mi mathvariant="normal">∇</mi><mi>θ</mi></msub><mi>l</mi><mi>o</mi><mi>g</mi><mo stretchy="false">[</mo><mi>μ</mi><mo stretchy="false">(</mo><msub><mi>s</mi><mn>0</mn></msub><mo stretchy="false">)</mo><msubsup><mo>∏</mo><mrow><mi>t</mi><mo>=</mo><mn>0</mn></mrow><mi>H</mi></msubsup><mi>P</mi><mo stretchy="false">(</mo><msubsup><mi>s</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow><mrow><mo stretchy="false">(</mo><mi>i</mi><mo stretchy="false">)</mo></mrow></msubsup><mi mathvariant="normal">∣</mi><msubsup><mi>s</mi><mi>t</mi><mrow><mo stretchy="false">(</mo><mi>i</mi><mo stretchy="false">)</mo></mrow></msubsup><mo separator="true">,</mo><msubsup><mi>a</mi><mi>t</mi><mrow><mo stretchy="false">(</mo><mi>i</mi><mo stretchy="false">)</mo></mrow></msubsup><mo stretchy="false">)</mo><msub><mi>π</mi><mi>θ</mi></msub><mo stretchy="false">(</mo><msubsup><mi>a</mi><mi>t</mi><mrow><mo stretchy="false">(</mo><mi>i</mi><mo stretchy="false">)</mo></mrow></msubsup><mi mathvariant="normal">∣</mi><msubsup><mi>s</mi><mi>t</mi><mrow><mo stretchy="false">(</mo><mi>i</mi><mo stretchy="false">)</mo></mrow></msubsup><mo stretchy="false">)</mo><mo stretchy="false">]</mo></mrow><annotation encoding="application/x-tex">\nabla_\theta log P(\tau^{(i)};\theta)= \nabla_\theta log[ \mu(s_0) \prod_{t=0}^{H} P(s_{t+1}^{(i)}|s_{t}^{(i)}, a_{t}^{(i)}) \pi_\theta(a_{t}^{(i)}|s_{t}^{(i)})]</annotation></semantics></math>∇θ​logP(τ(i);θ)=∇θ​log[μ(s0​)∏t=0H​P(st+1(i)​∣st(i)​,at(i)​)πθ​(at(i)​∣st(i)​)]

其中<math><semantics><mrow><mi>μ</mi><mo stretchy="false">(</mo><msub><mi>s</mi><mn>0</mn></msub><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">\mu(s_0)</annotation></semantics></math>μ(s0​)是初始状态分布，而<math><semantics><mrow><mi>P</mi><mo stretchy="false">(</mo><msubsup><mi>s</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow><mrow><mo stretchy="false">(</mo><mi>i</mi><mo stretchy="false">)</mo></mrow></msubsup><mi mathvariant="normal">∣</mi><msubsup><mi>s</mi><mi>t</mi><mrow><mo stretchy="false">(</mo><mi>i</mi><mo stretchy="false">)</mo></mrow></msubsup><mo separator="true">,</mo><msubsup><mi>a</mi><mi>t</mi><mrow><mo stretchy="false">(</mo><mi>i</mi><mo stretchy="false">)</mo></mrow></msubsup><mo stretchy="false">)</mo></mrow> <annotation encoding="application/x-tex">P(s_{t+1}^{(i)}|s_{t}^{(i)}, a_{t}^{(i)})</annotation></semantics></math> P(st+1(i)​∣st(i)​,at(i)​)是MDP的状态转移动力学。

我们知道一个乘积的对数等于对数的和：∇θ​logP(τ(i);θ)=∇θ​[logμ(s0)+t=0∑HlogP(st+1(i)∣st(i)at(i))+t=0∑Hlogπθ(at(i)∣st(i))]

我们还知道总和的梯度等于梯度的总和：∇θ log P(τ(i);θ) = ∇θ log μ(s0) + ∇θ ∑t=0H log P(st+1(i)|st(i)at(i)) + ∇θ ∑t=0H log πθ(at(i)|st(i))

由于MDP的初始状态分布或状态转移动态不依赖于θ，因此这两个项的导数为0。因此我们可以移除它们：

根据以下公式，我们可以将总和的梯度重写为梯度的总和：

∇θ​logP(τ(i);θ)=∑t=0H​∇θ​logπθ​(at(i)​∣st(i​))

因此，估计策略梯度的最终公式是：∇θ J(θ) = g^ = 1/m ∑i=1m ∑t=0H ∇θ log πθ(a^(i)_t | s_t^(i))R(τ^(i))
