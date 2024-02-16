# 在CPU上本地使用TEI

> 原文链接：[https://huggingface.co/docs/text-embeddings-inference/local_cpu](https://huggingface.co/docs/text-embeddings-inference/local_cpu)

您可以在本地安装`text-embeddings-inference`以在您自己的机器上运行。以下是安装的逐步说明：

## 步骤1：安装Rust

在您的终端中运行以下命令安装Rust到您的机器上，然后按照说明进行操作：[安装Rust]([https://rustup.rs/](https://rustup.rs/))

```py
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## 步骤2：安装必要的软件包

根据您的机器架构，运行以下命令之一：

### 对于x86机器

```py
cargo install --path router -F candle -F mkl
```

### 对于M1或M2机器

```py
cargo install --path router -F candle -F accelerate
```

## 步骤3：启动文本嵌入推断

安装成功后，您可以使用以下命令在CPU上启动文本嵌入推断：

```py
model=BAAI/bge-large-en-v1.5
revision=refs/pr/5

text-embeddings-router --model-id $model --revision $revision --port 8080
```

在某些情况下，您可能还需要安装OpenSSL库和gcc。在Linux机器上，运行以下命令：

```py
sudo apt-get install libssl-dev gcc -y
```

现在您已经准备好在本地机器上使用`text-embeddings-inference`。如果您想在本地使用GPU运行TEI，请查看[在GPU上本地使用TEI](local_gpu)页面。
