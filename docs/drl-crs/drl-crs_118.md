# Godot RL Agents

> 原文：[`huggingface.co/learn/deep-rl-course/unitbonus3/godotrl`](https://huggingface.co/learn/deep-rl-course/unitbonus3/godotrl)

[Godot RL Agents](https://github.com/edbeeching/godot_rl_agents)是一个开源软件包，允许视频游戏创作者、AI 研究人员和爱好者有机会**为他们的非玩家角色或代理学习复杂的行为**。

该库提供：

+   在[Godot Engine](https://godotengine.org/)创建的游戏和在 Python 中运行的机器学习算法之间的接口

+   四个知名强化学习框架的包装器：[StableBaselines3](https://stable-baselines3.readthedocs.io/en/master/)、[CleanRL](https://docs.cleanrl.dev/)、[Sample Factory](https://www.samplefactory.dev/)和[Ray RLLib](https://docs.ray.io/en/latest/rllib-algorithms.html)

+   支持基于记忆的代理，具有 LSTM 或基于注意力的接口

+   支持*2D 和 3D 游戏*

+   一套*AI 传感器*，增强您的代理观察游戏世界的能力

+   Godot 和 Godot RL Agents 都是**完全免费且在非常宽松的 MIT 许可下开源**。没有任何附加条件，没有版税，没有任何限制。

您可以在他们的[GitHub 页面](https://github.com/edbeeching/godot_rl_agents)或他们的 AAAI-2022 研讨会[论文](https://arxiv.org/abs/2112.03636)中了解更多关于 Godot RL 代理的信息。该库的创建者[Ed Beeching](https://edbeeching.github.io/)是 Hugging Face 的研究科学家。

安装库很简单：`pip install godot-rl`

## 使用 Godot RL Agents 创建自定义 RL 环境

在本节中，您将**学习如何在 Godot 游戏引擎中创建自定义环境**，然后实现一个学习使用深度强化学习玩游戏的 AI 控制器。

我们今天创建的示例游戏很简单，**但展示了 Godot Engine 和 Godot RL Agents 库的许多功能**。然后，您可以深入研究更复杂的环境和行为示例。

我们今天将构建的环境称为 Ring Pong，这是乒乓球游戏，但球场是一个环，球拍在环内移动。**目标是让球在环内弹跳**。

![Ring Pong](img/a4881f2cd962031d5828c7bf00344a3c.png)

### 安装 Godot 游戏引擎

[Godot 游戏引擎](https://godotengine.org/)是一个用于**创建视频游戏、工具和用户界面**的开源工具。

Godot Engine 是一个功能丰富的跨平台游戏引擎，旨在通过统一界面创建 2D 和 3D 游戏。它提供了一套常用工具，因此用户**可以专注于制作游戏，而无需重新发明轮子**。游戏可以一键导出到多个平台，包括主要的桌面平台（Linux、macOS、Windows）以及移动（Android、iOS）和基于 Web 的（HTML5）平台。

虽然我们将指导您实施代理的步骤，但您可能希望了解更多关于 Godot 游戏引擎的信息。他们的[文档](https://docs.godotengine.org/en/latest/index.html)非常详尽，YouTube 上也有许多教程，我们还推荐[GDQuest](https://www.gdquest.com/)、[KidsCanCode](https://kidscancode.org/godot_recipes/4.x/)和[Bramwell](https://www.youtube.com/channel/UCczi7Aq_dTKrQPF5ZV5J3gg)作为信息来源。

为了在 Godot 中创建游戏，**你必须首先下载编辑器**。Godot RL Agents 支持最新版本的 Godot，即 Godot 4.0。

可在以下链接下载：

+   [Windows](https://downloads.tuxfamily.org/godotengine/4.0.1/Godot_v4.0.1-stable_win64.exe.zip)

+   [Mac](https://downloads.tuxfamily.org/godotengine/4.0.1/Godot_v4.0.1-stable_macos.universal.zip)

+   [Linux](https://downloads.tuxfamily.org/godotengine/4.0.1/Godot_v4.0.1-stable_linux.x86_64.zip)

### 加载起始项目

我们提供两个版本的代码库：

+   [一个起始项目，可供下载并跟随本教程进行学习](https://drive.google.com/file/d/1C7xd3TibJHlxFEJPBgBLpksgxrFZ3D8e/view?usp=share_link)

+   [项目的最终版本，用于比较和调试。](https://drive.google.com/file/d/1k-b2Bu7uIA6poApbouX4c3sq98xqogpZ/view?usp=share_link)

在 Godot 项目管理器中点击**导入**来加载项目，导航到文件所在位置并加载**project.godot**文件。

如果按下 F5 或在编辑器中播放，您应该能够以人类模式玩游戏。游戏有几个实例在运行，这是因为我们希望通过许多并行环境加快训练我们的 AI 代理。

### 安装 Godot RL Agents 插件

Godot RL Agents 插件可以从 Github 仓库或在编辑器中的 Godot Asset Lib 中安装。

首先点击 AssetLib 并搜索“rl”

![Godot](img/d91cd6fd93e5d0a18afe0b076e727b0e.png)

然后点击 Godot RL Agents，点击下载并取消选择 LICENSE 和 README .md 文件。然后点击安装。

![Godot](img/26d4383db35914c111438c9d622a0477.png)

Godot RL Agents 插件现在已下载到您的计算机。现在点击项目→项目设置并启用插件：

![Godot](img/e03584370aca5150857b56f123707645.png)

### 添加 AI 控制器

现在我们想要在游戏中添加一个 AI 控制器。打开 player.tscn 场景，在左侧您应该看到一个节点层次结构，看起来像这样：

![Godot](img/b80e88fb3e9787bebe7132a2d76b405b.png)

右键单击**Player**节点，然后点击**添加子节点**。这里列出了许多节点，搜索 AIController3D 并创建它。

![Godot](img/1055ec5b346e15d208fbe0f03ab4751f.png)

AI Controller 节点应该已经添加到场景树中，旁边是一个滚动条。点击它以打开附加到 AIController 的脚本。Godot 游戏引擎使用一种名为 GDScript 的脚本语言，语法与 python 类似。脚本包含需要实现的方法，以使我们的 AI 控制器正常工作。

```py
#-- Methods that need implementing using the "extend script" option in Godot --#
func get_obs() -> Dictionary:
	assert(false, "the get_obs method is not implemented when extending from ai_controller")
	return {"obs":[]}

func get_reward() -> float:
	assert(false, "the get_reward method is not implemented when extending from ai_controller")
	return 0.0

func get_action_space() -> Dictionary:
	assert(false, "the get get_action_space method is not implemented when extending from ai_controller")
	return {
		"example_actions_continous" : {
			"size": 2,
			"action_type": "continuous"
		},
		"example_actions_discrete" : {
			"size": 2,
			"action_type": "discrete"
		},
		}

func set_action(action) -> void:
	assert(false, "the get set_action method is not implemented when extending from ai_controller")
# -----------------------------------------------------------------------------#
```

为了实现这些方法，我们需要创建一个继承自 AIController3D 的类。在 Godot 中，这很容易做到，称为“扩展”一个类。

右键单击 AIController3D 节点，然后点击“扩展脚本”，并将新脚本命名为`controller.gd`。现在你应该有一个几乎空白的脚本文件，看起来像这样：

```py
extends AIController3D

# Called when the node enters the scene tree for the first time.
func _ready():
	pass # Replace with function body.

# Called every frame. 'delta' is the elapsed time since the previous frame.
func _process(delta):
	pass
```

我们现在将实现 4 个缺失的方法，删除这段代码，并替换为以下内容：

```py
extends AIController3D

# Stores the action sampled for the agent's policy, running in python
var move_action : float = 0.0

func get_obs() -> Dictionary:
	# get the balls position and velocity in the paddle's frame of reference
	var ball_pos = to_local(_player.ball.global_position)
	var ball_vel = to_local(_player.ball.linear_velocity)
	var obs = [ball_pos.x, ball_pos.z, ball_vel.x/10.0, ball_vel.z/10.0]

	return {"obs":obs}

func get_reward() -> float:
	return reward

func get_action_space() -> Dictionary:
	return {
		"move_action" : {
			"size": 1,
			"action_type": "continuous"
		},
		}

func set_action(action) -> void:
	move_action = clamp(action["move_action"][0], -1.0, 1.0)
```

我们现在已经定义了代理的观察，即球在其本地坐标空间中的位置和速度。我们还定义了代理的动作空间，这是一个从-1 到+1 的单一连续值。

下一步是更新玩家的脚本以使用来自 AIController 的动作，通过点击玩家节点旁边的滚动条编辑玩家的脚本，将`Player.gd`中的代码更新为以下内容：

```py
extends Node3D

@export var rotation_speed = 3.0
@onready var ball = get_node("../Ball")
@onready var ai_controller = $AIController3D

func _ready():
	ai_controller.init(self)

func game_over():
	ai_controller.done = true
	ai_controller.needs_reset = true

func _physics_process(delta):
	if ai_controller.needs_reset:
		ai_controller.reset()
		ball.reset()
		return

	var movement : float
	if ai_controller.heuristic == "human":
		movement = Input.get_axis("rotate_anticlockwise", "rotate_clockwise")
	else:
		movement = ai_controller.move_action
	rotate_y(movement*delta*rotation_speed)

func _on_area_3d_body_entered(body):
	ai_controller.reward += 1.0
```

现在我们需要在 Godot 中运行的游戏与在 Python 中训练的神经网络之间进行同步。Godot RL agents 提供了一个节点来实现这一点。打开 train.tscn 场景，在根节点上右键单击，然后点击“添加子节点”。然后搜索“sync”并添加一个 Godot RL Agents Sync 节点。该节点处理 Python 和 Godot 之间的 TCP 通信。

您可以在编辑器中实时运行训练，首先使用`gdrl`启动 python 训练。

在这个简单的示例中，几分钟内学习了一个合理的策略。您可能希望加快训练速度，点击 train 场景中的 Sync 节点，您会看到编辑器中暴露了一个“加速”属性：

![Godot](img/a61acc5ada930f05e34ac3d7492e9adf.png)

尝试将此属性设置为 8 以加快训练速度。在更复杂的环境中，比如我们将在下一章学习的多人 FPS 中，这可能会带来很大的好处。

### 还有更多！

我们只是触及了 Godot RL Agents 可以实现的一小部分，该库包括自定义传感器和摄像头，以丰富可供代理使用的信息。查看[示例](https://github.com/edbeeching/godot_rl_agents_examples)以了解更多！

对于将训练好的模型导出为 .onnx 格式，这样您就可以直接从 Godot 中运行推理，而无需使用 Python 服务器，以及其他有用的训练选项，请查看[高级 SB3 教程](https://github.com/edbeeching/godot_rl_agents/blob/main/docs/ADV_STABLE_BASELINES_3.md)。

## 作者

本节由[Edward Beeching](https://twitter.com/edwardbeeching)撰写
