# 设置AWS Trainium实例

> 原文链接：[https://huggingface.co/docs/optimum-neuron/guides/setup_aws_instance](https://huggingface.co/docs/optimum-neuron/guides/setup_aws_instance)

使用[Hugging Face Neuron深度学习AMI](https://aws.amazon.com/marketplace/pp/prodview-gr3e6yiscria2)（DLAMI）和Hugging Face Transformers的最简单方法。DLAMI为您预打包了所有所需的库，包括Neuron Drivers、Transformers、Datasets和Accelerate。

要创建EC2 Trainium实例，您可以从控制台或市场开始。本指南将从[EC2控制台](https://console.aws.amazon.com/ec2sp/v2/)开始。

从us-east-1地区的[EC2控制台](https://console.aws.amazon.com/ec2sp/v2/)开始，首先点击**启动实例**并为实例定义一个名称（`trainium-huggingface-demo`）。

![命名实例](../Images/4ae118d91de241a02eac5a60a7c84c19.png)

接下来，您可以在亚马逊市场搜索Hugging Face AMI。在“应用程序和操作系统映像”的搜索栏中输入“Hugging Face”并点击“enter”。

![搜索ami](../Images/6772b24124cd0cb5cd9fdc06ab3b2ff2.png)

现在应该打开“选择Amazon机器映像”视图并进行搜索。您现在可以导航到“AWS Marketplace AMIs”并找到[Hugging Face Neuron深度学习AMI](https://aws.amazon.com/marketplace/pp/prodview-gr3e6yiscria2)，然后点击选择。

![选择ami](../Images/ab2cb4242e9e18c0b05b0ac11d48ff22.png)

*如果您尚未订阅，系统将要求您订阅。AMI完全免费，您只需支付EC2计算费用。*

然后您需要定义一个密钥对，用于通过`ssh`连接到实例。如果没有密钥对，您可以现场创建一个。

![选择ssh密钥](../Images/a99df5d0152559b2dcff6e7ff9c05679.png)

之后，创建或选择一个[安全组](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)。重要的是要允许`ssh`流量。

![选择安全组](../Images/32154190fe81f40c25c6c084be8f12b2.png)

您已准备好启动我们的实例。因此，请在右侧点击“启动实例”。

![选择ssh密钥](../Images/a0f0c6ff8e4c6b8b3663e667ef1ade7f.png)

AWS现在将使用[Hugging Face Neuron深度学习AMI](https://aws.amazon.com/marketplace/pp/prodview-gr3e6yiscria2)来配置实例。可以通过增加磁盘空间或创建实例配置文件来进行其他配置以访问其他AWS服务。

实例运行后，您可以查看并复制公共IPv4地址以通过`ssh`连接到该机器。

![选择公共DNS](../Images/34825c7d759e01cf6360f461f88978e1.png)

在下面的片段中，用您的实例的IP地址和启动实例时创建/选择的密钥对的路径替换空字符串`""`。

```py
PUBLIC_DNS="" # IP address
KEY_PATH="" # local path to key pair

ssh -i $KEY_PATH ubuntu@$PUBLIC_DNS
```

连接后，您可以运行`neuron-ls`来确保您可以访问Trainium加速器。您应该看到类似于下面的输出。

```py
ubuntu@ip-172-31-79-164:~$ neuron-ls
instance-type: trn1.2xlarge
instance-id: i-0570615e41700a481
+--------+--------+--------+---------+
| NEURON | NEURON | NEURON |   PCI   |
| DEVICE | CORES  | MEMORY |   BDF   |
+--------+--------+--------+---------+
| 0      | 2      | 32 GB  | 00:1e.0 |
+--------+--------+--------+---------+
```

完成了！您现在可以开始使用Hugging Face Transformers的Trainium加速器。查看[Fine-tune Transformers with AWS Trainium](./fine_tune)指南以开始使用。
