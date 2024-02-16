# 快速浏览

> 原始文本：[`huggingface.co/docs/text-generation-inference/quicktour`](https://huggingface.co/docs/text-generation-inference/quicktour)

开始的最简单方法是使用官方的 Docker 容器。按照[它们的安装说明](https://docs.docker.com/get-docker/)安装 Docker。

假设您想要部署[Falcon-7B Instruct](https://huggingface.co/tiiuae/falcon-7b-instruct)模型与 TGI。以下是如何执行的示例：

```py
model=tiiuae/falcon-7b-instruct
volume=$PWD/data # share a volume with the Docker container to avoid downloading weights every run

docker run --gpus all --shm-size 1g -p 8080:80 -v $volume:/data ghcr.io/huggingface/text-generation-inference:1.4 --model-id $model
```

要使用 NVIDIA GPU，您需要安装[NVIDIA 容器工具包](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)。我们还建议使用带有 CUDA 版本 12.2 或更高版本的 NVIDIA 驱动程序。

TGI 还支持启用 ROCm 的 AMD GPU（仅 MI210 和 MI250 经过测试），详细信息请参阅支持的硬件部分和[AMD 文档](https://rocm.docs.amd.com/en/latest/deploy/docker.html)。要在 ROCm GPU 上启动 TGI，请改用：

```py
docker run --cap-add=SYS_PTRACE --security-opt seccomp=unconfined --device=/dev/kfd --device=/dev/dri --group-add video --ipc=host --shm-size 1g -p 8080:80 -v $volume:/data ghcr.io/huggingface/text-generation-inference:1.4-rocm --model-id $model
```

一旦 TGI 运行起来，您可以通过请求使用`generate`端点。要了解有关如何查询端点的更多信息，请查看使用 TGI 部分，我们在那里展示了使用实用程序库和 UI 的示例。下面是一个查询端点的简单代码片段。

PythonJavaScriptcURL

```py
import requests

headers = {
    "Content-Type": "application/json",
}

data = {
    'inputs': 'What is Deep Learning?',
    'parameters': {
        'max_new_tokens': 20,
    },
}

response = requests.post('http://127.0.0.1:8080/generate', headers=headers, json=data)
print(response.json())
# {'generated_text': '\n\nDeep Learning is a subset of Machine Learning that is concerned with the development of algorithms that can'}
```

要查看所有可能的部署标志和选项，您可以使用`--help`标志。可以配置分片数、量化、生成参数等。

```py
docker run ghcr.io/huggingface/text-generation-inference:1.4 --help
```
