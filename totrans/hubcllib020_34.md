# 文件系统 API

> [`huggingface.co/docs/huggingface_hub/package_reference/hf_file_system`](https://huggingface.co/docs/huggingface_hub/package_reference/hf_file_system)

`HfFileSystem`类提供了一个基于[`fsspec`](https://filesystem-spec.readthedocs.io/en/latest/)的 Python 文件接口，用于 Hugging Face Hub。

## HfFileSystem

`HfFileSystem`基于[fsspec](https://filesystem-spec.readthedocs.io/en/latest/)，因此与其提供的大多数 API 兼容。有关更多详细信息，请查看我们的指南和 fsspec 的[API 参考](https://filesystem-spec.readthedocs.io/en/latest/api.html#fsspec.spec.AbstractFileSystem)。

### `class huggingface_hub.HfFileSystem`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_file_system.py#L61)

```py
( *args **kwargs )
```

参数

+   `token` (`str`, *可选*) — 认证令牌，使用`HfApi.login`方法获取。将默认使用存储的令牌。

像访问本地文件系统一样访问远程 Hugging Face Hub 存储库。

用法：

```py
>>> from huggingface_hub import HfFileSystem

>>> fs = HfFileSystem()

>>> # List files
>>> fs.glob("my-username/my-model/*.bin")
['my-username/my-model/pytorch_model.bin']
>>> fs.ls("datasets/my-username/my-dataset", detail=False)
['datasets/my-username/my-dataset/.gitattributes', 'datasets/my-username/my-dataset/README.md', 'datasets/my-username/my-dataset/data.json']

>>> # Read/write files
>>> with fs.open("my-username/my-model/pytorch_model.bin") as f:
...     data = f.read()
>>> with fs.open("my-username/my-model/pytorch_model.bin", "wb") as f:
...     f.write(data)
```

#### `__init__`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_file_system.py#L93)

```py
( *args endpoint: Optional = None token: Optional = None **storage_options )
```

#### `resolve_path`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_file_system.py#L128)

```py
( path: str revision: Optional = None )
```

#### `ls`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_file_system.py#L266)

```py
( path: str detail: bool = True refresh: bool = False revision: Optional = None **kwargs )
```

列出目录的内容。
