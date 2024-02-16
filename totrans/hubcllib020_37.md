# 缓存系统参考

> 原文：[https://huggingface.co/docs/huggingface_hub/package_reference/cache](https://huggingface.co/docs/huggingface_hub/package_reference/cache)

缓存系统在v0.8.0中进行了更新，成为依赖于Hub的库之间共享的中央缓存系统。阅读[缓存系统指南](../guides/manage-cache)以详细介绍HF的缓存。

## 助手

### try_to_load_from_cache

#### `huggingface_hub.try_to_load_from_cache`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/file_download.py#L1495)

```py
( repo_id: str filename: str cache_dir: Union = None revision: Optional = None repo_type: Optional = None ) → export const metadata = 'undefined';Optional[str] or _CACHED_NO_EXIST
```

参数

+   `cache_dir` (`str`或`os.PathLike`) — 缓存文件所在的文件夹。

+   `repo_id` (`str`) — huggingface.co上存储库的ID。

+   `filename` (`str`) — 在`repo_id`内查找的文件名。

+   `revision` (`str`, *可选*) — 要使用的特定模型版本。如果未提供且未提供`commit_hash`，则默认为`"main"`。

+   `repo_type` (`str`, *可选*) — 存储库的类型。默认为`"model"`。

返回

`Optional[str]` 或 `_CACHED_NO_EXIST`

如果文件未被缓存，则返回`None`。否则：

+   如果在缓存中找到，则返回缓存文件的确切路径

+   如果文件不存在于给定的提交哈希中，并且这一事实已被缓存，则特殊值`_CACHED_NO_EXIST`。

探索缓存以返回给定修订版的最新缓存文件（如果找到）。

如果文件未被缓存，此函数不会引发任何异常。

示例：

```py
from huggingface_hub import try_to_load_from_cache, _CACHED_NO_EXIST

filepath = try_to_load_from_cache()
if isinstance(filepath, str):
    # file exists and is cached
    ...
elif filepath is _CACHED_NO_EXIST:
    # non-existence of file is cached
    ...
else:
    # file is not cached
    ...
```

### cached_assets_path

#### `huggingface_hub.cached_assets_path`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_assets.py#L21)

```py
( library_name: str namespace: str = 'default' subfolder: str = 'default' assets_dir: Union = None )
```

参数

+   `library_name` (`str`) — 将管理缓存文件夹的库的名称。示例："dataset"。

+   `namespace` (`str`, *可选*, 默认为"default") — 数据所属的命名空间。示例："SQuAD"。

+   `subfolder` (`str`, *可选*, 默认为"default") — 数据将存储在其中的子文件夹。示例：`extracted`。

+   `assets_dir` (`str`, `Path`, *可选*) — 资产被缓存的文件夹路径。这不应该是Hub文件被缓存的相同文件夹。如果未提供，默认为`HF_HOME / "assets"`。也可以使用`HF_ASSETS_CACHE`环境变量设置。

返回一个用于缓存任意文件的文件夹路径。

`huggingface_hub`提供了一个规范的文件夹路径来存储资产。这是在下游库中集成缓存的推荐方式，因为它将受益于内置工具来正确扫描和删除缓存。

从Hub缓存的文件和资产之间进行区分。从Hub缓存的文件以git感知方式缓存，并由`huggingface_hub`完全管理。查看[相关文档](https://huggingface.co/docs/huggingface_hub/how-to-cache)。下游库缓存的所有其他文件被视为“资产”（从外部来源下载的文件，从.tar存档中提取的文件，为训练预处理的文件等）。

生成文件夹路径后，保证其存在且为目录。路径基于3个深度级别：库名称，命名空间和子文件夹。这3个级别在允许`huggingface_hub`扫描/删除资产缓存部分时提供了灵活性。在库内，预期所有命名空间共享相同的子文件夹名称，但这不是强制性规则。下游库完全控制其缓存中采用的文件结构。命名空间和子文件夹是可选的（默认为`"default/"`子文件夹），但库名称是强制的，因为我们希望每个下游库管理自己的缓存。

预期树：

```py
    assets/
    └── datasets/
    │   ├── SQuAD/
    │   │   ├── downloaded/
    │   │   ├── extracted/
    │   │   └── processed/
    │   ├── Helsinki-NLP--tatoeba_mt/
    │       ├── downloaded/
    │       ├── extracted/
    │       └── processed/
    └── transformers/
        ├── default/
        │   ├── something/
        ├── bert-base-cased/
        │   ├── default/
        │   └── training/
    hub/
    └── models--julien-c--EsperBERTo-small/
        ├── blobs/
        │   ├── (...)
        │   ├── (...)
        ├── refs/
        │   └── (...)
        └── [ 128]  snapshots/
            ├── 2439f60ef33a0d46d85da5001d52aeda5b00ce9f/
            │   ├── (...)
            └── bbc77c8132af1cc5cf678da3f1ddf2de43606d48/
                └── (...)
```

示例：

```py
>>> from huggingface_hub import cached_assets_path

>>> cached_assets_path(library_name="datasets", namespace="SQuAD", subfolder="download")
PosixPath('/home/wauplin/.cache/huggingface/extra/datasets/SQuAD/download')

>>> cached_assets_path(library_name="datasets", namespace="SQuAD", subfolder="extracted")
PosixPath('/home/wauplin/.cache/huggingface/extra/datasets/SQuAD/extracted')

>>> cached_assets_path(library_name="datasets", namespace="Helsinki-NLP/tatoeba_mt")
PosixPath('/home/wauplin/.cache/huggingface/extra/datasets/Helsinki-NLP--tatoeba_mt/default')

>>> cached_assets_path(library_name="datasets", assets_dir="/tmp/tmp123456")
PosixPath('/tmp/tmp123456/datasets/default/default')
```

### scan_cache_dir

#### `huggingface_hub.scan_cache_dir`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L496)

```py
( cache_dir: Union = None )
```

参数

+   `cache_dir` (`str` or `Path`, `optional`) — 要缓存的缓存目录。默认为默认的 HF 缓存目录。

Raises

`CacheNotFound` 或 `ValueError`

+   `CacheNotFound` — 如果缓存目录不存在。

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) — 如果缓存目录是文件而不是目录。

扫描整个 HF 缓存系统并返回一个 [~HFCacheInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.HFCacheInfo) 结构。

使用 `scan_cache_dir` 来以编程方式扫描您的缓存系统。缓存将逐个仓库进行扫描。如果一个仓库损坏，将在内部抛出一个 [~CorruptedCacheException](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.CorruptedCacheException) 但会被捕获并在 [~HFCacheInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.HFCacheInfo) 结构中返回。只有有效的仓库会得到适当的报告。

```py
>>> from huggingface_hub import scan_cache_dir

>>> hf_cache_info = scan_cache_dir()
HFCacheInfo(
    size_on_disk=3398085269,
    repos=frozenset({
        CachedRepoInfo(
            repo_id='t5-small',
            repo_type='model',
            repo_path=PosixPath(...),
            size_on_disk=970726914,
            nb_files=11,
            revisions=frozenset({
                CachedRevisionInfo(
                    commit_hash='d78aea13fa7ecd06c29e3e46195d6341255065d5',
                    size_on_disk=970726339,
                    snapshot_path=PosixPath(...),
                    files=frozenset({
                        CachedFileInfo(
                            file_name='config.json',
                            size_on_disk=1197
                            file_path=PosixPath(...),
                            blob_path=PosixPath(...),
                        ),
                        CachedFileInfo(...),
                        ...
                    }),
                ),
                CachedRevisionInfo(...),
                ...
            }),
        ),
        CachedRepoInfo(...),
        ...
    }),
    warnings=[
        CorruptedCacheException("Snapshots dir doesn't exist in cached repo: ..."),
        CorruptedCacheException(...),
        ...
    ],
)
```

您也可以直接从 `huggingface-cli` 打印详细报告：

```py
> huggingface-cli scan-cache
REPO ID                     REPO TYPE SIZE ON DISK NB FILES REFS                LOCAL PATH
--------------------------- --------- ------------ -------- ------------------- -------------------------------------------------------------------------
glue                        dataset         116.3K       15 1.17.0, main, 2.4.0 /Users/lucain/.cache/huggingface/hub/datasets--glue
google/fleurs               dataset          64.9M        6 main, refs/pr/1     /Users/lucain/.cache/huggingface/hub/datasets--google--fleurs
Jean-Baptiste/camembert-ner model           441.0M        7 main                /Users/lucain/.cache/huggingface/hub/models--Jean-Baptiste--camembert-ner
bert-base-cased             model             1.9G       13 main                /Users/lucain/.cache/huggingface/hub/models--bert-base-cased
t5-base                     model            10.1K        3 main                /Users/lucain/.cache/huggingface/hub/models--t5-base
t5-small                    model           970.7M       11 refs/pr/1, main     /Users/lucain/.cache/huggingface/hub/models--t5-small

Done in 0.0s. Scanned 6 repo(s) for a total of 3.4G.
Got 1 warning(s) while scanning. Use -vvv to print details.
```

返回：一个 [~HFCacheInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.HFCacheInfo) 对象。

## 数据结构

所有结构都是由 [scan_cache_dir()](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.scan_cache_dir) 构建并返回的，是不可变的。

### HFCacheInfo

### `class huggingface_hub.HFCacheInfo`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L353)

```py
( size_on_disk: int repos: FrozenSet warnings: List )
```

Parameters

+   `size_on_disk` (`int`) — 缓存系统中所有有效仓库大小的总和。

+   `repos` (`FrozenSet[CachedRepoInfo]`) — 描述在扫描时在缓存系统中找到的所有有效缓存仓库的 [~CachedRepoInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.CachedRepoInfo) 集合。

+   `warnings` (`List[CorruptedCacheException]`) — 扫描缓存时发生的 [~CorruptedCacheException](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.CorruptedCacheException) 列表。这些异常被捕获以便扫描可以继续进行。损坏的仓库将被跳过扫描。

包含有关整个缓存系统的信息的冻结数据结构。

这个数据结构是由 [scan_cache_dir()](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.scan_cache_dir) 返回的，是不可变的。

这里 `size_on_disk` 等于所有仓库大小的总和（仅包括 blob）。但是如果某些缓存仓库损坏，它们的大小不会被计入。

#### `delete_revisions`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L392)

```py
( *revisions: str )
```

准备删除本地缓存的一个或多个修订版本的策略。

输入的修订版本可以是任何修订哈希。如果本地缓存中找不到修订哈希，会发出警告但不会引发错误。修订版本可以来自不同的缓存仓库，因为哈希在仓库之间是唯一的，

示例：

```py
>>> from huggingface_hub import scan_cache_dir
>>> cache_info = scan_cache_dir()
>>> delete_strategy = cache_info.delete_revisions(
...     "81fd1d6e7847c99f5862c9fb81387956d99ec7aa"
... )
>>> print(f"Will free {delete_strategy.expected_freed_size_str}.")
Will free 7.9K.
>>> delete_strategy.execute()
Cache deletion done. Saved 7.9K.
```

```py
>>> from huggingface_hub import scan_cache_dir
>>> scan_cache_dir().delete_revisions(
...     "81fd1d6e7847c99f5862c9fb81387956d99ec7aa",
...     "e2983b237dccf3ab4937c97fa717319a9ca1a96d",
...     "6c0e6080953db56375760c0471a8c5f2929baf11",
... ).execute()
Cache deletion done. Saved 8.6G.
```

`delete_revisions` 返回一个需要执行的 [DeleteCacheStrategy](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.DeleteCacheStrategy) 对象。[DeleteCacheStrategy](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.DeleteCacheStrategy) 不应该被修改，但允许在实际执行删除之前进行干预运行。

### CachedRepoInfo

### `class huggingface_hub.CachedRepoInfo`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L191)

```py
( repo_id: str repo_type: Literal repo_path: Path size_on_disk: int nb_files: int revisions: FrozenSet last_accessed: float last_modified: float )
```

Parameters

+   `repo_id` (`str`) — Hub 上仓库的 Repo id。示例："google/fleurs"。

+   `repo_type` (`Literal["dataset", "model", "space"]`) — 缓存仓库的类型。

+   `repo_path` (`Path`) — 缓存仓库的本地路径。

+   `size_on_disk` (`int`) — 缓存仓库中 blob 文件大小的总和。

+   `nb_files` (`int`) — 缓存仓库中的 blob 文件总数。

+   `revisions`（`FrozenSet[CachedRevisionInfo]`）- 描述存储库中缓存的所有修订版本的 [~CachedRevisionInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.CachedRevisionInfo) 集合。

+   `last_accessed`（`float`）- 存储库中 blob 文件上次被访问的时间戳。

+   `last_modified`（`float`）- 存储库中 blob 文件上次修改/创建的时间戳。

包含有关缓存存储库的信息的冻结数据结构。

`size_on_disk` 不一定是所有修订版本大小的总和，因为存在重复的文件。此外，只考虑 blob，不考虑文件夹和符号链接的（可忽略的）大小。

`last_accessed` 和 `last_modified` 的可靠性可能取决于您使用的操作系统。有关更多详细信息，请参阅 [python 文档](https://docs.python.org/3/library/os.html#os.stat_result)。

#### `size_on_disk_str`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L260)

```py
( )
```

（属性）blob 文件大小的总和，以人类可读的字符串形式。

示例：“42.2K”。

#### `refs`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L269)

```py
( )
```

（属性）`refs` 和修订版本数据结构之间的映射。

### CachedRevisionInfo

### `class huggingface_hub.CachedRevisionInfo`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L114)

```py
( commit_hash: str snapshot_path: Path size_on_disk: int files: FrozenSet refs: FrozenSet last_modified: float )
```

参数

+   `commit_hash`（`str`）- 修订版本的哈希值（唯一）。示例：`"9338f7b671827df886678df2bdd7cc7b4f36dffd"`。

+   `snapshot_path`（`Path`）- `snapshots` 文件夹中修订版本目录的路径。它包含与 Hub 上的存储库完全相同的树结构。files - (`FrozenSet[CachedFileInfo]`): 描述快照中包含的所有文件的 [~CachedFileInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.CachedFileInfo) 集合。

+   `refs`（`FrozenSet[str]`）- 指向此修订版本的 `refs` 集合。如果修订版本没有 `refs`，则被视为分离的。示例：`{"main", "2.4.0"}` 或 `{"refs/pr/1"}`。

+   `size_on_disk`（`int`）- 由修订版本符号链接的 blob 文件大小的总和。

+   `last_modified`（`float`）- 修订版本上次创建/修改的时间戳。

包含有关修订版本的信息的冻结数据结构。

修订版本对应于 `snapshots` 文件夹中的一个文件夹，并且填充有与 Hub 上的存储库完全相同的树结构，但只包含符号链接。修订版本可以被 1 个或多个 `refs` 引用，也可以是“分离的”（没有引用）。

`last_accessed` 无法在单个修订版本上正确确定，因为 blob 文件在不同的修订版本之间共享。

`size_on_disk` 不一定是所有文件大小的总和，因为可能存在重复的文件。此外，只考虑 blob，不考虑文件夹和符号链接的（可忽略的）大小。

#### `size_on_disk_str`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L174)

```py
( )
```

（属性）blob 文件大小的总和，以人类可读的字符串形式。

示例：“42.2K”。

#### `nb_files`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L183)

```py
( )
```

（属性）修订版本中的文件总数。

### CachedFileInfo

### `class huggingface_hub.CachedFileInfo`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L47)

```py
( file_name: str file_path: Path blob_path: Path size_on_disk: int blob_last_accessed: float blob_last_modified: float )
```

参数

+   `file_name`（`str`）- 文件的名称。示例：`config.json`。

+   `file_path`（`Path`）- `snapshots` 目录中文件的路径。文件路径是指向 `blobs` 文件夹中的 blob 的符号链接。

+   `blob_path`（`Path`）- blob 文件的路径。这等同于 `file_path.resolve()`。

+   `size_on_disk`（`int`）- blob 文件的大小（字节）。

+   `blob_last_accessed`（`float`）- blob 文件上次被访问的时间戳（从任何修订版本）。

+   `blob_last_modified` (`float`) — blob 文件上次修改/创建的时间戳。

冻结的数据结构，保存有关单个缓存文件的信息。

`blob_last_accessed` 和 `blob_last_modified` 的可靠性可能取决于您使用的操作系统。有关更多详细信息，请参阅 [python 文档](https://docs.python.org/3/library/os.html#os.stat_result)。

#### `size_on_disk_str`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L104)

```py
( )
```

（属性）blob 文件的大小，以人类可读的字符串表示。

示例：“42.2K”。

### DeleteCacheStrategy

### `class huggingface_hub.DeleteCacheStrategy`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L277)

```py
( expected_freed_size: int blobs: FrozenSet refs: FrozenSet repos: FrozenSet snapshots: FrozenSet )
```

参数

+   `expected_freed_size` (`float`) — 执行策略后预期释放的大小。

+   `blobs` (`FrozenSet[Path]`) — 要删除的 blob 文件路径集合。

+   `refs` (`FrozenSet[Path]`) — 要删除的引用文件路径集合。

+   `repos` (`FrozenSet[Path]`) — 要删除的整个仓库路径集合。

+   `snapshots` (`FrozenSet[Path]`) — 要删除的快照集合（符号链接目录）。

冻结的数据结构，保存有关删除缓存修订的策略。

此对象不是用于编程实例化的，而是由 [delete_revisions()](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.HFCacheInfo.delete_revisions) 返回。请参阅用法示例的文档。

#### `expected_freed_size_str`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L303)

```py
( )
```

（属性）预期释放的大小，以人类可读的字符串表示。

示例：“42.2K”。

## 异常

### CorruptedCacheException

### `class huggingface_hub.CorruptedCacheException`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_cache_manager.py#L43)

```py
( )
```

Huggingface 缓存系统中任何意外结构的异常。
