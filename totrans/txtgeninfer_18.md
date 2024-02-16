# PagedAttention

> 原文链接：[https://huggingface.co/docs/text-generation-inference/conceptual/paged_attention](https://huggingface.co/docs/text-generation-inference/conceptual/paged_attention)

在生成过程中，LLMs面临内存限制的挑战。在生成的解码部分，为以前的标记生成的所有注意力键和值存储在GPU内存中以便重复使用。这被称为*KV缓存*，对于大型模型和长序列可能占用大量内存。

PagedAttention尝试通过将KV缓存分成通过查找表访问的块来优化内存使用。因此，KV缓存不需要存储在连续的内存中，块会根据需要分配。内存效率可以增加GPU在内存受限工作负载上的利用率，从而支持更多的推理批次。

使用查找表访问内存块也有助于在多代之间共享KV。这对于诸如*并行采样*之类的技术非常有帮助，其中多个输出同时为相同提示生成。在这种情况下，缓存的KV块可以在各代之间共享。

TGI的PagedAttention实现利用了[vLLM项目](https://github.com/vllm-project/vllm)开发的自定义cuda内核。您可以在[项目页面](https://vllm.ai/)了解更多关于这种技术的信息。
