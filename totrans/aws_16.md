# 在亚马逊SageMaker上使用Optimum Neuron

> 原文链接：[https://huggingface.co/docs/optimum-neuron/guides/sagemaker](https://huggingface.co/docs/optimum-neuron/guides/sagemaker)

[Optimum Neuron](https://github.com/huggingface/optimum-neuron)已经集成到亚马逊SageMaker中，通过Hugging Face深度学习容器支持AWS加速器，如Inferentia2和Trainium1。这使得您可以轻松地在亚马逊SageMaker上训练和部署🤗 Transformers和Diffusers模型，利用AWS加速器。

Hugging Face DLC镜像预装了Optimum Neuron和用于在Inferentia2和Trainium1上进行高效推理模型编译的工具。这使得部署大型转换器模型变得简单且开箱即用优化。

以下是使用Hugging Face DLC通过Optimum Neuron在亚马逊SageMaker上训练和部署模型的端到端教程列表。跟随端到端示例学习Optimum Neuron如何通过Hugging Face DLC镜像与SageMaker集成，解锁性能和成本优势。

## 在Inferentia2上部署嵌入模型以进行高效的相似性搜索

教程介绍如何在AWS Inferentia2上使用亚马逊SageMaker部署文本嵌入模型（BGE-Base），实现高效快速的嵌入生成；该文章展示了Inferentia2不仅可以成为高效快速，而且成本效益的嵌入推理选择，相比GPU或OpenAI和Amazon Bedrock等服务。

+   [教程](https://www.philschmid.de/inferentia2-embeddings)

+   [GitHub 仓库](https://github.com/philschmid/huggingface-inferentia2-samples/blob/main/llama2-7b/sagemaker-notebook.ipynb)

## 在AWS Inferentia2上使用亚马逊SageMaker部署Llama 2 7B

教程介绍如何在AWS Inferentia2上使用亚马逊SageMaker部署对话Llama 2模型进行低延迟推理；展示了如何利用Inferentia2和SageMaker从模型训练到生产部署只需几行代码。

+   [教程](https://www.philschmid.de/inferentia2-llama-7b)

+   [GitHub 仓库](https://github.com/philschmid/huggingface-inferentia2-samples/blob/main/stable-diffusion-xl/sagemaker-notebook.ipynb)

## 在AWS Inferentia2上使用亚马逊SageMaker部署Stable Diffusion XL

教程介绍如何在AWS Inferentia2上使用Optimum Neuron和亚马逊SageMaker部署Stable Diffusion XL模型，实现1024x1024图像生成，每张图像约6秒；该文章展示了如何通过单个`inf2.xlarge`实例（每小时成本为$0.99）可以实现每分钟约10张图像，使得Inferentia2成为图像推理的高效、快速且成本效益的选择，相比GPU。

+   [教程](https://www.philschmid.de/inferentia2-stable-diffusion-xl)

+   [GitHub 仓库](https://github.com/Placeholder/stable-diffusion-inferentia)

## 在AWS Inferentia2上使用亚马逊SageMaker部署BERT进行文本分类

教程介绍如何使用Optimum Neuron和亚马逊SageMaker在AWS Inferentia2上优化和部署BERT模型，实现高效的文本分类，达到4毫秒的延迟；该文章展示了如何通过单个inf2.xlarge实例（每小时成本为$0.99）可以实现116次推理/秒和500次推理/秒，无需网络开销，使得Inferentia2相比GPU成为低延迟和成本效益推理的绝佳选择。

+   [教程](https://www.philschmid.de/optimize-deploy-bert-inf2)

+   [GitHub 仓库](https://github.com/philschmid/huggingface-inferentia2-samples/blob/main/bert-transformers/sagemaker-notebook.ipynb)
