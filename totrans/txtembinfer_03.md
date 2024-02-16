# 快速浏览

> 原始文本：[https://huggingface.co/docs/text-embeddings-inference/quick_tour](https://huggingface.co/docs/text-embeddings-inference/quick_tour)

## 文本嵌入

开始使用TEI的最简单方法是使用官方的Docker容器之一（请参阅[支持的模型和硬件](supported_models)以选择正确的容器）。

确保您的硬件受支持后，如果您计划使用GPU，则安装[NVIDIA容器工具包](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)。您设备上的NVIDIA驱动程序需要与CUDA版本12.2或更高版本兼容。

接下来，按照他们的[安装说明](https://docs.docker.com/get-docker/)安装Docker。

最后，部署您的模型。假设您想使用`BAAI/bge-large-en-v1.5`。以下是您可以这样做的方法：

```py
model=BAAI/bge-large-en-v1.5
revision=refs/pr/5
volume=$PWD/data 

docker run --gpus all -p 8080:80 -v $volume:/data --pull always ghcr.io/huggingface/text-embeddings-inference:0.6 --model-id $model --revision $revision
```

在这里，我们传递了`revision=refs/pr/5`，因为此模型的`safetensors`变体目前在拉取请求中。我们还建议与Docker容器共享卷（`volume=$PWD/data`）以避免在每次运行时下载权重。

一旦您部署了模型，您可以通过发送请求来使用`embed`端点：

```py
curl 127.0.0.1:8080/embed \
    -X POST \
    -d '{"inputs":"What is Deep Learning?"}' \
    -H 'Content-Type: application/json'
```

## 重新排列器

重新排列器模型是具有单个类别的序列分类交叉编码器模型，用于评分查询和文本之间的相似性。

请参阅LlamaIndex团队的[此博文](https://blog.llamaindex.ai/boosting-rag-picking-the-best-embedding-reranker-models-42d079022e83)以了解如何在RAG管道中使用重新排列器模型来提高下游性能。

假设您想使用`BAAI/bge-reranker-large`：

```py
model=BAAI/bge-reranker-large
revision=refs/pr/4
volume=$PWD/data 

docker run --gpus all -p 8080:80 -v $volume:/data --pull always ghcr.io/huggingface/text-embeddings-inference:0.6 --model-id $model --revision $revision
```

一旦您部署了模型，您可以使用`rerank`端点来对查询和文本列表之间的相似性进行排名：

```py
curl 127.0.0.1:8080/rerank \
    -X POST \
    -d '{"query":"What is Deep Learning?", "texts": ["Deep Learning is not...", "Deep learning is..."], "raw_scores": false}' \
    -H 'Content-Type: application/json'
```

## 序列分类

您还可以使用经典的序列分类模型，比如`SamLowe/roberta-base-go_emotions`：

```py
model=SamLowe/roberta-base-go_emotions
volume=$PWD/data 

docker run --gpus all -p 8080:80 -v $volume:/data --pull always ghcr.io/huggingface/text-embeddings-inference:0.6 --model-id $model 
```

一旦您部署了模型，您可以使用`predict`端点来获取与输入最相关的情绪：

```py
curl 127.0.0.1:8080/predict \
    -X POST \
    -d '{"inputs":"I like you."}' \
    -H 'Content-Type: application/json'
```
