# Safetensors

> 原始文本：[https://huggingface.co/docs/safetensors/index](https://huggingface.co/docs/safetensors/index)

![](../Images/f81f8740014e559d011303a31005d4c2.png) ![](../Images/ec98aa52c47a74abd8f4859770db315d.png)

Safetensors是一种新的简单格式，用于安全地存储张量（与pickle相反），并且仍然很快（零拷贝）。Safetensors真的很[快🚀](./speed)。

## 安装

使用pip：

```py
pip install safetensors
```

使用conda：

```py
conda install -c huggingface safetensors
```

## 用法

### 加载张量

```py
from safetensors import safe_open

tensors = {}
with safe_open("model.safetensors", framework="pt", device=0) as f:
    for k in f.keys():
        tensors[k] = f.get_tensor(k)
```

仅加载部分张量（在多个GPU上运行时很有趣）

```py
from safetensors import safe_open

tensors = {}
with safe_open("model.safetensors", framework="pt", device=0) as f:
    tensor_slice = f.get_slice("embedding")
    vocab_size, hidden_dim = tensor_slice.get_shape()
    tensor = tensor_slice[:, :hidden_dim]
```

### 保存张量

```py
import torch
from safetensors.torch import save_file

tensors = {
    "embedding": torch.zeros((2, 2)),
    "attention": torch.zeros((2, 3))
}
save_file(tensors, "model.safetensors")
```

## 格式

假设您有名为`model.safetensors`的safetensors文件，那么`model.safetensors`将具有以下内部格式：

![](../Images/1b995e3f75fd30b5761dbd64064c3663.png)

## 特色项目

Safetensors广泛应用于领先的人工智能企业，如[Hugging Face](https://huggingface.co/)，[EleutherAI](https://www.eleuther.ai/)和[StabilityAI](https://stability.ai/)。以下是一些正在使用safetensors的项目的非详尽列表：

+   [huggingface/transformers](https://github.com/huggingface/transformers)

+   [ml-explore/mlx](https://github.com/ml-explore/mlx)

+   [huggingface/candle](https://github.com/huggingface/candle)

+   [AUTOMATIC1111/stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui)

+   [Llama-cpp](https://github.com/ggerganov/llama.cpp/blob/e6a46b0ed1884c77267dc70693183e3b7164e0e0/convert.py#L537)

+   [microsoft/TaskMatrix](https://github.com/microsoft/TaskMatrix)

+   [hpcaitech/ColossalAI](https://github.com/hpcaitech/ColossalAI)

+   [huggingface/pytorch-image-models](https://github.com/huggingface/pytorch-image-models)

+   [CivitAI](https://civitai.com/)

+   [huggingface/diffusers](https://github.com/huggingface/diffusers)

+   [coreylowman/dfdx](https://github.com/coreylowman/dfdx)

+   [invoke-ai/InvokeAI](https://github.com/invoke-ai/InvokeAI)

+   [oobabooga/text-generation-webui](https://github.com/oobabooga/text-generation-webui)

+   [Sanster/lama-cleaner](https://github.com/Sanster/lama-cleaner)

+   [PaddlePaddle/PaddleNLP](https://github.com/PaddlePaddle/PaddleNLP)

+   [AIGC-Audio/AudioGPT](https://github.com/AIGC-Audio/AudioGPT)

+   [brycedrennan/imaginAIry](https://github.com/brycedrennan/imaginAIry)

+   [comfyanonymous/ComfyUI](https://github.com/comfyanonymous/ComfyUI)

+   [LianjiaTech/BELLE](https://github.com/LianjiaTech/BELLE)

+   [alvarobartt/safejax](https://github.com/alvarobartt/safejax)

+   [MaartenGr/BERTopic](https://github.com/MaartenGr/BERTopic)
