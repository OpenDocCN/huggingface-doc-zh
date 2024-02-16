# 速度比较

> 原始文本：[`huggingface.co/docs/safetensors/speed`](https://huggingface.co/docs/safetensors/speed)

![在 Colab 中打开](https://colab.research.google.com/github/huggingface/notebooks/blob/main/safetensors_doc/en/speed.ipynb)

`Safetensors`非常快。让我们将其与`PyTorch`进行比较，加载[gpt2](https://huggingface.co/gpt2)权重。要运行 GPU 基准测试，请确保您的机器有 GPU，或者如果您在使用 Google Colab，则已选择`GPU runtime`。

在开始之前，请确保您已安装所有必要的库：

```py
pip install safetensors huggingface_hub torch
```

让我们从导入将要使用的所有包开始：

```py
>>> import os
>>> import datetime
>>> from huggingface_hub import hf_hub_download
>>> from safetensors.torch import load_file
>>> import torch
```

下载 safetensors 和 gpt2 的 torch 权重：

```py
>>> sf_filename = hf_hub_download("gpt2", filename="model.safetensors")
>>> pt_filename = hf_hub_download("gpt2", filename="pytorch_model.bin")
```

### CPU 基准测试

```py
>>> start_st = datetime.datetime.now()
>>> weights = load_file(sf_filename, device="cpu")
>>> load_time_st = datetime.datetime.now() - start_st
>>> print(f"Loaded safetensors {load_time_st}")

>>> start_pt = datetime.datetime.now()
>>> weights = torch.load(pt_filename, map_location="cpu")
>>> load_time_pt = datetime.datetime.now() - start_pt
>>> print(f"Loaded pytorch {load_time_pt}")

>>> print(f"on CPU, safetensors is faster than pytorch by: {load_time_pt/load_time_st:.1f} X")
Loaded safetensors 0:00:00.004015
Loaded pytorch 0:00:00.307460
on CPU, safetensors is faster than pytorch by: 76.6 X
```

这种加速是因为这个库通过直接映射文件来避免不必要的拷贝。实际上，可以在[pure pytorch](https://gist.github.com/Narsil/3edeec2669a5e94e4707aa0f901d2282)上实现。当前显示的加速是在以下环境中获得的：

+   操作系统：Ubuntu 18.04.6 LTS

+   CPU：Intel(R) Xeon(R) CPU @ 2.00GHz

### GPU 基准测试

```py
>>> # This is required because this feature hasn't been fully verified yet, but 
>>> # it's been tested on many different environments
>>> os.environ["SAFETENSORS_FAST_GPU"] = "1"

>>> # CUDA startup out of the measurement
>>> torch.zeros((2, 2)).cuda()

>>> start_st = datetime.datetime.now()
>>> weights = load_file(sf_filename, device="cuda:0")
>>> load_time_st = datetime.datetime.now() - start_st
>>> print(f"Loaded safetensors {load_time_st}")

>>> start_pt = datetime.datetime.now()
>>> weights = torch.load(pt_filename, map_location="cuda:0")
>>> load_time_pt = datetime.datetime.now() - start_pt
>>> print(f"Loaded pytorch {load_time_pt}")

>>> print(f"on GPU, safetensors is faster than pytorch by: {load_time_pt/load_time_st:.1f} X")
Loaded safetensors 0:00:00.165206
Loaded pytorch 0:00:00.353889
on GPU, safetensors is faster than pytorch by: 2.1 X
```

这种加速的原因是这个库能够跳过不必要的 CPU 分配。不幸的是，据我们所知，纯 pytorch 中无法复制这种加速效果。该库通过内存映射文件、使用 pytorch 创建空张量，并直接调用`cudaMemcpy`来将张量直接移动到 GPU 上。当前显示的加速是在以下环境中获得的：

+   操作系统：Ubuntu 18.04.6 LTS。

+   GPU：Tesla T4

+   驱动程序版本：460.32.03

+   CUDA 版本：11.2
