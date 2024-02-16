# Safetensors

> 原文链接：[https://huggingface.co/docs/text-generation-inference/conceptual/safetensors](https://huggingface.co/docs/text-generation-inference/conceptual/safetensors)

Safetensors是用于深度学习模型的模型序列化格式。与pickle等其他序列化格式相比，它更快速和安全（在许多深度学习库的内部使用pickle）。

TGI主要依赖safetensors格式来实现张量并行分片。在服务期间，对于给定的模型存储库，TGI会寻找safetensors权重。如果没有safetensors权重，TGI会将PyTorch权重转换为safetensors格式。

您可以通过阅读[safetensors文档](https://huggingface.co/docs/safetensors/index)了解更多信息。
