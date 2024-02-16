# 在 CPU 上本地使用 TEI

> 原文链接：[`huggingface.co/docs/text-embeddings-inference/local_cpu`](https://huggingface.co/docs/text-embeddings-inference/local_cpu)

您可以在本地安装`text-embeddings-inference`以在您自己的机器上运行。以下是安装的逐步说明：

## 步骤 1：安装 Rust

在您的终端中运行以下命令安装 Rust 到您的机器上，然后按照说明进行操作：安装 Rust)

```py
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## 步骤 2：安装必要的软件包

根据您的机器架构，运行以下命令之一：

### 对于 x86 机器

```py
cargo install --path router -F candle -F mkl
```

### 对于 M1 或 M2 机器

```py
cargo install --path router -F candle -F accelerate
```

## 步骤 3：启动文本嵌入推断

安装成功后，您可以使用以下命令在 CPU 上启动文本嵌入推断：

```py
model=BAAI/bge-large-en-v1.5
revision=refs/pr/5

text-embeddings-router --model-id $model --revision $revision --port 8080
```

在某些情况下，您可能还需要安装 OpenSSL 库和 gcc。在 Linux 机器上，运行以下命令：

```py
sudo apt-get install libssl-dev gcc -y
```

现在您已经准备好在本地机器上使用`text-embeddings-inference`。如果您想在本地使用 GPU 运行 TEI，请查看在 GPU 上本地使用 TEI 页面。
