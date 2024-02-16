# 推理端点版本

> 原文：[https://huggingface.co/docs/inference-endpoints/others/runtime](https://huggingface.co/docs/inference-endpoints/others/runtime)

Hugging Face推理端点配备了一个默认的服务容器，用于所有[支持的Transformers和Sentence-Transformers任务](/docs/inference-endpoints/supported_tasks)，以及[自定义推理处理程序](/docs/inference-endpoints/guides/custom_handler)和实现批处理。下面您将找到有关已安装软件包和使用的版本的信息。

您可以通过向模型存储库添加`requirements.txt`文件来随时升级已安装的软件包和自定义软件包。在[添加自定义依赖项](/docs/inference-endpoints/guides/custom_dependencies)中了解更多信息。

## 已安装软件包和版本

已安装的软件包分为`general`、`CPU`和`GPU`软件包。`general`软件包安装在所有容器中，`CPU`和`GPU`软件包仅安装在相应的容器中。Hugging Face推理运行时针对`CPU`和`GPU`分别具有`PyTorch`和`TensorFlow`的不同版本，根据创建推理端点时选择的`framework`来使用。在下面的列表中，`TensorFlow`和`PyTorch`版本被分组在一起。

### General

+   `Python`: `3.9.13`

### CPU

+   `transformers[sklearn,sentencepiece,audio,vision]`: `4.27.2`

+   `diffusers`: `0.14.0`

+   `accelerate`: `0.17.1`

+   `sentence_transformers`: `latest`

+   `pandas`: `latest`

+   `pytorch`: `1.13.1`

+   `torchvision`: `0.14.1`

+   `tensorflow`: `2.9.1`

### GPU

+   `transformers[sklearn,sentencepiece,audio,vision]`: `4.27.2`

+   `diffusers`: `0.14.0`

+   `accelerate`: `0.17.1`

+   `sentence_transformers`: `latest`

+   `pandas`: `latest`

+   `pytorch`: `1.13.1=py3.9_cuda11.8*`

+   `torchvision`: `0.14.1`

+   `tensorflow`: `2.9.1=*cuda112*py39*`
