# 高级设置（实例类型、自动缩放、版本控制）

> 原始文本：[`huggingface.co/docs/inference-endpoints/guides/advanced`](https://huggingface.co/docs/inference-endpoints/guides/advanced)

我们已经看到在创建您的第一个端点中部署端点是多么快速简便，但这并不是您可以管理的全部。在创建过程中并在选择云提供商和区域后，单击[高级配置]按钮以显示有关端点的进一步配置选项。

**实例类型**

🤗 推理端点提供了一系列经过筛选的 CPU 和 GPU 实例。

*注意：您的 Hugging Face 帐户配备了用于 CPU 和 GPU 实例的容量配额。要增加配额或请求新的实例类型，请与我们联系。*

*默认：CPU-中等*

![复制 curl](img/698d77f5e435ec43634b873963aae15b.png)

**副本自动缩放**

设置您希望您的端点根据利用率自动缩放的副本范围（最小（>=1）和最大）。

*默认：最小 1；最大 2*

**任务**

选择一个支持的机器学习任务，或设置为自定义。当您不使用基于 Transformers 的模型或想要自定义推理流程时，应该使用自定义，请参阅创建自己的推理处理程序。

*默认：派生自模型存储库。*

**框架**

对于 Transformers 模型，如果 PyTorch 和 TensorFlow 权重都可用，您可以选择使用哪个模型权重。这将有助于减少图像伪影大小，并加快启动/扩展您的端点。

*默认：如果可用，则为 PyTorch。*

**修订**

为您的端点创建一个针对其源 Hugging Face 模型存储库的特定修订提交。这允许您对端点进行版本控制，并确保即使更新模型存储库，您始终使用相同的权重。

*默认：最近的提交。*

**图像**

允许您提供要部署到端点的自定义容器映像。这些可以是公共映像，例如*tensorflow/serving:2.7.3*，或托管在[Docker hub](https://hub.docker.com/)、[AWS ECR](https://aws.amazon.com/ecr/?nc1=h_ls)、[Azure ACR](https://azure.microsoft.com/de-de/services/container-registry/)或[Google GCR](https://cloud.google.com/container-registry?hl=de)上的私有映像。

有关如何在下面“使用您自己的自定义容器”。
