# 访问和查看指标

> 原文链接：[https://huggingface.co/docs/inference-endpoints/guides/metrics](https://huggingface.co/docs/inference-endpoints/guides/metrics)

Hugging Face端点通过端点的“分析”选项卡中的详细概述界面提供对端点的指标和分析的访问。

![指标仪表板](../Images/de0b9e7c5164004a48e0ecbdf23ff922.png)

## 通过API访问指标

Hugging Face推理端点API公开了一个[路由来访问端点的指标](/docs/inference-endpoints/api_reference#getendpointmetrics)。您可以使用此路由向端点发送定制的Prometheus查询。
