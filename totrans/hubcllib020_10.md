# 通过文件系统 API 与 Hub 进行交互

> 原始文本：[`huggingface.co/docs/huggingface_hub/guides/hf_file_system`](https://huggingface.co/docs/huggingface_hub/guides/hf_file_system)

除了 HfApi，`huggingface_hub`库提供 HfFileSystem，这是一个符合 Python 风格的[fsspec 兼容](https://filesystem-spec.readthedocs.io/en/latest/)文件接口，用于 Hugging Face Hub。HfFileSystem 基于 HfApi 构建，并提供像`cp`、`mv`、`ls`、`du`、`glob`、`get_file`和`put_file`等典型的文件系统风格操作。

## 用法

```py
>>> from huggingface_hub import HfFileSystem
>>> fs = HfFileSystem()

>>> # List all files in a directory
>>> fs.ls("datasets/my-username/my-dataset-repo/data", detail=False)
['datasets/my-username/my-dataset-repo/data/train.csv', 'datasets/my-username/my-dataset-repo/data/test.csv']

>>> # List all ".csv" files in a repo
>>> fs.glob("datasets/my-username/my-dataset-repo/**.csv")
['datasets/my-username/my-dataset-repo/data/train.csv', 'datasets/my-username/my-dataset-repo/data/test.csv']

>>> # Read a remote file 
>>> with fs.open("datasets/my-username/my-dataset-repo/data/train.csv", "r") as f:
...     train_data = f.readlines()

>>> # Read the content of a remote file as a string
>>> train_data = fs.read_text("datasets/my-username/my-dataset-repo/data/train.csv", revision="dev")

>>> # Write a remote file
>>> with fs.open("datasets/my-username/my-dataset-repo/data/validation.csv", "w") as f:
...     f.write("text,label")
...     f.write("Fantastic movie!,good")
```

可传递可选的`revision`参数来运行特定提交的操作，如分支、标签名称或提交哈希。

与 Python 内置的`open`不同，`fsspec`的`open`默认为二进制模式，`"rb"`。这意味着您必须明确将模式设置为`"r"`以进行读取和`"w"`以进行写入文本模式。目前不支持追加到文件（模式`"a"`和`"ab"`）。

## 集成

HfFileSystem 可以与任何集成了`fsspec`的库一起使用，只要 URL 遵循以下方案：

```py
hf://[<repo_type_prefix>]<repo_id>[@<revision>]/<path/in/repo>
```

`repo_type_prefix`对于数据集是`datasets/`，对于空间是`spaces/`，模型在 URL 中不需要前缀。

一些 HfFileSystem 简化与 Hub 交互的有趣集成如下列出：

+   从/向 Hub 存储库读取/写入[Pandas](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html#reading-writing-remote-files) DataFrame：

    ```py
    >>> import pandas as pd

    >>> # Read a remote CSV file into a dataframe
    >>> df = pd.read_csv("hf://datasets/my-username/my-dataset-repo/train.csv")

    >>> # Write a dataframe to a remote CSV file
    >>> df.to_csv("hf://datasets/my-username/my-dataset-repo/test.csv")
    ```

相同的工作流程也适用于[Dask](https://docs.dask.org/en/stable/how-to/connect-to-remote-data.html)和[Polars](https://pola-rs.github.io/polars/py-polars/html/reference/io.html) DataFrames。

+   使用[DuckDB](https://duckdb.org/docs/guides/python/filesystems)查询（远程）Hub 文件：

    ```py
    >>> from huggingface_hub import HfFileSystem
    >>> import duckdb

    >>> fs = HfFileSystem()
    >>> duckdb.register_filesystem(fs)
    >>> # Query a remote file and get the result back as a dataframe
    >>> fs_query_file = "hf://datasets/my-username/my-dataset-repo/data_dir/data.parquet"
    >>> df = duckdb.query(f"SELECT * FROM '{fs_query_file}' LIMIT 10").df()
    ```

+   使用[Zarr](https://zarr.readthedocs.io/en/stable/tutorial.html#io-with-fsspec)将 Hub 用作数组存储库：

    ```py
    >>> import numpy as np
    >>> import zarr

    >>> embeddings = np.random.randn(50000, 1000).astype("float32")

    >>> # Write an array to a repo
    >>> with zarr.open_group("hf://my-username/my-model-repo/array-store", mode="w") as root:
    ...    foo = root.create_group("embeddings")
    ...    foobar = foo.zeros('experiment_0', shape=(50000, 1000), chunks=(10000, 1000), dtype='f4')
    ...    foobar[:] = embeddings

    >>> # Read an array from a repo
    >>> with zarr.open_group("hf://my-username/my-model-repo/array-store", mode="r") as root:
    ...    first_row = root["embeddings/experiment_0"][0]
    ```

## 认证

在许多情况下，您必须使用 Hugging Face 帐户登录以与 Hub 进行交互。请参考文档中的认证部分，了解有关 Hub 上认证方法的更多信息。

也可以通过将您的`token`作为参数传递给 HfFileSystem 来以编程方式登录：

```py
>>> from huggingface_hub import HfFileSystem
>>> fs = HfFileSystem(token=token)
```

如果以这种方式登录，请注意在分享源代码时不要意外泄漏令牌！
