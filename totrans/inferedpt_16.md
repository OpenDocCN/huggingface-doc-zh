# 使用AWS PrivateLink创建私有端点

> 原始文本：[https://huggingface.co/docs/inference-endpoints/guides/private_link](https://huggingface.co/docs/inference-endpoints/guides/private_link)

安全性和安全推理是推理端点的关键原则。我们目前提供三种不同级别的安全性：[公共、受保护和私有](/docs/inference-endpoints/security)。

公共和受保护的端点不需要任何额外配置。但是，为了为安全的区域内连接创建私有端点，您需要提供应该访问推理端点的帐户的AWS帐户ID。

![选择私有链接](../Images/104eb2651d8eacabd93025f434269011.png)

在提供AWS帐户ID并单击**创建端点**后，推理服务将创建一个VPC端点，并且您应该在概述中看到VPC服务名称。

![服务链接](../Images/375b74bc76c3fe92bb6fa60d34f08339.png)

VPC服务名称用于在您（客户）的云帐户中创建VPC接口端点。打开您的云帐户[控制台](https://console.aws.amazon.com/vpc/home?#Endpoints)以创建VPC接口端点。

下面显示了VPC端点配置的示例。您需要选择要使用的VPC和子网，以及安全组。

![aws管理控制台](../Images/4f16181f80f314067949da7713219793.png)

成功创建推理端点后，请转到相应的AWS帐户并将VPC端点添加为端点。

![端点URL](../Images/a611300328b83363c08624919fe9bab5.png)

在VPC端点状态从**pending**更改为**available**后，您应该在概述中看到一个端点URL。现在，您可以在您的VPC内使用此URL以安全和受保护的方式访问您的端点，确保流量仅在两个端点之间发生，永远不会离开AWS。
