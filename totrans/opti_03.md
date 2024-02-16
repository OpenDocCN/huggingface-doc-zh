# 安装

> 原文链接: [https://huggingface.co/docs/optimum/installation](https://huggingface.co/docs/optimum/installation)

🤗 Optimum可以通过以下方式使用`pip`安装：

```py
python -m pip install optimum
```

如果您想使用🤗 Optimum的特定加速器功能，可以根据下表安装所需的依赖项：

| 加速器 | 安装 |
| :-- | :-- |
| [ONNX runtime](https://onnxruntime.ai/docs/) | `pip install --upgrade-strategy eager install optimum[onnxruntime]` |
| [Intel Neural Compressor (INC)](https://www.intel.com/content/www/us/en/developer/tools/oneapi/neural-compressor.html) | `pip install --upgrade-strategy eager optimum[neural-compressor]` |
| [Intel OpenVINO](https://docs.openvino.ai/latest/index.html) | `pip install --upgrade-strategy eager optimum[openvino,nncf]` |
| [AMD Instinct GPUs and Ryzen AI NPU](https://www.amd.com/en/graphics/instinct-server-accelerators) | `pip install --upgrade-strategy eager optimum[amd]` |
| [Habana Gaudi Processor (HPU)](https://habana.ai/training/) | `pip install --upgrade-strategy eager optimum[habana]` |
| [FuriosaAI](https://www.furiosa.ai/) | `pip install --upgrade-strategy eager optimum[furiosa]` |

需要使用`--upgrade-strategy eager`选项来确保不同的软件包升级到最新可能的版本。

如果您想尝试示例或需要代码的最新版本，并且不能等待新版本发布，可以按照以下方式从源代码安装基本库：

```py
python -m pip install git+https://github.com/huggingface/optimum.git
```

对于特定加速器的功能，您可以通过在`pip`命令后附加`optimum[accelerator_type]`来安装它们，例如。

```py
python -m pip install optimum[onnxruntime]@git+https://github.com/huggingface/optimum.git
```
