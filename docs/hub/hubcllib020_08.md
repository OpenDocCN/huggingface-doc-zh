# 上传文件到 Hub

> 原始文本：[`huggingface.co/docs/huggingface_hub/guides/upload`](https://huggingface.co/docs/huggingface_hub/guides/upload)

分享您的文件和工作是 Hub 的重要方面。`huggingface_hub`为将文件上传到 Hub 提供了几种选项。您可以独立使用这些功能，也可以将它们集成到您的库中，使用户更方便地与 Hub 交互。本指南将向您展示如何推送文件：

+   不使用 Git。

+   对于非常大的文件，可以使用[Git LFS](https://git-lfs.github.com/)。

+   使用`commit`上下文管理器。

+   使用 push_to_hub()函数。

每当您想要将文件上传到 Hub 时，您需要登录到您的 Hugging Face 帐户。有关身份验证的更多详细信息，请查看此部分。

## 上传文件

创建了一个存储库后，您可以使用 upload_file()将文件上传到您的存储库。

指定要上传的文件的路径，在存储库中要上传文件的位置，以及要将文件添加到的存储库的名称。根据您的存储库类型，您可以选择将存储库类型设置为`dataset`、`model`或`space`。

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> api.upload_file(
...     path_or_fileobj="/path/to/local/folder/README.md",
...     path_in_repo="README.md",
...     repo_id="username/test-dataset",
...     repo_type="dataset",
... )
```

## 上传文件夹

使用 upload_folder()函数将本地文件夹上传到现有存储库。指定要上传的本地文件夹的路径，在存储库中要上传文件夹的位置，以及要将文件夹添加到的存储库的名称。根据您的存储库类型，您可以选择将存储库类型设置为`dataset`、`model`或`space`。

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()

# Upload all the content from the local folder to your remote Space.
# By default, files are uploaded at the root of the repo
>>> api.upload_folder(
...     folder_path="/path/to/local/space",
...     repo_id="username/my-cool-space",
...     repo_type="space",
... )
```

默认情况下，将考虑`.gitignore`文件以确定应提交哪些文件。默认情况下，我们会检查提交中是否存在`.gitignore`文件，如果没有，则会检查 Hub 上是否存在。请注意，只有根目录中存在的`.gitignore`文件才会被使用。我们不会检查子目录中的`.gitignore`文件。

如果您不想使用硬编码的`.gitignore`文件，可以使用`allow_patterns`和`ignore_patterns`参数来过滤要上传的文件。这些参数接受单个模式或模式列表。模式是标准通配符（globbing patterns），如[此处](https://tldp.org/LDP/GNU-Linux-Tools-Summary/html/x11655.htm)所述。如果同时提供`allow_patterns`和`ignore_patterns`，则两个约束都适用。

除了`.gitignore`文件和允许/忽略模式外，任何子目录中存在的`.git/`文件夹都将被忽略。

```py
>>> api.upload_folder(
...     folder_path="/path/to/local/folder",
...     path_in_repo="my-dataset/train", # Upload to a specific folder
...     repo_id="username/test-dataset",
...     repo_type="dataset",
...     ignore_patterns="**/logs/*.txt", # Ignore all text logs
... )
```

您还可以使用`delete_patterns`参数指定要在同一提交中从存储库中删除的文件。如果您想要在推送文件之前清理远程文件夹并且不知道哪些文件已经存在，这可能会很有用。

下面的示例将本地的`./logs`文件夹上传到远程的`/experiment/logs/`文件夹。只有 txt 文件被上传，但在此之前，存储库中的所有先前日志都被删除。所有这些都在一个提交中完成。

```py
>>> api.upload_folder(
...     folder_path="/path/to/local/folder/logs",
...     repo_id="username/trained-model",
...     path_in_repo="experiment/logs/",
...     allow_patterns="*.txt", # Upload all local text files
...     delete_patterns="*.txt", # Delete all remote text files before
... )
```

## 从 CLI 上传

您可以使用终端中的`huggingface-cli upload`命令直接将文件上传到 Hub。内部使用上述描述的相同 upload_file()和 upload_folder()助手。

您可以上传单个文件或整个文件夹：

```py
# Usage:  huggingface-cli upload [repo_id] [local_path] [path_in_repo]
>>> huggingface-cli upload Wauplin/my-cool-model ./models/model.safetensors model.safetensors
https://huggingface.co/Wauplin/my-cool-model/blob/main/model.safetensors

>>> huggingface-cli upload Wauplin/my-cool-model ./models .
https://huggingface.co/Wauplin/my-cool-model/tree/main
```

`local_path`和`path_in_repo`是可选的，可以被隐式推断。如果未设置`local_path`，则工具将检查本地文件夹或文件是否与`repo_id`具有相同的名称。如果是这样，其内容将被上传。否则，将引发异常，要求用户明确设置`local_path`。无论如何，如果未设置`path_in_repo`，文件将被上传到存储库的根目录。

有关 CLI 上传命令的更多详细信息，请参考 CLI 指南。

## 高级功能

在大多数情况下，您不需要更多的 upload_file()和 upload_folder()来将文件上传到 Hub。然而，`huggingface_hub`具有更多高级功能，使事情变得更容易。让我们来看看它们！

### 非阻塞上传

在某些情况下，您希望在不阻塞主线程的情况下推送数据。这对于在继续训练的同时上传日志和工件特别有用。为此，您可以在 upload_file()和 upload_folder()中使用`run_as_future`参数。这将返回一个[`concurrent.futures.Future`](https://docs.python.org/3/library/concurrent.futures.html#future-objects)对象，您可以使用它来检查上传的状态。

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> future = api.upload_folder( # Upload in the background (non-blocking action)
...     repo_id="username/my-model",
...     folder_path="checkpoints-001",
...     run_as_future=True,
... )
>>> future
Future(...)
>>> future.done()
False
>>> future.result() # Wait for the upload to complete (blocking action)
...
```

使用`run_as_future=True`时，后台作业将被排队。这意味着您可以确保作业将按正确的顺序执行。

即使后台作业大多用于上传数据/创建提交，您也可以使用 run_as_future()来排队任何您喜欢的方法。例如，您可以使用它在后台创建一个存储库，然后上传数据到其中。上传方法中内置的`run_as_future`参数只是它的一个别名。

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> api.run_as_future(api.create_repo, "username/my-model", exists_ok=True)
Future(...)
>>> api.upload_file(
...     repo_id="username/my-model",
...     path_in_repo="file.txt",
...     path_or_fileobj=b"file content",
...     run_as_future=True,
... )
Future(...)
```

### 按块上传文件夹

upload_folder()使得将整个文件夹上传到 Hub 变得容易。然而，对于大型文件夹（数千个文件或数百 GB），仍然可能具有挑战性。如果您有一个包含大量文件的文件夹，您可能希望将其分批上传。如果在上传过程中遇到错误或连接问题，您将不必从头开始恢复该过程。

要将文件夹上传到多个提交中，只需将`multi_commits=True`作为参数传递。在幕后，`huggingface_hub`将列出要上传/删除的文件并将它们分成几个提交。“策略”（即如何分割提交）基于要上传的文件数量和大小。在 Hub 上打开了一个 PR 来推送所有提交。一旦 PR 准备就绪，提交将被压缩为单个提交。如果在完成之前中断了该过程，您可以重新运行脚本以恢复上传。创建的 PR 将被自动检测到，并且上传将从停止的地方继续。建议传递`multi_commits_verbose=True`以更好地了解上传及其进度。

下面的示例将把检查点文件夹上传到数据集中的多个提交中。在 Hub 上将创建一个 PR，并在上传完成后自动合并。如果您希望 PR 保持打开状态并手动审查它，可以传递`create_pr=True`。

```py
>>> upload_folder(
...     folder_path="local/checkpoints",
...     repo_id="username/my-dataset",
...     repo_type="dataset",
...     multi_commits=True,
...     multi_commits_verbose=True,
... )
```

如果您希望更好地控制上传策略（即创建的提交），您可以查看低级别的 plan_multi_commits()和 create_commits_on_pr()方法。

`multi_commits`仍然是一个实验性功能。其 API 和行为可能会在未来发生变化，恕不另行通知。

### 定期上传

Hugging Face Hub 使保存和版本化数据变得简单。然而，当更新同一文件数千次时会有一些限制。例如，您可能希望保存训练过程的日志或用户对部署空间的反馈。在这些情况下，将数据作为数据集上传到 Hub 是有意义的，但可能很难正确执行。主要原因是您不希望对数据的每次更新进行版本化，因为这会使 git 存储库无法使用。CommitScheduler 类提供了解决这个问题的方法。

这个想法是运行一个定期将本地文件夹推送到 Hub 的后台作业。假设您有一个 Gradio 空间，该空间以一些文本作为输入，并生成其两种翻译。然后，用户可以选择他们喜欢的翻译。对于每次运行，您希望保存输入、输出和用户偏好以分析结果。这是 CommitScheduler 的一个完美用例；您希望将数据保存到 Hub（可能是数百万条用户反馈），但您不*需要*实时保存每个用户的输入。相反，您可以将数据保存在本地的 JSON 文件中，并每 10 分钟上传一次。例如：

```py
>>> import json
>>> import uuid
>>> from pathlib import Path
>>> import gradio as gr
>>> from huggingface_hub import CommitScheduler

# Define the file where to save the data. Use UUID to make sure not to overwrite existing data from a previous run.
>>> feedback_file = Path("user_feedback/") / f"data_{uuid.uuid4()}.json"
>>> feedback_folder = feedback_file.parent

# Schedule regular uploads. Remote repo and local folder are created if they don't already exist.
>>> scheduler = CommitScheduler(
...     repo_id="report-translation-feedback",
...     repo_type="dataset",
...     folder_path=feedback_folder,
...     path_in_repo="data",
...     every=10,
... )

# Define the function that will be called when the user submits its feedback (to be called in Gradio)
>>> def save_feedback(input_text:str, output_1: str, output_2:str, user_choice: int) -> None:
...     """
...     Append input/outputs and user feedback to a JSON Lines file using a thread lock to avoid concurrent writes from different users.
...     """
...     with scheduler.lock:
...         with feedback_file.open("a") as f:
...             f.write(json.dumps({"input": input_text, "output_1": output_1, "output_2": output_2, "user_choice": user_choice}))
...             f.write("\n")

# Start Gradio
>>> with gr.Blocks() as demo:
>>>     ... # define Gradio demo + use `save_feedback`
>>> demo.launch()
```

就是这样！用户输入/输出和反馈将作为 Hub 上的数据集可用。通过使用唯一的 JSON 文件名，您可以确保不会覆盖以前运行的数据或同时推送到同一存储库的其他空间/副本的数据。

有关 CommitScheduler 的更多详细信息，请看以下内容：

+   **仅追加:** 假定您只会向文件夹添加内容。您只能向现有文件追加数据或创建新文件。删除或覆盖文件可能会损坏您的存储库。

+   **git 历史记录:** 调度程序将每`every`分钟提交一次文件夹。为了避免过多地污染 git 存储库，建议将最小值设置为 5 分钟。此外，调度程序旨在避免空提交。如果在文件夹中未检测到新内容，则计划的提交将被丢弃。

+   **错误:** 调度程序作为后台线程运行。当您实例化类时启动它，并且永远不会停止。特别是，如果在上传过程中发生错误（例如：连接问题），调度程序将默默地忽略它，并在下一个计划的提交时重试。

+   **线程安全:** 在大多数情况下，可以安全地假设可以写入文件而无需担心锁文件。如果在上传时向文件夹写入内容，调度程序不会崩溃或损坏。实际上，对于负载较重的应用程序，可能会发生并发问题。在这种情况下，我们建议使用`scheduler.lock`锁来确保线程安全。只有在调度程序扫描文件夹以查找更改时，锁才会被阻塞，而在上传数据时不会。您可以放心地认为它不会影响您的空间上的用户体验。

#### 空间持久性演示

从空间持久化数据到 Hub 上的数据集是 CommitScheduler 的主要用例。根据用例的不同，您可能希望以不同的方式结构化数据。结构必须对并发用户和重新启动具有鲁棒性，这通常意味着生成 UUID。除了鲁棒性，您还应该以🤗 Datasets 库可读的格式上传数据，以便以后重用。我们创建了一个[Space](https://huggingface.co/spaces/Wauplin/space_to_dataset_saver)，演示了如何保存几种不同的数据格式（您可能需要根据自己的特定需求进行调整）。

#### 自定义上传

CommitScheduler 假定您的数据是追加的，并且应该按原样上传。但是，您可能希望自定义数据上传方式。您可以通过创建一个从 CommitScheduler 继承的类，并覆盖`push_to_hub`方法（随意以任何方式覆盖它）来实现。保证它将在后台线程中每隔`every`分钟调用一次。您无需担心并发和错误，但必须注意其他方面，例如推送空提交或重复数据。

在下面（简化的）示例中，我们覆盖`push_to_hub`以将所有 PNG 文件压缩到单个存档中，以避免在 Hub 上过载存储库：

```py
class ZipScheduler(CommitScheduler):
    def push_to_hub(self):
        # 1\. List PNG files
          png_files = list(self.folder_path.glob("*.png"))
          if len(png_files) == 0:
              return None  # return early if nothing to commit

        # 2\. Zip png files in a single archive
        with tempfile.TemporaryDirectory() as tmpdir:
            archive_path = Path(tmpdir) / "train.zip"
            with zipfile.ZipFile(archive_path, "w", zipfile.ZIP_DEFLATED) as zip:
                for png_file in png_files:
                    zip.write(filename=png_file, arcname=png_file.name)

            # 3\. Upload archive
            self.api.upload_file(..., path_or_fileobj=archive_path)

        # 4\. Delete local png files to avoid re-uploading them later
        for png_file in png_files:
            png_file.unlink()
```

当覆盖`push_to_hub`时，您可以访问 CommitScheduler 的属性，特别是：

+   HfApi 客户端：`api`

+   文件夹参数：`folder_path`和`path_in_repo`

+   仓库参数：`repo_id`、`repo_type`、`revision`

+   线程锁：`lock`

查看我们的[演示空间](https://huggingface.co/spaces/Wauplin/space_to_dataset_saver)中的更多自定义调度程序示例，其中包含根据您的用例不同的不同实现。

### create_commit

upload_file()和 upload_folder()函数是通常方便使用的高级 API。如果您不需要在较低级别工作，我们建议首先尝试这些函数。但是，如果您想在提交级别工作，可以直接使用 create_commit()函数。

create_commit()支持三种类型的操作：

+   CommitOperationAdd 将文件上传到 Hub。如果文件已经存在，则文件内容将被覆盖。此操作接受两个参数：

    +   `path_in_repo`：要上传文件的存储库路径。

    +   `path_or_fileobj`：要上传到 Hub 的文件的路径或类似文件的对象。这是要上传到 Hub 的文件内容。

+   CommitOperationDelete 从存储库中删除文件或文件夹。此操作接受`path_in_repo`作为参数。

+   CommitOperationCopy 在存储库内复制文件。此操作接受三个参数：

    +   `src_path_in_repo`：要复制的文件的存储库路径。

    +   `path_in_repo`：应将文件复制到的存储库路径。

    +   `src_revision`：可选 - 如果要从不同的分支/修订版复制文件，则为要复制的文件的修订版。

例如，如果您想要在 Hub 存储库中上传两个文件并删除一个文件：

1.  使用适当的 `CommitOperation` 来添加或删除文件以及删除文件夹：

```py
>>> from huggingface_hub import HfApi, CommitOperationAdd, CommitOperationDelete
>>> api = HfApi()
>>> operations = [
...     CommitOperationAdd(path_in_repo="LICENSE.md", path_or_fileobj="~/repo/LICENSE.md"),
...     CommitOperationAdd(path_in_repo="weights.h5", path_or_fileobj="~/repo/weights-final.h5"),
...     CommitOperationDelete(path_in_repo="old-weights.h5"),
...     CommitOperationDelete(path_in_repo="logs/"),
...     CommitOperationCopy(src_path_in_repo="image.png", path_in_repo="duplicate_image.png"),
... ]
```

1.  将您的操作传递给 create_commit()：

```py
>>> api.create_commit(
...     repo_id="lysandre/test-model",
...     operations=operations,
...     commit_message="Upload my model weights and license",
... )
```

除了 upload_file() 和 upload_folder() 外，以下功能还在内部使用 create_commit()：

+   delete_file() 从 Hub 上的存储库中删除单个文件。

+   delete_folder() 从 Hub 上的存储库中删除整个文件夹。

+   metadata_update() 更新存储库的元数据。

有关更详细的信息，请查看 HfApi 参考。

### 在提交之前预先上传 LFS 文件

在某些情况下，您可能希望在进行提交调用之前将大型文件上传到 S3 **之前**。例如，如果您正在提交一个在内存中生成的数据集的多个分片，您需要逐个上传这些分片以避免内存不足问题。一种解决方案是将每个分片作为存储库上的单独提交上传。虽然这是完全有效的，但这种解决方案的缺点是可能通过生成数十个提交来搞乱 git 历史。为了解决这个问题，您可以逐个将文件上传到 S3，然后在最后创建一个单独的提交。这可以使用 preupload_lfs_files() 结合 create_commit() 来实现。

这是一种高级用户方法。在绝大多数情况下，直接使用 upload_file()、upload_folder() 或 create_commit() 而不是处理预先上传文件的低级逻辑是更好的选择。preupload_lfs_files() 的主要注意事项是，在实际进行提交之前，上传的文件在 Hub 上的存储库上是不可访问的。如果您有任何问题，请随时在我们的 Discord 或 GitHub 问题中联系我们。

以下是一个简单的示例，说明如何预先上传文件：

```py
>>> from huggingface_hub import CommitOperationAdd, preupload_lfs_files, create_commit, create_repo

>>> repo_id = create_repo("test_preupload").repo_id

>>> operations = [] # List of all `CommitOperationAdd` objects that will be generated
>>> for i in range(5):
...     content = ... # generate binary content
...     addition = CommitOperationAdd(path_in_repo=f"shard_{i}_of_5.bin", path_or_fileobj=content)
...     preupload_lfs_files(repo_id, additions=[addition])
...     operations.append(addition)

>>> # Create commit
>>> create_commit(repo_id, operations=operations, commit_message="Commit all shards")
```

首先，我们逐个创建 CommitOperationAdd 对象。在实际示例中，这些对象将包含生成的分片。在生成下一个文件之前，每个文件都会被上传。在 preupload_lfs_files() 步骤中，**`CommitOperationAdd` 对象会被改变**。您应该只使用它直接传递给 create_commit()。对象的主要更新是**二进制内容被从中删除**，这意味着如果您没有存储另一个引用，它将被垃圾回收。这是预期的，因为我们不希望在内存中保留已经上传的内容。最后，通过将所有操作传递给 create_commit() 来创建提交。您可以传递尚未处理的其他操作（添加、删除或复制），它们将被正确处理。

## 大文件上传的技巧和窍门

在处理存储库中的大量数据时，有一些限制需要注意。由于流式传输数据所需的时间，使得上传/推送在流程结束时失败或遇到降级体验，无论是在 hf.co 上还是在本地工作时，都可能非常恼人。

查看我们的 [存储库限制和建议](https://huggingface.co/docs/hub/repositories-recommendations) 指南，了解如何在 Hub 上构建存储库的最佳实践。接下来，让我们继续一些实用的提示，使您的上传过程尽可能顺利。

+   **从小开始**：我们建议从少量数据开始测试您的上传脚本。当失败只需要很少的时间时，更容易迭代脚本。

+   **期望失败**：流式传输大量数据是具有挑战性的。你不知道会发生什么，但最好始终考虑到至少会有一次失败 - 无论是由于您的机器、您的连接还是我们的服务器。例如，如果您计划上传大量文件，最好在上传下一批文件之前在本地跟踪您已经上传的文件。您可以确保已经提交的 LFS 文件永远不会被重新上传两次，但在客户端检查它仍然可以节省一些时间。

+   **使用 `hf_transfer`**：这是一个基于 Rust 的 [库](https://github.com/huggingface/hf_transfer)，旨在加快具有非常高带宽的机器上的上传速度。要使用它，您必须安装它（`pip install hf_transfer`）并通过将 `HF_HUB_ENABLE_HF_TRANSFER=1` 设置为环境变量来启用它。然后您可以正常使用 `huggingface_hub`。免责声明：这是一个面向高级用户的工具。它经过测试并已准备投入生产，但缺乏像高级错误处理或代理等用户友好的功能。有关更多详细信息，请参阅此 [部分](https://huggingface.co/docs/huggingface_hub/hf_transfer)。

进度条在 `hf_transfer` 版本 `0.1.4` 开始得到支持。如果您计划启用更快的上传，请考虑升级（`pip install -U hf-transfer`）。

## （传统）使用 Git LFS 上传文件

上述所有方法都使用 Hub 的 API 来上传文件。这是上传文件到 Hub 的推荐方式。但是，我们还提供 Repository，这是一个围绕 git 工具的包装器，用于管理本地存储库。

虽然 Repository 没有正式弃用，但我们建议使用上面描述的基于 HTTP 的方法。有关此建议的更多详细信息，请查看解释基于 HTTP 和基于 Git 方法之间核心差异的此指南。

Git LFS 会自动处理大于 10MB 的文件。但对于非常大的文件（>5GB），您需要为 Git LFS 安装自定义传输代理：

```py
huggingface-cli lfs-enable-largefiles
```

您应该为每个具有非常大文件的存储库安装此。安装后，您将能够推送大于 5GB 的文件。

### commit 上下文管理器

`commit`上下文管理器处理四个最常见的 Git 命令：pull、add、commit 和 push。`git-lfs`会自动跟踪任何大于 10MB 的文件。在下面的示例中，`commit`上下文管理器：

1.  从`text-files`存储库中拉取。

1.  添加对`file.txt`所做的更改。

1.  提交更改。

1.  将更改推送到`text-files`存储库。

```py
>>> from huggingface_hub import Repository
>>> with Repository(local_dir="text-files", clone_from="<user>/text-files").commit(commit_message="My first file :)"):
...     with open("file.txt", "w+") as f:
...         f.write(json.dumps({"hey": 8}))
```

以下是如何使用`commit`上下文管理器将文件保存并上传到存储库的另一个示例：

```py
>>> import torch
>>> model = torch.nn.Transformer()
>>> with Repository("torch-model", clone_from="<user>/torch-model", token=True).commit(commit_message="My cool model :)"):
...     torch.save(model.state_dict(), "model.pt")
```

如果要异步推送提交，请设置`blocking=False`。非阻塞行为在您希望在提交被推送时继续运行脚本时很有帮助。

```py
>>> with repo.commit(commit_message="My cool model :)", blocking=False)
```

您可以使用`command_queue`方法检查推送的状态：

```py
>>> last_command = repo.command_queue[-1]
>>> last_command.status
```

请参考下表以获取可能的状态：

| 状态 | 描述 |
| --- | --- |
| -1 | 推送正在进行中。 |
| 0 | 推送已成功完成。 |
| 非零 | 发生了错误。 |

当`blocking=False`时，命令会被跟踪，只有当所有推送完成时，您的脚本才会退出，即使在脚本中发生其他错误。用于检查推送状态的一些额外有用的命令包括：

```py
# Inspect an error.
>>> last_command.stderr

# Check whether a push is completed or ongoing.
>>> last_command.is_done

# Check whether a push command has errored.
>>> last_command.failed
```

### push_to_hub

Repository 类有一个 push_to_hub()函数，用于添加文件，进行提交并将它们推送到存储库。与`commit`上下文管理器不同，您需要先从存储库中拉取，然后才能调用 push_to_hub()。

例如，如果您已经从 Hub 克隆了一个存储库，那么您可以从本地目录初始化`repo`：

```py
>>> from huggingface_hub import Repository
>>> repo = Repository(local_dir="path/to/local/repo")
```

使用 git_pull()更新本地克隆，然后将文件推送到 Hub：

```py
>>> repo.git_pull()
>>> repo.push_to_hub(commit_message="Commit my-awesome-file to the Hub")
```

但是，如果您还没有准备好推送文件，您可以使用 git_add()和 git_commit()仅添加和提交您的文件：

```py
>>> repo.git_add("path/to/file")
>>> repo.git_commit(commit_message="add my first model config file :)")
```

准备好后，使用 git_push()将文件推送到您的存储库：

```py
>>> repo.git_push()
```
