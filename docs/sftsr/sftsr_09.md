# Tensorflow API

> 原文链接：[`huggingface.co/docs/safetensors/api/tensorflow`](https://huggingface.co/docs/safetensors/api/tensorflow)

#### `safetensors.tensorflow.load_file`

[< source >](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/tensorflow.py#L101)

```py
( filename: Union ) → export const metadata = 'undefined';Dict[str, tf.Tensor]
```

参数

+   `filename` (`str` 或 `os.PathLike`)) — 包含张量的文件的名称

返回

`Dict[str, tf.Tensor]`

包含名称作为键，值为 `tf.Tensor` 的字典

将 safetensors 文件加载到 tensorflow 格式中。

示例：

```py
from safetensors.tensorflow import load_file

file_path = "./my_folder/bert.safetensors"
loaded = load_file(file_path)
```

#### `safetensors.tensorflow.load`

[< source >](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/tensorflow.py#L74)

```py
( data: bytes ) → export const metadata = 'undefined';Dict[str, tf.Tensor]
```

参数

+   `data` (`bytes`) — 一个 safetensors 文件的内容

返回

`Dict[str, tf.Tensor]`

包含名称作为键，值为 `tf.Tensor` 的字典在 CPU 上

从纯字节加载 safetensors 文件到 tensorflow 格式。

示例：

```py
from safetensors.tensorflow import load

file_path = "./my_folder/bert.safetensors"
with open(file_path, "rb") as f:
    data = f.read()

loaded = load(data)
```

#### `safetensors.tensorflow.save_file`

[< source >](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/tensorflow.py#L39)

```py
( tensors: Dict filename: Union metadata: Optional = None ) → export const metadata = 'undefined';None
```

参数

+   `tensors` (`Dict[str, tf.Tensor]`) — 传入的张量。张量需要是连续且密集的。

+   `filename` (`str` 或 `os.PathLike`)) — 我们要保存的文件名。

+   `metadata` (`Dict[str, str]`, *可选*, 默认为 `None`) — 可选的仅文本元数据，您可能希望保存在头部。例如，可以指定有关底层张量的更多信息。这仅仅是信息性的，不会影响张量加载。

返回

`None`

将张量字典保存为 safetensors 格式的原始字节。

示例：

```py
from safetensors.tensorflow import save_file
import tensorflow as tf

tensors = {"embedding": tf.zeros((512, 1024)), "attention": tf.zeros((256, 256))}
save_file(tensors, "model.safetensors")
```

#### `safetensors.tensorflow.save`

[< source >](https://github.com/huggingface/safetensors/blob/main/bindings/python/py_src/safetensors/tensorflow.py#L10)

```py
( tensors: Dict metadata: Optional = None ) → export const metadata = 'undefined';bytes
```

参数

+   `tensors` (`Dict[str, tf.Tensor]`) — 传入的张量。张量需要是连续且密集的。

+   `metadata` (`Dict[str, str]`, *可选*, 默认为 `None`) — 可选的仅文本元数据，您可能希望保存在头部。例如，可以指定有关底层张量的更多信息。这仅仅是信息性的，不会影响张量加载。

返回

`bytes`

表示格式的原始字节

将张量字典保存为 safetensors 格式的原始字节。

示例：

```py
from safetensors.tensorflow import save
import tensorflow as tf

tensors = {"embedding": tf.zeros((512, 1024)), "attention": tf.zeros((256, 256))}
byte_data = save(tensors)
```
