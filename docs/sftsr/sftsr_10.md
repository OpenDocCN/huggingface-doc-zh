# PaddlePaddle API

> 原始文本: [`huggingface.co/docs/safetensors/api/paddle`](https://huggingface.co/docs/safetensors/api/paddle)

#### `safetensors.paddle.load_file`

[< source >](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/paddle.py#L101)

```py
( filename: Union device = 'cpu' ) → export const metadata = 'undefined';Dict[str, paddle.Tensor]
```

参数

+   `filename` (`str`, 或 `os.PathLike`)) — 包含张量的文件的名称

+   `device` (`Dict[str, any]`, *可选*, 默认为 `cpu`) — 加载后张量需要位于的设备。可用选项为所有常规的 Paddle 设备位置

返回

`Dict[str, paddle.Tensor]`

包含名称作为键，值为 `paddle.Tensor` 的字典

将 safetensors 文件加载到 Paddle 格式中。

示例:

```py
from safetensors.paddle import load_file

file_path = "./my_folder/bert.safetensors"
loaded = load_file(file_path)
```

#### `safetensors.paddle.load`

[< source >](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/paddle.py#L74)

```py
( data: bytes device: str = 'cpu' ) → export const metadata = 'undefined';Dict[str, paddle.Tensor]
```

参数

+   `data` (`bytes`) — safetensors 文件的内容

返回

`Dict[str, paddle.Tensor]`

包含名称作为键，值为 `paddle.Tensor` 在 cpu 上的字典

从纯字节将 safetensors 文件加载到 Paddle 格式。

示例:

```py
from safetensors.paddle import load

file_path = "./my_folder/bert.safetensors"
with open(file_path, "rb") as f:
    data = f.read()

loaded = load(data)
```

#### `safetensors.paddle.save_file`

[< source >](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/paddle.py#L39)

```py
( tensors: Dict filename: Union metadata: Optional = None ) → export const metadata = 'undefined';None
```

参数

+   `tensors` (`Dict[str, paddle.Tensor]`) — 传入的张量。张量需要是连续且密集的。

+   `filename` (`str`, 或 `os.PathLike`)) — 我们要保存的文件名。

+   `metadata` (`Dict[str, str]`, *可选*, 默认为 `None`) — 可选的仅文本元数据，您可能希望保存在标头中。例如，可以有助于更详细地指定底层张量的信息。这仅供参考，不影响张量加载。

返回

`None`

将张量字典以 safetensors 格式保存为原始字节。

示例:

```py
from safetensors.paddle import save_file
import paddle

tensors = {"embedding": paddle.zeros((512, 1024)), "attention": paddle.zeros((256, 256))}
save_file(tensors, "model.safetensors")
```

#### `safetensors.paddle.save`

[< source >](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/paddle.py#L10)

```py
( tensors: Dict metadata: Optional = None ) → export const metadata = 'undefined';bytes
```

参数

+   `tensors` (`Dict[str, paddle.Tensor]`) — 传入的张量。张量需要是连续且密集的。

+   `metadata` (`Dict[str, str]`, *可选*, 默认为 `None`) — 可选的仅文本元数据，您可能希望保存在标头中。例如，可以有助于更详细地指定底层张量的信息。这仅供参考，不影响张量加载。

返回

`bytes`

表示格式的原始字节

将张量字典以 safetensors 格式保存为原始字节。

示例:

```py
from safetensors.paddle import save
import paddle

tensors = {"embedding": paddle.zeros((512, 1024)), "attention": paddle.zeros((256, 256))}
byte_data = save(tensors)
```
