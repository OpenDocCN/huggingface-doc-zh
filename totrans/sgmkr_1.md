# 在Amazon SageMaker上的Hugging Face

> 原文：[https://huggingface.co/docs/sagemaker/index](https://huggingface.co/docs/sagemaker/index)

![封面](../Images/050af512638f1fb24402f93714ea3aad.png)

## 深度学习容器

深度学习容器（DLCs）是预先安装有深度学习框架和库的Docker镜像，如🤗 Transformers、🤗 Datasets和🤗 Tokenizers。DLCs允许您立即开始训练模型，跳过从头开始构建和优化训练环境的复杂过程。我们的DLCs经过彻底测试和优化，适用于深度学习环境，无需您进行任何配置或维护。特别是，Hugging Face推理DLC配备了一个预先编写的服务堆栈，大大降低了深度学习服务的技术门槛。

我们的DLCs在[Amazon SageMaker](https://aws.amazon.com/sagemaker/)可用的任何地方都可用。虽然可以在没有SageMaker Python SDK的情况下使用DLCs，但使用SageMaker训练模型有许多优势：

+   成本效益：训练实例仅在作业期间处于活动状态。一旦作业完成，训练集群停止，您将不再收费。SageMaker还支持[Spot实例]((https://docs.aws.amazon.com/sagemaker/latest/dg/model-managed-spot-training.html))，可将成本降低高达90%。

+   内置自动化：SageMaker自动将训练元数据和日志存储在无服务器托管的元存储中，并完全管理S3的I/O操作，用于您的数据集、检查点和模型工件。

+   多种安全机制：SageMaker提供[静态加密](https://docs.aws.amazon.com/sagemaker/latest/dg/encryption-at-rest-nbi.html)、[传输中加密](https://docs.aws.amazon.com/sagemaker/latest/dg/encryption-in-transit.html)、[虚拟私有云](https://docs.aws.amazon.com/sagemaker/latest/dg/interface-vpc-endpoint.html)连接和[身份和访问管理](https://docs.aws.amazon.com/sagemaker/latest/dg/security_iam_service-with-iam.html)以保护您的数据和代码。

Hugging Face DLCs是开源的，根据Apache 2.0许可。如果您有任何问题，请随时在我们的[社区论坛](https://discuss.huggingface.co/c/sagemaker/17)上联系我们。对于高级支持，我们的[专家加速计划](https://huggingface.co/support)为您提供来自我们团队的直接专门支持。

## 功能和优势🔥

Hugging Face Deep DLCs使在SageMaker中训练Transformer模型比以往更容易。以下是您应该考虑使用Hugging Face DLCs来训练和部署下一个机器学习模型的原因：

**只需一条命令**

使用新的Hugging Face DLCs，在一行代码中训练最先进的基于Transformer的NLP模型。选择多个DLC变体，每个变体都针对TensorFlow和PyTorch进行了优化，包括单GPU、单节点多GPU和多节点集群。

**加速从科学到生产的机器学习**

除了Hugging Face DLCs，我们还为SageMaker Python SDK创建了一流的Hugging Face扩展，以加速数据科学团队，将设置和运行实验所需的时间从几天缩短到几分钟。

您可以使用Hugging Face DLCs与SageMaker的自动模型调整来优化您的训练超参数，并提高模型的准确性。

只需再加一行代码即可将训练好的模型部署到推理中，或者从[模型Hub](https://huggingface.co/models)中选择任何10000多个公开可用的模型，并使用SageMaker部署它们。

在SageMaker Studio的基于Web的集成开发环境（IDE）中轻松跟踪和比较您的实验和训练工件。

**内置性能**

Hugging Face DLCs具有针对PyTorch和TensorFlow的内置性能优化，可加快训练NLP模型的速度。 DLCs还为您提供了灵活性，可以选择最符合您工作负载价格/性能比的训练基础设施。

Hugging Face Training DLCs与SageMaker分布式训练库完全集成，可以使用Amazon Elastic Compute Cloud上提供的最新一代实例更快地训练模型。

Hugging Face推理DLCs为您提供了可快速扩展到AWS环境的生产就绪端点、内置监控和大量企业功能。

* * *

## 资源、文档和示例 📄

查看我们发布的博客文章、视频、文档、示例笔记本和脚本，以获取有关Hugging Face DLCs在SageMaker上的更多帮助和更多上下文。

### 博客和视频

+   [AWS：与Hugging Face一起拥抱自然语言处理](https://aws.amazon.com/de/blogs/opensource/embracing-natural-language-processing-with-hugging-face/)

+   [使用Amazon SageMaker轻松部署Hugging Face模型](https://huggingface.co/blog/deploy-hugging-face-models-easily-with-amazon-sagemaker)

+   [AWS和Hugging Face合作简化和加速自然语言处理模型的采用](https://aws.amazon.com/blogs/machine-learning/aws-and-hugging-face-collaborate-to-simplify-and-accelerate-adoption-of-natural-language-processing-models/)

+   [演示：端到端文本分类](https://youtu.be/ok3hetb42gU)

+   [在Amazon SageMaker上使用Hugging Face模型](https://youtu.be/leyrCgLAGjMn)

+   [分布式训练：使用🤗 Transformers和Amazon SageMaker为摘要训练BART/T5](https://huggingface.co/blog/sagemaker-distributed-training-seq2seq)

+   [将Hugging Face Transformers模型从S3部署到Amazon SageMaker](https://youtu.be/pfBGgSGnYLs)

+   [将Hugging Face Transformers模型从模型中心部署到Amazon SageMaker](https://youtu.be/l9QZuazbzWM)

### 文档

+   [在Amazon SageMaker上运行训练](/docs/sagemaker/train)

+   [将模型部署到Amazon SageMaker](/docs/sagemaker/inference)

+   [参考资料](/docs/sagemaker/reference)

+   [Hugging Face的Amazon SageMaker文档](https://docs.aws.amazon.com/sagemaker/latest/dg/hugging-face.html)

+   [Hugging Face的Python SDK SageMaker文档](https://sagemaker.readthedocs.io/en/stable/frameworks/huggingface/index.html)

+   [深度学习容器](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#huggingface-training-containers)

+   [SageMaker的分布式数据并行库](https://docs.aws.amazon.com/sagemaker/latest/dg/data-parallel.html)

+   [SageMaker的分布式模型并行库](https://docs.aws.amazon.com/sagemaker/latest/dg/model-parallel.html)

### 示例笔记本

+   [所有笔记本](https://github.com/huggingface/notebooks/tree/master/sagemaker)

+   [使用Pytorch入门](https://github.com/huggingface/notebooks/blob/main/sagemaker/01_getting_started_pytorch/sagemaker-notebook.ipynb)

+   [使用Tensorflow入门](https://github.com/huggingface/notebooks/blob/main/sagemaker/02_getting_started_tensorflow/sagemaker-notebook.ipynb)

+   [分布式训练数据并行性](https://github.com/huggingface/notebooks/blob/main/sagemaker/03_distributed_training_data_parallelism/sagemaker-notebook.ipynb)

+   [分布式训练模型并行性](https://github.com/huggingface/notebooks/blob/main/sagemaker/04_distributed_training_model_parallelism/sagemaker-notebook.ipynb)

+   [使用折扣实例继续训练](https://github.com/huggingface/notebooks/blob/main/sagemaker/05_spot_instances/sagemaker-notebook.ipynb)

+   [SageMaker指标](https://github.com/huggingface/notebooks/blob/main/sagemaker/06_sagemaker_metrics/sagemaker-notebook.ipynb)

+   [Tensorflow的分布式训练数据并行性](https://github.com/huggingface/notebooks/blob/main/sagemaker/07_tensorflow_distributed_training_data_parallelism/sagemaker-notebook.ipynb)

+   [分布式训练摘要](https://github.com/huggingface/notebooks/blob/main/sagemaker/08_distributed_summarization_bart_t5/sagemaker-notebook.ipynb)

+   [使用 Vision Transformer 进行图像分类](https://github.com/huggingface/notebooks/blob/main/sagemaker/09_image_classification_vision_transformer/sagemaker-notebook.ipynb)

+   [将 10,000 多个 Hugging Face Transformers 之一部署到 Amazon SageMaker 进行推理](https://github.com/huggingface/notebooks/blob/main/sagemaker/11_deploy_model_from_hf_hub/deploy_transformer_model_from_hf_hub.ipynb)

+   [将 Hugging Face Transformer 模型从 S3 部署到 SageMaker 进行推理](https://github.com/huggingface/notebooks/blob/main/sagemaker/10_deploy_model_from_s3/deploy_transformer_model_from_s3.ipynb)
