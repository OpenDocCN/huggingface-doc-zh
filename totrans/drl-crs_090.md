# 自我对弈：在对抗性游戏中训练竞争性代理的经典技术

> 原文链接：[https://huggingface.co/learn/deep-rl-course/unit7/self-play](https://huggingface.co/learn/deep-rl-course/unit7/self-play)

现在我们已经学习了多智能体的基础知识，我们准备深入研究。正如在介绍中提到的，我们将**在SoccerTwos中进行2对2的对抗游戏训练代理**。

![SoccerTwos](../Images/b8d7d800c316a50a5f64472742088b73.png)

这个环境是由[Unity MLAgents团队](https://github.com/Unity-Technologies/ml-agents)制作的

## 什么是自我对弈？

在对抗性游戏中正确训练代理可能会**非常复杂**。

一方面，我们需要找到一个训练有素的对手来对抗你的训练代理。另一方面，如果你找到一个训练有素的对手，当对手太强时，你的代理如何改进其策略？

想象一个刚开始学习踢足球的孩子。与一位非常优秀的足球运动员对战将是没有意义的，因为很难赢得比赛或者至少偶尔抢到球。所以孩子将不断失败，没有时间学习一个好的策略。

最好的解决方案是**有一个与代理水平相同的对手，并且随着代理提升自身水平而提升对手水平**。因为如果对手太强，我们将学不到东西；如果对手太弱，我们将在与更强对手对战时学到无用的行为。

这个解决方案被称为*自我对弈*。在自我对弈中，**代理使用自身以前的副本（其策略）作为对手**。这样，代理将与同一水平的代理对战（具有挑战性但不会太难），有机会逐渐改进其策略，然后在变得更好时更新其对手。这是一种启动对手并逐渐增加对手复杂性的方法。

这是人类在竞争中学习的方式：

+   我们开始训练与相似水平的对手对战

+   然后我们从中学习，当我们获得一些技能时，我们可以继续与更强的对手对战。

我们对自我对弈也是一样的：

+   我们**从我们的代理副本开始作为对手**，这样，这个对手就处于相似水平。

+   我们从中**学习**，当我们获得一些技能时，我们会**用我们训练策略的最新副本更新我们的对手**。

自我对弈背后的理论并不是什么新鲜事。它早在五十年代由Arthur Samuel的跳棋玩家系统和1995年由Gerald Tesauro的TD-Gammon中就已经使用过。如果你想了解更多关于自我对弈历史的内容，请查看Andrew Cohen的这篇非常好的博文：[点击这里](https://blog.unity.com/technology/training-intelligent-adversaries-using-self-play-with-ml-agents)

## MLAgents中的自我对弈

自我对弈已经集成到MLAgents库中，并由多个超参数管理，我们将对其进行研究。但正如文档中所解释的那样，主要关注点是**技能水平和最终策略的普遍性与学习稳定性之间的权衡**。

训练对抗一组缓慢变化或不变的对手，具有低多样性**会导致更稳定的训练。但如果变化太慢，就会有过拟合的风险。**

因此我们需要控制：

+   我们通过`swap_steps`和`team_change`参数来确定**多久改变对手**。

+   通过`window`参数保存的**对手数量**。较大的`window`值意味着代理的对手池将包含更多行为的多样性，因为它将包含训练运行中较早的策略。

+   从`play_against_latest_model_ratio`中的池中抽样，确定**与当前自身对手对战的概率**。较大的`play_against_latest_model_ratio`值表示代理将更频繁地与当前对手对战。

+   在`save_steps`参数中保存新对手之前的**训练步数。**较大的`save_steps`值将产生一组涵盖更广泛技能水平和可能的游戏风格的对手，因为策略接受了更多的训练。

要了解更多关于这些超参数的详细信息，您绝对需要[查看文档的这一部分](https://github.com/Unity-Technologies/ml-agents/blob/develop/docs/Training-Configuration-File.md#self-play)

## ELO分数用于评估我们的代理

### 什么是ELO分数？

在对抗性游戏中，跟踪**累积奖励并不总是一个有意义的度量标准：**因为这个度量标准**仅取决于对手的技能。**

相反，我们使用一个名为***ELO评分系统***（以Arpad Elo命名）来计算给定人群中2名玩家之间的**相对技能水平**。

在零和游戏中：一个代理赢，另一个代理输。这是一种数学表示，其中每个参与者的效用增减**正好被其他参与者的效用增减所平衡。**我们谈论零和游戏，因为效用的总和等于零。

这个ELO（从特定分数开始：通常为1200）最初可能会下降，但在训练过程中应逐渐增加。

Elo系统是**从与其他玩家的失败和平局中推断出来的。**这意味着玩家的评分取决于**他们的对手的评分以及对他们的比赛结果。**

Elo定义了一个Elo分数，表示玩家在零和游戏中的相对技能。**我们说相对是因为它取决于对手的表现。**

中心思想是将玩家的表现**视为一个正态分布的随机变量。**

两名玩家之间的评分差异作为**比赛结果的预测器。**如果玩家赢了，但获胜的概率很高，它只会从对手那里赢得一些分数，因为这意味着它比对手强得多。

每场比赛结束后：

+   获胜的玩家从失败的玩家**那里获得分数。**

+   得分的数量由**两名玩家评分的差异（因此是相对的）**决定。

    +   如果高分玩家获胜→将从低分玩家那里获得一些分数。

    +   如果低分玩家获胜→将从高分玩家那里获得很多分数。

    +   如果是平局→低分玩家从高分玩家那里获得一些分数。

因此，如果A和B的评分分别为Ra和Rb，则**预期得分为**：

![ELO分数](../Images/170b63d61ac8a30759838921d82860c5.png)

然后，在游戏结束时，我们需要更新玩家的实际Elo分数。我们使用线性调整**与玩家表现超出或不足的量成比例。**

我们还定义了每场比赛的最大调整等级：K因子。

+   K=16适用于大师。

+   K=32适用于较弱的玩家。

如果玩家A有Ea分，但得到了Sa分，那么玩家的评分将使用以下公式进行更新：

![ELO分数](../Images/1e0296c75fde41fbfd850f8d5b2c8cdd.png)

### 例子

如果我们举个例子：

玩家A的评分为2600

玩家B的评分为2300

+   我们首先计算预期得分：<math><semantics><mrow><msub><mi>E</mi><mi>A</mi></msub><mo>=</mo><mfrac><mn>1</mn><mrow><mn>1</mn><mo>+</mo><mn>1</mn><msup><mn>0</mn><mrow><mo stretchy="false">(</mo><mn>2300</mn><mo>−</mo><mn>2600</mn><mo stretchy="false">)</mo><mi mathvariant="normal">/</mi><mn>400</mn></mrow></msup></mrow></mfrac><mo>=</mo><mn>0.849</mn></mrow><annotation encoding="application/x-tex">E_{A} = \frac{1}{1+10^{(2300-2600)/400}} = 0.849</annotation></semantics></math> EA​=1+10(2300−2600)/4001​=0.849 <math><semantics><mrow><msub><mi>E</mi><mi>B</mi></msub><mo>=</mo><mfrac><mn>1</mn><mrow><mn>1</mn><mo>+</mo><mn>1</mn><msup><mn>0</mn><mrow><mo stretchy="false">(</mo><mn>2600</mn><mo>−</mo><mn>2300</mn><mo stretchy="false">)</mo><mi mathvariant="normal">/</mi><mn>400</mn></mrow></msup></mrow></mfrac><mo>=</mo><mn>0.151</mn></mrow><annotation encoding="application/x-tex">E_{B} = \frac{1}{1+10^{(2600-2300)/400}} = 0.151</annotation></semantics></math> EB​=1+10(2600−2300)/4001​=0.151

+   如果组织者确定K=16并且A获胜，新的评分将是：<math><semantics><mrow><mi>E</mi><mi>L</mi><msub><mi>O</mi><mi>A</mi></msub><mo>=</mo><mn>2600</mn><mo>+</mo><mn>16</mn><mo>∗</mo><mo stretchy="false">(</mo><mn>1</mn><mo>−</mo><mn>0.849</mn><mo stretchy="false">)</mo><mo>=</mo><mn>2602</mn></mrow><annotation encoding="application/x-tex">ELO_A = 2600 + 16*(1-0.849) = 2602</annotation></semantics></math> ELOA​=2600+16∗(1−0.849)=2602 <math><semantics><mrow><mi>E</mi><mi>L</mi><msub><mi>O</mi><mi>B</mi></msub><mo>=</mo><mn>2300</mn><mo>+</mo><mn>16</mn><mo>∗</mo><mo stretchy="false">(</mo><mn>0</mn><mo>−</mo><mn>0.151</mn><mo stretchy="false">)</mo><mo>=</mo><mn>2298</mn></mrow><annotation encoding="application/x-tex">ELO_B = 2300 + 16*(0-0.151) = 2298</annotation></semantics></math> ELOB​=2300+16∗(0−0.151)=2298

+   如果组织者确定K=16并且B获胜，新的评分将是：<math><semantics><mrow><mi>E</mi><mi>L</mi><msub><mi>O</mi><mi>A</mi></msub><mo>=</mo><mn>2600</mn><mo>+</mo><mn>16</mn><mo>∗</mo><mo stretchy="false">(</mo><mn>0</mn><mo>−</mo><mn>0.849</mn><mo stretchy="false">)</mo><mo>=</mo><mn>2586</mn></mrow><annotation encoding="application/x-tex">ELO_A = 2600 + 16*(0-0.849) = 2586</annotation></semantics></math> ELOA​=2600+16∗(0−0.849)=2586 <math><semantics><mrow><mi>E</mi><mi>L</mi><msub><mi>O</mi><mi>B</mi></msub><mo>=</mo><mn>2300</mn><mo>+</mo><mn>16</mn><mo>∗</mo><mo stretchy="false">(</mo><mn>1</mn><mo>−</mo><mn>0.151</mn><mo stretchy="false">)</mo><mo>=</mo><mn>2314</mn></mrow><annotation encoding="application/x-tex">ELO_B = 2300 + 16 *(1-0.151) = 2314</annotation></semantics></math> ELOB​=2300+16∗(1−0.151)=2314

### 优点

使用ELO分数有多个优点：

+   积分**始终平衡**（当有意外结果时，会交换更多积分，但总和始终相同）。

+   这是一个**自我纠正的系统**，因为如果一个玩家赢了一个弱玩家，他们只会赢得一点。

+   它**适用于团队比赛**：我们为每个团队计算平均值，并在Elo中使用它。

### 缺点

+   ELO**不考虑**团队中每个人的个人贡献。

+   评分通缩：**一个好的评分需要随着时间的推移保持相同的评分**。

+   **无法比较历史评分**。
