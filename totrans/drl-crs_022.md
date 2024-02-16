# 让我们训练和玩Huggy 🐶

> 原始文本：[https://huggingface.co/learn/deep-rl-course/unitbonus1/train](https://huggingface.co/learn/deep-rl-course/unitbonus1/train)

[![提问](../Images/255e59f8542cbd6d3f1c72646b2fff13.png)](http://hf.co/join/discord) [![在Colab中打开](../Images/7e2db436150c38a00650f96925aa5581.png)](https://colab.research.google.com/github/huggingface/deep-rl-class/blob/master/notebooks/bonus-unit1/bonus-unit1.ipynb)

我们强烈建议学生使用Google Colab进行实践练习，而不是在个人计算机上运行它们。

通过使用Google Colab，**您可以专注于学习和实验，而不必担心设置环境的技术细节**。

## 让我们训练Huggy 🐶

**要开始训练Huggy，请点击在Colab中打开按钮**👇：

[![在Colab中打开](../Images/7e2db436150c38a00650f96925aa5581.png)](https://colab.research.google.com/github/huggingface/deep-rl-class/blob/master/notebooks/bonus-unit1/bonus-unit1.ipynb)

![Bonus Unit 1缩略图](../Images/7875c1859ac22866c086670b47651bdc.png)

在这个笔记本中，我们将通过**教Huggy狗拿棍子，然后直接在浏览器中玩**来强化我们在第一单元学到的知识

![Huggy](../Images/57b557f6c646b773c9c9c0173a5adf4f.png)

### 环境 🎮

+   Huggy the Dog，由[Thomas Simonini](https://twitter.com/ThomasSimonini)创建的环境，基于[Puppo The Corgi](https://blog.unity.com/technology/puppo-the-corgi-cuteness-overload-with-the-unity-ml-agents-toolkit)

### 使用的库 📚

+   [MLAgents](https://github.com/Unity-Technologies/ml-agents)

我们不断努力改进我们的教程，所以**如果您在这个笔记本中发现一些问题**，请[在Github Repo上提出问题](https://github.com/huggingface/deep-rl-class/issues)。

## 本笔记本的目标 🏆

在笔记本的结尾，您将：

+   了解用于训练Huggy的**状态空间、动作空间和奖励函数**。

+   **训练您自己的Huggy** 去拿棍子。

+   能够**直接在浏览器中与您训练过的Huggy一起玩**。

## 先决条件🏗️

在深入笔记本之前，您需要：

🔲 📚 **通过完成第1单元来理解强化学习的基础**（MC，TD，奖励假设...）

🔲 📚 **通过完成Bonus Unit 1来阅读Huggy的介绍**

## 设置GPU💪

+   为了**加速代理的训练，我们将使用GPU**。要做到这一点，请转到`运行时 > 更改运行时类型`

![GPU步骤1](../Images/5378127c314cdd92729aa31b7e11ca44.png)

+   `硬件加速器 > GPU`

![GPU步骤2](../Images/e0fec252447f98378386ccca8e57a80a.png)

## 克隆存储库并安装依赖项🔽

+   我们需要克隆包含ML-Agents的存储库。

```py
# Clone the repository (can take 3min)
git clone --depth 1 https://github.com/Unity-Technologies/ml-agents
```

```py
# Go inside the repository and install the package (can take 3min)
%cd ml-agents
pip3 install -e ./ml-agents-envs
pip3 install -e ./ml-agents
```

## 下载并将环境zip文件移动到./trained-envs-executables/linux/

+   我们的环境可执行文件在一个zip文件中。

+   我们需要下载它并将其放置在`./trained-envs-executables/linux/`中

```py
mkdir ./trained-envs-executables
mkdir ./trained-envs-executables/linux
```

我们使用`wget`从[https://github.com/huggingface/Huggy](https://github.com/huggingface/Huggy)下载了文件Huggy.zip

```py
wget "https://github.com/huggingface/Huggy/raw/main/Huggy.zip" -O ./trained-envs-executables/linux/Huggy.zip
```

```py
%%capture
unzip -d ./trained-envs-executables/linux/ ./trained-envs-executables/linux/Huggy.zip
```

确保您的文件可以访问

```py
chmod -R 755 ./trained-envs-executables/linux/Huggy
```

## 让我们回顾一下这个环境是如何工作的

### 状态空间：Huggy感知到的内容。

Huggy不“看到”他的环境。相反，我们为他提供有关环境的信息：

+   目标（棍子）位置

+   他自己和目标之间的相对位置

+   他的腿的方向。

鉴于所有这些信息，Huggy**可以决定下一步采取哪些行动来实现他的目标**。

![Huggy](../Images/57b557f6c646b773c9c9c0173a5adf4f.png)

### 动作空间：Huggy可以执行的动作

![Huggy动作](../Images/df930c385e7a4439c314cb4356dc2db4.png)

**关节驱动Huggy的腿**。这意味着为了到达目标，Huggy需要**学会正确旋转他每条腿的关节马达，以便他可以移动**。

### 奖励函数

奖励函数设计成**Huggy将实现他的目标**：拿到棍子。

请记住，强化学习的基础之一是*奖励假设*：目标可以描述为**最大化预期的累积奖励**。

在这里，我们的目标是让Huggy**朝着棍子走，但不要转得太多**。因此，我们的奖励函数必须体现这一目标。

我们的奖励函数：

![Huggy reward function](../Images/02ce46ce021fe61f090b79e963e5fc0e.png)

+   *方向奖励*：我们**奖励他靠近目标**。

+   *时间惩罚*：在每个动作中给予的固定时间惩罚，**强迫他尽快到达棍子**。

+   *旋转惩罚*：如果**他转得太多并且转得太快**，我们会惩罚Huggy。

+   *达到目标奖励*：我们奖励Huggy**达到目标**。

## 检查Huggy配置文件

+   在ML-Agents中，您可以在config.yaml文件中定义**训练超参数。**

+   在本笔记本的范围内，我们不打算修改超参数，但如果您想尝试作为实验，Unity提供了非常[好的文档，在这里解释每一个](https://github.com/Unity-Technologies/ml-agents/blob/main/docs/Training-Configuration-File.md)。

+   我们需要为Huggy创建一个配置文件。

+   转到`/content/ml-agents/config/ppo`

+   创建一个名为`Huggy.yaml`的新文件

+   复制并粘贴下面的内容🔽

```py
behaviors:
  Huggy:
    trainer_type: ppo
    hyperparameters:
      batch_size: 2048
      buffer_size: 20480
      learning_rate: 0.0003
      beta: 0.005
      epsilon: 0.2
      lambd: 0.95
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: true
      hidden_units: 512
      num_layers: 3
      vis_encode_type: simple
    reward_signals:
      extrinsic:
        gamma: 0.995
        strength: 1.0
    checkpoint_interval: 200000
    keep_checkpoints: 15
    max_steps: 2e6
    time_horizon: 1000
    summary_freq: 50000
```

+   不要忘记保存文件！

+   **如果您想修改超参数**，在Google Colab笔记本中，您可以单击此处打开config.yaml文件：`/content/ml-agents/config/ppo/Huggy.yaml`

我们现在准备好训练我们的agent🔥。

## 训练我们的agent

要训练我们的agent，我们只需要**启动mlagents-learn并选择包含环境的可执行文件。**

![ml learn function](../Images/7345c12d798a0cc836b7f858c8315e94.png)

使用ML Agents，我们运行一个训练脚本。我们定义了四个参数：

1.  `mlagents-learn <config>`：超参数配置文件的路径。

1.  `--env`：环境可执行文件的位置。

1.  `--run-id`：您要为训练运行ID指定的名称。

1.  `--no-graphics`：在训练期间不启动可视化。

训练模型并使用`--resume`标志继续训练以防中断。

> 第一次使用`--resume`时会失败，请尝试再次运行该块以绕过错误。

训练将花费30到45分钟，取决于您的机器（不要忘记**设置GPU**），去喝杯☕️，您值得拥有🤗。

```py
mlagents-learn ./config/ppo/Huggy.yaml --env=./trained-envs-executables/linux/Huggy/Huggy --run-id="Huggy" --no-graphics
```

## 将代理推送到🤗 Hub

+   现在我们已经训练了我们的agent，我们**准备将其推送到Hub，以便在浏览器上与Huggy一起玩🔥。**

为了能够与社区分享您的模型，还有三个步骤要遵循：

1️⃣（如果尚未完成）创建一个HF帐户➡[https://huggingface.co/join](https://huggingface.co/join)

2️⃣ 登录，然后从Hugging Face网站获取您的令牌。

+   创建一个新的令牌（[https://huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)）**具有写入权限**

![Create HF Token](../Images/d21a97c736edaab9119d2d1c1da9deac.png)

+   复制令牌

+   运行下面的单元格并粘贴令牌

```py
from huggingface_hub import notebook_login

notebook_login()
```

如果您不想使用Google Colab或Jupyter Notebook，您需要使用此命令：`huggingface-cli login`

然后，我们只需要运行`mlagents-push-to-hf`。

![ml learn function](../Images/15fdd1817bf2650c653297025f24cfe5.png)

我们定义了4个参数：

1.  `--run-id`：训练运行ID的名称。

1.  `--local-dir`：代理保存的位置，是results/<run_id名称>，所以在我的情况下是results/First Training。

1.  `--repo-id`：您要创建或更新的Hugging Face repo的名称。它始终是<您的Hugging Face用户名>/<repo名称>如果repo不存在，**将自动创建**

1.  `--commit-message`：由于HF repos是git存储库，您需要提供提交消息。

```py
mlagents-push-to-hf --run-id="HuggyTraining" --local-dir="./results/Huggy" --repo-id="ThomasSimonini/ppo-Huggy" --commit-message="Huggy"
```

如果一切正常，您应该在过程结束时看到这个（但是网址不同😆）：

```py
Your model is pushed to the hub. You can view your model here: https://huggingface.co/ThomasSimonini/ppo-Huggy
```

这是你的模型存储库的链接。存储库包含一个模型卡，解释如何使用模型，你的Tensorboard日志和配置文件。**最棒的是它是一个git存储库，这意味着你可以有不同的提交，用新的推送更新你的存储库，打开Pull Requests等。**

![ml learn function](../Images/c394875425468d5d1214245cdd809c69.png)

但现在最好的部分来了：**能够在线与Huggy玩耍👀。**

## 和你的Huggy 🐕一起玩

这一步是最简单的：

+   在浏览器中打开Huggy游戏：[https://huggingface.co/spaces/ThomasSimonini/Huggy](https://huggingface.co/spaces/ThomasSimonini/Huggy)

+   点击Play with my Huggy model

![load-huggy](../Images/9ed3c5a5aa1a1ccd093d892f43f68ac3.png)

1.  在第1步，选择你的模型存储库，即模型ID（在我的情况下是ThomasSimonini/ppo-Huggy）。

1.  在第2步，**选择你想要重播的模型**：

+   我有多个，因为我们每500000个时间步保存一个模型。

+   但是因为我想要最新的那个，我选择了 `Huggy.onnx`

👉 **尝试使用不同的模型步骤来查看代理的改进。**

恭喜你完成了这个奖励单元！

现在你可以坐下来享受和你的Huggy 🐶玩耍了。而且**不要忘记通过与朋友分享Huggy来传递爱 🤗**。如果你在社交媒体上分享了关于它的内容，**请标记我们 @huggingface 和我 @simoninithomas**

![Huggy封面](../Images/43d3b16262aca3954e9ac9ccf73b9c3d.png)

## 继续学习，保持棒棒的 🤗
