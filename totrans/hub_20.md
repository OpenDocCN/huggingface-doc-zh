# 推理API

> 原始文本：[https://huggingface.co/docs/hub/models-inference](https://huggingface.co/docs/hub/models-inference)

请参考[推理API文档](https://huggingface.co/docs/api-inference)获取详细信息。

## 您使用什么技术来支持推理API？

对于🤗 Transformers模型，[Pipelines](https://huggingface.co/docs/transformers/main_classes/pipelines)支持API。

除了`Pipelines`之外，根据模型类型，还有几种生产优化，比如：

+   编译模型为优化的中间表示（例如[ONNX](https://medium.com/microsoftazure/accelerate-your-nlp-pipelines-using-hugging-face-transformers-and-onnx-runtime-2443578f4333)），

+   维护一个最近最少使用的缓存，确保最受欢迎的模型始终被加载，

+   根据负载约束动态扩展基础计算基础设施。

对于来自[其他库](./models-libraries)的模型，API使用[Starlette](https://www.starlette.io)并在[Docker容器](https://github.com/huggingface/api-inference-community/tree/main/docker_images)中运行。每个库定义了[不同流水线的实现](https://github.com/huggingface/api-inference-community/tree/main/docker_images/sentence_transformers/app/pipelines)。

## 如何关闭我的模型的推理API？

在您的模型卡片元数据中指定`inference: false`。

## 为什么我看不到推理小部件或者为什么我不能使用推理API？

对于一些任务，推理API可能没有支持，因此没有小部件。对于所有库（除了🤗 Transformers），API中有一个[library-to-tasks.ts文件](https://github.com/huggingface/huggingface.js/blob/main/packages/tasks/src/library-to-tasks.ts)列出了支持的任务。当一个模型存储库有一个存储库库不支持的任务时，默认情况下存储库为`inference: false`。

## 我可以发送大量请求吗？我可以获得加速的API吗？

如果您对加速推理、更高量的请求或SLA感兴趣，请联系我们，邮箱为`api-enterprise at huggingface.co`。

## 我如何查看我的使用情况？

您可以前往[推理API仪表板](https://api-inference.huggingface.co/dashboard/)。在[推理API文档](https://huggingface.co/docs/api-inference/usage)中了解更多信息。

## 推理API是否可以通过编程访问？

是的，`huggingface_hub`库有一个客户端包装器，文档在[这里](https://huggingface.co/docs/huggingface_hub/how-to-inference)。
