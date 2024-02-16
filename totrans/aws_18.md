# 使用 AWS Trainium 对 Transformers 进行微调

> 原始文本：[`huggingface.co/docs/optimum-neuron/guides/fine_tune`](https://huggingface.co/docs/optimum-neuron/guides/fine_tune)

在 AWS Trainium 上进行训练与 Transformers 一样简单：

+   您需要将 Transformers 的[`Trainer`](https://huggingface.co/docs/transformers/main_classes/trainer)类替换为[`NeuronTrainer`](https://huggingface.co/docs/optimum/neuron/package_reference/trainer)类。

您可以在官方存储库中找到以下任务的几个示例：

+   [语言建模](https://github.com/huggingface/optimum-neuron/tree/main/examples/language-modeling),

+   [问答](https://github.com/huggingface/optimum-neuron/tree/main/examples/question-answering),

+   [摘要](https://github.com/huggingface/optimum-neuron/tree/main/examples/summarization),

+   [文本分类](https://github.com/huggingface/optimum-neuron/tree/main/examples/text-classification),

+   [翻译](https://github.com/huggingface/optimum-neuron/tree/main/examples/translation),

+   [图像分类](https://github.com/huggingface/optimum-neuron/tree/main/examples/image-classification),

+   [音频分类](https://github.com/huggingface/optimum-neuron/tree/main/examples/audio-classification),

+   [语音识别](https://github.com/huggingface/optimum-neuron/tree/main/examples/speech-recognition),

+   [对比图像文本训练](https://github.com/huggingface/optimum-neuron/tree/main/examples/contrastive-image-text).

如果您想逐步进行示例，请查看开始使用 AWS Trainium 和 Hugging Face Transformers 指南。
