# 安装

> 原文链接：[https://huggingface.co/docs/optimum-neuron/installation](https://huggingface.co/docs/optimum-neuron/installation)

我们建议使用[Hugging Face Neuron Deep Learning AMI](https://aws.amazon.com/marketplace/pp/prodview-gr3e6yiscria2)（DLAMI）。DLAMI已经为您预先打包了所有必需的库，包括Optimum Neuron、Neuron Drivers、Transformers、Datasets和Accelerate。

但也可以按照以下描述使用`pip`进行安装。

在安装`optimum-neuron`之前，请确保您已安装了Neuron驱动程序和工具，查看[更详细的指南](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/general/setup/torch-neuronx.html#setup-torch-neuronx)。

## 添加pip软件包URL

指向AWS Neuron存储库：

```py
python -m pip config set global.extra-index-url https://pip.repos.neuron.amazonaws.com
```

## 为AWS Trainium（trn1）或AWS inferentia2（inf2）安装optimum-neuron

```py
python -m pip install optimum[neuronx]
```

## 为AWS inferentia（inf1）安装optimum-neuron

```py
python -m pip install optimum[neuron]
```
