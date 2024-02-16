# 在本地使用 GPU 进行 TEI

> 原始文本：[`huggingface.co/docs/text-embeddings-inference/local_gpu`](https://huggingface.co/docs/text-embeddings-inference/local_gpu)

您可以在本地安装`text-embeddings-inference`，在拥有 GPU 的自己的机器上运行它。为了确保您的硬件受支持，请查看支持的模型和硬件页面。

## 步骤 1：CUDA 和 NVIDIA 驱动程序

确保您已安装 CUDA 和 NVIDIA 驱动程序 - 设备上的 NVIDIA 驱动程序需要与 CUDA 版本 12.2 或更高版本兼容。

将 NVIDIA 二进制文件添加到您的路径中：

```py
export PATH=$PATH:/usr/local/cuda/bin
```

## 步骤 2：安装 Rust

在您的终端中运行以下命令，在您的机器上安装 Rust)，然后按照说明操作：

```py
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## 步骤 3：安装必要的软件包

这一步可能需要一段时间，因为我们需要编译大量的 cuda 内核。

### 对于图灵 GPU（T4，RTX 2000 系列...）

```py
cargo install --path router -F candle-cuda-turing --no-default-features
```

### 对于安培和霍普尔

```py
cargo install --path router -F candle-cuda --no-default-features
```

## 步骤 4：启动文本嵌入推理

您现在可以在 GPU 上启动文本嵌入推理：

```py
model=BAAI/bge-large-en-v1.5
revision=refs/pr/5

text-embeddings-router --model-id $model --revision $revision --port 8080
```
