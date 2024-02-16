# 为TEI构建自定义容器

> 原文链接：[https://huggingface.co/docs/text-embeddings-inference/custom_container](https://huggingface.co/docs/text-embeddings-inference/custom_container)

您可以使用Docker构建自己的CPU或CUDA TEI容器。要构建CPU容器，请在包含您的自定义Dockerfile的目录中运行以下命令：

```py
docker build .
```

构建CUDA容器时，必须确定将在运行时使用的GPU的计算能力（compute cap）。这些信息对于正确配置CUDA容器至关重要。以下是各种GPU类型的运行时计算能力的示例：

+   Turing（T4，RTX 2000系列，...）- `runtime_compute_cap=75`

+   A100 - `runtime_compute_cap=80`

+   A10 - `runtime_compute_cap=86`

+   Ada Lovelace（RTX 4000系列，...）- `runtime_compute_cap=89`

+   H100 - `runtime_compute_cap=90`

一旦确定了计算能力，将其设置为`runtime_compute_cap`变量，并按照下面的示例构建容器：

```py
runtime_compute_cap=80

docker build . -f Dockerfile-cuda --build-arg CUDA_COMPUTE_CAP=$runtime_compute_cap
```
