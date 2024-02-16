# 库

> 原文链接：[https://huggingface.co/docs/hub/models-libraries](https://huggingface.co/docs/hub/models-libraries)

Hub支持Open Source生态系统中数十个库。借助`huggingface_hub` Python库，很容易在Hub上共享您的模型。Hub支持许多库，我们正在努力扩展这种支持。我们很高兴欢迎一组推动机器学习发展的开源库加入Hub。

下表总结了支持的库及其集成级别。在[model-libraries.ts文件](https://github.com/huggingface/huggingface.js/blob/main/packages/tasks/src/model-libraries.ts)中找到我们支持的所有库。

| 库 | 描述 | 推断API | 小部件 | 从Hub下载 | 推送到Hub |
| --- | --- | --- | --: | --- | --- |
| [Adapters](./adapters) | 用于参数高效和模块化微调的统一Transformers附加组件。 | ✅ | ✅ | ✅ | ✅ |
| [AllenNLP](./allennlp) | 基于PyTorch构建的开源NLP研究库。 | ✅ | ✅ | ✅ | ❌ |
| [Asteroid](./asteroid) | 基于PyTorch的音频源分离工具包 | ✅ | ✅ | ✅ | ❌ |
| [BERTopic](./bertopic) | BERTopic是用于文本和图像的主题建模库 | ✅ | ✅ | ✅ | ✅ |
| [Diffusers](./diffusers) | 推理和扩散模型训练的模块化工具箱 | ✅ | ✅ | ✅ | ✅ |
| [docTR](https://github.com/mindee/doctr) | PyTorch和TensorFlow中与OCR相关任务的模型和数据集 | ✅ | ✅ | ✅ | ❌ |
| [ESPnet](./espnet) | 端到端语音处理工具包（例如TTS） | ✅ | ✅ | ✅ | ❌ |
| [fastai](./fastai) | 用于训练快速准确模型的库，具有最先进的输出。 | ✅ | ✅ | ✅ | ✅ |
| [Keras](https://huggingface.co/docs/hub/keras) | 使用一致简单的API构建模型，利用TensorFlow及其生态系统。 | ❌ | ❌ | ✅ | ✅ |
| [Flair](./flair) | 用于最先进的NLP的非常简单的框架。 | ✅ | ✅ | ✅ | ✅ |
| [MBRL-Lib](https://github.com/facebookresearch/mbrl-lib) | MBRL算法的PyTorch实现。 | ❌ | ❌ | ✅ | ✅ |
| [MidiTok](https://github.com/Natooz/MidiTok) | 用于符号音乐/MIDI文件的分词器。 | ❌ | ❌ | ✅ | ✅ |
| [ML-Agents](./ml-agents) | 使使用Unity制作的游戏和模拟环境能够用作训练智能代理的环境。 | ❌ | ❌ | ✅ | ✅ |
| [MLX](./mlx) | 苹果制造的基于Apple硅的模型训练和服务框架。 | ❌ | ❌ | ✅ | ✅ |
| [NeMo](https://github.com/NVIDIA/NeMo) | 为研究人员构建的对话AI工具包 | ✅ | ✅ | ✅ | ❌ |
| [OpenCLIP](./open_clip) | OpenAI的CLIP的开源实现库 | ❌ | ❌ | ✅ | ✅ |
| [PaddleNLP](./paddlenlp) | 基于PaddlePaddle构建的易于使用且功能强大的NLP库 | ✅ | ✅ | ✅ | ✅ |
| [PEFT](./peft) | 尖端的参数高效微调库 | ✅ | ✅ | ✅ | ✅ |
| [Pyannote](https://github.com/pyannote/pyannote-audio) | 说话人分离的神经构建模块。 | ❌ | ❌ | ✅ | ❌ |
| [PyCTCDecode](https://github.com/kensho-technologies/pyctcdecode) | 语言模型支持的语音识别CTC解码 | ❌ | ❌ | ✅ | ❌ |
| [Pythae](https://github.com/clementchadebec/benchmark_VAE) | Python中生成自动编码器的统一框架 | ❌ | ❌ | ✅ | ✅ |
| [RL-Baselines3-Zoo](./rl-baselines3-zoo) | 使用[Stable Baselines3](https://github.com/DLR-RM/stable-baselines3)的强化学习训练框架。 | ❌ | ✅ | ✅ | ✅ |
| [Sample Factory](./sample-factory) | 高吞吐量异步强化学习的代码库。 | ❌ | ✅ | ✅ | ✅ |
| [Sentence Transformers](./sentence-transformers) | 为句子、段落和图像计算密集向量表示。 | ✅ | ✅ | ✅ | ✅ |
| [SetFit](./setfit) | 使用Sentence Transformers进行高效的少样本文本分类 | ✅ | ✅ | ✅ | ✅ |
| [spaCy](./spacy) | Python和Cython中的高级自然语言处理。 | ✅ | ✅ | ✅ | ✅ |
| [SpanMarker](./span_marker) | 熟悉、简单和最先进的命名实体识别。 | ✅ | ✅ | ✅ | ✅ |
| [Scikit Learn (using skops)](https://skops.readthedocs.io/en/stable/) | Python中的机器学习。 | ✅ | ✅ | ✅ | ✅ |
| [Speechbrain](./speechbrain) | 由PyTorch提供支持的语音工具包。 | ✅ | ✅ | ✅ | ❌ |
| [Stable-Baselines3](./stable-baselines3) | PyTorch中深度强化学习算法的可靠实现集合 | ❌ | ✅ | ✅ | ✅ |
| [TensorFlowTTS](https://github.com/TensorSpeech/TensorFlowTTS) | 实时最先进的语音合成架构。 | ❌ | ❌ | ✅ | ❌ |
| [Timm](./timm) | 图像模型、脚本、预训练权重等的集合。 | ✅ | ✅ | ✅ | ✅ |
| [Transformers](./transformers) | 用于PyTorch、TensorFlow和JAX的最先进的自然语言处理 | ✅ | ✅ | ✅ | ✅ |
| [Transformers.js](./transformers-js) | 用于网络的最先进的机器学习。在浏览器中直接运行🤗 Transformers，无需服务器！ | ❌ | ❌ | ✅ | ❌ |
| [Unity Sentis](./unity-sentis) | 用于Unity 3D游戏引擎的推理引擎 | ❌ | ❌ | ❌ | ❌ |

### 如何将新库添加到推理API中？

如果您有兴趣添加您的库，请与我们联系！阅读有关此的[添加库指南](./models-adding-libraries)。
