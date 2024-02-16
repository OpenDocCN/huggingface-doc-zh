# 实践

> 原文链接：[https://huggingface.co/learn/deep-rl-course/unit7/hands-on](https://huggingface.co/learn/deep-rl-course/unit7/hands-on)

现在您已经了解了多代理的基础知识，您可以开始训练您的第一个多代理系统中的代理：**一个2对2的足球团队需要击败对手团队**。

您将参加AI vs. AI挑战，您训练的代理将每天与其他同学的**代理竞争，并在新的排行榜上排名。**

要验证这个实践项目是否符合认证要求，您只需要推送一个经过训练的模型。**没有最低要求来验证它。**

有关认证过程的更多信息，请查看此部分👉[https://huggingface.co/deep-rl-course/en/unit0/introduction#certification-process](https://huggingface.co/deep-rl-course/en/unit0/introduction#certification-process)

这个实践项目将会有所不同，因为为了获得正确的结果**您需要训练您的代理从4小时到8小时**。鉴于在Colab中超时的风险，我们建议您在您的计算机上进行训练。您不需要超级计算机：一台简单的笔记本电脑就足够了。

让我们开始吧！🔥

## 什么是AI vs. AI？

AI vs. AI是我们在Hugging Face开发的一个开源工具，用于在Hub上的代理之间进行多代理设置中的竞争。然后这些模型将在排行榜上排名。

这个工具的想法是拥有一个强大的评估工具：**通过将您的代理与许多其他代理进行评估，您将对您的策略质量有一个很好的了解**。

更准确地说，AI vs. AI包括三个工具：

+   一个*匹配过程*定义比赛（哪个模型对哪个模型）并在空间中使用后台任务运行模型对抗。

+   一个*排行榜*获取比赛历史结果并显示模型的ELO评分：[https://huggingface.co/spaces/huggingface-projects/AIvsAI-SoccerTwos](https://huggingface.co/spaces/huggingface-projects/AIvsAI-SoccerTwos)

+   一个*Space演示*，可视化您的代理与其他代理的对战：[https://huggingface.co/spaces/unity/ML-Agents-SoccerTwos](https://huggingface.co/spaces/unity/ML-Agents-SoccerTwos)

除了这三个工具，您的同学cyllum创建了一个🤗 SoccerTwos Challenge Analytics，您可以在其中查看模型的详细比赛结果：[https://huggingface.co/spaces/cyllum/soccertwos-analytics](https://huggingface.co/spaces/cyllum/soccertwos-analytics)

我们[撰写了一篇博文详细解释这个AI vs. AI工具](https://huggingface.co/blog/aivsai)，但为了让您有一个大致的了解，它的工作方式如下：

+   每四小时，我们的算法**获取给定环境（在我们的情况下是ML-Agents-SoccerTwos）中的所有可用模型。**

+   它使用匹配算法创建一个**比赛队列。**

+   我们在Unity无头进程中模拟比赛并**收集比赛结果**（如果第一个模型赢得比赛则为1，如果是平局则为0.5，如果第二个模型赢得比赛则为0）。

+   然后，当所有比赛队列中的比赛都完成时，**我们更新每个模型的ELO分数并更新排行榜。**

### 比赛规则

这个第一个AI vs. AI比赛**是一个实验**：目标是通过您的反馈来改进未来的工具。因此在挑战中可能会发生一些**中断**。但不用担心**所有结果都保存在数据集中，因此我们可以始终正确重新启动计算而不会丢失信息**。

为了使您的模型能够正确地与其他模型进行评估，您需要遵循以下规则：

1.  **您不能更改代理的观察空间或行动空间。**这样做会导致您的模型在评估过程中无法工作。

1.  **目前不能使用自定义训练器**，您需要使用Unity MLAgents。

1.  我们提供可执行文件来训练您的代理。您也可以使用Unity编辑器，如果您愿意**，但为了避免错误，我们建议您使用我们的可执行文件**。

在这个挑战中将会有什么不同的是**您选择的超参数**。

我们不断努力改进我们的教程，所以**如果您在这个笔记本中发现了一些问题**，请在GitHub Repo上[提出问题](https://github.com/huggingface/deep-rl-class/issues)。

### 在Discord上与同学聊天，分享建议和提问

+   我们创建了一个名为`ai-vs-ai-challenge`的新频道，用于交流建议和提问。

+   如果您还没有加入discord服务器，您可以[在这里加入](https://discord.gg/ydHrjt3WP5)

## 步骤0：安装MLAgents并下载正确的可执行文件

我们建议您使用[conda](https://docs.conda.io/en/latest/)作为包管理器并创建一个新环境。

使用conda，我们创建一个名为rl的新环境，其中包含**Python 3.10.12**：

```py
conda create --name rl python=3.10.12
conda activate rl
```

为了能够正确训练我们的代理并推送到Hub，我们需要安装ML-Agents

```py
git clone https://github.com/Unity-Technologies/ml-agents
```

克隆完成后（需要2.63 GB），我们进入存储库并安装软件包

```py
cd ml-agents
pip install -e ./ml-agents-envs
pip install -e ./ml-agents
```

最后，您需要安装git-lfs：[https://git-lfs.com/](https://git-lfs.com/)

现在已经安装好了，我们需要添加环境训练可执行文件。根据您的操作系统，您需要下载其中一个，解压缩并将其放在`ml-agents`中的一个新文件夹中，命名为`training-envs-executables`

最后，您的可执行文件应该在`ml-agents/training-envs-executables/SoccerTwos`中

Windows：下载[这个可执行文件](https://drive.google.com/file/d/1sqFxbEdTMubjVktnV4C6ICjp89wLhUcP/view?usp=sharing)

Linux（Ubuntu）：下载[这个可执行文件](https://drive.google.com/file/d/1KuqBKYiXiIcU4kNMqEzhgypuFP5_45CL/view?usp=sharing)

Mac：下载[这个可执行文件](https://drive.google.com/drive/folders/1h7YB0qwjoxxghApQdEUQmk95ZwIDxrPG?usp=share_link) ⚠ 对于Mac，您还需要调用`xattr -cr training-envs-executables/SoccerTwos/SoccerTwos.app`才能运行SoccerTwos

## 步骤1：了解环境

这个环境被称为`SoccerTwos`。Unity MLAgents团队制作了它。您可以在[这里](https://github.com/Unity-Technologies/ml-agents/blob/develop/docs/Learning-Environment-Examples.md#soccer-twos)找到它的文档

在这个环境中，目标是**将球进入对手的球门，同时防止球进入自己的球门。**

![SoccerTwos](../Images/b8d7d800c316a50a5f64472742088b73.png)

这个环境是由[Unity MLAgents团队](https://github.com/Unity-Technologies/ml-agents)制作的

### 奖励函数

奖励函数是：

![SoccerTwos Reward](../Images/236ec2816e80441049618048e60a2b89.png)

### 观察空间

观察空间由大小为336的向量组成：

+   11个向前的射线分布在120度上（264个状态维度）

+   3个向后的射线分布在90度上（72个状态维度）

+   这两个射线都可以检测到6个对象：

    +   球

    +   蓝色目标

    +   紫色目标

    +   墙

    +   蓝色代理

    +   紫色代理

### 行动空间

行动空间有三个离散分支：

![SoccerTwos Action](../Images/8afca8f83c98b99eae38b8072b97e32c.png)

## 步骤2：了解MA-POCA

我们知道如何训练代理与其他人对战：**我们可以使用自我对弈。**这是一个完美的技术，适用于1对1。

但在我们的情况下，我们是2对2，每个团队有2个代理。那么我们如何**训练代理组的合作行为呢？**

如[Unity Blog](https://blog.unity.com/technology/ml-agents-v20-release-now-supports-training-complex-cooperative-behaviors)中所解释的，当团队进球时，代理通常作为一个团体收到奖励（+1 - 罚分）。这意味着**即使每个代理没有对胜利做出相同的贡献，团队中的每个代理都会受到奖励**，这使得独立学习变得困难。

Unity MLAgents团队开发了一个新的多代理训练器解决方案，称为*MA-POCA（多代理后期信用分配）*。

这个想法简单但强大：一个集中的评论者**处理团队中所有代理的状态，以估计每个代理的表现**。把这个评论者想象成一个教练。

这使得每个代理**只基于其本地感知做出决策**，并**同时评估其行为在整个团队环境中的表现**。

![MA POCA](../Images/7ad7814daeea1597757fe72e8d0ff01a.png)

这说明了 MA-POCA 的集中式学习和分散式执行。来源：[MLAgents Plays Dodgeball](https://blog.unity.com/technology/ml-agents-plays-dodgeball)

解决方案是使用自我对弈和 MA-POCA 训练器（称为 poca）。poca 训练器将帮助我们训练合作行为和自我对弈以赢得对手团队。

如果您想深入了解这个 MA-POCA 算法，您需要阅读他们发表的论文[这里](https://arxiv.org/pdf/2111.05992.pdf)以及我们放在附加阅读部分的来源。

## 第 3 步：定义配置文件

我们已经在[Unit 5](https://huggingface.co/deep-rl-course/unit5/introduction)中学到，在 ML-Agents 中，您可以在 `config.yaml` 文件中定义**训练超参数。**

有多个超参数。要更好地理解它们，您应该阅读**[文档](https://github.com/Unity-Technologies/ml-agents/blob/release_20_docs/docs/Training-Configuration-File.md)**中每个超参数的解释。

我们将在这里使用的配置文件在 `./config/poca/SoccerTwos.yaml` 中。它看起来像这样：

```py
behaviors:
  SoccerTwos:
    trainer_type: poca
    hyperparameters:
      batch_size: 2048
      buffer_size: 20480
      learning_rate: 0.0003
      beta: 0.005
      epsilon: 0.2
      lambd: 0.95
      num_epoch: 3
      learning_rate_schedule: constant
    network_settings:
      normalize: false
      hidden_units: 512
      num_layers: 2
      vis_encode_type: simple
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    keep_checkpoints: 5
    max_steps: 5000000
    time_horizon: 1000
    summary_freq: 10000
    self_play:
      save_steps: 50000
      team_change: 200000
      swap_steps: 2000
      window: 10
      play_against_latest_model_ratio: 0.5
      initial_elo: 1200.0
```

与 Pyramids 或 SnowballTarget 相比，我们有一个包含自我对弈部分的新超参数。您如何修改它们可能对获得良好结果至关重要。

我可以给您的建议是检查每个参数的解释和推荐值（特别是自我对弈参数）与**[文档](https://github.com/Unity-Technologies/ml-agents/blob/release_20_docs/docs/Training-Configuration-File.md)**进行对比。

现在您已经修改了我们的配置文件，准备好训练您的代理了。

## 第 4 步：开始训练

训练代理，我们需要**启动 mlagents-learn 并选择包含环境的可执行文件。**

我们定义了四个参数：

1.  `mlagents-learn <config>`：超参数配置文件所在路径。

1.  `-env`：环境可执行文件所在位置。

1.  `-run_id`：您要为训练运行 ID 指定的名称。

1.  `-no-graphics`：在训练期间不启动可视化。

根据您的硬件，500 万个时间步（推荐值，但也可以尝试 1000 万）需要 5 到 8 小时的训练时间。在此期间，您可以继续使用计算机，但我建议停用计算机待机模式以防止训练被停止。

根据您使用的可执行文件（windows、ubuntu、mac），训练命令将如下所示（您的可执行文件路径可能不同，因此在运行之前请务必检查）。

```py
mlagents-learn ./config/poca/SoccerTwos.yaml --env=./training-envs-executables/SoccerTwos.exe --run-id="SoccerTwos" --no-graphics
```

可执行文件包含 8 个 SoccerTwos 的副本。

⚠️ 如果在 200 万个时间步之前看不到 ELO 分数的大幅增加（甚至在 1200 以下下降）是正常的，因为您的代理将在能够进球之前大部分时间随机移动在场地上。

⚠️ 您可以使用 Ctrl + C 停止训练，但要注意只键入此命令一次以停止训练，因为 MLAgents 需要在关闭运行之前生成最终的 .onnx 文件。

## 第 5 步：将代理推送到 Hugging Face Hub

现在我们已经训练了我们的代理，我们**准备将它们推送到 Hub，以便参加 AI 对抗赛并在浏览器上观看它们的比赛🔥。**

要能够与社区共享您的模型，还有三个步骤要遵循：

1️⃣（如果尚未完成）创建一个 HF 帐户 ➡ [https://huggingface.co/join](https://huggingface.co/join)

2️⃣ 登录并存储来自 Hugging Face 网站的身份验证令牌。

创建一个新令牌（[https://huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)）**具有写入权限**

![创建 HF 令牌](../Images/d21a97c736edaab9119d2d1c1da9deac.png)

复制令牌，运行此命令，然后粘贴令牌

```py
huggingface-cli login
```

然后，我们需要运行 `mlagents-push-to-hf`。

我们定义了四个参数：

1.  `-run-id`：训练运行ID的名称。

1.  `-local-dir`：代理保存的位置，是results/<run_id名称>，所以在我的情况下是results/First Training。

1.  `-repo-id`：您要创建或更新的Hugging Face repo的名称。它始终是<您的Hugging Face用户名>/<repo名称>如果仓库不存在**将自动创建**

1.  `--commit-message`：由于HF仓库是git存储库，您需要提供提交消息。

在我的情况下

```py
mlagents-push-to-hf  --run-id="SoccerTwos" --local-dir="./results/SoccerTwos" --repo-id="ThomasSimonini/poca-SoccerTwos" --commit-message="First Push"`
```

```py
mlagents-push-to-hf  --run-id= # Add your run id  --local-dir= # Your local dir  --repo-id= # Your repo id --commit-message="First Push"
```

如果一切正常，您应该在过程结束时看到这个（但网址不同😆）：

您的模型已推送到Hub。您可以在此查看您的模型：[https://huggingface.co/ThomasSimonini/poca-SoccerTwos](https://huggingface.co/ThomasSimonini/poca-SoccerTwos)

这是您模型的链接。它包含一个解释如何使用它的模型卡片，您的Tensorboard和配置文件。**令人惊奇的是，它是一个git存储库，这意味着您可以有不同的提交，使用新的推送更新您的存储库等。**

## 第6步：验证您的模型是否准备好参加AI vs AI挑战

现在您的模型已推送到Hub，**它将自动添加到AI vs AI挑战模型池中。**在您的模型被添加到排行榜之前可能需要一点时间，因为我们每4小时进行一次比赛。

但为了确保一切都能完美运行，您需要检查：

1.  您的模型中有这个标签：ML-Agents-SoccerTwos。这是我们用来选择要添加到挑战池中的模型的标签。要做到这一点，请转到您的模型并检查标签

![验证](../Images/f293f2ed5fa03cd6a2cd77086f6200a5.png)

如果不是这种情况，您只需要修改自述文件并添加它

![验证](../Images/9cec0da6fa3195c208f23e9300898d7e.png)

1.  您有一个`SoccerTwos.onnx`文件

![验证](../Images/99478ac1cd0c73aef674a98bc5457f48.png)

我们强烈建议您在推送到Hub时创建一个新模型，如果您想再次训练它或训练一个新版本。

## 第7步：在我们的演示中可视化一些匹配

现在您的模型是AI vs AI挑战的一部分，**您可以可视化它与其他模型的优劣**：[https://huggingface.co/spaces/unity/ML-Agents-SoccerTwos](https://huggingface.co/spaces/unity/ML-Agents-SoccerTwos)

为了做到这一点，您只需要转到此演示：

+   选择您的模型作为蓝队（或者如果您喜欢，作为紫队），并选择另一个模型进行竞争。与您的模型进行比较的最佳对手要么是排行榜上的第一名，要么是[基准模型](https://huggingface.co/unity/MLAgents-SoccerTwos)

您实时看到的比赛不会用于计算您的结果**但它们是可视化您的代理有多好的好方法**。

并且请不要犹豫在discord的#rl-i-made-this频道中分享您的代理获得的最佳分数🔥
