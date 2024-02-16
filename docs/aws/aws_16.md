# 在亚马逊 SageMaker 上使用 Optimum Neuron

> 原文链接：[`huggingface.co/docs/optimum-neuron/guides/sagemaker`](https://huggingface.co/docs/optimum-neuron/guides/sagemaker)

[Optimum Neuron](https://github.com/huggingface/optimum-neuron)已经集成到亚马逊 SageMaker 中，通过 Hugging Face 深度学习容器支持 AWS 加速器，如 Inferentia2 和 Trainium1。这使得您可以轻松地在亚马逊 SageMaker 上训练和部署🤗 Transformers 和 Diffusers 模型，利用 AWS 加速器。

Hugging Face DLC 镜像预装了 Optimum Neuron 和用于在 Inferentia2 和 Trainium1 上进行高效推理模型编译的工具。这使得部署大型转换器模型变得简单且开箱即用优化。

以下是使用 Hugging Face DLC 通过 Optimum Neuron 在亚马逊 SageMaker 上训练和部署模型的端到端教程列表。跟随端到端示例学习 Optimum Neuron 如何通过 Hugging Face DLC 镜像与 SageMaker 集成，解锁性能和成本优势。

## 在 Inferentia2 上部署嵌入模型以进行高效的相似性搜索

教程介绍如何在 AWS Inferentia2 上使用亚马逊 SageMaker 部署文本嵌入模型（BGE-Base），实现高效快速的嵌入生成；该文章展示了 Inferentia2 不仅可以成为高效快速，而且成本效益的嵌入推理选择，相比 GPU 或 OpenAI 和 Amazon Bedrock 等服务。

+   [教程](https://www.philschmid.de/inferentia2-embeddings)

+   [GitHub 仓库](https://github.com/philschmid/huggingface-inferentia2-samples/blob/main/llama2-7b/sagemaker-notebook.ipynb)

## 在 AWS Inferentia2 上使用亚马逊 SageMaker 部署 Llama 2 7B

教程介绍如何在 AWS Inferentia2 上使用亚马逊 SageMaker 部署对话 Llama 2 模型进行低延迟推理；展示了如何利用 Inferentia2 和 SageMaker 从模型训练到生产部署只需几行代码。

+   [教程](https://www.philschmid.de/inferentia2-llama-7b)

+   [GitHub 仓库](https://github.com/philschmid/huggingface-inferentia2-samples/blob/main/stable-diffusion-xl/sagemaker-notebook.ipynb)

## 在 AWS Inferentia2 上使用亚马逊 SageMaker 部署 Stable Diffusion XL

教程介绍如何在 AWS Inferentia2 上使用 Optimum Neuron 和亚马逊 SageMaker 部署 Stable Diffusion XL 模型，实现 1024x1024 图像生成，每张图像约 6 秒；该文章展示了如何通过单个`inf2.xlarge`实例（每小时成本为$0.99）可以实现每分钟约 10 张图像，使得 Inferentia2 成为图像推理的高效、快速且成本效益的选择，相比 GPU。

+   [教程](https://www.philschmid.de/inferentia2-stable-diffusion-xl)

+   [GitHub 仓库](https://github.com/Placeholder/stable-diffusion-inferentia)

## 在 AWS Inferentia2 上使用亚马逊 SageMaker 部署 BERT 进行文本分类

教程介绍如何使用 Optimum Neuron 和亚马逊 SageMaker 在 AWS Inferentia2 上优化和部署 BERT 模型，实现高效的文本分类，达到 4 毫秒的延迟；该文章展示了如何通过单个 inf2.xlarge 实例（每小时成本为$0.99）可以实现 116 次推理/秒和 500 次推理/秒，无需网络开销，使得 Inferentia2 相比 GPU 成为低延迟和成本效益推理的绝佳选择。

+   [教程](https://www.philschmid.de/optimize-deploy-bert-inf2)

+   [GitHub 仓库](https://github.com/philschmid/huggingface-inferentia2-samples/blob/main/bert-transformers/sagemaker-notebook.ipynb)
