# RLHF

> 原始文本：[`huggingface.co/learn/deep-rl-course/unitbonus3/rlhf`](https://huggingface.co/learn/deep-rl-course/unitbonus3/rlhf)

从人类反馈中学习强化学习（RLHF）是一种**将人类数据标签整合到基于 RL 的优化过程中的方法论**。它受到**建模人类偏好挑战**的启发。

对于许多问题，即使你可以尝试写下一个理想的方程，人们在他们的偏好上也存在差异。

基于测量数据更新模型**是一种尝试缓解这些固有的人类 ML 问题的途径**。

## 开始学习关于 RLHF 的知识

开始学习关于 RLHF 的知识：

1.  阅读这篇介绍：[Illustrating Reinforcement Learning from Human Feedback (RLHF)](https://huggingface.co/blog/rlhf)。

1.  观看我们几周前录制的现场直播，Nathan 在其中介绍了从人类反馈中学习强化学习（RLHF）的基础知识，以及这项技术如何被用来实现像 ChatGPT 这样的最先进 ML 工具。大部分讨论是关于相互连接的 ML 模型的概述。它涵盖了自然语言处理和 RL 的基础知识，以及 RLHF 如何在大型语言模型上使用。最后，我们提出了 RLHF 中的开放问题。

[`www.youtube-nocookie.com/embed/2MBJOuVq380`](https://www.youtube-nocookie.com/embed/2MBJOuVq380)

1.  阅读其他关于这个主题的博客，比如[封闭 API vs 开源持续：RLHF，ChatGPT，数据壕沟](https://robotic.substack.com/p/rlhf-chatgpt-data-moats)。如果你喜欢的话，告诉我们还有更多！

## 额外阅读

这是迄今为止关于 RLHF 的最流行论文列表。该领域最近因 DeepRL 的出现（大约在 2017 年）而变得流行，并已发展成为许多大型技术公司 LLMs 应用的广泛研究。以下是一些关于 RLHF 的早期论文，先于 LM 焦点：

+   [TAMER：通过评估强化手动训练代理](https://www.cs.utexas.edu/~pstone/Papers/bib2html-links/ICDL08-knox.pdf)（Knox 和 Stone，2008 年）：提出了一个学习代理，其中人类提供了对迭代学习奖励模型的行为评分。

+   [基于政策相关人类反馈的交互式学习](http://proceedings.mlr.press/v70/macglashan17a/macglashan17a.pdf)（MacGlashan 等人，2017 年）：提出了一个演员-评论家算法 COACH，其中人类反馈（积极和消极）用于调整优势函数。

+   [Deep Reinforcement Learning from Human Preferences](https://proceedings.neurips.cc/paper/2017/hash/d5e2c0adad503c91f91df240d0cd4e49-Abstract.html)（Christiano 等人，2017 年）：在 Atari 轨迹之间应用 RLHF。

+   [Deep TAMER：在高维状态空间中的交互式代理塑造](https://ojs.aaai.org/index.php/AAAI/article/view/11485)（Warnell 等人，2018 年）：扩展了 TAMER 框架，其中使用深度神经网络来建模奖励预测。

以下是一组不断增长的论文快照，展示了 RLHF 在 LM 中的表现：

+   [基于人类偏好微调语言模型](https://arxiv.org/abs/1909.08593)（Zieglar 等人，2019 年）：一篇早期论文，研究了奖励学习对四个特定任务的影响。

+   [学习使用人类反馈进行摘要](https://proceedings.neurips.cc/paper/2020/hash/1f89885d556929e98d3ef9b86448f951-Abstract.html)（Stiennon 等人，2020 年）：将 RLHF 应用于文本摘要任务。此外，[使用人类反馈递归总结书籍](https://arxiv.org/abs/2109.10862)（OpenAI Alignment Team 2021），后续工作总结书籍。

+   [WebGPT：使用人类反馈进行浏览器辅助问答](https://arxiv.org/abs/2112.09332)（OpenAI，2021 年）：使用 RLHF 训练一个代理来浏览网络。

+   InstructGPT: [训练语言模型遵循人类反馈的指令](https://arxiv.org/abs/2203.02155) (OpenAI Alignment Team 2022 年): 将 RLHF 应用于通用语言模型[[关于 InstructGPT 的博文](https://openai.com/blog/instruction-following/)]

+   GopherCite: [教导语言模型支持带有验证引用的答案](https://www.deepmind.com/publications/gophercite-teaching-language-models-to-support-answers-with-verified-quotes) (Menick 等人，2022 年): 使用 RLHF 训练 LM 以返回带有特定引用的答案

+   Sparrow: [通过有针对性的人类判断改善对话代理的对齐](https://arxiv.org/abs/2209.14375) (Glaese 等人，2022 年): 使用 RLHF 对对话代理进行微调

+   [ChatGPT: 优化语言模型用于对话](https://openai.com/blog/chatgpt/) (OpenAI 2022 年): 使用 RLHF 训练 LM，以适合作为通用聊天机器人使用

+   [奖励模型过度优化的扩展定律](https://arxiv.org/abs/2210.10760) (Gao 等人，2022 年): 研究 RLHF 中学习偏好模型的扩展特性

+   [使用人类反馈的强化学习训练一个有用且无害的助手](https://arxiv.org/abs/2204.05862) (Anthropic, 2022 年): 对使用 RLHF 训练 LM 助手的详细文档

+   [对抗性团队语言模型以减少伤害: 方法、扩展行为和经验教训](https://arxiv.org/abs/2209.07858) (Ganguli 等人，2022 年): 关于“发现、衡量和尝试减少[语言模型]潜在有害输出”的详细文档

+   [使用强化学习进行开放式对话的动态规划](https://arxiv.org/abs/2208.02294) (Cohen 等人，2022 年): 使用 RL 增强开放式对话代理的会话技能

+   [强化学习是否适用于自然语言处理？: 自然语言政策优化的基准、基线和构建块](https://arxiv.org/abs/2210.01241) (Ramamurthy 和 Ammanabrolu 等人，2022 年): 讨论 RLHF 中开源工具的设计空间，并提出一种新算法 NLPO（自然语言政策优化）作为 PPO 的替代方案。

## 作者

这一部分是由[Nathan Lambert](https://twitter.com/natolambert)撰写的
