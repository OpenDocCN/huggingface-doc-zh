# 闪存注意力

> 原文链接：[https://huggingface.co/docs/text-generation-inference/conceptual/flash_attention](https://huggingface.co/docs/text-generation-inference/conceptual/flash_attention)

变压器架构的扩展受到自注意力机制的严重瓶颈限制，自注意力机制具有二次时间和内存复杂度。最近加速器硬件的发展主要集中在增强计算能力，而不是内存和硬件之间的数据传输。这导致注意力操作存在内存瓶颈。**Flash Attention**是一种注意力算法，用于减少这个问题，更有效地扩展基于变压器的模型，实现更快的训练和推断。

标准注意力机制使用高带宽内存（HBM）来存储、读取和写入键、查询和数值。HBM在内存中很大，但在处理速度上较慢，与此同时，SRAM在内存中较小，但在操作速度上更快。在标准注意力实现中，从HBM加载和写入键、查询和数值的成本很高。它将键、查询和数值从HBM加载到GPU片上SRAM，执行一步注意力机制，将其写回到HBM，并为每个单独的注意力步骤重复此操作。相反，Flash Attention只加载一次键、查询和数值，融合注意力机制的操作，并将它们写回。

![Flash Attention](../Images/8ca529e4366f4e68a965efc7a0a9ff8d.png)

它已经在支持的模型中实现。您可以在此处查看支持Flash Attention的完整模型列表，这些模型具有flash前缀。

您可以通过阅读此链接中的论文了解更多关于Flash Attention的信息：[https://arxiv.org/abs/2205.14135](https://arxiv.org/abs/2205.14135)
