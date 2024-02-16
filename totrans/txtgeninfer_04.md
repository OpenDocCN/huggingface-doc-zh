# 安装

> 原始文本：[`huggingface.co/docs/text-generation-inference/installation`](https://huggingface.co/docs/text-generation-inference/installation)

本节介绍如何安装 CLI 工具以及从源代码安装 TGI。**强烈推荐的方法是使用 Docker，因为它不需要太多设置。查看快速入门了解如何使用 Docker 运行 TGI。**

## 安装 CLI

您可以使用 TGI 命令行界面（CLI）来下载权重，提供和量化模型，或获取有关提供参数的信息。

要安装 CLI，您需要首先克隆 TGI 存储库，然后运行`make`。

```py
git clone https://github.com/huggingface/text-generation-inference.git && cd text-generation-inference
make install
```

如果您想要使用自定义内核提供模型，请运行

```py
BUILD_EXTENSIONS=True make install
```

## 从源代码进行本地安装

在开始之前，您需要设置您的环境，并安装文本生成推理。文本生成推理已在**Python 3.9+**上进行了测试。

文本生成推理可在 pypi、conda 和 GitHub 上获得。

要进行安装和本地启动，首先[安装 Rust](https://rustup.rs/)并创建一个至少包含 Python 3.9 的 Python 虚拟环境，例如使用 conda：

```py
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

conda create -n text-generation-inference python=3.9
conda activate text-generation-inference
```

您可能还需要安装 Protoc。

在 Linux 上：

```py
PROTOC_ZIP=protoc-21.12-linux-x86_64.zip
curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v21.12/$PROTOC_ZIP
sudo unzip -o $PROTOC_ZIP -d /usr/local bin/protoc
sudo unzip -o $PROTOC_ZIP -d /usr/local 'include/*'
rm -f $PROTOC_ZIP
```

在 MacOS 上，使用 Homebrew：

```py
brew install protobuf
```

然后运行以安装文本生成推理：

```py
git clone https://github.com/huggingface/text-generation-inference.git && cd text-generation-inference
BUILD_EXTENSIONS=True make install
```

在某些机器上，您可能还需要 OpenSSL 库和 gcc。在 Linux 机器上运行：

```py
sudo apt-get install libssl-dev gcc -y
```

安装完成后，只需运行：

```py
make run-falcon-7b-instruct
```

这将从端口 8080 提供 Falcon 7B Instruct 模型，我们可以查询。
