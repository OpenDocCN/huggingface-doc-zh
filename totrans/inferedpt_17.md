# 添加自定义依赖项

> 原始文本：[https://huggingface.co/docs/inference-endpoints/guides/custom_dependencies](https://huggingface.co/docs/inference-endpoints/guides/custom_dependencies)

推理端点的基础镜像包含运行🤗 Transformers模型推理所需的所有库，但也支持自定义依赖项。如果您想要：

+   [自定义推理管道](/docs/inference-endpoints/guides/custom_handler)并需要额外的Python依赖项

+   运行需要特殊依赖项的模型，例如最新版本或固定版本的库（例如，`tapas`（`torch-scatter`））。

要添加自定义依赖项，请在Hugging Face Hub上的模型存储库中添加一个带有您想要安装的Python依赖项的`requirements.txt`[文件](https://huggingface.co/philschmid/distilbert-onnx-banking77/blob/main/requirements.txt)。当创建您的端点和镜像工件时，推理端点会检查模型存储库是否包含`requirements.txt`文件，并安装列出的依赖项。

```py
optimum[onnxruntime]==1.2.3
mkl-include
mkl
```

查看以下模型存储库中的`requirements.txt`文件以获取示例：

+   [Optimum and onnxruntime](https://huggingface.co/philschmid/distilbert-onnx-banking77/blob/main/requirements.txt)

+   [diffusers](https://huggingface.co/philschmid/stable-diffusion-v1-4-endpoints/blob/main/requirements.txt)

有关更多信息，请查看如何在推理时[使用自定义容器](/docs/inference-endpoints/guides/custom_container)创建和安装依赖项。
