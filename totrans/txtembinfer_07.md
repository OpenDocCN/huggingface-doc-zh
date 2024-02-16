# 在本地使用 Metal 安装 TEI

> 原文链接：[https://huggingface.co/docs/text-embeddings-inference/local_metal](https://huggingface.co/docs/text-embeddings-inference/local_metal)

您可以在自己的 Mac 上安装 `text-embeddings-inference`，以便在 Metal 支持下运行它。以下是安装的逐步说明：

## 步骤1：安装 Rust

在您的终端中运行以下命令，然后按照说明安装 Rust ([https://rustup.rs/](https://rustup.rs/))：

```py
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## 步骤2：安装 Metal 支持

```py
cargo install --path router -F candle -F metal
```

## 步骤3：启动文本嵌入推理

安装成功后，您可以使用以下命令启动带有 Metal 的文本嵌入推理。

```py
model=BAAI/bge-large-en-v1.5
revision=refs/pr/5

text-embeddings-router --model-id $model --revision $revision --port 8080
```

现在您已经准备好在您的机器上本地使用 `text-embeddings-inference`。
