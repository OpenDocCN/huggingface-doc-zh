# 访问和阅读日志

> 原文：[https://huggingface.co/docs/inference-endpoints/guides/logs](https://huggingface.co/docs/inference-endpoints/guides/logs)

Hugging Face Endpoints通过UI提供对端点日志的访问，您可以在端点的“日志”选项卡中查看。

您将可以访问图像构件的构建日志，以及推理期间的容器日志。

![选择日志](../Images/17399f28d3a63eb6d12b299e8960316f.png)

只有当您的端点处于“运行”状态时，才能访问容器日志。

*注意：如果您的端点创建处于“失败”状态，您可以检查构建日志以查看失败的原因，例如依赖项的错误版本等。*

**构建日志：**

![构建日志](../Images/70fb5364c95a754e5a83dd834da427e5.png)

**容器日志：**

![容器日志](../Images/7e58277afdb5f967ce6a511c2127dedb.png)
