# Flax API

> 原始文本：[https://huggingface.co/docs/safetensors/api/flax](https://huggingface.co/docs/safetensors/api/flax)

#### `safetensors.flax.load_file`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/flax.py#L102)

```py
( filename: Union ) → export const metadata = 'undefined';Dict[str, Array]
```

参数

+   `filename` (`str`，或 `os.PathLike`)) — 包含张量的文件的名称

返回

`Dict[str, Array]`

包含名称作为键，值为 `Array` 的字典

将 safetensors 文件加载为 flax 格式。

示例：

```py
from safetensors.flax import load_file

file_path = "./my_folder/bert.safetensors"
loaded = load_file(file_path)
```

#### `safetensors.flax.load`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/flax.py#L75)

```py
( data: bytes ) → export const metadata = 'undefined';Dict[str, Array]
```

参数

+   `data` (`bytes`) — safetensors 文件的内容

返回

`Dict[str, Array]`

包含名称作为键，值为 `Array` 在 CPU 上的字典

从纯字节将 safetensors 文件加载为 flax 格式。

示例：

```py
from safetensors.flax import load

file_path = "./my_folder/bert.safetensors"
with open(file_path, "rb") as f:
    data = f.read()

loaded = load(data)
```

#### `safetensors.flax.save_file`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/flax.py#L40)

```py
( tensors: Dict filename: Union metadata: Optional = None ) → export const metadata = 'undefined';None
```

参数

+   `tensors` (`Dict[str, Array]`) — 传入的张量。张量需要是连续且密集的。

+   `filename` (`str`，或 `os.PathLike`)) — 我们要保存的文件名。

+   `metadata` (`Dict[str, str]`, *optional*, 默认为 `None`) — 可选的仅文本元数据，您可能希望保存在标头中。例如，可以指定有关底层张量的更多信息。这仅仅是信息性的，不会影响张量加载。

返回

`None`

将张量字典保存为 safetensors 格式的原始字节。

示例：

```py
from safetensors.flax import save_file
from jax import numpy as jnp

tensors = {"embedding": jnp.zeros((512, 1024)), "attention": jnp.zeros((256, 256))}
save_file(tensors, "model.safetensors")
```

#### `safetensors.flax.save`

[<来源>](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/flax.py#L11)

```py
( tensors: Dict metadata: Optional = None ) → export const metadata = 'undefined';bytes
```

参数

+   `tensors` (`Dict[str, Array]`) — 传入的张量。张量需要是连续且密集的。

+   `metadata` (`Dict[str, str]`, *optional*, 默认为 `None`) — 可选的仅文本元数据，您可能希望保存在标头中。例如，可以指定有关底层张量的更多信息。这仅仅是信息性的，不会影响张量加载。

返回

`bytes`

表示格式的原始字节

将张量字典保存为 safetensors 格式的原始字节。

示例：

```py
from safetensors.flax import save
from jax import numpy as jnp

tensors = {"embedding": jnp.zeros((512, 1024)), "attention": jnp.zeros((256, 256))}
byte_data = save(tensors)
```
