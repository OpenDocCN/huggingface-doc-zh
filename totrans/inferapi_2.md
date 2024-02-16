# 🤗 托管推理API

> 原文链接：[https://huggingface.co/docs/api-inference/index](https://huggingface.co/docs/api-inference/index)

免费测试和评估超过150,000个公开可访问的机器学习模型，或您自己的私有模型，通过简单的HTTP请求，快速推理托管在Hugging Face共享基础设施上。

推理API可免费使用，并受速率限制。如果您需要用于生产的推理解决方案，请查看我们的[推理端点](https://huggingface.co/docs/inference-endpoints/index)服务。通过推理端点，您可以轻松部署任何机器学习模型在专用和完全托管的基础设施上。选择云、区域、计算实例、自动缩放范围和安全级别以匹配您的模型、延迟、吞吐量和合规需求。

## 主要特点：

+   从**150,000+个Transformers、Diffusers或Timm模型**（T5、Blenderbot、Bart、GPT-2、Pegasus...）获取预测

+   使用内置集成与**超过20个开源库**（spaCy、SpeechBrain、Keras等）。

+   通过仅切换模型ID来切换到下一个模型

+   上传、管理和私密地提供您的**自己的模型**

+   运行分类、图像分割、自动语音识别、NER、对话、摘要、翻译、问答、嵌入提取任务

+   由Intel Xeon Ice Lake提供动力的**CPU**上的即插即用加速推理

## 第三方库模型：

+   [Hub](https://huggingface.co)支持许多新库，如SpaCy、Timm、Keras、fastai等。您可以在[这里](https://hf.co/docs/hub/libraries)阅读完整列表。

+   这些模型得以在API上启用，得益于一些docker集成[api-inference-community](https://github.com/huggingface/api-inference-community/)。

但请注意，这些模型将不允许您（[跟踪问题](https://github.com/huggingface/huggingface_hub/issues/85)）：

+   要获得完整的优化

+   运行私有模型

+   要获得GPU推理访问权限

## 如果您正在寻找来自Hugging Face团队的定制支持

[![HuggingFace专家加速计划](../Images/4e3f8848a914f36cc180b9e654070ef1.png)](https://huggingface.co/support)

## Hugging Face在生产中受到超过10,000家公司的信任

![](../Images/6ae7702af262288012014841128c4a68.png) ![](../Images/224eaf3c23f0fc3f8f157c596cd4b70c.png)
