# 安装

> 原始文本：[https://huggingface.co/docs/text-generation-inference/installation](https://huggingface.co/docs/text-generation-inference/installation)

本节介绍如何安装CLI工具以及从源代码安装TGI。**强烈推荐的方法是使用Docker，因为它不需要太多设置。查看[快速入门](./quicktour)了解如何使用Docker运行TGI。**

## 安装CLI

您可以使用TGI命令行界面（CLI）来下载权重，提供和量化模型，或获取有关提供参数的信息。

要安装CLI，您需要首先克隆TGI存储库，然后运行`make`。

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

文本生成推理可在pypi、conda和GitHub上获得。

要进行安装和本地启动，首先[安装Rust](https://rustup.rs/)并创建一个至少包含Python 3.9的Python虚拟环境，例如使用conda：

```py
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

conda create -n text-generation-inference python=3.9
conda activate text-generation-inference
```

您可能还需要安装Protoc。

在Linux上：

```py
PROTOC_ZIP=protoc-21.12-linux-x86_64.zip
curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v21.12/$PROTOC_ZIP
sudo unzip -o $PROTOC_ZIP -d /usr/local bin/protoc
sudo unzip -o $PROTOC_ZIP -d /usr/local 'include/*'
rm -f $PROTOC_ZIP
```

在MacOS上，使用Homebrew：

```py
brew install protobuf
```

然后运行以安装文本生成推理：

```py
git clone https://github.com/huggingface/text-generation-inference.git && cd text-generation-inference
BUILD_EXTENSIONS=True make install
```

在某些机器上，您可能还需要OpenSSL库和gcc。在Linux机器上运行：

```py
sudo apt-get install libssl-dev gcc -y
```

安装完成后，只需运行：

```py
make run-falcon-7b-instruct
```

这将从端口8080提供Falcon 7B Instruct模型，我们可以查询。
