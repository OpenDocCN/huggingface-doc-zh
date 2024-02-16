# 访问和查看指标

> 原文链接：[`huggingface.co/docs/inference-endpoints/guides/metrics`](https://huggingface.co/docs/inference-endpoints/guides/metrics)

Hugging Face 端点通过端点的“分析”选项卡中的详细概述界面提供对端点的指标和分析的访问。

![指标仪表板](img/de0b9e7c5164004a48e0ecbdf23ff922.png)

## 通过 API 访问指标

Hugging Face 推理端点 API 公开了一个路由来访问端点的指标。您可以使用此路由向端点发送定制的 Prometheus 查询。
