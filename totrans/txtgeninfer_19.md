# Safetensors

> 原文链接：[`huggingface.co/docs/text-generation-inference/conceptual/safetensors`](https://huggingface.co/docs/text-generation-inference/conceptual/safetensors)

Safetensors 是用于深度学习模型的模型序列化格式。与 pickle 等其他序列化格式相比，它更快速和安全（在许多深度学习库的内部使用 pickle）。

TGI 主要依赖 safetensors 格式来实现张量并行分片。在服务期间，对于给定的模型存储库，TGI 会寻找 safetensors 权重。如果没有 safetensors 权重，TGI 会将 PyTorch 权重转换为 safetensors 格式。

您可以通过阅读[safetensors 文档](https://huggingface.co/docs/safetensors/index)了解更多信息。
