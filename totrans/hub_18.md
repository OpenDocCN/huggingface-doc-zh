# 库

> 原文链接：[`huggingface.co/docs/hub/models-libraries`](https://huggingface.co/docs/hub/models-libraries)

Hub 支持 Open Source 生态系统中数十个库。借助`huggingface_hub` Python 库，很容易在 Hub 上共享您的模型。Hub 支持许多库，我们正在努力扩展这种支持。我们很高兴欢迎一组推动机器学习发展的开源库加入 Hub。

下表总结了支持的库及其集成级别。在[model-libraries.ts 文件](https://github.com/huggingface/huggingface.js/blob/main/packages/tasks/src/model-libraries.ts)中找到我们支持的所有库。

| 库 | 描述 | 推断 API | 小部件 | 从 Hub 下载 | 推送到 Hub |
| --- | --- | --- | --: | --- | --- |
| Adapters | 用于参数高效和模块化微调的统一 Transformers 附加组件。 | ✅ | ✅ | ✅ | ✅ |
| AllenNLP | 基于 PyTorch 构建的开源 NLP 研究库。 | ✅ | ✅ | ✅ | ❌ |
| Asteroid | 基于 PyTorch 的音频源分离工具包 | ✅ | ✅ | ✅ | ❌ |
| BERTopic | BERTopic 是用于文本和图像的主题建模库 | ✅ | ✅ | ✅ | ✅ |
| Diffusers | 推理和扩散模型训练的模块化工具箱 | ✅ | ✅ | ✅ | ✅ |
| [docTR](https://github.com/mindee/doctr) | PyTorch 和 TensorFlow 中与 OCR 相关任务的模型和数据集 | ✅ | ✅ | ✅ | ❌ |
| ESPnet | 端到端语音处理工具包（例如 TTS） | ✅ | ✅ | ✅ | ❌ |
| fastai | 用于训练快速准确模型的库，具有最先进的输出。 | ✅ | ✅ | ✅ | ✅ |
| [Keras](https://huggingface.co/docs/hub/keras) | 使用一致简单的 API 构建模型，利用 TensorFlow 及其生态系统。 | ❌ | ❌ | ✅ | ✅ |
| Flair | 用于最先进的 NLP 的非常简单的框架。 | ✅ | ✅ | ✅ | ✅ |
| [MBRL-Lib](https://github.com/facebookresearch/mbrl-lib) | MBRL 算法的 PyTorch 实现。 | ❌ | ❌ | ✅ | ✅ |
| [MidiTok](https://github.com/Natooz/MidiTok) | 用于符号音乐/MIDI 文件的分词器。 | ❌ | ❌ | ✅ | ✅ |
| ML-Agents | 使使用 Unity 制作的游戏和模拟环境能够用作训练智能代理的环境。 | ❌ | ❌ | ✅ | ✅ |
| MLX | 苹果制造的基于 Apple 硅的模型训练和服务框架。 | ❌ | ❌ | ✅ | ✅ |
| [NeMo](https://github.com/NVIDIA/NeMo) | 为研究人员构建的对话 AI 工具包 | ✅ | ✅ | ✅ | ❌ |
| OpenCLIP | OpenAI 的 CLIP 的开源实现库 | ❌ | ❌ | ✅ | ✅ |
| PaddleNLP | 基于 PaddlePaddle 构建的易于使用且功能强大的 NLP 库 | ✅ | ✅ | ✅ | ✅ |
| PEFT | 尖端的参数高效微调库 | ✅ | ✅ | ✅ | ✅ |
| [Pyannote](https://github.com/pyannote/pyannote-audio) | 说话人分离的神经构建模块。 | ❌ | ❌ | ✅ | ❌ |
| [PyCTCDecode](https://github.com/kensho-technologies/pyctcdecode) | 语言模型支持的语音识别 CTC 解码 | ❌ | ❌ | ✅ | ❌ |
| [Pythae](https://github.com/clementchadebec/benchmark_VAE) | Python 中生成自动编码器的统一框架 | ❌ | ❌ | ✅ | ✅ |
| RL-Baselines3-Zoo | 使用[Stable Baselines3](https://github.com/DLR-RM/stable-baselines3)的强化学习训练框架。 | ❌ | ✅ | ✅ | ✅ |
| Sample Factory | 高吞吐量异步强化学习的代码库。 | ❌ | ✅ | ✅ | ✅ |
| Sentence Transformers | 为句子、段落和图像计算密集向量表示。 | ✅ | ✅ | ✅ | ✅ |
| SetFit | 使用 Sentence Transformers 进行高效的少样本文本分类 | ✅ | ✅ | ✅ | ✅ |
| spaCy | Python 和 Cython 中的高级自然语言处理。 | ✅ | ✅ | ✅ | ✅ |
| SpanMarker | 熟悉、简单和最先进的命名实体识别。 | ✅ | ✅ | ✅ | ✅ |
| [Scikit Learn (using skops)](https://skops.readthedocs.io/en/stable/) | Python 中的机器学习。 | ✅ | ✅ | ✅ | ✅ |
| Speechbrain | 由 PyTorch 提供支持的语音工具包。 | ✅ | ✅ | ✅ | ❌ |
| Stable-Baselines3 | PyTorch 中深度强化学习算法的可靠实现集合 | ❌ | ✅ | ✅ | ✅ |
| [TensorFlowTTS](https://github.com/TensorSpeech/TensorFlowTTS) | 实时最先进的语音合成架构。 | ❌ | ❌ | ✅ | ❌ |
| Timm | 图像模型、脚本、预训练权重等的集合。 | ✅ | ✅ | ✅ | ✅ |
| Transformers | 用于 PyTorch、TensorFlow 和 JAX 的最先进的自然语言处理 | ✅ | ✅ | ✅ | ✅ |
| Transformers.js | 用于网络的最先进的机器学习。在浏览器中直接运行🤗 Transformers，无需服务器！ | ❌ | ❌ | ✅ | ❌ |
| Unity Sentis | 用于 Unity 3D 游戏引擎的推理引擎 | ❌ | ❌ | ❌ | ❌ |

### 如何将新库添加到推理 API 中？

如果您有兴趣添加您的库，请与我们联系！阅读有关此的添加库指南。
