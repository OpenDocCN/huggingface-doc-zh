# 准备模型

> 原文链接：[`huggingface.co/docs/text-generation-inference/basic_tutorials/preparing_model`](https://huggingface.co/docs/text-generation-inference/basic_tutorials/preparing_model)

文本生成推断在几个方面改进了模型。

## 量化

TGI 支持[bits-and-bytes](https://github.com/TimDettmers/bitsandbytes#bitsandbytes)，[GPT-Q](https://arxiv.org/abs/2210.17323)和[AWQ](https://arxiv.org/abs/2306.00978)量化。为了加速量化推断，只需将`quantize`标志设置为`bitsandbytes`，`gptq`或`awq`，具体取决于您希望使用的量化技术。当使用 GPT-Q 量化时，您需要指向其中一个模型[这里](https://huggingface.co/models?search=gptq)，当使用 AWQ 量化时，您需要指向其中一个模型[这里](https://huggingface.co/models?search=awq)。要获取有关量化的更多信息，请参考量化指南

## RoPE 缩放

RoPE 缩放可用于在推断时增加模型的序列长度，而无需必要地微调它。要启用 RoPE 缩放，只需在通过 CLI 运行时传递`--rope-scaling`，`--max-input-length`和`--rope-factors`标志。`--rope-scaling`可以取值`linear`或`dynamic`。如果您的模型没有微调到更长的序列长度，请使用`dynamic`。`--rope-factor`是预期最大序列长度与模型原始最大序列长度之间的比率。确保传递`--max-input-length`以提供扩展的最大输入长度。

我们建议使用`dynamic` RoPE 缩放。

## Safetensors

[Safetensors](https://github.com/huggingface/safetensors)是用于深度学习模型的快速且安全的持久化格式，对于张量并行性是必需的。TGI 在底层支持`Safetensors`模型加载。默认情况下，给定一个带有`safetensors`和`pytorch`权重的存储库，TGI 将始终加载`safetensors`。如果没有`pytorch`权重，TGI 将把权重转换为`safetensors`格式。
