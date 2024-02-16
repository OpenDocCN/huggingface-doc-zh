# 实践

> 原始文本：[https://huggingface.co/learn/deep-rl-course/unit4/hands-on](https://huggingface.co/learn/deep-rl-course/unit4/hands-on)

[![提问](../Images/255e59f8542cbd6d3f1c72646b2fff13.png)](http://hf.co/join/discord) [![在Colab中打开](../Images/7e2db436150c38a00650f96925aa5581.png)](https://colab.research.google.com/github/huggingface/deep-rl-class/blob/main/notebooks/unit4/unit4.ipynb)

现在我们已经研究了Reinforce背后的理论，**您已经准备好使用PyTorch编写您的Reinforce代理**。并且您将使用CartPole-v1和PixelCopter测试其稳健性。

然后，您将能够迭代和改进此实现，以适用于更高级的环境。

![Environments](../Images/3b1f63eab47a364ef05dcdca4df7bf08.png)

为了验证这个实践过程，您需要将训练好的模型推送到Hub并：

+   获得`Cartpole-v1`的结果>= 350

+   获得`PixelCopter`的结果>= 5。

要找到您的结果，请转到排行榜并找到您的模型，**结果=平均奖励-奖励的标准差**。**如果您在排行榜上找不到您的模型，请转到排行榜页面底部，然后单击刷新按钮**。

**如果您找不到您的模型，请转到页面底部，然后单击刷新按钮。**

有关认证过程的更多信息，请查看此部分👉 [https://huggingface.co/deep-rl-course/en/unit0/introduction#certification-process](https://huggingface.co/deep-rl-course/en/unit0/introduction#certification-process)

您可以在这里检查您的进度👉 [https://huggingface.co/spaces/ThomasSimonini/Check-my-progress-Deep-RL-Course](https://huggingface.co/spaces/ThomasSimonini/Check-my-progress-Deep-RL-Course)

**要开始实践，请点击“在Colab中打开”按钮**👇：

[![在Colab中打开](../Images/7e2db436150c38a00650f96925aa5581.png)](https://colab.research.google.com/github/huggingface/deep-rl-class/blob/master/notebooks/unit4/unit4.ipynb)

我们强烈建议学生们在进行实践练习时使用Google Colab**，而不是在个人计算机上运行它们**。

通过使用Google Colab，**您可以专注于学习和实验，而不必担心设置环境的技术细节**。

# 第4单元：使用PyTorch编写您的第一个深度强化学习算法：Reinforce。并测试其稳健性💪

![缩略图](../Images/207886028f30a9a8c43010256f915e88.png)

在这本笔记本中，您将从头开始编写您的第一个深度强化学习算法：Reinforce（也称为蒙特卡洛策略梯度）。

Reinforce是一种*基于策略的方法*：一种深度强化学习算法，试图**直接优化策略，而不使用动作值函数**。

更准确地说，Reinforce是一种*策略梯度方法*，是*基于策略的方法*的一个子类，旨在**通过估计最优策略的权重来直接优化策略，使用梯度上升**。

为了测试其稳健性，我们将在2个不同的简单环境中对其进行训练：

+   Cartpole-v1

+   PixelcopterEnv

⬇️ 这是**您将在本笔记本末尾实现的示例。** ⬇️

![Environments](../Images/3b1f63eab47a364ef05dcdca4df7bf08.png)

### 🎮 环境：

+   [CartPole-v1](https://www.gymlibrary.dev/environments/classic_control/cart_pole/)

+   [PixelCopter](https://pygame-learning-environment.readthedocs.io/en/latest/user/games/pixelcopter.html)

### 📚 RL-Library:

+   Python

+   PyTorch

我们不断努力改进我们的教程，所以**如果您在本笔记本中发现一些问题**，请[在GitHub Repo上提出问题](https://github.com/huggingface/deep-rl-class/issues)。

## 本笔记本的目标🏆

在笔记本的末尾，您将：

+   能够**使用PyTorch从头开始编写一个Reinforce算法**。

+   能够**使用简单环境测试您的代理的稳健性**。

+   能够**将训练好的代理推送到Hub**，并附带一个漂亮的视频回放和评估分数🔥。

## 先决条件🏗️

在深入研究笔记本之前，您需要：

🔲 📚 [通过阅读第4单元来学习策略梯度](https://huggingface.co/deep-rl-course/unit4/introduction)

# 让我们从头开始编写Reinforce算法 🔥

## 一些建议 💡

最好在您的谷歌驱动器上运行此colab，这样**如果超时**，您仍然可以在您的谷歌驱动器上保存笔记本，并且不需要从头开始填写所有内容。

为此，您可以使用 `Ctrl + S` 或 `文件 > 在谷歌驱动器中保存副本。`

## 设置GPU 💪

+   为了**加速代理的训练，我们将使用GPU**。为此，请转到 `运行时 > 更改运行时类型`

![GPU 步骤 1](../Images/5378127c314cdd92729aa31b7e11ca44.png)

+   `硬件加速器 > GPU`

![GPU 步骤 2](../Images/e0fec252447f98378386ccca8e57a80a.png)

## 创建一个虚拟显示 🖥

在笔记本中，我们需要生成一个重播视频。为此，在colab中，**我们需要有一个虚拟屏幕来渲染环境**（从而记录帧）。

以下单元格将安装库并创建并运行一个虚拟屏幕 🖥

```py
%%capture
!apt install python-opengl
!apt install ffmpeg
!apt install xvfb
!pip install pyvirtualdisplay
!pip install pyglet==1.5.1
```

```py
# Virtual display
from pyvirtualdisplay import Display

virtual_display = Display(visible=0, size=(1400, 900))
virtual_display.start()
```

## 安装依赖 🔽

第一步是安装依赖项。我们将安装多个：

+   `gym`

+   `gym-games`：使用PyGame制作的额外gym环境。

+   `huggingface_hub`：Hub作为一个中心地方，任何人都可以分享和探索模型和数据集。它具有版本控制、指标、可视化和其他功能，可以让您轻松与他人合作。

您可能想知道为什么我们安装gym而不是gymnasium，一个更新版本的gym？**因为我们使用的gym-games还没有与gymnasium更新。**

在这里你会遇到的区别：

+   在`gym`中，我们没有`terminated`和`truncated`，只有`done`。

+   在`gym`中，使用`env.step()`返回`state, reward, done, info`

您可以在这里了解Gym和Gymnasium之间的区别 👉 [https://gymnasium.farama.org/content/migration-guide/](https://gymnasium.farama.org/content/migration-guide/)

您可以在这里看到所有可用的Reinforce模型 👉 [https://huggingface.co/models?other=reinforce](https://huggingface.co/models?other=reinforce)

您可以在这里找到所有深度强化学习模型 👉 [https://huggingface.co/models?pipeline_tag=reinforcement-learning](https://huggingface.co/models?pipeline_tag=reinforcement-learning)

```py
!pip install -r https://raw.githubusercontent.com/huggingface/deep-rl-class/main/notebooks/unit4/requirements-unit4.txt
```

## 导入包 📦

除了导入已安装的库之外，我们还导入：

+   `imageio`：一个可以帮助我们生成重播视频的库

```py
import numpy as np

from collections import deque

import matplotlib.pyplot as plt
%matplotlib inline

# PyTorch
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torch.distributions import Categorical

# Gym
import gym
import gym_pygame

# Hugging Face Hub
from huggingface_hub import notebook_login # To log to our Hugging Face account to be able to upload models to the Hub.
import imageio
```

## 检查一下是否有GPU

+   让我们检查一下是否有GPU

+   如果是的话，你应该看到 `device:cuda0`

```py
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
```

```py
print(device)
```

我们现在准备实现我们的Reinforce算法 🔥

# 第一个代理：玩CartPole-v1 🤖

## 创建CartPole环境并了解其工作原理

### 环境 🎮

### 为什么我们要使用像CartPole-v1这样简单的环境？

如[强化学习技巧和技巧](https://stable-baselines3.readthedocs.io/en/master/guide/rl_tips.html)中所解释的，当您从头开始实现代理时，您需要**确保它能够正确运行，并在更简单的环境中找到错误**，因为在简单环境中找到错误会更容易。

> 尝试在玩具问题上有一些“生命迹象”
> 
> 通过在越来越困难的环境中运行来验证实现（您可以将结果与RL动物园进行比较）。通常需要运行超参数优化来进行这一步。

### CartPole-v1环境

> 一个杆通过一个未激活的关节连接到一个小车上，该小车沿着一个无摩擦的轨道移动。摆放在小车上方，并且目标是通过在小车左右方向上施加力来平衡杆。

所以，我们从CartPole-v1开始。目标是将小车向左或向右推动，**使杆保持在平衡状态。**

如果：

+   杆角大于±12°

+   小车位置大于±2.4

+   该集数长度大于500

我们在每个时间步获得 +1 的奖励 💰，杆保持在平衡状态。

```py
env_id = "CartPole-v1"
# Create the env
env = gym.make(env_id)

# Create the evaluation env
eval_env = gym.make(env_id)

# Get the state space and action space
s_size = env.observation_space.shape[0]
a_size = env.action_space.n
```

```py
print("_____OBSERVATION SPACE_____ \n")
print("The State Space is: ", s_size)
print("Sample observation", env.observation_space.sample())  # Get a random observation
```

```py
print("\n _____ACTION SPACE_____ \n")
print("The Action Space is: ", a_size)
print("Action Space Sample", env.action_space.sample())  # Take a random action
```

## 让我们构建Reinforce架构

这个实现基于三个实现：

+   [PyTorch官方强化学习示例](https://github.com/pytorch/examples/blob/main/reinforcement_learning/reinforce.py)

+   [Udacity Reinforce](https://github.com/udacity/deep-reinforcement-learning/blob/master/reinforce/REINFORCE.ipynb)

+   [由Chris1nexus改进的集成](https://github.com/huggingface/deep-rl-class/pull/95)

![Reinforce](../Images/36cf0376b1e1c1f32df0bf4cc6199001.png)

所以我们希望：

+   两个全连接层（fc1和fc2）。

+   使用ReLU作为fc1的激活函数

+   使用Softmax输出动作的概率分布

```py
class Policy(nn.Module):
    def __init__(self, s_size, a_size, h_size):
        super(Policy, self).__init__()
        # Create two fully connected layers

    def forward(self, x):
        # Define the forward pass
        # state goes to fc1 then we apply ReLU activation function

        # fc1 outputs goes to fc2

        # We output the softmax

    def act(self, state):
        """
        Given a state, take action
        """
        state = torch.from_numpy(state).float().unsqueeze(0).to(device)
        probs = self.forward(state).cpu()
        m = Categorical(probs)
        action = np.argmax(m)
        return action.item(), m.log_prob(action)
```

### 解决方案

```py
class Policy(nn.Module):
    def __init__(self, s_size, a_size, h_size):
        super(Policy, self).__init__()
        self.fc1 = nn.Linear(s_size, h_size)
        self.fc2 = nn.Linear(h_size, a_size)

    def forward(self, x):
        x = F.relu(self.fc1(x))
        x = self.fc2(x)
        return F.softmax(x, dim=1)

    def act(self, state):
        state = torch.from_numpy(state).float().unsqueeze(0).to(device)
        probs = self.forward(state).cpu()
        m = Categorical(probs)
        action = np.argmax(m)
        return action.item(), m.log_prob(action)
```

我犯了一个错误，你能猜到在哪里吗？

+   为了找出来，让我们进行一次前向传播：

```py
debug_policy = Policy(s_size, a_size, 64).to(device)
debug_policy.act(env.reset())
```

+   在这里我们看到错误说`ValueError: The value argument to log_prob must be a Tensor`

+   这意味着`m.log_prob(action)`中的`action`必须是一个张量，但它不是。

+   你知道为什么吗？检查act函数，看看为什么它不起作用。

建议💡：这个实现中有问题。记住对于act函数，我们想要从动作的概率分布中采样一个动作。

### （真正的）解决方案

```py
class Policy(nn.Module):
    def __init__(self, s_size, a_size, h_size):
        super(Policy, self).__init__()
        self.fc1 = nn.Linear(s_size, h_size)
        self.fc2 = nn.Linear(h_size, a_size)

    def forward(self, x):
        x = F.relu(self.fc1(x))
        x = self.fc2(x)
        return F.softmax(x, dim=1)

    def act(self, state):
        state = torch.from_numpy(state).float().unsqueeze(0).to(device)
        probs = self.forward(state).cpu()
        m = Categorical(probs)
        action = m.sample()
        return action.item(), m.log_prob(action)
```

通过使用CartPole，更容易调试，因为我们知道错误来自于我们的集成，而不是我们简单的环境。

+   因为我们想要从动作概率分布中采样一个动作，所以不能使用`action = np.argmax(m)`，因为它总是输出具有最高概率的动作。

+   我们需要用`action = m.sample()`替换这个，它将从概率分布P(.|s)中采样一个动作

### 让我们构建Reinforce训练算法

这是Reinforce算法的伪代码：

![策略梯度伪代码](../Images/98fc23971db3e1a9e35baeee827641dc.png)

+   当我们计算回报Gt（第6行）时，我们看到我们计算从时间步t开始的折扣奖励的总和。

+   为什么？因为我们的策略应该只根据后果来强化动作：因此在采取行动之前获得的奖励是无用的（因为它们不是因为行动），只有之后的奖励才重要。

+   在编写这之前，你应该阅读这一部分[不要让过去分散你的注意力](https://spinningup.openai.com/en/latest/spinningup/rl_intro3.html#don-t-let-the-past-distract-you)，解释了为什么我们使用奖励逐步策略梯度。

我们使用了一个由[Chris1nexus](https://github.com/Chris1nexus)编写的有趣的技术来高效地计算每个时间步的回报。评论中解释了该过程。也不要犹豫[查看PR解释](https://github.com/huggingface/deep-rl-class/pull/95)，但总体思路是高效地计算每个时间步的回报。

第二个问题你可能会问的是为什么我们要最小化损失？之前不是讨论过梯度上升，而不是梯度下降吗？

+   我们想要最大化我们的效用函数$J(\theta)$，但在PyTorch和TensorFlow中，更好的做法是最小化一个目标函数。

    +   假设我们想要在某个时间步强化动作3。在训练这个动作之前，P为0.25。

    +   所以我们想要修改θ theta，使得πθ​(a3​∣s;θ)>0.25

    +   因为所有P必须总和为1，max<math><semantics><mrow><mi>p</mi><msub><mi>i</mi><mi>θ</mi></msub><mo stretchy="false">(</mo><msub><mi>a</mi><mn>3</mn></msub><mi mathvariant="normal">∣</mi><mi>s</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">pi_\theta(a_3|s; \theta)</annotation></semantics></math>piθ​(a3​∣s;θ)将**最小化其他动作的概率。**

    +   所以我们应该告诉PyTorch**最小化<math><semantics><mrow><mn>1</mn><mo>−</mo><msub><mi>π</mi><mi>θ</mi></msub><mo stretchy="false">(</mo><msub><mi>a</mi><mn>3</mn></msub><mi mathvariant="normal">∣</mi><mi>s</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">1 - \pi_\theta(a_3|s; \theta)</annotation></semantics></math>1−πθ​(a3​∣s;θ)。**

    +   这个损失函数在<math><semantics><mrow><msub><mi>π</mi><mi>θ</mi></msub><mo stretchy="false">(</mo><msub><mi>a</mi><mn>3</mn></msub><mi mathvariant="normal">∣</mi><mi>s</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">\pi_\theta(a_3|s; \theta)</annotation></semantics></math>πθ​(a3​∣s;θ)接近1时趋近于0。

    +   因此，我们鼓励梯度最大化<math><semantics><mrow><msub><mi>π</mi><mi>θ</mi></msub><mo stretchy="false">(</mo><msub><mi>a</mi><mn>3</mn></msub><mi mathvariant="normal">∣</mi><mi>s</mi><mo separator="true">;</mo><mi>θ</mi><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">\pi_\theta(a_3|s; \theta)</annotation></semantics></math>πθ​(a3​∣s;θ)

```py
def reinforce(policy, optimizer, n_training_episodes, max_t, gamma, print_every):
    # Help us to calculate the score during the training
    scores_deque = deque(maxlen=100)
    scores = []
    # Line 3 of pseudocode
    for i_episode in range(1, n_training_episodes+1):
        saved_log_probs = []
        rewards = []
        state = # TODO: reset the environment
        # Line 4 of pseudocode
        for t in range(max_t):
            action, log_prob = # TODO get the action
            saved_log_probs.append(log_prob)
            state, reward, done, _ = # TODO: take an env step
            rewards.append(reward)
            if done:
                break
        scores_deque.append(sum(rewards))
        scores.append(sum(rewards))

        # Line 6 of pseudocode: calculate the return
        returns = deque(maxlen=max_t)
        n_steps = len(rewards)
        # Compute the discounted returns at each timestep,
        # as the sum of the gamma-discounted return at time t (G_t) + the reward at time t

        # In O(N) time, where N is the number of time steps
        # (this definition of the discounted return G_t follows the definition of this quantity
        # shown at page 44 of Sutton&Barto 2017 2nd draft)
        # G_t = r_(t+1) + r_(t+2) + ...

        # Given this formulation, the returns at each timestep t can be computed
        # by re-using the computed future returns G_(t+1) to compute the current return G_t
        # G_t = r_(t+1) + gamma*G_(t+1)
        # G_(t-1) = r_t + gamma* G_t
        # (this follows a dynamic programming approach, with which we memorize solutions in order
        # to avoid computing them multiple times)

        # This is correct since the above is equivalent to (see also page 46 of Sutton&Barto 2017 2nd draft)
        # G_(t-1) = r_t + gamma*r_(t+1) + gamma*gamma*r_(t+2) + ...

        ## Given the above, we calculate the returns at timestep t as:
        #               gamma[t] * return[t] + reward[t]
        #
        ## We compute this starting from the last timestep to the first, in order
        ## to employ the formula presented above and avoid redundant computations that would be needed
        ## if we were to do it from first to last.

        ## Hence, the queue "returns" will hold the returns in chronological order, from t=0 to t=n_steps
        ## thanks to the appendleft() function which allows to append to the position 0 in constant time O(1)
        ## a normal python list would instead require O(N) to do this.
        for t in range(n_steps)[::-1]:
            disc_return_t = (returns[0] if len(returns)>0 else 0)
            returns.appendleft(    ) # TODO: complete here

        ## standardization of the returns is employed to make training more stable
        eps = np.finfo(np.float32).eps.item()

        ## eps is the smallest representable float, which is
        # added to the standard deviation of the returns to avoid numerical instabilities
        returns = torch.tensor(returns)
        returns = (returns - returns.mean()) / (returns.std() + eps)

        # Line 7:
        policy_loss = []
        for log_prob, disc_return in zip(saved_log_probs, returns):
            policy_loss.append(-log_prob * disc_return)
        policy_loss = torch.cat(policy_loss).sum()

        # Line 8: PyTorch prefers gradient descent
        optimizer.zero_grad()
        policy_loss.backward()
        optimizer.step()

        if i_episode % print_every == 0:
            print('Episode {}\tAverage Score: {:.2f}'.format(i_episode, np.mean(scores_deque)))

    return scores
```

#### 解决方案

```py
def reinforce(policy, optimizer, n_training_episodes, max_t, gamma, print_every):
    # Help us to calculate the score during the training
    scores_deque = deque(maxlen=100)
    scores = []
    # Line 3 of pseudocode
    for i_episode in range(1, n_training_episodes + 1):
        saved_log_probs = []
        rewards = []
        state = env.reset()
        # Line 4 of pseudocode
        for t in range(max_t):
            action, log_prob = policy.act(state)
            saved_log_probs.append(log_prob)
            state, reward, done, _ = env.step(action)
            rewards.append(reward)
            if done:
                break
        scores_deque.append(sum(rewards))
        scores.append(sum(rewards))

        # Line 6 of pseudocode: calculate the return
        returns = deque(maxlen=max_t)
        n_steps = len(rewards)
        # Compute the discounted returns at each timestep,
        # as
        #      the sum of the gamma-discounted return at time t (G_t) + the reward at time t
        #
        # In O(N) time, where N is the number of time steps
        # (this definition of the discounted return G_t follows the definition of this quantity
        # shown at page 44 of Sutton&Barto 2017 2nd draft)
        # G_t = r_(t+1) + r_(t+2) + ...

        # Given this formulation, the returns at each timestep t can be computed
        # by re-using the computed future returns G_(t+1) to compute the current return G_t
        # G_t = r_(t+1) + gamma*G_(t+1)
        # G_(t-1) = r_t + gamma* G_t
        # (this follows a dynamic programming approach, with which we memorize solutions in order
        # to avoid computing them multiple times)

        # This is correct since the above is equivalent to (see also page 46 of Sutton&Barto 2017 2nd draft)
        # G_(t-1) = r_t + gamma*r_(t+1) + gamma*gamma*r_(t+2) + ...

        ## Given the above, we calculate the returns at timestep t as:
        #               gamma[t] * return[t] + reward[t]
        #
        ## We compute this starting from the last timestep to the first, in order
        ## to employ the formula presented above and avoid redundant computations that would be needed
        ## if we were to do it from first to last.

        ## Hence, the queue "returns" will hold the returns in chronological order, from t=0 to t=n_steps
        ## thanks to the appendleft() function which allows to append to the position 0 in constant time O(1)
        ## a normal python list would instead require O(N) to do this.
        for t in range(n_steps)[::-1]:
            disc_return_t = returns[0] if len(returns) > 0 else 0
            returns.appendleft(gamma * disc_return_t + rewards[t])

        ## standardization of the returns is employed to make training more stable
        eps = np.finfo(np.float32).eps.item()
        ## eps is the smallest representable float, which is
        # added to the standard deviation of the returns to avoid numerical instabilities
        returns = torch.tensor(returns)
        returns = (returns - returns.mean()) / (returns.std() + eps)

        # Line 7:
        policy_loss = []
        for log_prob, disc_return in zip(saved_log_probs, returns):
            policy_loss.append(-log_prob * disc_return)
        policy_loss = torch.cat(policy_loss).sum()

        # Line 8: PyTorch prefers gradient descent
        optimizer.zero_grad()
        policy_loss.backward()
        optimizer.step()

        if i_episode % print_every == 0:
            print("Episode {}\tAverage Score: {:.2f}".format(i_episode, np.mean(scores_deque)))

    return scores
```

## 训练它

+   我们现在准备训练我们的代理。

+   但首先，我们定义一个包含所有训练超参数的变量。

+   您可以更改训练参数（并应该😉）

```py
cartpole_hyperparameters = {
    "h_size": 16,
    "n_training_episodes": 1000,
    "n_evaluation_episodes": 10,
    "max_t": 1000,
    "gamma": 1.0,
    "lr": 1e-2,
    "env_id": env_id,
    "state_space": s_size,
    "action_space": a_size,
}
```

```py
# Create policy and place it to the device
cartpole_policy = Policy(
    cartpole_hyperparameters["state_space"],
    cartpole_hyperparameters["action_space"],
    cartpole_hyperparameters["h_size"],
).to(device)
cartpole_optimizer = optim.Adam(cartpole_policy.parameters(), lr=cartpole_hyperparameters["lr"])
```

```py
scores = reinforce(
    cartpole_policy,
    cartpole_optimizer,
    cartpole_hyperparameters["n_training_episodes"],
    cartpole_hyperparameters["max_t"],
    cartpole_hyperparameters["gamma"],
    100,
)
```

## 定义评估方法 📝

+   在这里，我们定义了要使用的评估方法来测试我们的Reinforce代理。

```py
def evaluate_agent(env, max_steps, n_eval_episodes, policy):
    """
    Evaluate the agent for ``n_eval_episodes`` episodes and returns average reward and std of reward.
    :param env: The evaluation environment
    :param n_eval_episodes: Number of episode to evaluate the agent
    :param policy: The Reinforce agent
    """
    episode_rewards = []
    for episode in range(n_eval_episodes):
        state = env.reset()
        step = 0
        done = False
        total_rewards_ep = 0

        for step in range(max_steps):
            action, _ = policy.act(state)
            new_state, reward, done, info = env.step(action)
            total_rewards_ep += reward

            if done:
                break
            state = new_state
        episode_rewards.append(total_rewards_ep)
    mean_reward = np.mean(episode_rewards)
    std_reward = np.std(episode_rewards)

    return mean_reward, std_reward
```

## 评估我们的代理 📈

```py
evaluate_agent(
    eval_env, cartpole_hyperparameters["max_t"], cartpole_hyperparameters["n_evaluation_episodes"], cartpole_policy
)
```

### 在Hub上发布我们训练的模型🔥

现在我们看到训练后取得了良好的结果，我们可以用一行代码将我们训练的模型发布到hub 🤗。

这是一个模型卡的示例：

![](../Images/afdb3be9b09aa528c9ee40968ca15774.png)

### 推送到Hub

#### 不要修改这段代码

```py
from huggingface_hub import HfApi, snapshot_download
from huggingface_hub.repocard import metadata_eval_result, metadata_save

from pathlib import Path
import datetime
import json
import imageio

import tempfile

import os
```

```py
def record_video(env, policy, out_directory, fps=30):
    """
    Generate a replay video of the agent
    :param env
    :param Qtable: Qtable of our agent
    :param out_directory
    :param fps: how many frame per seconds (with taxi-v3 and frozenlake-v1 we use 1)
    """
    images = []
    done = False
    state = env.reset()
    img = env.render(mode="rgb_array")
    images.append(img)
    while not done:
        # Take the action (index) that have the maximum expected future reward given that state
        action, _ = policy.act(state)
        state, reward, done, info = env.step(action)  # We directly put next_state = state for recording logic
        img = env.render(mode="rgb_array")
        images.append(img)
    imageio.mimsave(out_directory, [np.array(img) for i, img in enumerate(images)], fps=fps)
```

```py
def push_to_hub(repo_id,
                model,
                hyperparameters,
                eval_env,
                video_fps=30 ):
  """
  Evaluate, Generate a video and Upload a model to Hugging Face Hub.
  This method does the complete pipeline:
  - It evaluates the model
  - It generates the model card
  - It generates a replay video of the agent
  - It pushes everything to the Hub

  :param repo_id: repo_id: id of the model repository from the Hugging Face Hub
  :param model: the pytorch model we want to save
  :param hyperparameters: training hyperparameters
  :param eval_env: evaluation environment
  :param video_fps: how many frame per seconds to record our video replay
  """

  _, repo_name = repo_id.split("/")
  api = HfApi()

  # Step 1: Create the repo
  repo_url = api.create_repo(
        repo_id=repo_id,
        exist_ok=True,
  )

  with tempfile.TemporaryDirectory() as tmpdirname:
    local_directory = Path(tmpdirname)

    # Step 2: Save the model
    torch.save(model, local_directory / "model.pt")

    # Step 3: Save the hyperparameters to JSON
    with open(local_directory / "hyperparameters.json", "w") as outfile:
      json.dump(hyperparameters, outfile)

    # Step 4: Evaluate the model and build JSON
    mean_reward, std_reward = evaluate_agent(eval_env,
                                            hyperparameters["max_t"],
                                            hyperparameters["n_evaluation_episodes"],
                                            model)
    # Get datetime
    eval_datetime = datetime.datetime.now()
    eval_form_datetime = eval_datetime.isoformat()

    evaluate_data = {
          "env_id": hyperparameters["env_id"],
          "mean_reward": mean_reward,
          "n_evaluation_episodes": hyperparameters["n_evaluation_episodes"],
          "eval_datetime": eval_form_datetime,
    }

    # Write a JSON file
    with open(local_directory / "results.json", "w") as outfile:
        json.dump(evaluate_data, outfile)

    # Step 5: Create the model card
    env_name = hyperparameters["env_id"]

    metadata = {}
    metadata["tags"] = [
          env_name,
          "reinforce",
          "reinforcement-learning",
          "custom-implementation",
          "deep-rl-class"
      ]

    # Add metrics
    eval = metadata_eval_result(
        model_pretty_name=repo_name,
        task_pretty_name="reinforcement-learning",
        task_id="reinforcement-learning",
        metrics_pretty_name="mean_reward",
        metrics_id="mean_reward",
        metrics_value=f"{mean_reward:.2f} +/- {std_reward:.2f}",
        dataset_pretty_name=env_name,
        dataset_id=env_name,
      )

    # Merges both dictionaries
    metadata = {**metadata, **eval}

    model_card = f"""
  # **Reinforce** Agent playing **{env_id}**
  This is a trained model of a **Reinforce** agent playing **{env_id}** .
  To learn to use this model and train yours check Unit 4 of the Deep Reinforcement Learning Course: https://huggingface.co/deep-rl-course/unit4/introduction
  """

    readme_path = local_directory / "README.md"
    readme = ""
    if readme_path.exists():
        with readme_path.open("r", encoding="utf8") as f:
          readme = f.read()
    else:
      readme = model_card

    with readme_path.open("w", encoding="utf-8") as f:
      f.write(readme)

    # Save our metrics to Readme metadata
    metadata_save(readme_path, metadata)

    # Step 6: Record a video
    video_path =  local_directory / "replay.mp4"
    record_video(env, model, video_path, video_fps)

    # Step 7\. Push everything to the Hub
    api.upload_folder(
          repo_id=repo_id,
          folder_path=local_directory,
          path_in_repo=".",
    )

    print(f"Your model is pushed to the Hub. You can view your model here: {repo_url}")
```

通过使用`push_to_hub`，**您可以评估、记录重播、生成代理的模型卡，并将其推送到Hub**。

这样：

+   您可以**展示我们的工作**🔥

+   您可以**可视化您的代理进行游戏**👀

+   您可以**与社区分享一个其他人可以使用的代理**💾

+   您可以**访问排行榜🏆，查看您的代理与同学相比表现如何**👉 [https://huggingface.co/spaces/huggingface-projects/Deep-Reinforcement-Learning-Leaderboard](https://huggingface.co/spaces/huggingface-projects/Deep-Reinforcement-Learning-Leaderboard)

要与社区分享您的模型，需要遵循三个步骤：

1️⃣（如果尚未完成）创建一个HF帐户➡ [https://huggingface.co/join](https://huggingface.co/join)

2️⃣ 登录，然后，您需要存储来自Hugging Face网站的身份验证令牌。

+   创建一个新的令牌（[https://huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)）**具有写入角色**

![创建HF令牌](../Images/d21a97c736edaab9119d2d1c1da9deac.png)

```py
notebook_login()
```

如果您不想使用Google Colab或Jupyter Notebook，您需要使用这个命令：`huggingface-cli login`（或`login`）

3️⃣ 现在我们准备将我们训练的代理推送到🤗 Hub 🔥，使用`package_to_hub()`函数

```py
repo_id = ""  # TODO Define your repo id {username/Reinforce-{model-id}}
push_to_hub(
    repo_id,
    cartpole_policy,  # The model we want to save
    cartpole_hyperparameters,  # Hyperparameters
    eval_env,  # Evaluation environment
    video_fps=30
)
```

现在我们测试了我们实现的鲁棒性，让我们尝试一个更复杂的环境：PixelCopter 🚁

## 第二个代理：PixelCopter 🚁

### 研究PixelCopter环境👀

+   [环境文档](https://pygame-learning-environment.readthedocs.io/en/latest/user/games/pixelcopter.html)

```py
env_id = "Pixelcopter-PLE-v0"
env = gym.make(env_id)
eval_env = gym.make(env_id)
s_size = env.observation_space.shape[0]
a_size = env.action_space.n
```

```py
print("_____OBSERVATION SPACE_____ \n")
print("The State Space is: ", s_size)
print("Sample observation", env.observation_space.sample())  # Get a random observation
```

```py
print("\n _____ACTION SPACE_____ \n")
print("The Action Space is: ", a_size)
print("Action Space Sample", env.action_space.sample())  # Take a random action
```

观察空间（7）👀：

+   玩家y位置

+   玩家速度

+   玩家到地板的距离

+   玩家到天花板的距离

+   下一个方块x距离玩家

+   下一个方块的顶部y位置

+   下一个方块的底部y位置

动作空间（2）：

+   加速（按下加速器）

+   什么都不做（不按下加速器）

奖励函数：

+   每通过一个垂直块，它获得+1的正奖励。每次达到终止状态时，它会获得-1的负奖励。

### 定义新策略

+   由于环境更加复杂，我们需要一个更深层的神经网络

```py
class Policy(nn.Module):
    def __init__(self, s_size, a_size, h_size):
        super(Policy, self).__init__()
        # Define the three layers here

    def forward(self, x):
        # Define the forward process here
        return F.softmax(x, dim=1)

    def act(self, state):
        state = torch.from_numpy(state).float().unsqueeze(0).to(device)
        probs = self.forward(state).cpu()
        m = Categorical(probs)
        action = m.sample()
        return action.item(), m.log_prob(action)
```

#### 解决方案

```py
class Policy(nn.Module):
    def __init__(self, s_size, a_size, h_size):
        super(Policy, self).__init__()
        self.fc1 = nn.Linear(s_size, h_size)
        self.fc2 = nn.Linear(h_size, h_size * 2)
        self.fc3 = nn.Linear(h_size * 2, a_size)

    def forward(self, x):
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return F.softmax(x, dim=1)

    def act(self, state):
        state = torch.from_numpy(state).float().unsqueeze(0).to(device)
        probs = self.forward(state).cpu()
        m = Categorical(probs)
        action = m.sample()
        return action.item(), m.log_prob(action)
```

### 定义超参数

+   因为这个环境更加复杂。

+   特别是对于隐藏大小，我们需要更多的神经元。

```py
pixelcopter_hyperparameters = {
    "h_size": 64,
    "n_training_episodes": 50000,
    "n_evaluation_episodes": 10,
    "max_t": 10000,
    "gamma": 0.99,
    "lr": 1e-4,
    "env_id": env_id,
    "state_space": s_size,
    "action_space": a_size,
}
```

### 训练它

+   我们现在准备训练我们的代理。

```py
# Create policy and place it to the device
# torch.manual_seed(50)
pixelcopter_policy = Policy(
    pixelcopter_hyperparameters["state_space"],
    pixelcopter_hyperparameters["action_space"],
    pixelcopter_hyperparameters["h_size"],
).to(device)
pixelcopter_optimizer = optim.Adam(pixelcopter_policy.parameters(), lr=pixelcopter_hyperparameters["lr"])
```

```py
scores = reinforce(
    pixelcopter_policy,
    pixelcopter_optimizer,
    pixelcopter_hyperparameters["n_training_episodes"],
    pixelcopter_hyperparameters["max_t"],
    pixelcopter_hyperparameters["gamma"],
    1000,
)
```

### 在Hub上发布我们训练的模型

```py
repo_id = ""  # TODO Define your repo id {username/Reinforce-{model-id}}
push_to_hub(
    repo_id,
    pixelcopter_policy,  # The model we want to save
    pixelcopter_hyperparameters,  # Hyperparameters
    eval_env,  # Evaluation environment
    video_fps=30
)
```

## 一些额外的挑战

学习的最佳方式是自己尝试！正如您所看到的，当前代理表现不佳。作为第一个建议，您可以训练更多步骤。但也尝试找到更好的参数。

在排行榜中，您将找到您的代理。您能够达到榜首吗？

以下是一些提升排行榜的想法：

+   训练更多步骤

+   尝试不同的超参数，查看同学们的做法

+   将您的新训练模型发布到Hub

+   改进实现以适应更复杂的环境（例如，将网络更改为卷积神经网络以处理帧作为观察？）

* * *

祝贺您完成这个单元！这里有很多信息。祝贺您完成教程。您刚刚使用PyTorch从头开始编写了您的第一个深度强化学习代理，并在Hub上分享了它。

不要犹豫，通过改进实现以适应更复杂的环境（例如，将网络更改为卷积神经网络以处理帧作为观察）来迭代这个单元。

在下一个单元中，我们将学习更多关于Unity MLAgents，通过在Unity环境中训练代理。这样，您将准备好参加AI对AI挑战，您将训练您的代理来与其他代理在雪仗和足球比赛中竞争。

听起来很有趣吗？下次见！

最后，我们很想知道您对课程的看法以及我们如何改进它。如果您有反馈意见，请填写此表格。

下次见在第5单元！

### 继续学习，保持精彩
