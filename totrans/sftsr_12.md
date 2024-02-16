# Numpy API

> 原始文本: [`huggingface.co/docs/safetensors/api/numpy`](https://huggingface.co/docs/safetensors/api/numpy)

#### `safetensors.numpy.load_file`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/numpy.py#L107)

```py
( filename: Union ) → export const metadata = 'undefined';Dict[str, np.ndarray]
```

参数

+   `filename` (`str`，或`os.PathLike`)) — 包含张量的文件的名称

返回

`Dict[str, np.ndarray]`

包含名称作为键，值为`np.ndarray`的字典

将一个 safetensors 文件加载到 numpy 格式中。

示例:

```py
from safetensors.numpy import load_file

file_path = "./my_folder/bert.safetensors"
loaded = load_file(file_path)
```

#### `safetensors.numpy.load`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/numpy.py#L80)

```py
( data: bytes ) → export const metadata = 'undefined';Dict[str, np.ndarray]
```

参数

+   `data` (`bytes`) — 一个 safetensors 文件的内容

返回

`Dict[str, np.ndarray]`

包含名称作为键，值为`np.ndarray`在 CPU 上的字典

将一个 safetensors 文件从纯字节加载到 numpy 格式中。

示例:

```py
from safetensors.numpy import load

file_path = "./my_folder/bert.safetensors"
with open(file_path, "rb") as f:
    data = f.read()

loaded = load(data)
```

#### `safetensors.numpy.save_file`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/numpy.py#L47)

```py
( tensor_dict: Dict filename: Union metadata: Optional = None ) → export const metadata = 'undefined';None
```

参数

+   `tensor_dict` (`Dict[str, np.ndarray]`) — 传入的张量。张量需要是连续且密集的。

+   `filename` (`str`，或`os.PathLike`)) — 我们要保存的文件名。

+   `metadata` (`Dict[str, str]`, *可选*, 默认为`None`) — 可选的仅文本元数据，您可能希望保存在头部。例如，可以更详细地指定底层张量的信息。这仅仅是信息性的，不会影响张量加载。

返回

`None`

将一个张量字典保存为 safetensors 格式的原始字节。

示例:

```py
from safetensors.numpy import save_file
import numpy as np

tensors = {"embedding": np.zeros((512, 1024)), "attention": np.zeros((256, 256))}
save_file(tensors, "model.safetensors")
```

#### `safetensors.numpy.save`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/numpy.py#L16)

```py
( tensor_dict: Dict metadata: Optional = None ) → export const metadata = 'undefined';bytes
```

参数

+   `tensor_dict` (`Dict[str, np.ndarray]`) — 传入的张量。张量需要是连续且密集的。

+   `metadata` (`Dict[str, str]`, *可选*, 默认为`None`) — 可选的仅文本元数据，您可能希望保存在头部。例如，可以更详细地指定底层张量的信息。这仅仅是信息性的，不会影响张量加载。

返回

`bytes`

表示格式的原始字节

将张量字典保存为 safetensors 格式的原始字节。

示例:

```py
from safetensors.numpy import save
import numpy as np

tensors = {"embedding": np.zeros((512, 1024)), "attention": np.zeros((256, 256))}
byte_data = save(tensors)
```
