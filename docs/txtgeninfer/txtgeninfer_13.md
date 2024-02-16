# 非核心模型服务

> 原文链接：[`huggingface.co/docs/text-generation-inference/basic_tutorials/non_core_models`](https://huggingface.co/docs/text-generation-inference/basic_tutorials/non_core_models)

TGI 支持各种 LLM 架构（请参见完整列表此处）。如果您希望提供的模型不是受支持的模型之一，TGI 将退回到该模型的`transformers`实现。这意味着您将无法使用 TGI 引入的一些功能，例如张量并行分片或闪光注意力。但是，您仍然可以获得 TGI 的许多好处，例如连续批处理或流式输出。

您可以使用与完全支持的模型相同的 Docker 命令行调用来提供这些模型👇

```py
docker run --gpus all --shm-size 1g -p 8080:80 -v $volume:/data ghcr.io/huggingface/text-generation-inference:latest --model-id gpt2
```

如果您希望提供的模型是自定义的 transformers 模型，并且其权重和实现在 Hub 上可用，您仍然可以通过向`docker run`命令传递`--trust-remote-code`标志来提供模型，如下所示👇

```py
docker run --gpus all --shm-size 1g -p 8080:80 -v $volume:/data ghcr.io/huggingface/text-generation-inference:latest --model-id <CUSTOM_MODEL_ID> --trust-remote-code
```

最后，如果模型不在 Hugging Face Hub 上，而是在本地，您可以像下面这样传递包含您的模型的文件夹路径👇

```py
# Make sure your model is in the $volume directory
docker run --shm-size 1g -p 8080:80 -v $volume:/data  ghcr.io/huggingface/text-generation-inference:latest --model-id /data/<PATH-TO-FOLDER>
```

您可以参考[transformers 文档上的自定义模型](https://huggingface.co/docs/transformers/main/en/custom_models)获取更多信息。
