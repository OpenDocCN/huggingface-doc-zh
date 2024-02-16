# 实践：高级深度强化学习。使用Sample Factory从像素中玩Doom

> 原始文本：[https://huggingface.co/learn/deep-rl-course/unit8/hands-on-sf](https://huggingface.co/learn/deep-rl-course/unit8/hands-on-sf)

[![提问](../Images/255e59f8542cbd6d3f1c72646b2fff13.png)](http://hf.co/join/discord) [![在Colab中打开](../Images/7e2db436150c38a00650f96925aa5581.png)](https://colab.research.google.com/github/huggingface/deep-rl-class/blob/main/notebooks/unit8/unit8_part2.ipynb)

Colab笔记本：[![在Colab中打开](../Images/7e2db436150c38a00650f96925aa5581.png)](https://colab.research.google.com/github/huggingface/deep-rl-class/blob/master/notebooks/unit8/unit8_part2.ipynb)

# 第8单元 第2部分：高级深度强化学习。使用Sample Factory从像素中玩Doom

![缩略图](../Images/1636e0752d93a1e41c70f4a1147a2563.png)

在这个笔记本中，我们将学习如何训练一个深度神经网络，在基于Doom游戏的3D环境中收集物体，下面显示了结果策略的视频。我们使用[Sample Factory](https://www.samplefactory.dev/)来训练这个策略，这是PPO算法的异步实现。

请注意以下几点：

+   [Sample Factory](https://www.samplefactory.dev/)是一个先进的RL框架，**仅在Linux和Mac上运行**（不支持Windows）。

+   该框架在具有许多CPU核心的**GPU机器上表现最佳**，在那里可以实现每秒100k次交互的速度。标准Colab笔记本上可用的资源**限制了此库的性能**。因此，在这种设置中的速度**不反映真实世界的性能**。

+   Sample Factory的基准测试在多种设置中都可用，请查看[示例](https://github.com/alex-petrenko/sample-factory/tree/master/sf_examples)以了解更多信息。

```py
from IPython.display import HTML

HTML(
    """<video width="640" height="480" controls>
  <source src="https://huggingface.co/edbeeching/doom_health_gathering_supreme_3333/resolve/main/replay.mp4"
  type="video/mp4">Your browser does not support the video tag.</video>"""
)
```

为了验证这个实践过程的[认证过程](https://huggingface.co/deep-rl-course/en/unit0/introduction#certification-process)，您需要推送一个模型：

+   `doom_health_gathering_supreme` 获得 >= 5 的结果。

要找到您的结果，请转到[排行榜](https://huggingface.co/spaces/huggingface-projects/Deep-Reinforcement-Learning-Leaderboard)并找到您的模型，**结果 = 平均奖励 - 奖励的标准差**

如果找不到您的模型，**请转到页面底部并单击刷新按钮**

有关认证过程的更多信息，请查看此部分👉[https://huggingface.co/deep-rl-course/en/unit0/introduction#certification-process](https://huggingface.co/deep-rl-course/en/unit0/introduction#certification-process)

## 设置GPU 💪

+   为了**加速代理的训练，我们将使用GPU**。要做到这一点，请转到`运行时 > 更改运行时类型`

![GPU 步骤 1](../Images/5378127c314cdd92729aa31b7e11ca44.png)

+   `硬件加速器 > GPU`

![GPU 步骤 2](../Images/e0fec252447f98378386ccca8e57a80a.png)

在开始训练我们的代理之前，让我们**研究一下我们将要使用的库和环境**。

## Sample Factory

[Sample Factory](https://www.samplefactory.dev/)是一个**专注于非常高效的同步和异步策略梯度（PPO）实现的最快RL库之一**。

Sample Factory经过**彻底测试，被许多研究人员和从业者使用**，并且正在积极维护。我们的实现已知在各种领域中**达到SOTA性能，同时最小化RL实验训练时间和硬件要求**。

![Sample factory](../Images/cec8dbebc3e783f8e71a5698f72f4450.png)

### 主要特点

+   高度优化的算法[架构](https://www.samplefactory.dev/06-architecture/overview/)，以实现最大学习吞吐量

+   [同步和异步](https://www.samplefactory.dev/07-advanced-topics/sync-async/)训练制度

+   [串行（单进程）模式](https://www.samplefactory.dev/07-advanced-topics/serial-mode/)用于简单调试

+   在基于CPU和[GPU加速环境](https://www.samplefactory.dev/09-environment-integrations/isaacgym/)中实现最佳性能

+   单一和多代理训练，自我对弈，支持一次在一个或多个GPU上[训练多个策略](https://www.samplefactory.dev/07-advanced-topics/multi-policy-training/)

+   基于人口的训练（[PBT](https://www.samplefactory.dev/07-advanced-topics/pbt/)）

+   离散、连续、混合动作空间

+   基于向量、基于图像、基于字典的观察空间

+   通过解析动作/观察空间规范自动创建模型架构。支持[自定义模型架构](https://www.samplefactory.dev/03-customization/custom-models/)

+   设计为可导入其他项目，[自定义环境](https://www.samplefactory.dev/03-customization/custom-environments/)是一等公民

+   详细的[WandB和Tensorboard摘要](https://www.samplefactory.dev/05-monitoring/metrics-reference/)，[自定义指标](https://www.samplefactory.dev/05-monitoring/custom-metrics/)

+   [HuggingFace 🤗 集成](https://www.samplefactory.dev/10-huggingface/huggingface/)（上传训练模型和指标到Hub）

+   [多个](https://www.samplefactory.dev/09-environment-integrations/mujoco/) [示例](https://www.samplefactory.dev/09-environment-integrations/atari/) [环境](https://www.samplefactory.dev/09-environment-integrations/vizdoom/) [集成](https://www.samplefactory.dev/09-environment-integrations/dmlab/)，具有调整参数和训练模型

以上所有策略都可以在🤗 hub上找到。搜索标签[sample-factory](https://huggingface.co/models?library=sample-factory&sort=downloads)

### Sample-factory的工作原理

Sample-factory是**社区中最高度优化的RL实现之一**。

它通过**生成多个进程来运行推出工作者、推理工作者和学习者工作者**。

*工作者* **通过共享内存进行通信，降低了进程之间的通信成本**。

*推出工作者* 与环境交互并将观察发送给*推理工作者*。

*推理工作者* 查询策略的固定版本并**将动作发送回推出工作者**。

在*k*步骤之后，推出工作发送一条经验轨迹给学习者工作者，**用于更新代理的策略网络**。

![Sample factory](../Images/dbf151279f91bb01f87ae6359c6f520f.png)

### Sample-factory中的Actor Critic模型

Sample Factory中的Actor Critic模型由三个组件组成：

+   **编码器** - 处理输入观察（图像、向量）并将它们映射到一个向量。这是您最有可能想要自定义的模型的部分。

+   **核心** - 集成来自一个或多个编码器的向量，可以选择包括一个单层或多层LSTM/GRU在基于内存的代理中。

+   **解码器** - 在计算策略和价值输出之前，对模型核心的输出应用额外层。

该库已被设计为自动支持任何观察和动作空间。用户可以轻松添加自定义模型。您可以在[文档](https://www.samplefactory.dev/03-customization/custom-models/#actor-critic-models-in-sample-factory)中找到更多信息。

## ViZDoom

[ViZDoom](https://vizdoom.cs.put.edu.pl/)是一个**Doom引擎的开源Python接口**。

该库由波兰波兹南理工大学计算科学研究所的Marek Wydmuch、Michal Kempka于2016年创建。

该库使得**直接从屏幕像素训练代理在许多场景中成为可能**，包括在下面的视频中展示的团队死斗。由于ViZDoom环境基于90年代创建的游戏，它可以在现代硬件上以加速速度运行，**使我们能够相当快速地学习复杂的AI行为**。

该库包括以下功能：

+   多平台（Linux、macOS、Windows），

+   Python和C++的API，

+   [OpenAI Gym](https://www.gymlibrary.dev/)环境包装器

+   易于创建自定义场景（可视化编辑器，脚本语言和示例可用），

+   异步和同步单人和多人模式，

+   轻量级（几MB）和快速（同步模式下最高可达7000 fps，单线程），

+   可定制的分辨率和渲染参数，

+   访问深度缓冲区（3D视觉），

+   游戏中可见的游戏对象的自动标记，

+   访问音频缓冲区

+   访问演员/对象列表和地图几何，

+   离屏渲染和剧集录制，

+   异步模式中的时间缩放。

## 我们首先需要安装一些ViZDoom环境所需的依赖项

现在我们的Colab运行时已经设置好，我们可以开始安装运行ViZDoom所需的依赖项。

如果您正在使用Mac上的计算机，请按照[github页面](https://github.com/Farama-Foundation/ViZDoom/blob/master/doc/Quickstart.md#-quickstart-for-macos-and-anaconda3-python-36)上的安装说明进行操作。

```py
# Install ViZDoom deps from
# https://github.com/mwydmuch/ViZDoom/blob/master/doc/Building.md#-linux

apt-get install build-essential zlib1g-dev libsdl2-dev libjpeg-dev \
nasm tar libbz2-dev libgtk2.0-dev cmake git libfluidsynth-dev libgme-dev \
libopenal-dev timidity libwildmidi-dev unzip ffmpeg

# Boost libraries
apt-get install libboost-all-dev

# Lua binding dependencies
apt-get install liblua5.1-dev
```

## 然后我们可以安装Sample Factory和ViZDoom

+   这可能需要7分钟

```py
pip install sample-factory
pip install vizdoom
```

## 在sample-factory中设置毁灭环境

```py
import functools

from sample_factory.algo.utils.context import global_model_factory
from sample_factory.cfg.arguments import parse_full_cfg, parse_sf_args
from sample_factory.envs.env_utils import register_env
from sample_factory.train import run_rl

from sf_examples.vizdoom.doom.doom_model import make_vizdoom_encoder
from sf_examples.vizdoom.doom.doom_params import add_doom_env_args, doom_override_defaults
from sf_examples.vizdoom.doom.doom_utils import DOOM_ENVS, make_doom_env_from_spec

# Registers all the ViZDoom environments
def register_vizdoom_envs():
    for env_spec in DOOM_ENVS:
        make_env_func = functools.partial(make_doom_env_from_spec, env_spec)
        register_env(env_spec.name, make_env_func)

# Sample Factory allows the registration of a custom Neural Network architecture
# See https://github.com/alex-petrenko/sample-factory/blob/master/sf_examples/vizdoom/doom/doom_model.py for more details
def register_vizdoom_models():
    global_model_factory().register_encoder_factory(make_vizdoom_encoder)

def register_vizdoom_components():
    register_vizdoom_envs()
    register_vizdoom_models()

# parse the command line args and create a config
def parse_vizdoom_cfg(argv=None, evaluation=False):
    parser, _ = parse_sf_args(argv=argv, evaluation=evaluation)
    # parameters specific to Doom envs
    add_doom_env_args(parser)
    # override Doom default values for algo parameters
    doom_override_defaults(parser)
    # second parsing pass yields the final configuration
    final_cfg = parse_full_cfg(parser, argv)
    return final_cfg
```

现在设置完成，我们可以训练代理了。我们选择了学习一个名为`Health Gathering Supreme`的ViZDoom任务。

### 场景：Health Gathering Supreme

![Health-Gathering-Supreme](../Images/08a1a85695f5485b036e974dd75dc6b6.png)

这个场景的目标是**教会代理如何在不知道什么让它生存的情况下生存**。代理只知道**生命是宝贵的**，死亡是不好的，所以**它必须学会延长自己的存在，并且它的健康与生存有关**。

地图是一个包含墙壁的矩形，地板是绿色的，有毒，**会定期伤害玩家**。最初地图上均匀分布着一些医疗包。不时会有新的医疗包从天而降。**医疗包可以治愈玩家的一些健康部分** - 为了生存，代理需要捡起它们。当玩家死亡或超时时，剧集结束。

进一步配置：

+   Living_reward = 1

+   3个可用按钮：向左转，向右转，向前移动

+   1个可用的游戏变量：HEALTH

+   死刑 = 100

您可以在ViZDoom中找到更多可用的场景[这里](https://github.com/Farama-Foundation/ViZDoom/tree/master/scenarios)。

还有一些更复杂的场景已经为ViZDoom创建，例如在[此github页面](https://github.com/edbeeching/3d_control_deep_rl)上详细介绍的场景。

## 训练代理

+   我们将对代理进行4000000步的训练。大约需要20分钟

```py
## Start the training, this should take around 15 minutes
register_vizdoom_components()

# The scenario we train on today is health gathering
# other scenarios include "doom_basic", "doom_two_colors_easy", "doom_dm", "doom_dwango5", "doom_my_way_home", "doom_deadly_corridor", "doom_defend_the_center", "doom_defend_the_line"
env = "doom_health_gathering_supreme"
cfg = parse_vizdoom_cfg(
    argv=[f"--env={env}", "--num_workers=8", "--num_envs_per_worker=4", "--train_for_env_steps=4000000"]
)

status = run_rl(cfg)
```

## 让我们看一下训练策略的表现，并输出代理的视频。

```py
from sample_factory.enjoy import enjoy

cfg = parse_vizdoom_cfg(
    argv=[f"--env={env}", "--num_workers=1", "--save_video", "--no_render", "--max_num_episodes=10"], evaluation=True
)
status = enjoy(cfg)
```

## 现在让我们可视化代理的表现

```py
from base64 import b64encode
from IPython.display import HTML

mp4 = open("/content/train_dir/default_experiment/replay.mp4", "rb").read()
data_url = "data:video/mp4;base64," + b64encode(mp4).decode()
HTML(
    """
<video width=640 controls>
      <source src="%s" type="video/mp4">
</video>
"""
    % data_url
)
```

代理已经学到了一些东西，但它的表现可能会更好。我们显然需要进行更长时间的训练。但让我们将这个模型上传到Hub。

## 现在让我们将您的检查点和视频上传到Hugging Face Hub

为了能够与社区分享您的模型，还有三个步骤需要遵循：

1️⃣（如果尚未完成）在HF上创建一个帐户➡[https://huggingface.co/join](https://huggingface.co/join)

2️⃣ 登录并从Hugging Face网站获取您的身份验证令牌。

+   创建一个新的令牌（[https://huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)）**具有写入权限**

![创建HF令牌](../Images/d21a97c736edaab9119d2d1c1da9deac.png)

+   复制令牌

+   运行下面的单元格并粘贴令牌

如果您不想使用Google Colab或Jupyter Notebook，则需要使用此命令：`huggingface-cli login`

```py
from huggingface_hub import notebook_login
notebook_login()
!git config --global credential.helper store
```

```py
from sample_factory.enjoy import enjoy

hf_username = "ThomasSimonini"  # insert your HuggingFace username here

cfg = parse_vizdoom_cfg(
    argv=[
        f"--env={env}",
        "--num_workers=1",
        "--save_video",
        "--no_render",
        "--max_num_episodes=10",
        "--max_num_frames=100000",
        "--push_to_hub",
        f"--hf_repository={hf_username}/rl_course_vizdoom_health_gathering_supreme",
    ],
    evaluation=True,
)
status = enjoy(cfg)
```

## 让我们加载另一个模型

这个代理的表现很好，但我们可以做得更好！让我们从hub下载并可视化一个经过10B个时间步训练的代理。

```py
#download the agent from the hub
python -m sample_factory.huggingface.load_from_hub -r edbeeching/doom_health_gathering_supreme_2222 -d ./train_dir
```

```py
ls train_dir/doom_health_gathering_supreme_2222
```

```py
env = "doom_health_gathering_supreme"
cfg = parse_vizdoom_cfg(
    argv=[
        f"--env={env}",
        "--num_workers=1",
        "--save_video",
        "--no_render",
        "--max_num_episodes=10",
        "--experiment=doom_health_gathering_supreme_2222",
        "--train_dir=train_dir",
    ],
    evaluation=True,
)
status = enjoy(cfg)
```

```py
mp4 = open("/content/train_dir/doom_health_gathering_supreme_2222/replay.mp4", "rb").read()
data_url = "data:video/mp4;base64," + b64encode(mp4).decode()
HTML(
    """
<video width=640 controls>
      <source src="%s" type="video/mp4">
</video>
"""
    % data_url
)
```

## 一些额外的挑战🏆：毁灭对决

在Colab上进行Doom deathmatch的代理训练**需要比Colab中可用的更强大的机器花费很多小时**。

幸运的是，在这种情况下，我们已经训练过一个代理，并且它可以在🤗 Hub中使用！让我们下载模型并可视化代理的表现。

```py
# Download the agent from the hub
python -m sample_factory.huggingface.load_from_hub -r edbeeching/doom_deathmatch_bots_2222 -d ./train_dir
```

考虑到代理玩了很长时间，视频生成可能需要10分钟。

```py
from sample_factory.enjoy import enjoy

register_vizdoom_components()
env = "doom_deathmatch_bots"
cfg = parse_vizdoom_cfg(
    argv=[
        f"--env={env}",
        "--num_workers=1",
        "--save_video",
        "--no_render",
        "--max_num_episodes=1",
        "--experiment=doom_deathmatch_bots_2222",
        "--train_dir=train_dir",
    ],
    evaluation=True,
)
status = enjoy(cfg)
mp4 = open("/content/train_dir/doom_deathmatch_bots_2222/replay.mp4", "rb").read()
data_url = "data:video/mp4;base64," + b64encode(mp4).decode()
HTML(
    """
<video width=640 controls>
      <source src="%s" type="video/mp4">
</video>
"""
    % data_url
)
```

你可以尝试使用上面的代码在这个环境中训练你的代理，但不要在colab上。祝你好运🤞

如果你更喜欢一个更简单的情景，为什么不尝试在另一个ViZDoom情景中训练，比如`doom_deadly_corridor`或`doom_defend_the_center`。

* * *

这结束了最后一个单元。但我们还没有完成！🤗 接下来的**奖励部分包括一些最有趣、先进和尖端的深度强化学习工作**。

## 继续学习，保持出色🤗
