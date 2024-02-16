# 创建端点

> 原文链接：[https://huggingface.co/docs/inference-endpoints/guides/create_endpoint](https://huggingface.co/docs/inference-endpoints/guides/create_endpoint)

首次登录后，您将被引导至[端点创建页面](https://ui.endpoints.huggingface.co/new)。作为示例，本指南将介绍部署[distilbert-base-uncased-finetuned-sst-2-english](https://huggingface.co/distilbert-base-uncased-finetuned-sst-2-english)进行文本分类的步骤。

## 1. 输入Hugging Face仓库ID和您想要的端点名称：

![选择仓库](../Images/ea792df9c6f00101a25e79865ae6e98c.png)

## 2. 选择您的云服务提供商和地区。最初，只有AWS作为云服务提供商，提供us-east-1和eu-west-1地区。我们将很快添加Azure，如果您需要测试其他云服务提供商或地区的端点，请告诉我们。

![选择地区](../Images/9e6d3b1d6aa30a006712c9a9fab9390b.png)

## 3. 为端点定义安全级别：

![定义安全性](../Images/edace6faae5d85f8e688c5b8769a52da.png)

## 4. 点击创建端点 创建您的端点。默认情况下，您的端点将使用中等CPU创建（2 x 4GB vCPUs，采用Intel Xeon Ice Lake）。成本估计假设端点将持续一个月，并不考虑自动扩展。

![创建端点](../Images/d2fd09d11587c60d44abaeee747b8e6d.png)

## 5. 等待端点构建、初始化和运行，这可能需要1到5分钟。

![概览](../Images/75b9ac34c88b445f1c03e7cd063efb4a.png)

## 6. 在概览中使用推理小部件 🏁 🎉 测试您的端点！

![运行推理](../Images/e73cf109815a3811021081b1dc139977.png)
