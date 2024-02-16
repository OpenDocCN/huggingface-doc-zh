# 为 TEI 构建自定义容器

> 原文链接：[`huggingface.co/docs/text-embeddings-inference/custom_container`](https://huggingface.co/docs/text-embeddings-inference/custom_container)

您可以使用 Docker 构建自己的 CPU 或 CUDA TEI 容器。要构建 CPU 容器，请在包含您的自定义 Dockerfile 的目录中运行以下命令：

```py
docker build .
```

构建 CUDA 容器时，必须确定将在运行时使用的 GPU 的计算能力（compute cap）。这些信息对于正确配置 CUDA 容器至关重要。以下是各种 GPU 类型的运行时计算能力的示例：

+   Turing（T4，RTX 2000 系列，...）- `runtime_compute_cap=75`

+   A100 - `runtime_compute_cap=80`

+   A10 - `runtime_compute_cap=86`

+   Ada Lovelace（RTX 4000 系列，...）- `runtime_compute_cap=89`

+   H100 - `runtime_compute_cap=90`

一旦确定了计算能力，将其设置为`runtime_compute_cap`变量，并按照下面的示例构建容器：

```py
runtime_compute_cap=80

docker build . -f Dockerfile-cuda --build-arg CUDA_COMPUTE_CAP=$runtime_compute_cap
```
