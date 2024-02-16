# 安全与合规

> 原文：[`huggingface.co/docs/inference-endpoints/security`](https://huggingface.co/docs/inference-endpoints/security)

🤗推理端点是以安全和安全推理为核心构建的。在下面，您可以找到我们已经采取的安全措施的概述。

## 数据安全/隐私

Hugging Face 不会存储任何客户数据，例如传递给推理端点的有效负载或令牌。我们会保存 30 天的日志。每个推理端点都使用 TLS/SSL 加密传输的数据。

我们还建议组织使用 AWS 或 Azure Private Link。这样可以通过私人连接访问您的推理端点，而不必将其暴露在互联网上。

Hugging Face 还通过推理端点企业计划提供商业合作伙伴附加协议或 GDPR 数据处理协议。

## 模型安全/隐私：

如果您不希望将模型存储库公开，可以将其设置为私有。Hugging Face 不拥有您上传到 Hugging Face Hub 的任何模型或数据。Hugging Face 确实对模型存储库中的内容进行恶意软件和 pickle 扫描，就像 Hub 中的所有项目一样。

## 推理端点和 Hub 安全

Hugging Face Hub，其中包括推理端点，也是 SOC2 Type 2 认证的。Hugging Face Hub 提供基于角色的访问控制。有关更多关于 Hub 安全的信息：[`huggingface.co/docs/hub/security`](https://huggingface.co/docs/hub/security)

![](img/5ff53729a41500fde15477ec3f4b596c.png)

## 推理端点安全级别

我们目前提供三种类型的端点，按安全级别递增的顺序：

+   **公共**：公共端点可以从互联网访问，通过 TLS/SSL 进行保护，并且不需要身份验证。

+   **受保护**：受保护的端点可以从互联网访问，通过 TLS/SSL 进行保护，并需要有效的 Hugging Face 令牌进行身份验证。

+   **私有** 私有端点仅通过区域内安全的 AWS 或 Azure PrivateLink 连接可用。私有端点无法从互联网访问。

公共和受保护的端点不需要任何额外配置。对于私有端点，您需要提供具有访问权限的 AWS 帐户 ID，该帐户还应该可以访问🤗推理端点。

![端点类型](img/09e279b8aa1ea62ec39625de308d6ca3.png)

Hugging Face 隐私政策 - [`huggingface.co/privacy`](https://huggingface.co/privacy)
