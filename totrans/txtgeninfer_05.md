# 支持的模型和硬件

> 原始文本：[https://huggingface.co/docs/text-generation-inference/supported_models](https://huggingface.co/docs/text-generation-inference/supported_models)

文本生成推理使得在特定硬件上为最高性能提供优化模型成为可能。以下部分列出了支持哪些模型和硬件。

## 支持的模型

以下模型已经优化，并可以通过TGI提供服务，TGI使用自定义的CUDA内核以获得更好的推理效果。如果您希望禁用它们，可以在`docker run`命令的末尾添加标志`--disable-custom-kernels`。

+   [BLOOM](https://huggingface.co/bigscience/bloom)

+   [FLAN-T5](https://huggingface.co/google/flan-t5-xxl)

+   [Galactica](https://huggingface.co/facebook/galactica-120b)

+   [GPT-Neox](https://huggingface.co/EleutherAI/gpt-neox-20b)

+   [Llama](https://github.com/facebookresearch/llama)

+   [OPT](https://huggingface.co/facebook/opt-66b)

+   [SantaCoder](https://huggingface.co/bigcode/santacoder)

+   [Starcoder](https://huggingface.co/bigcode/starcoder)

+   [Falcon 7B](https://huggingface.co/tiiuae/falcon-7b)

+   [Falcon 40B](https://huggingface.co/tiiuae/falcon-40b)

+   [MPT](https://huggingface.co/mosaicml/mpt-30b)

+   [Llama V2](https://huggingface.co/meta-llama)

+   [Code Llama](https://huggingface.co/codellama)

+   [Mistral](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.2)

+   [Mixtral](https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1)

+   [Phi](https://huggingface.co/microsoft/phi-2)

如果上述列表中缺少您想要提供的模型，可以根据模型的管道类型尝试初始化和提供模型，以查看其性能如何，但对于非优化模型，性能不能保证。

```py
# for causal LMs/text-generation models
AutoModelForCausalLM.from_pretrained(<model>, device_map="auto")`
# or, for text-to-text generation models
AutoModelForSeq2SeqLM.from_pretrained(<model>, device_map="auto")
```

如果您希望提供一个已经存在于本地文件夹中的支持模型，只需指向本地文件夹。

```py
text-generation-launcher --model-id <PATH-TO-LOCAL-BLOOM>
```

## 支持的硬件

TGI优化模型支持在CUDA 12.2+的NVIDIA [A100](https://www.nvidia.com/en-us/data-center/a100/)、[A10G](https://www.nvidia.com/en-us/data-center/products/a10-gpu/)和[T4](https://www.nvidia.com/en-us/data-center/tesla-t4/) GPU上运行。请注意，您必须安装[NVIDIA容器工具包](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)才能使用它。对于其他NVIDIA GPU，仍将应用连续批处理，但一些操作，如flash attention和分页注意力将不会执行。

TGI还支持启用ROCm的AMD Instinct MI210和MI250 GPU，具有分页注意力、GPTQ量化、flash attention v2支持。以下功能目前在ROCm版本的TGI中不受支持，支持可能会在未来扩展：

+   加载[AWQ](https://huggingface.co/docs/transformers/quantization#awq)检查点。

+   Flash [层归一化内核](https://github.com/Dao-AILab/flash-attention/tree/main/csrc/layer_norm)

+   滑动窗口注意力的内核（Mistral）

TGI还支持以下人工智能硬件加速器：

+   *Habana第一代Gaudi和Gaudi2:* 查看这个[存储库](https://github.com/huggingface/tgi-gaudi)，以使用TGI在Gaudi和Gaudi2上为模型提供服务，使用[Optimum Habana](https://huggingface.co/docs/optimum/habana/index)

+   *AWS Inferentia2:* 查看这个[指南](https://github.com/huggingface/optimum-neuron/tree/main/text-generation-inference)，了解如何在Inferentia2上使用TGI为模型提供服务。
