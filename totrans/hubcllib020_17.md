# 管理huggingface_hub缓存系统

> 原文链接：[https://huggingface.co/docs/huggingface_hub/guides/manage-cache](https://huggingface.co/docs/huggingface_hub/guides/manage-cache)

## 理解缓存

Hugging Face Hub缓存系统旨在成为依赖于Hub的库之间共享的中央缓存。在v0.8.0中已更新，以防止在不同版本之间重新下载相同的文件。

缓存系统设计如下：

```py
<CACHE_DIR>
├─ <MODELS>
├─ <DATASETS>
├─ <SPACES>
```

`<CACHE_DIR>`通常是您的用户主目录。但是，可以通过所有方法的`cache_dir`参数进行自定义，或者通过指定`HF_HOME`或`HF_HUB_CACHE`环境变量来进行自定义。

模型、数据集和空间共享一个共同的根。每个这些存储库包含存储库类型、命名空间（组织或用户名）（如果存在）和存储库名称：

```py
<CACHE_DIR>
├─ models--julien-c--EsperBERTo-small
├─ models--lysandrejik--arxiv-nlp
├─ models--bert-base-cased
├─ datasets--glue
├─ datasets--huggingface--DataMeasurementsFiles
├─ spaces--dalle-mini--dalle-mini
```

现在所有文件将从Hub中下载到这些文件夹中。缓存确保如果文件已经存在且未更新，则不会下载两次；但如果文件已更新，并且您要求最新文件，则它将下载最新文件（同时保留以防您再次需要的先前文件）。

为了实现这一点，所有文件夹都包含相同的骨架：

```py
<CACHE_DIR>
├─ datasets--glue
│  ├─ refs
│  ├─ blobs
│  ├─ snapshots
...
```

每个文件夹设计为包含以下内容：

### 引用

`refs`文件夹包含指示给定引用的最新修订版本的文件。例如，如果我们之前从存储库的`main`分支中获取了一个文件，`refs`文件夹将包含一个名为`main`的文件，其中将包含当前头部的提交标识符。

如果`main`的最新提交具有`aaaaaa`作为标识符，则它将包含`aaaaaa`。

如果同一分支更新为具有`bbbbbb`作为标识符的新提交，则重新从该引用下载文件将更新`refs/main`文件以包含`bbbbbb`。

### 块

`blobs`文件夹包含我们下载的实际文件。每个文件的名称是它们的哈希值。

### 快照

`snapshots`文件夹包含对上述块的符号链接。它本身由几个文件夹组成：每个已知修订版本一个文件夹！

在上面的解释中，我们最初从`aaaaaa`修订版本中获取了一个文件，然后从`bbbbbb`修订版本中获取了一个文件。在这种情况下，`snapshots`文件夹中现在会有两个文件夹：`aaaaaa`和`bbbbbb`。

在这些文件夹中，存在着我们已下载文件的名称的符号链接。例如，如果我们在修订版本`aaaaaa`中下载了`README.md`文件，我们将有以下路径：

```py
<CACHE_DIR>/<REPO_NAME>/snapshots/aaaaaa/README.md
```

那个`README.md`文件实际上是一个符号链接，链接到具有文件哈希值的块。

通过以这种方式创建骨架，我们打开了文件共享的机制：如果在修订版本`bbbbbb`中获取了相同的文件，它将具有相同的哈希值，文件就不需要重新下载。

### .no_exist（高级）

除了`blobs`、`refs`和`snapshots`文件夹外，您还可能在缓存中找到一个`.no_exist`文件夹。这个文件夹用于跟踪您尝试下载但在Hub上不存在的文件。它的结构与`snapshots`文件夹相同，每个已知修订版本一个子文件夹：

```py
<CACHE_DIR>/<REPO_NAME>/.no_exist/aaaaaa/config_that_does_not_exist.json
```

与`snapshots`文件夹不同，文件是简单的空文件（没有符号链接）。在这个例子中，文件`"config_that_does_not_exist.json"`在修订版本`"aaaaaa"`上不存在于Hub上。由于它只存储空文件，这个文件夹在磁盘使用方面可以忽略不计。

现在您可能会想，为什么这些信息甚至相关呢？在某些情况下，框架尝试加载模型的可选文件。保存可选文件不存在会使加载模型更快，因为它可以节省每个可能的可选文件的 1 次 HTTP 调用。例如，在 `transformers` 中，每个分词器都可以支持额外的文件。第一次在您的机器上加载分词器时，它将缓存哪些可选文件存在（哪些不存在），以便在下一次初始化时加快加载速度。

要测试文件是否在本地缓存中（而不进行任何 HTTP 请求），可以使用 [try_to_load_from_cache()](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.try_to_load_from_cache) 助手。它将返回文件路径（如果存在且已缓存），对象 `_CACHED_NO_EXIST`（如果不存在已缓存）或 `None`（如果我们不知道）。

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

### 在实践中

在实践中，您的缓存应该如下所示：

```py
    [  96]  .
    └── [ 160]  models--julien-c--EsperBERTo-small
        ├── [ 160]  blobs
        │   ├── [321M]  403450e234d65943a7dcf7e05a771ce3c92faa84dd07db4ac20f592037a1e4bd
        │   ├── [ 398]  7cb18dc9bafbfcf74629a4b760af1b160957a83e
        │   └── [1.4K]  d7edf6bd2a681fb0175f7735299831ee1b22b812
        ├── [  96]  refs
        │   └── [  40]  main
        └── [ 128]  snapshots
            ├── [ 128]  2439f60ef33a0d46d85da5001d52aeda5b00ce9f
            │   ├── [  52]  README.md -> ../../blobs/d7edf6bd2a681fb0175f7735299831ee1b22b812
            │   └── [  76]  pytorch_model.bin -> ../../blobs/403450e234d65943a7dcf7e05a771ce3c92faa84dd07db4ac20f592037a1e4bd
            └── [ 128]  bbc77c8132af1cc5cf678da3f1ddf2de43606d48
                ├── [  52]  README.md -> ../../blobs/7cb18dc9bafbfcf74629a4b760af1b160957a83e
                └── [  76]  pytorch_model.bin -> ../../blobs/403450e234d65943a7dcf7e05a771ce3c92faa84dd07db4ac20f592037a1e4bd
```

### 限制

为了拥有高效的缓存系统，`huggingface-hub` 使用符号链接。然而，并非所有机器都支持符号链接。这是一个已知的限制，尤其是在 Windows 上。在这种情况下，`huggingface_hub` 不使用 `blobs/` 目录，而是直接将文件存储在 `snapshots/` 目录中。这种解决方法允许用户以完全相同的方式从 Hub 下载和缓存文件。还支持用于检查和删除缓存的工具（见下文）。然而，缓存系统效率较低，因为如果下载了同一存储库的多个版本，可能会多次下载同一个文件。

如果您想在 Windows 机器上受益于基于符号链接的缓存系统，您需要要么[激活开发者模式](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development)，要么以管理员身份运行 Python。

当不支持符号链接时，会向用户显示警告消息，提醒他们正在使用降级版本的缓存系统。可以通过将 `HF_HUB_DISABLE_SYMLINKS_WARNING` 环境变量设置为 true 来禁用此警告。

## 缓存资产

除了从 Hub 缓存文件外，下游库通常需要缓存与 HF 相关但不直接由 `huggingface_hub` 处理的其他文件（例如：从 GitHub 下载的文件，预处理数据，日志等）。为了缓存这些称为 `assets` 的文件，可以使用 [cached_assets_path()](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.cached_assets_path)。这个小助手以请求它的库的名称为基础，在 HF 缓存中以统一的方式生成路径，还可以选择性地使用命名空间和子文件夹名称。目标是让每个下游库以自己的方式管理其资产（例如：不规定结构），只要它保持在正确的资产文件夹中。然后这些库可以利用 `huggingface_hub` 提供的工具来管理缓存，特别是通过 CLI 命令扫描和删除部分资产。

```py
from huggingface_hub import cached_assets_path

assets_path = cached_assets_path(library_name="datasets", namespace="SQuAD", subfolder="download")
something_path = assets_path / "something.json" # Do anything you like in your assets folder !
```

[cached_assets_path()](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.cached_assets_path) 是存储资产的推荐方式，但不是强制性的。如果您的库已经使用自己的缓存，请随意使用！

### 实践中的资产

在实践中，您的资产缓存应该如下所示：

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

## 扫描您的缓存

目前，缓存文件从本地目录中永远不会被删除：当您下载分支的新版本时，以前的文件会被保留，以防您再次需要它们。因此，扫描您的缓存目录以了解哪些存储库和版本占用了最多的磁盘空间可能是有用的。`huggingface_hub` 提供了一个助手来做到这一点，可以通过 `huggingface-cli` 或在 Python 脚本中使用。

### 从终端扫描缓存

扫描您的HF缓存系统的最简单方法是使用`huggingface-cli`工具中的`scan-cache`命令。此命令扫描缓存并打印一个报告，其中包含有关存储库id、存储库类型、磁盘使用情况、引用和完整本地路径的信息。

下面的片段显示了一个文件夹中的扫描报告，其中缓存了4个模型和2个数据集。

```py
➜ huggingface-cli scan-cache
REPO ID                     REPO TYPE SIZE ON DISK NB FILES LAST_ACCESSED LAST_MODIFIED REFS                LOCAL PATH
--------------------------- --------- ------------ -------- ------------- ------------- ------------------- -------------------------------------------------------------------------
glue                        dataset         116.3K       15 4 days ago    4 days ago    2.4.0, main, 1.17.0 /home/wauplin/.cache/huggingface/hub/datasets--glue
google/fleurs               dataset          64.9M        6 1 week ago    1 week ago    refs/pr/1, main     /home/wauplin/.cache/huggingface/hub/datasets--google--fleurs
Jean-Baptiste/camembert-ner model           441.0M        7 2 weeks ago   16 hours ago  main                /home/wauplin/.cache/huggingface/hub/models--Jean-Baptiste--camembert-ner
bert-base-cased             model             1.9G       13 1 week ago    2 years ago                       /home/wauplin/.cache/huggingface/hub/models--bert-base-cased
t5-base                     model            10.1K        3 3 months ago  3 months ago  main                /home/wauplin/.cache/huggingface/hub/models--t5-base
t5-small                    model           970.7M       11 3 days ago    3 days ago    refs/pr/1, main     /home/wauplin/.cache/huggingface/hub/models--t5-small

Done in 0.0s. Scanned 6 repo(s) for a total of 3.4G.
Got 1 warning(s) while scanning. Use -vvv to print details.
```

要获得更详细的报告，请使用`--verbose`选项。对于每个存储库，您将获得已下载的所有修订的列表。如上所述，不在2个修订之间更改的文件通过符号链接共享。这意味着磁盘上的存储库大小预计会小于每个修订的大小之和。例如，这里`bert-base-cased`有1.4G和1.5G的2个修订，但总磁盘使用量仅为1.9G。

```py
➜ huggingface-cli scan-cache -v
REPO ID                     REPO TYPE REVISION                                 SIZE ON DISK NB FILES LAST_MODIFIED REFS        LOCAL PATH
--------------------------- --------- ---------------------------------------- ------------ -------- ------------- ----------- ----------------------------------------------------------------------------------------------------------------------------
glue                        dataset   9338f7b671827df886678df2bdd7cc7b4f36dffd        97.7K       14 4 days ago    main, 2.4.0 /home/wauplin/.cache/huggingface/hub/datasets--glue/snapshots/9338f7b671827df886678df2bdd7cc7b4f36dffd
glue                        dataset   f021ae41c879fcabcf823648ec685e3fead91fe7        97.8K       14 1 week ago    1.17.0      /home/wauplin/.cache/huggingface/hub/datasets--glue/snapshots/f021ae41c879fcabcf823648ec685e3fead91fe7
google/fleurs               dataset   129b6e96cf1967cd5d2b9b6aec75ce6cce7c89e8        25.4K        3 2 weeks ago   refs/pr/1   /home/wauplin/.cache/huggingface/hub/datasets--google--fleurs/snapshots/129b6e96cf1967cd5d2b9b6aec75ce6cce7c89e8
google/fleurs               dataset   24f85a01eb955224ca3946e70050869c56446805        64.9M        4 1 week ago    main        /home/wauplin/.cache/huggingface/hub/datasets--google--fleurs/snapshots/24f85a01eb955224ca3946e70050869c56446805
Jean-Baptiste/camembert-ner model     dbec8489a1c44ecad9da8a9185115bccabd799fe       441.0M        7 16 hours ago  main        /home/wauplin/.cache/huggingface/hub/models--Jean-Baptiste--camembert-ner/snapshots/dbec8489a1c44ecad9da8a9185115bccabd799fe
bert-base-cased             model     378aa1bda6387fd00e824948ebe3488630ad8565         1.5G        9 2 years ago               /home/wauplin/.cache/huggingface/hub/models--bert-base-cased/snapshots/378aa1bda6387fd00e824948ebe3488630ad8565
bert-base-cased             model     a8d257ba9925ef39f3036bfc338acf5283c512d9         1.4G        9 3 days ago    main        /home/wauplin/.cache/huggingface/hub/models--bert-base-cased/snapshots/a8d257ba9925ef39f3036bfc338acf5283c512d9
t5-base                     model     23aa4f41cb7c08d4b05c8f327b22bfa0eb8c7ad9        10.1K        3 1 week ago    main        /home/wauplin/.cache/huggingface/hub/models--t5-base/snapshots/23aa4f41cb7c08d4b05c8f327b22bfa0eb8c7ad9
t5-small                    model     98ffebbb27340ec1b1abd7c45da12c253ee1882a       726.2M        6 1 week ago    refs/pr/1   /home/wauplin/.cache/huggingface/hub/models--t5-small/snapshots/98ffebbb27340ec1b1abd7c45da12c253ee1882a
t5-small                    model     d0a119eedb3718e34c648e594394474cf95e0617       485.8M        6 4 weeks ago               /home/wauplin/.cache/huggingface/hub/models--t5-small/snapshots/d0a119eedb3718e34c648e594394474cf95e0617
t5-small                    model     d78aea13fa7ecd06c29e3e46195d6341255065d5       970.7M        9 1 week ago    main        /home/wauplin/.cache/huggingface/hub/models--t5-small/snapshots/d78aea13fa7ecd06c29e3e46195d6341255065d5

Done in 0.0s. Scanned 6 repo(s) for a total of 3.4G.
Got 1 warning(s) while scanning. Use -vvv to print details.
```

#### Grep示例

由于输出是表格格式，您可以将其与任何类似`grep`的工具结合使用来过滤条目。以下是在基于Unix的机器上仅过滤“t5-small”模型的修订的示例。

```py
➜ eval "huggingface-cli scan-cache -v" | grep "t5-small"
t5-small                    model     98ffebbb27340ec1b1abd7c45da12c253ee1882a       726.2M        6 1 week ago    refs/pr/1   /home/wauplin/.cache/huggingface/hub/models--t5-small/snapshots/98ffebbb27340ec1b1abd7c45da12c253ee1882a
t5-small                    model     d0a119eedb3718e34c648e594394474cf95e0617       485.8M        6 4 weeks ago               /home/wauplin/.cache/huggingface/hub/models--t5-small/snapshots/d0a119eedb3718e34c648e594394474cf95e0617
t5-small                    model     d78aea13fa7ecd06c29e3e46195d6341255065d5       970.7M        9 1 week ago    main        /home/wauplin/.cache/huggingface/hub/models--t5-small/snapshots/d78aea13fa7ecd06c29e3e46195d6341255065d5
```

### 从Python扫描缓存

对于更高级的用法，请使用[scan_cache_dir()](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.scan_cache_dir)，这是CLI工具调用的Python实用程序。

您可以使用它来获取一个围绕4个数据类结构化的详细报告：

+   [HFCacheInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.HFCacheInfo)：由[scan_cache_dir()](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.scan_cache_dir)返回的完整报告

+   [CachedRepoInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.CachedRepoInfo)：有关缓存存储库的信息

+   [CachedRevisionInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.CachedRevisionInfo)：有关存储库内缓存修订（例如“快照”）的信息

+   [CachedFileInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.CachedFileInfo)：有关快照中缓存文件的信息

这里是一个简单的用法示例。有关详细信息，请参阅参考资料。

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
            last_accessed=1662971707.3567169,
            last_modified=1662971107.3567169,
            revisions=frozenset({
                CachedRevisionInfo(
                    commit_hash='d78aea13fa7ecd06c29e3e46195d6341255065d5',
                    size_on_disk=970726339,
                    snapshot_path=PosixPath(...),
                    # No `last_accessed` as blobs are shared among revisions
                    last_modified=1662971107.3567169,
                    files=frozenset({
                        CachedFileInfo(
                            file_name='config.json',
                            size_on_disk=1197
                            file_path=PosixPath(...),
                            blob_path=PosixPath(...),
                            blob_last_accessed=1662971707.3567169,
                            blob_last_modified=1662971107.3567169,
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

## 清理您的缓存

扫描您的缓存是有趣的，但您真正想要做的下一步通常是删除一些部分以释放驱动器上的一些空间。使用`delete-cache` CLI命令可以实现这一点。也可以在扫描缓存时使用返回的[HFCacheInfo](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.HFCacheInfo)对象中的[delete_revisions()](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.HFCacheInfo.delete_revisions)助手进行编程。

### 删除策略

要删除某些缓存，您需要传递一个要删除的修订列表。工具将根据此列表定义释放空间的策略。它返回一个描述将要删除哪些文件和文件夹的[DeleteCacheStrategy](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.DeleteCacheStrategy)对象。[DeleteCacheStrategy](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.DeleteCacheStrategy)允许您知道预计将释放多少空间。一旦您同意删除，必须执行它以使删除生效。为避免不一致，您不能手动编辑策略对象。

删除修订的策略如下：

+   包含修订符号链接的`snapshot`文件夹将被删除。

+   仅被要删除的修订目标的blob文件也将被删除。

+   如果一个修订与1个或多个`refs`相关联，则引用将被删除。

+   如果一个存储库的所有修订都被删除，整个缓存的存储库也将被删除。

修订哈希在所有存储库中是唯一的。这意味着在删除修订时，您无需提供任何`repo_id`或`repo_type`。

如果在缓存中找不到修订版本，它将被静默忽略。此外，如果在尝试删除文件或文件夹时找不到它，将记录警告但不会抛出错误。删除将继续进行其他包含在 [DeleteCacheStrategy](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.DeleteCacheStrategy) 对象中的路径。

### 从终端清除缓存

从HF缓存系统中删除一些修订版本的最简单方法是使用 `huggingface-cli` 工具中的 `delete-cache` 命令。该命令有两种模式。默认情况下，会显示一个 TUI（终端用户界面）供用户选择要删除的修订版本。此 TUI 目前处于测试阶段，尚未在所有平台上进行测试。如果 TUI 在您的计算机上无法工作，您可以使用 `--disable-tui` 标志禁用它。

#### 使用TUI

这是默认模式。要使用它，您首先需要通过运行以下命令安装额外的依赖项：

```py
pip install huggingface_hub["cli"]
```

然后运行命令：

```py
huggingface-cli delete-cache
```

现在您应该看到一个修订版本列表，您可以选择/取消选择：

![](../Images/aafc157d3819eea90ace56496a84b8e6.png)

说明：

+   按键盘箭头键 `<上>` 和 `<下>` 移动光标。

+   按下 `<空格>` 键切换（选择/取消选择）项目。

+   选择修订版本后，第一行将更新以显示将释放多少空间。

+   按 `<回车>` 键确认您的选择。

+   如果要取消操作并退出，可以选择第一个项目（“以下无任何内容”）。如果选择了此项目，无论选择了其他项目，删除过程都将被取消。否则，您也可以按 `<ctrl+c>` 退出 TUI。

选择要删除的修订版本并按 `<回车>` 键后，将提示最后的确认消息。再次按 `<回车>` 键，删除将生效。如果要取消，请输入 `n`。

```py
✗ huggingface-cli delete-cache --dir ~/.cache/huggingface/hub
? Select revisions to delete: 2 revision(s) selected.
? 2 revisions selected counting for 3.1G. Confirm deletion ? Yes
Start deletion.
Done. Deleted 1 repo(s) and 0 revision(s) for a total of 3.1G.
```

#### 无TUI

如上所述，TUI 模式目前处于测试阶段，是可选的。可能是它在您的计算机上无法工作，或者您认为它不方便。

另一种方法是使用 `--disable-tui` 标志。该过程与您被要求手动审核要删除的修订版本列表非常相似。但是，这个手动步骤不会直接在终端中进行，而是在临时生成的文件中进行，您可以手动编辑。

此文件的标题中包含您需要的所有说明。在您喜爱的文本编辑器中打开它。要选择/取消选择修订版本，只需使用 `#` 进行注释/取消注释。完成手动审核并编辑文件后，您可以保存它。返回到终端并按 `<回车>` 键。默认情况下，它将计算更新的修订版本列表将释放多少空间。您可以继续编辑文件或使用 `"y"` 确认。

```py
huggingface-cli delete-cache --disable-tui
```

命令文件示例：

```py
# INSTRUCTIONS
# ------------
# This is a temporary file created by running `huggingface-cli delete-cache` with the
# `--disable-tui` option. It contains a set of revisions that can be deleted from your
# local cache directory.
#
# Please manually review the revisions you want to delete:
#   - Revision hashes can be commented out with '#'.
#   - Only non-commented revisions in this file will be deleted.
#   - Revision hashes that are removed from this file are ignored as well.
#   - If `CANCEL_DELETION` line is uncommented, the all cache deletion is cancelled and
#     no changes will be applied.
#
# Once you've manually reviewed this file, please confirm deletion in the terminal. This
# file will be automatically removed once done.
# ------------

# KILL SWITCH
# ------------
# Un-comment following line to completely cancel the deletion process
# CANCEL_DELETION
# ------------

# REVISIONS
# ------------
# Dataset chrisjay/crowd-speech-africa (761.7M, used 5 days ago)
    ebedcd8c55c90d39fd27126d29d8484566cd27ca # Refs: main # modified 5 days ago

# Dataset oscar (3.3M, used 4 days ago)
#    916f956518279c5e60c63902ebdf3ddf9fa9d629 # Refs: main # modified 4 days ago

# Dataset wikiann (804.1K, used 2 weeks ago)
    89d089624b6323d69dcd9e5eb2def0551887a73a # Refs: main # modified 2 weeks ago

# Dataset z-uo/male-LJSpeech-italian (5.5G, used 5 days ago)
#    9cfa5647b32c0a30d0adfca06bf198d82192a0d1 # Refs: main # modified 5 days ago
```

### 从Python清除缓存

为了更灵活，您还可以以编程方式使用 [delete_revisions()](/docs/huggingface_hub/v0.20.3/en/package_reference/cache#huggingface_hub.HFCacheInfo.delete_revisions) 方法。这里是一个简单的示例。有关详细信息，请参阅参考资料。

```py
>>> from huggingface_hub import scan_cache_dir

>>> delete_strategy = scan_cache_dir().delete_revisions(
...     "81fd1d6e7847c99f5862c9fb81387956d99ec7aa"
...     "e2983b237dccf3ab4937c97fa717319a9ca1a96d",
...     "6c0e6080953db56375760c0471a8c5f2929baf11",
... )
>>> print("Will free " + delete_strategy.expected_freed_size_str)
Will free 8.6G

>>> delete_strategy.execute()
Cache deletion done. Saved 8.6G.
```
