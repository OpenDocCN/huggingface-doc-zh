# 使用自定义容器镜像

> 原文链接：[https://huggingface.co/docs/inference-endpoints/guides/custom_container](https://huggingface.co/docs/inference-endpoints/guides/custom_container)

推理端点不仅允许您[自定义推理处理程序](/docs/inference-endpoints/guides/custom_handler)，还允许您提供自定义容器镜像。这些可以是公共镜像，如`tensorflow/serving:2.7.3`，也可以是托管在[Docker Hub](https://hub.docker.com/)、[AWS ECR](https://aws.amazon.com/ecr/?nc1=h_ls)、[Azure ACR](https://azure.microsoft.com/de-de/services/container-registry/)或[Google GCR](https://cloud.google.com/container-registry?hl=de)上的私有镜像。

![自定义容器配置](../Images/2edd4889d6faa4c5faa3ba08780c78a9.png)

从自定义镜像创建图像工件的[创建流程](/docs/inference-endpoints/guides/create_endpoint)与基础镜像相同。这意味着推理端点将创建一个从您提供的图像派生的唯一图像工件，包括所有模型权重。

模型权重存储在`/repository`下。例如，如果您使用`tensorflow/serving`作为自定义镜像，那么您必须设置`model_base_path=`/repository`：

```py
tensorflow_model_server \
  --rest_api_port=5000 \
  --model_name=my_model \
  --model_base_path="/repository"
```
