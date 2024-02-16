# Torch API

> 原文：[`huggingface.co/docs/safetensors/api/torch`](https://huggingface.co/docs/safetensors/api/torch)

#### `safetensors.torch.load_file`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/torch.py#L284)

```py
( filename: Union device = 'cpu' ) → export const metadata = 'undefined';Dict[str, torch.Tensor]
```

参数

+   `filename` (`str`或`os.PathLike`) — 包含张量的文件的名称

+   `device` (`Dict[str, any]`, *可选*, 默认为`cpu`) — 加载后张量需要位于的设备。可用选项是所有常规的 torch 设备位置

返回

`Dict[str, torch.Tensor]`

包含名称为键，值为`torch.Tensor`的字典

将 safetensors 文件加载到 torch 格式。

示例：

```py
from safetensors.torch import load_file

file_path = "./my_folder/bert.safetensors"
loaded = load_file(file_path)
```

#### `safetensors.torch.load`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/torch.py#L314)

```py
( data: bytes ) → export const metadata = 'undefined';Dict[str, torch.Tensor]
```

参数

+   `data` (`bytes`) — safetensors 文件的内容

返回

`Dict[str, torch.Tensor]`

包含名称为键，值为在 cpu 上的`torch.Tensor`的字典

将 safetensors 文件从纯字节加载到 torch 格式。

示例：

```py
from safetensors.torch import load

file_path = "./my_folder/bert.safetensors"
with open(file_path, "rb") as f:
    data = f.read()

loaded = load(data)
```

#### `safetensors.torch.save_file`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/torch.py#L250)

```py
( tensors: Dict filename: Union metadata: Optional = None ) → export const metadata = 'undefined';None
```

参数

+   `tensors` (`Dict[str, torch.Tensor]`) — 传入的张量。张量需要是连续且密集的。

+   `filename` (`str`或`os.PathLike`)) — 我们要保存到的文件名。

+   `metadata` (`Dict[str, str]`, *可选*, 默认为`None`) — 您可能希望在头部保存的仅文本元数据。例如，指定有关底层张量的更多信息可能很有用。这仅仅是信息性的，不会影响张量加载。

返回

`None`

将张量字典保存为 safetensors 格式的原始字节。

示例：

```py
from safetensors.torch import save_file
import torch

tensors = {"embedding": torch.zeros((512, 1024)), "attention": torch.zeros((256, 256))}
save_file(tensors, "model.safetensors")
```

#### `safetensors.torch.save`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/torch.py#L220)

```py
( tensors: Dict metadata: Optional = None ) → export const metadata = 'undefined';bytes
```

参数

+   `tensors` (`Dict[str, torch.Tensor]`) — 传入的张量。张量需要是连续且密集的。

+   `metadata` (`Dict[str, str]`, *可选*, 默认为`None`) — 您可能希望在头部保存的仅文本元数据。例如，指定有关底层张量的更多信息可能很有用。这仅仅是信息性的，不会影响张量加载。

返回

`bytes`

表示格式的原始字节

将张量字典保存为 safetensors 格式的原始字节。

示例：

```py
from safetensors.torch import save
import torch

tensors = {"embedding": torch.zeros((512, 1024)), "attention": torch.zeros((256, 256))}
byte_data = save(tensors)
```

#### `safetensors.torch.load_model`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/torch.py#L176)

```py
( model: Module filename: Union strict = True ) → export const metadata = 'undefined';`(missing, unexpected)
```

参数

+   `model` (`torch.nn.Module`) — 要加载到的模型。

+   `filename` (`str`或`os.PathLike`) — 要从中加载文件的文件名位置。

+   `strict` (`bool`, *可选*, 默认为 True) — 是否在缺少键或存在意外键时失败。当为 false 时，该函数仅返回缺失和意外的名称。

返回

`(缺失, 意外)

(List[str], List[str])缺失的是在加载过程中未修改的模型中的名称，意外的是文件中存在但在加载过程中未使用的名称。

将给定的文件名加载到 torch 模型上。此方法专门存在以避免在`safetensors`中不允许的张量共享问题。有关张量共享的更多信息

#### `safetensors.torch.save_model`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/torch.py#L130)

```py
( model: Module filename: str metadata: Optional = None force_contiguous: bool = True )
```

参数

+   `model` (`torch.nn.Module`) — 要保存在磁盘上的模型。

+   `filename` (`str`) — 要保存文件的文件名位置

+   `metadata` (`Dict[str, str]`, *可选*) — 要与文件一起保存的额外信息。对于每个丢弃的张量，将添加一些元数据。这些信息将不足以恢复整个共享结构，但可能有助于理解事物

+   `force_contiguous` (`boolean`, *optional*, defaults to True) — 强制将 state_dict 保存为连续张量。这不会影响模型的正确性，但如果张量的布局是为了特定原因而选择的，可能会改变性能。

将给定的 torch 模型保存到指定的文件名。此方法专门存在以避免在`safetensors`中不允许的张量共享问题。有关张量共享的更多信息
