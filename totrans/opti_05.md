# 🤗 Optimum 笔记本

> 原文链接: [`huggingface.co/docs/optimum/notebooks`](https://huggingface.co/docs/optimum/notebooks)

您可以在🤗 Optimum 中找到与每个加速器相关的笔记本列表。

## Optimum Habana

| 笔记本 | 描述 | Colab | Studio Lab |
| :-- | :-- | :-- | --: |
| [如何使用 DeepSpeed 在 Habana Gaudi 上训练拥有数十亿参数的模型](https://github.com/huggingface/optimum-habana/blob/main/notebooks/AI_HW_Summit_2022.ipynb) | 展示如何使用 DeepSpeed 在 Habana Gaudi 上预训练/微调 1.6B 参数的 GPT2-XL，用于因果语言建模 | ![在 Colab 中打开](https://colab.research.google.com/github/huggingface/optimum-habana/blob/main/notebooks/AI_HW_Summit_2022.ipynb) | ![在 AWS Studio 中打开](https://studiolab.sagemaker.aws/import/github/huggingface/optimum-habana/blob/main/notebooks/AI_HW_Summit_2022.ipynb) |

## Optimum Intel

### OpenVINO

| 笔记本 | 描述 | Colab | Studio Lab |
| :-- | :-- | :-- | --: |
| [如何使用 OpenVINO 进行推理](https://github.com/huggingface/optimum-intel/blob/main/notebooks/openvino/optimum_openvino_inference.ipynb) | 解释如何将模型导出到 OpenVINO，并在各种任务上使用 OpenVINO Runtime 进行推理 | ![在 Colab 中打开](https://colab.research.google.com/github/huggingface/optimum-intel/blob/main/notebooks/openvino/optimum_openvino_inference.ipynb) | ![在 AWS Studio 中打开](https://studiolab.sagemaker.aws/import/github/huggingface/optimum-intel/blob/main/notebooks/openvino/optimum_openvino_inference.ipynb) |
| [如何使用 NNCF 对问答模型进行量化](https://github.com/huggingface/optimum-intel/blob/main/notebooks/openvino/question_answering_quantization.ipynb) | 展示如何使用[NNCF](https://github.com/openvinotoolkit/nncf)对问答模型进行训练后量化，并使用 OpenVINO 加速推理 | ![在 Colab 中打开](https://colab.research.google.com/github/huggingface/optimum-intel/blob/main/notebooks/openvino/question_answering_quantization.ipynb) | ![在 AWS Studio 中打开](https://studiolab.sagemaker.aws/import/github/huggingface/optimum-intel/blob/main/notebooks/openvino/question_answering_quantization.ipynb) |
| [使用量化的稳定扩散模型的输出与其全精度对应物进行比较](https://github.com/huggingface/optimum-intel/blob/main/notebooks/openvino/stable_diffusion_quantization.ipynb) | 展示如何加载和比较两个具有不同精度的稳定扩散模型的输出 | ![在 Colab 中打开](https://colab.research.google.com/github/huggingface/optimum-intel/blob/main/notebooks/openvino/stable_diffusion_quantization.ipynb) | ![在 AWS Studio 中打开](https://studiolab.sagemaker.aws/import/github/huggingface/optimum-intel/blob/main/notebooks/openvino/stable_diffusion_quantization.ipynb) |

### 神经压缩器

| 笔记本 | 描述 | Colab | Studio Lab |
| :-- | :-- | :-- | --: |
| [如何使用 Intel Neural Compressor 对文本分类模型进行量化](https://github.com/huggingface/notebooks/blob/main/examples/text_classification_quantization_inc.ipynb) | 展示如何在任何 GLUE 任务中使用 Intel [Neural Compressor](https://github.com/intel/neural-compressor)在训练模型时应用量化。 | ![在 Colab 中打开](https://colab.research.google.com/github/huggingface/notebooks/blob/main/examples/text_classification_quantization_inc.ipynb) | ![在 AWS Studio 中打开](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/main/examples/text_classification_quantization_inc.ipynb) |

## 最佳 ONNX Runtime

| 笔记本 | 描述 | Colab | Studio Lab |
| :-- | :-- | :-- | --: |
| [如何使用 ONNX Runtime 对文本分类模型进行量化](https://github.com/huggingface/notebooks/blob/main/examples/text_classification_quantization_ort.ipynb) | 展示如何在任何 GLUE 任务中使用[ONNX Runtime](https://github.com/microsoft/onnxruntime)对模型应用静态和动态量化。 | ![在 Colab 中打开](https://colab.research.google.com/github/huggingface/notebooks/blob/main/examples/text_classification_quantization_ort.ipynb) | ![在 AWS Studio 中打开](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/main/examples/text_classification_quantization_ort.ipynb) |
| [如何使用 ONNX Runtime 对文本分类模型进行微调](https://github.com/huggingface/notebooks/blob/main/examples/text_classification_ort.ipynb) | 展示如何在 GLUE 任务中使用[ONNX Runtime](https://github.com/microsoft/onnxruntime)微调 DistilBERT 模型。 | ![在 Colab 中打开](https://colab.research.google.com/github/huggingface/notebooks/blob/main/examples/text_classification_ort.ipynb) | ![在 AWS Studio 中打开](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/main/examples/text_classification_ort.ipynb) |
| [如何使用 ONNX Runtime 对摘要模型进行微调](https://github.com/huggingface/notebooks/blob/main/examples/summarization_ort.ipynb) | 展示如何在 BBC 新闻语料库上微调 T5 模型。 | ![在 Colab 中打开](https://colab.research.google.com/github/huggingface/notebooks/blob/main/examples/summarization_ort.ipynb) | ![在 AWS Studio 中打开](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/main/examples/summarization_ort.ipynb) |
| [如何使用 ONNX Runtime 对 DeBERTa 进行问答微调](https://github.com/huggingface/notebooks/blob/main/examples/question_answering_ort.ipynb) | 展示如何在 squad 上微调 DeBERTa 模型。 | ![在 Colab 中打开](https://colab.research.google.com/github/huggingface/notebooks/blob/main/examples/question_answering_ort.ipynb) | ![在 AWS Studio 中打开](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/main/examples/question_answering_ort.ipynb) |
