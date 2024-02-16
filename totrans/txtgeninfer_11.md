# 使用 TGI CLI

> 原始文本：[`huggingface.co/docs/text-generation-inference/basic_tutorials/using_cli`](https://huggingface.co/docs/text-generation-inference/basic_tutorials/using_cli)

您可以使用 TGI 命令行界面（CLI）来下载权重，提供和量化模型，或获取有关提供参数的信息。 要安装 CLI，请参考安装部分。

`text-generation-server`允许您使用`download-weights`命令下载模型，就像下面这样👇

```py
text-generation-server download-weights MODEL_HUB_ID
```

您还可以使用它来量化模型，就像下面这样👇

```py
text-generation-server quantize MODEL_HUB_ID OUTPUT_DIR 
```

您可以使用`text-generation-launcher`来提供模型。

```py
text-generation-launcher --model-id MODEL_HUB_ID --port 8080
```

您可以传递许多选项和参数给`text-generation-launcher`。 CLI 的文档保持最小化，旨在依赖于自动生成的文档，可以通过运行找到

```py
text-generation-launcher --help
```

您还可以在此[Swagger UI](https://huggingface.github.io/text-generation-inference/)中找到它托管。

`text-generation-server`的相同文档也可以找到。

```py
text-generation-server --help
```
