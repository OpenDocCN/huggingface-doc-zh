# 常见问题解答

> 原始文本：[https://huggingface.co/docs/inference-endpoints/faq](https://huggingface.co/docs/inference-endpoints/faq)

### 问：推理端点在哪些地区可用？

答：推理端点目前在AWS的us-east-1（北弗吉尼亚）和eu-west-1（爱尔兰）以及Azure的eastus（弗吉尼亚）上可用。如果您需要在其他地区部署，请告诉我们。

### 问：我可以访问我的端点正在运行的实例吗？

答：不，您无法访问托管您的端点的实例。但是，如果您缺少信息或需要有关运行端点的机器的更多见解，请联系我们。

### 问：我能看到我的私有端点在我的VPC帐户上运行吗？

答：不，当创建私有端点（一个通过AWS/Azure PrivateLink与您的VPC连接的Hugging Face推理端点）时，您只能看到端点可用的VPC中的ENI。

### 问：我可以批量运行推理吗？

答：这取决于任务。[支持的任务](/docs/inference-endpoints/supported_tasks)在幕后使用transformers或sentence-transformers管道。如果您的任务管道支持批处理，例如零样本分类，则支持批量推理。无论如何，您始终可以创建自己的[推理处理程序](/docs/inference-endpoints/guides/custom_handler)并实现批处理。

### 问：如何扩展我的部署？

答：端点会自动为您扩展，您需要提供的唯一信息是最小副本目标和最大副本目标。然后系统将根据负载来扩展您的端点。支持缩放到零。

### 问：如果不再处理请求，我的端点是否仍在运行？

答：是的，您的端点将始终保持可用/上线，与高级配置中定义的最小副本数量相匹配。

### 问：我想部署一个不在支持任务中的模型，这可能吗？

答：是的，您可以部署来自[Hugging Face Hub](https://huggingface.co/models)的任何存储库，如果您的任务/模型/框架不受支持，您可以[创建自己的推理处理程序](/docs/inference-endpoints/guides/custom_handler)，然后将您的模型部署到端点。

### 问：运行我的端点需要多少费用？

答：端点的计费基于运行端点的计算小时数和相关的实例类型。我们可能会在将来为负载均衡器和私有链接增加使用成本。

### 问：传输到端点的数据是否加密？

答：是的，数据在传输过程中使用TLS/SSL加密。

### 问：如何减少我的端点的延迟？

答：有几种方法可以减少您的端点的延迟。一种方法是将您的端点部署在靠近您的应用程序的地区，以减少网络开销。另一种方法是在创建端点之前使用[Hugging Face Optimum](https://huggingface.co/docs/optimum/index)优化您的模型。如果您需要帮助或有关减少延迟的更多问题，请联系我们。

### 问：如何监视我部署的端点？

答：您目前可以通过[🤗推理端点Web应用程序](https://ui.endpoints.huggingface.co/endpoints)监视您的端点，在那里您可以访问您的端点的[日志](/docs/inference-endpoints/guides/logs)以及[指标仪表板](/docs/inference-endpoints/guides/metrics)。如果您需要编程访问或更多信息，请联系我们。

### 问：如果我想部署到未列出的不同实例类型怎么办？

答：如果您觉得您的模型在不同于所列出的实例类型上表现更好，请联系我们。

### 问：我不小心泄露了我的令牌。我需要删除我的端点吗？

答：您可以在此处使现有的个人令牌无效并创建新的令牌：[https://huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)。对于组织令牌，请转到组织设置。
