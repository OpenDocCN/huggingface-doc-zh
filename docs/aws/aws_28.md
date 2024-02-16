# Inferentia 导出器

> 原始文本：[`huggingface.co/docs/optimum-neuron/package_reference/export`](https://huggingface.co/docs/optimum-neuron/package_reference/export)

您可以使用🤗 Optimum 将 PyTorch 模型导出到 Neuron，以在 AWS 上运行推理[Inferntia 1](https://aws.amazon.com/ec2/instance-types/inf1/)和[Inferentia 2](https://aws.amazon.com/ec2/instance-types/inf2/)。

## 导出函数

每一代 Inferentia 加速器都有一个导出函数，`export_neuron`用于 INF1，`export_neuronx`用于 INF2，但您可以直接使用导出函数`export`，它会根据环境选择适当的导出函数。

此外，您可以通过`validate_model_outputs`检查导出的模型是否有效，该函数会比较 Neuron 设备上编译模型的输出与 PyTorch 模型在 CPU 上的输出。

## 支持的架构

| 架构 | 任务 |
| --- | --- |
| ALBERT | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| BERT | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| BLOOM | 文本生成 |
| CamemBERT | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| ConvBERT | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| DeBERTa (仅限 INF2) | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| DeBERTa-v2 (仅限 INF2) | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| DistilBERT | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| ELECTRA | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| FlauBERT | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| GPT2 | 文本生成 |
| Llama, Llama 2 | 文本生成 |
| Mistral | 文本生成 |
| MobileBERT | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| MPNet | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| OPT | 文本生成 |
| RoBERTa | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| RoFormer | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| XLM | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| XLM-RoBERTa | 特征提取，填充掩码，多项选择，问答，文本分类，标记分类 |
| Stable Diffusion | 文本到图像，图像到图像，修复 |
| Stable Diffusion XL Base | 文本到图像，图像到图像，修复 |
| Stable Diffusion XL Refiner | 图像到图像，修复 |

更多检查支持任务的详细信息[在此处](https://huggingface.co/docs/optimum-neuron/guides/export_model#selecting-a-task)。

更多架构即将推出，敬请关注！🚀
