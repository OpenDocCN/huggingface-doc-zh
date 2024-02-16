# 提供私有和受限模型

> 原文链接：[`huggingface.co/docs/text-generation-inference/basic_tutorials/gated_model_access`](https://huggingface.co/docs/text-generation-inference/basic_tutorials/gated_model_access)

如果您希望提供的模型位于受限访问之后或 Hugging Face Hub 上的模型存储库是私有的，并且您可以访问该模型，您可以提供您的 Hugging Face Hub 访问令牌。您可以从[Hugging Face Hub 令牌页面](https://huggingface.co/settings/tokens)生成并复制一个读取令牌。

如果您正在使用 CLI，请设置`HUGGING_FACE_HUB_TOKEN`环境变量。例如：

```py
export HUGGING_FACE_HUB_TOKEN=<YOUR READ TOKEN>
```

如果您想通过 Docker 来做，可以按照下面所示提供您的令牌，指定`HUGGING_FACE_HUB_TOKEN`。

```py
model=meta-llama/Llama-2-7b-chat-hf
volume=$PWD/data
token=<your READ token>

docker run --gpus all \
    --shm-size 1g \
    -e HUGGING_FACE_HUB_TOKEN=$token \
    -p 8080:80 \
    -v $volume:/data ghcr.io/huggingface/text-generation-inference:1.4 \
    --model-id $model
```
