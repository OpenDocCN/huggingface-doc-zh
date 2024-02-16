# 提供私有和受限制的模型

> 原文链接：[https://huggingface.co/docs/text-embeddings-inference/private_models](https://huggingface.co/docs/text-embeddings-inference/private_models)

如果您希望提供的模型受限制或位于Hugging Face Hub上的私有模型仓库中，您需要访问该模型才能提供服务。

确认您可以访问模型后：

+   前往您账户的[个人资料 | 设置 | 访问令牌页面](https://huggingface.co/settings/tokens)。

+   生成并复制一个读取令牌。

如果您使用CLI，请设置`HUGGING_FACE_HUB_TOKEN`环境变量。例如：

```py
export HUGGING_FACE_HUB_TOKEN=<YOUR READ TOKEN>
```

或者，您可以在使用Docker部署模型时提供令牌。

```py
model=<your private model>
volume=$PWD/data
token=<your cli Hugging Face Hub token>

docker run --gpus all -e HUGGING_FACE_HUB_TOKEN=$token -p 8080:80 -v $volume:/data --pull always ghcr.io/huggingface/text-embeddings-inference:0.6 --model-id $model
```
