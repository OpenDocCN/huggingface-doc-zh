# 🤗 推理端点

> 原文链接：[`huggingface.co/docs/inference-endpoints/index`](https://huggingface.co/docs/inference-endpoints/index)

🤗 推理端点提供了一个安全的生产解决方案，可以轻松部署来自 Hub 的任何🤗 Transformers，Sentence-Transformers 和 Diffusion 模型，这些模型在由 Hugging Face 管理的专用和自动缩放基础设施上。

Hugging Face 端点是从[Hugging Face 模型仓库](https://huggingface.co/models)构建的。当创建端点时，服务会创建图像工件，这些工件可以是您选择的模型或自定义提供的容器图像。图像工件与 Hugging Face Hub 源代码库完全解耦，以确保最高的安全性和可靠性水平。

🤗 推理端点支持所有🤗 Transformers，Sentence-Transformers 和 Diffusion 任务，以及自定义任务，这些任务目前尚未得到🤗 Transformers 的支持，如说话人分离和扩散。

此外，🤗 推理端点还提供了使用在外部服务上管理的自定义容器图像的选项，例如[Docker Hub](https://hub.docker.com/)，[AWS ECR](https://aws.amazon.com/ecr/?nc1=h_ls)，[Azure ACR](https://azure.microsoft.com/de-de/services/container-registry/)或[Google GCR](https://cloud.google.com/container-registry?hl=de)。

![创建流程](img/34f696f0d1b59e0fb2b43ab7bba3b4f7.png)

## 文档和示例

+   安全性与合规性

+   支持的 Transformers 任务

+   API 参考

+   自动缩放

+   常见问题解答

+   帮助与支持

### 指南

+   访问解决方案（UI）

+   创建您的第一个端点

+   向端点发送请求

+   更新您的端点

+   高级设置（实例类型，自动缩放，版本控制）

+   使用 AWS PrivateLink 创建私有端点

+   添加自定义依赖项

+   创建自定义推理处理程序

+   使用自定义容器图像

+   访问和阅读日志

+   访问和查看指标

+   更改组织或帐户

### 其他

+   推理端点版本

+   请求的序列化和反序列化
