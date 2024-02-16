# 动手实践

> 原文链接: [`huggingface.co/learn/deep-rl-course/unit5/hands-on`](https://huggingface.co/learn/deep-rl-course/unit5/hands-on)

![提问](http://hf.co/join/discord) ![在 Colab 中打开](https://colab.research.google.com/github/huggingface/deep-rl-class/blob/main/notebooks/unit5/unit5.ipynb)

我们学习了什么是 ML-Agents 以及它是如何工作的。我们还研究了我们将要使用的两个环境。现在我们准备训练我们的代理！

![环境](img/a536e3342e9e887aef01941b6e7dc2d3.png)

为了验证这个动手实践的认证流程，您**只需要将训练好的模型推送到 Hub**。**没有最低结果要求**才能验证这个动手实践。但是，如果您想获得良好的结果，您可以尝试达到以下目标：

+   对于[金字塔](https://huggingface.co/spaces/unity/ML-Agents-Pyramids)：平均奖励 = 1.75

+   对于[雪球目标](https://huggingface.co/spaces/ThomasSimonini/ML-Agents-SnowballTarget)：平均奖励 = 15 或 30 个目标在一个剧集中射击。

有关认证流程的更多信息，请查看此部分👉 [`huggingface.co/deep-rl-course/en/unit0/introduction#certification-process`](https://huggingface.co/deep-rl-course/en/unit0/introduction#certification-process)

**要开始动手实践，请点击“在 Colab 中打开”按钮** 👇 :

![在 Colab 中打开](https://colab.research.google.com/github/huggingface/deep-rl-class/blob/master/notebooks/unit5/unit5.ipynb)

我们强烈建议学生在动手练习中使用 Google Colab，而不是在个人电脑上运行它们。

通过使用 Google Colab，**您可以专注于学习和实验，而不必担心设置环境的技术细节**。

# Unit 5: ML-Agents 简介

![缩略图](img/de76e57f9c05ee846deb2b4e602f2f0b.png)

在这个笔记本中，您将了解 ML-Agents 并训练两个代理。

+   第一个将学会**向生成目标发射雪球**。

+   第二个需要按一个按钮生成一个金字塔，然后导航到金字塔，将其推倒，**并移动到顶部的金砖**。为了做到这一点，它将需要探索其环境，我们将使用一种称为好奇心的技术。

之后，您将能够**在浏览器上直接观看您的代理进行游戏**。

有关认证流程的更多信息，请查看此部分👉 [`huggingface.co/deep-rl-course/en/unit0/introduction#certification-process`](https://huggingface.co/deep-rl-course/en/unit0/introduction#certification-process)

⬇️ 这是**您将在本单元结束时实现的示例**。⬇️

![金字塔](img/8aed616d792e17cf89f7b4d3fac5559c.png) ![雪球目标](img/fb625fa1ee3e280912baaaa565489f.png)

### 🎮 环境:

+   [金字塔](https://github.com/Unity-Technologies/ml-agents/blob/main/docs/Learning-Environment-Examples.md#pyramids)

+   雪球目标

### 📚 RL 库:

+   [ML-Agents](https://github.com/Unity-Technologies/ml-agents)

我们不断努力改进我们的教程，所以**如果您在这个笔记本中发现了一些问题**，请在 GitHub Repo 上[提出问题](https://github.com/huggingface/deep-rl-class/issues)。

## 本笔记本的目标 🏆

在笔记本的末尾，您将：

+   了解**ML-Agents**的工作原理和环境库。

+   能够**在 Unity 环境中训练代理**。

## 先决条件 🏗️

在深入研究笔记本之前，您需要：

🔲 📚 **阅读[Unit 5](https://huggingface.co/deep-rl-course/unit5/introduction)** 🤗以了解 ML-Agents 是什么以及它是如何工作的

# 让我们训练我们的代理 🚀

## 设置 GPU 💪

+   为了**加速代理的训练，我们将使用 GPU**。要做到这一点，转到`运行时 > 更改运行时类型`

![GPU 步骤 1](img/5378127c314cdd92729aa31b7e11ca44.png)

+   `硬件加速器 > GPU`

![GPU 步骤 2](img/e0fec252447f98378386ccca8e57a80a.png)

## 克隆存储库并安装依赖🔽

+   我们需要克隆**包含实验版本库的存储库，该库允许您将训练的代理推送到 Hub。**

```py
# Clone the repository
git clone --depth 1 https://github.com/Unity-Technologies/ml-agents
```

```py
# Go inside the repository and install the package
cd ml-agents
pip install -e ./ml-agents-envs
pip install -e ./ml-agents
```

## SnowballTarget ⛄

如果您需要了解这个环境如何工作的复习，请查看这个部分👉 [`huggingface.co/deep-rl-course/unit5/snowball-target`](https://huggingface.co/deep-rl-course/unit5/snowball-target)

### 下载并将环境 zip 文件移动到 ./training-envs-executables/linux/

+   我们的环境可执行文件在一个 zip 文件中。

+   我们需要下载它并将其放置在 `./training-envs-executables/linux/`

+   我们使用 Linux 可执行文件，因为我们使用 colab，而 colab 机器的操作系统是 Ubuntu（Linux）

```py
# Here, we create training-envs-executables and linux
mkdir ./training-envs-executables
mkdir ./training-envs-executables/linux
```

我们使用 `wget` 从 [`github.com/huggingface/Snowball-Target`](https://github.com/huggingface/Snowball-Target) 下载了文件 SnowballTarget.zip

```py
wget "https://github.com/huggingface/Snowball-Target/raw/main/SnowballTarget.zip" -O ./training-envs-executables/linux/SnowballTarget.zip
```

我们解压可执行文件 executable.zip

```py
unzip -d ./training-envs-executables/linux/ ./training-envs-executables/linux/SnowballTarget.zip
```

确保您的文件是可访问的

```py
chmod -R 755 ./training-envs-executables/linux/SnowballTarget
```

### 定义 SnowballTarget 配置文件

+   在 ML-Agents 中，您在 config.yaml 文件中定义**训练超参数。**

有多个超参数。为了更好地理解它们，您应该阅读[文档](https://github.com/Unity-Technologies/ml-agents/blob/release_20_docs/docs/Training-Configuration-File.md)中每个超参数的解释

您需要在 ./content/ml-agents/config/ppo/ 中创建一个 `SnowballTarget.yaml` 配置文件

我们将为您提供此配置的初步版本（复制并粘贴到您的 `SnowballTarget.yaml` 文件中），**但您应该进行修改**。

```py
behaviors:
  SnowballTarget:
    trainer_type: ppo
    summary_freq: 10000
    keep_checkpoints: 10
    checkpoint_interval: 50000
    max_steps: 200000
    time_horizon: 64
    threaded: true
    hyperparameters:
      learning_rate: 0.0003
      learning_rate_schedule: linear
      batch_size: 128
      buffer_size: 2048
      beta: 0.005
      epsilon: 0.2
      lambd: 0.95
      num_epoch: 3
    network_settings:
      normalize: false
      hidden_units: 256
      num_layers: 2
      vis_encode_type: simple
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
```

![配置 SnowballTarget](img/be3b76b22dafd564ace7c1b867395093.png) ![配置 SnowballTarget](img/3188bf2d59c42f6f7bc7435114c69c75.png)

作为实验，请尝试修改一些其他超参数。Unity 在[这里提供了非常好的文档，解释了每个超参数](https://github.com/Unity-Technologies/ml-agents/blob/main/docs/Training-Configuration-File.md)。

现在您已经创建了配置文件并了解了大多数超参数的作用，我们准备训练我们的代理🔥。

### 训练代理

要训练我们的代理，我们需要**启动 mlagents-learn 并选择包含环境的可执行文件。**

我们定义了四个参数：

1.  `mlagents-learn <config>`：超参数配置文件的路径。

1.  `--env`：环境可执行文件所在的位置。

1.  `--run_id`：您想要为训练运行 ID 指定的名称。

1.  `--no-graphics`：在训练期间不启动可视化。

![MlAgents learn](img/031eb13548c8d61ed2dbe5b169712883.png)

训练模型并使用`--resume`标志以便在中断的情况下继续训练。

> 如果您使用 `--resume` 时第一次失败，尝试重新运行该块以绕过错误。

训练将根据您的配置需要 10 到 35 分钟。去喝杯☕️，你值得的🤗。

```py
mlagents-learn ./config/ppo/SnowballTarget.yaml --env=./training-envs-executables/linux/SnowballTarget/SnowballTarget --run-id="SnowballTarget1" --no-graphics
```

### 将代理推送到 Hugging Face Hub

+   现在我们已经训练了我们的代理，我们**准备将其推送到 Hub 并在浏览器上可视化它的运行🔥。**

为了能够与社区分享您的模型，还有三个步骤需要遵循：

1️⃣（如果尚未完成）创建一个 HF 帐户 ➡ [`huggingface.co/join`](https://huggingface.co/join)

2️⃣ 登录并从 Hugging Face 网站存储您的身份验证令牌。

+   创建一个新的令牌（[`huggingface.co/settings/tokens`](https://huggingface.co/settings/tokens)）**具有写入权限**

![创建 HF 令牌](img/d21a97c736edaab9119d2d1c1da9deac.png)

+   复制令牌

+   运行下面的单元格并粘贴令牌

```py
from huggingface_hub import notebook_login

notebook_login()
```

如果您不想使用 Google Colab 或 Jupyter Notebook，您需要使用此命令：`huggingface-cli login`

然后我们需要运行 `mlagents-push-to-hf`。

我们定义了四个参数：

1.  `--run-id`：训练运行 ID 的名称。

1.  `--local-dir`：代理保存的位置，是 results/<run_id name>，所以在我的情况下是 results/First Training。

1.  `--repo-id`：您要创建或更新的 Hugging Face 存储库的名称。它始终是 <您的 huggingface 用户名>/<存储库名称> 如果存储库不存在**将自动创建**

1.  `--commit-message`：由于 HF 仓库是 git 仓库，您需要提供提交消息。

![推送到 Hub](img/e5f3533da18ced3904ea589dc1d88cec.png)

例如：

`mlagents-push-to-hf --run-id="SnowballTarget1" --local-dir="./results/SnowballTarget1" --repo-id="ThomasSimonini/ppo-SnowballTarget" --commit-message="First Push"`

```py
mlagents-push-to-hf  --run-id= # Add your run id  --local-dir= # Your local dir  --repo-id= # Your repo id  --commit-message= # Your commit message
```

如果一切正常，您应该在过程结束时看到这个（但网址不同😆）：

```py
Your model is pushed to the hub. You can view your model here: https://huggingface.co/ThomasSimonini/ppo-SnowballTarget
```

这是您模型的链接。它包含一个解释如何使用它的模型卡片，您的 Tensorboard 和配置文件。**令人惊奇的是，它是一个 git 仓库，这意味着您可以有不同的提交，使用新的推送更新您的仓库等。**

但现在最好的部分来了：**能够在线可视化您的代理👀。**

### 观看您的代理游戏👀

这一步很简单：

1.  记住您的 repo-id

1.  点击这里：[`huggingface.co/spaces/ThomasSimonini/ML-Agents-SnowballTarget`](https://huggingface.co/spaces/ThomasSimonini/ML-Agents-SnowballTarget)

1.  启动游戏并通过点击右下角按钮将其全屏显示

![Snowballtarget 加载](img/885269944cb24970c22e13dd919445f3.png)

1.  在第 1 步中，选择您的模型仓库，即模型 ID（在我这里是 ThomasSimonini/ppo-SnowballTarget）。

1.  在第 2 步中，**选择您想要重播的模型**：

+   我有多个，因为我们每 500000 个时间步保存一个模型。

+   但如果我想要更近期的，我选择`SnowballTarget.onnx`

👉很好地**尝试不同的模型阶段以查看代理的改进。**

并且不要犹豫在 discord 的#rl-i-made-this 频道分享您的代理获得的最佳分数🔥

现在让我们尝试一个更具挑战性的环境，名为 Pyramids。

## 金字塔🏆

### 下载并移动环境 zip 文件到`./training-envs-executables/linux/`

+   我们的环境可执行文件在一个 zip 文件中。

+   我们需要下载它并将其放入`./training-envs-executables/linux/`

+   我们使用 Linux 可执行文件，因为我们在使用 colab，而 colab 机器的操作系统是 Ubuntu（Linux）

使用`wget`从[`drive.google.com/uc?export=download&id=1UiFNdKlsH0NTu32xV-giYUEVKV4-vc7H`](https://drive.google.com/uc?export=download&id=1UiFNdKlsH0NTu32xV-giYUEVKV4-vc7H)下载文件 Pyramids.zip。查看从 GDrive 下载大文件的完整解决方案[这里](https://bcrf.biochem.wisc.edu/2021/02/05/download-google-drive-files-using-wget/)

```py
!wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=1UiFNdKlsH0NTu32xV-giYUEVKV4-vc7H' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=1UiFNdKlsH0NTu32xV-giYUEVKV4-vc7H" -O ./training-envs-executables/linux/Pyramids.zip && rm -rf /tmp/cookies.txt
```

解压缩它

```py
%%capture
!unzip -d ./training-envs-executables/linux/ ./training-envs-executables/linux/Pyramids.zip
```

确保您的文件可访问

```py
chmod -R 755 ./training-envs-executables/linux/Pyramids/Pyramids
```

### 修改 PyramidsRND 配置文件

+   与第一个环境相反，第一个环境是自定义的，**Pyramids 是由 Unity 团队制作的**。

+   因此，PyramidsRND 配置文件已经存在，并位于./content/ml-agents/config/ppo/PyramidsRND.yaml

+   您可能会问为什么“RND”在 PyramidsRND 中。RND 代表*随机网络蒸馏*，这是一种生成好奇奖励的方法。如果您想了解更多信息，请阅读我们撰写的解释此技术的文章：[`medium.com/data-from-the-trenches/curiosity-driven-learning-through-random-network-distillation-488ffd8e5938`](https://medium.com/data-from-the-trenches/curiosity-driven-learning-through-random-network-distillation-488ffd8e5938)

对于这次训练，我们将修改一件事：

+   总训练步骤的超参数太高了，因为我们只需在 100 万个训练步骤中就可以达到基准（平均奖励=1.75）。👉为了做到这一点，我们转到 config/ppo/PyramidsRND.yaml，**并将 max_steps 更改为 1000000。**

![Pyramids 配置](img/3a69b29716f0a6185c5dec0998d811f3.png)

作为一个实验，您还应该尝试修改一些其他超参数。Unity 提供了非常[好的文档，解释每一个超参数在这里](https://github.com/Unity-Technologies/ml-agents/blob/main/docs/Training-Configuration-File.md)。

我们现在准备好训练我们的代理🔥。

### 训练代理

训练将需要 30 到 45 分钟，取决于您的机器，去喝杯☕️，您值得拥有🤗。

```py
mlagents-learn ./config/ppo/PyramidsRND.yaml --env=./training-envs-executables/linux/Pyramids/Pyramids --run-id="Pyramids Training" --no-graphics
```

### 将代理推送到 Hugging Face Hub

+   现在我们已经训练了我们的代理，我们**准备将其推送到 Hub，以便能够在浏览器上可视化它的游戏🔥。**

```py
mlagents-push-to-hf  --run-id= # Add your run id  --local-dir= # Your local dir  --repo-id= # Your repo id  --commit-message= # Your commit message
```

### 看着你的 agent 玩耍👀

👉 [`huggingface.co/spaces/unity/ML-Agents-Pyramids`](https://huggingface.co/spaces/unity/ML-Agents-Pyramids)

### 🎁奖励：为什么不在另一个环境上训练呢？

现在你知道如何使用 MLAgents 训练一个 agent 了，为什么不尝试另一个环境呢？

MLAgents 提供了 17 个不同的环境，我们正在构建一些定制的环境。学习的最佳方式是尝试自己动手，玩得开心。

![cover](img/9cfcb902144d3f31941072483268cb93.png)

你可以在 Hugging Face 这里找到当前可用环境的完整列表👉 [`github.com/huggingface/ml-agents#the-environments`](https://github.com/huggingface/ml-agents#the-environments)

查看演示以可视化你的 agent 👉 [`huggingface.co/unity`](https://huggingface.co/unity)

目前我们已经集成了：

+   [Worm](https://huggingface.co/spaces/unity/ML-Agents-Worm)演示，你可以教一个**蠕虫爬行**。

+   [Walker](https://huggingface.co/spaces/unity/ML-Agents-Walker)演示，你可以教一个 agent**向目标走去**。

今天就到这里。恭喜你完成了这个教程！

学习的最佳方式是练习和尝试。为什么不尝试另一个环境呢？ML-Agents 有 18 个不同的环境，但你也可以创建自己的环境。查看文档，玩得开心！

我们在 Unit 6 见🔥，

## 继续学习，保持精彩🤗
