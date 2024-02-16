# 管理您的空间

> 原始文本：[`huggingface.co/docs/huggingface_hub/guides/manage-spaces`](https://huggingface.co/docs/huggingface_hub/guides/manage-spaces)

在本指南中，我们将看到如何使用`huggingface_hub`管理您的空间运行时（[秘密](https://huggingface.co/docs/hub/spaces-overview#managing-secrets)、[硬件](https://huggingface.co/docs/hub/spaces-gpus)和[存储](https://huggingface.co/docs/hub/spaces-storage#persistent-storage)）。

## 一个简单的例子：配置秘密和硬件。

以下是一个端到端的示例，用于在 Hub 上创建和设置一个空间。

**1\. 在 Hub 上创建一个空间。**

```py
>>> from huggingface_hub import HfApi
>>> repo_id = "Wauplin/my-cool-training-space"
>>> api = HfApi()

# For example with a Gradio SDK
>>> api.create_repo(repo_id=repo_id, repo_type="space", space_sdk="gradio")
```

**1\. （bis）复制一个空间。**

如果您想要从现有空间构建而不是从头开始，这可能会很有用。如果您想要控制公共空间的配置/设置，这也很有用。有关更多详细信息，请参阅 duplicate_space()。

```py
>>> api.duplicate_space("multimodalart/dreambooth-training")
```

**2\. 使用您喜欢的解决方案上传您的代码。**

以下是一个示例，将您机器上的本地文件夹`src/`上传到您的空间：

```py
>>> api.upload_folder(repo_id=repo_id, repo_type="space", folder_path="src/")
```

在这一步，您的应用程序应该已经在 Hub 上免费运行！但是，您可能希望进一步配置它，使用秘密和升级后的硬件。

**3\. 配置秘密和变量**

您的空间可能需要一些秘密密钥、令牌或变量才能正常工作。有关更多详细信息，请参阅[文档](https://huggingface.co/docs/hub/spaces-overview#managing-secrets)。例如，一次生成 HF 令牌后，将图像数据集上传到 Hub。

```py
>>> api.add_space_secret(repo_id=repo_id, key="HF_TOKEN", value="hf_api_***")
>>> api.add_space_variable(repo_id=repo_id, key="MODEL_REPO_ID", value="user/repo")
```

秘密和变量也可以被删除：

```py
>>> api.delete_space_secret(repo_id=repo_id, key="HF_TOKEN")
>>> api.delete_space_variable(repo_id=repo_id, key="MODEL_REPO_ID")
```

在您的空间内，秘密作为环境变量可用（如果使用 Streamlit，则为 Streamlit Secrets Management）。无需通过 API 获取它们！对空间配置（秘密或硬件）的任何更改都将触发应用程序的重新启动。

**奖励：在创建或复制空间时设置秘密和变量！**

在创建或复制空间时可以设置秘密和变量：

```py
>>> api.create_repo(
...     repo_id=repo_id,
...     repo_type="space",
...     space_sdk="gradio",
...     space_secrets=[{"key"="HF_TOKEN", "value"="hf_api_***"}, ...],
...     space_variables=[{"key"="MODEL_REPO_ID", "value"="user/repo"}, ...],
... )
```

```py
>>> api.duplicate_space(
...     from_id=repo_id,
...     secrets=[{"key"="HF_TOKEN", "value"="hf_api_***"}, ...],
...     variables=[{"key"="MODEL_REPO_ID", "value"="user/repo"}, ...],
... )
```

**4\. 配置硬件**

默认情况下，您的空间将免费在 CPU 环境上运行。您可以升级硬件以在 GPU 上运行。访问升级您的空间需要支付卡或社区资助。有关更多详细信息，请参阅[文档](https://huggingface.co/docs/hub/spaces-gpus)。

```py
# Use `SpaceHardware` enum
>>> from huggingface_hub import SpaceHardware
>>> api.request_space_hardware(repo_id=repo_id, hardware=SpaceHardware.T4_MEDIUM)

# Or simply pass a string value
>>> api.request_space_hardware(repo_id=repo_id, hardware="t4-medium")
```

硬件更新不会立即完成，因为您的空间必须重新加载到我们的服务器上。您可以随时检查您的空间正在运行的硬件，以查看您的请求是否已得到满足。

```py
>>> runtime = api.get_space_runtime(repo_id=repo_id)
>>> runtime.stage
"RUNNING_BUILDING"
>>> runtime.hardware
"cpu-basic"
>>> runtime.requested_hardware
"t4-medium"
```

您现在拥有一个完全配置的空间。确保在使用完毕后将您的空间降级为“cpu-classic”。

**奖励：在创建或复制空间时请求硬件！**

升级后的硬件将自动分配给您的空间一旦建立。

```py
>>> api.create_repo(
...     repo_id=repo_id,
...     repo_type="space",
...     space_sdk="gradio"
...     space_hardware="cpu-upgrade",
...     space_storage="small",
...     space_sleep_time="7200", # 2 hours in secs
... )
```

```py
>>> api.duplicate_space(
...     from_id=repo_id,
...     hardware="cpu-upgrade",
...     storage="small",
...     sleep_time="7200", # 2 hours in secs
... )
```

**5\. 暂停和重新启动您的空间**

默认情况下，如果您的空间正在升级的硬件上运行，它将永远不会停止。但是为了避免计费，当您不使用它时，您可能希望将其暂停。这可以使用 pause_space()来实现。暂停的空间将在空间所有者重新启动它之前处于非活动状态，可以使用 UI 或通过 API 使用 restart_space()重新启动。有关暂停模式的更多详细信息，请参考[此部分](https://huggingface.co/docs/hub/spaces-gpus#pause)

```py
# Pause your Space to avoid getting billed
>>> api.pause_space(repo_id=repo_id)
# (...)
# Restart it when you need it
>>> api.restart_space(repo_id=repo_id)
```

另一种可能性是为您的空间设置超时。如果您的空间在超时持续时间内处于非活动状态，它将进入睡眠状态。任何访问您的空间的访客都将重新启动它。您可以使用 set_space_sleep_time()设置超时。有关睡眠模式的更多详细信息，请参考[此部分](https://huggingface.co/docs/hub/spaces-gpus#sleep-time)。

```py
# Put your Space to sleep after 1h of inactivity
>>> api.set_space_sleep_time(repo_id=repo_id, sleep_time=3600)
```

注意：如果您使用的是“cpu-basic”硬件，则无法配置自定义睡眠时间。您的空间将在 48 小时不活动后自动暂停。

**奖励：在请求硬件时设置睡眠时间**

升级后的硬件将自动分配给您的空间一旦建立。

```py
>>> api.request_space_hardware(repo_id=repo_id, hardware=SpaceHardware.T4_MEDIUM, sleep_time=3600)
```

**奖励：在创建或复制空间时设置睡眠时间！**

```py
>>> api.create_repo(
...     repo_id=repo_id,
...     repo_type="space",
...     space_sdk="gradio"
...     space_hardware="t4-medium",
...     space_sleep_time="3600",
... )
```

```py
>>> api.duplicate_space(
...     from_id=repo_id,
...     hardware="t4-medium",
...     sleep_time="3600",
... )
```

**6. 为您的空间添加持久存储**

您可以选择您喜欢的存储层级以访问跨空间重新启动时持久存在的磁盘空间。这意味着您可以像使用传统硬盘驱动器一样从磁盘读取和写入数据。有关更多详细信息，请参阅[文档](https://huggingface.co/docs/hub/spaces-storage#persistent-storage)。

```py
>>> from huggingface_hub import SpaceStorage
>>> api.request_space_storage(repo_id=repo_id, storage=SpaceStorage.LARGE)
```

您还可以删除您的存储，永久丢失所有数据。

```py
>>> api.delete_space_storage(repo_id=repo_id)
```

注意：一旦授予存储层级，您就无法降低空间的存储层级。要这样做，您必须首先删除存储，然后请求新的所需层级。

**奖励：在创建或复制空间时请求存储！**

```py
>>> api.create_repo(
...     repo_id=repo_id,
...     repo_type="space",
...     space_sdk="gradio"
...     space_storage="large",
... )
```

```py
>>> api.duplicate_space(
...     from_id=repo_id,
...     storage="large",
... )
```

## 更高级：临时升级您的空间！

空间允许进行许多不同的用例。有时，您可能希望在特定硬件上临时运行一个空间，执行某些操作，然后关闭它。在本节中，我们将探讨如何利用空间根据需要微调模型。这只是解决这个特定问题的一种方式。必须将其视为建议并根据您的用例进行调整。

假设我们有一个用于微调模型的空间。这是一个 Gradio 应用程序，它以模型 ID 和数据集 ID 作为输入。工作流程如下：

1.  （提示用户输入模型和数据集）

1.  从 Hub 加载模型。

1.  从 Hub 加载数据集。

1.  在数据集上微调模型。

1.  将新模型上传到 Hub。

第 3 步需要自定义硬件，但您不希望您的空间一直在付费 GPU 上运行。一个解决方案是动态请求训练硬件，然后在训练后关闭它。由于请求硬件会重新启动您的空间，因此您的应用程序必须以某种方式“记住”它正在执行的当前任务。有多种方法可以做到这一点。在本指南中，我们将看到一种使用数据集作为“任务调度器”的解决方案。

### 应用程序骨架

这是您的应用程序的外观。在启动时，检查是否已安排任务，如果是，则在正确的硬件上运行它。完成后，将硬件设置回免费计划的 CPU，并提示用户进行新任务。

这样的工作流程不支持像正常演示那样的并发访问。特别是在进行训练时，界面将被禁用。最好将您的存储库设置为私有，以确保您是唯一的用户。

```py
# Space will need your token to request hardware: set it as a Secret !
HF_TOKEN = os.environ.get("HF_TOKEN")

# Space own repo_id
TRAINING_SPACE_ID = "Wauplin/dreambooth-training"

from huggingface_hub import HfApi, SpaceHardware
api = HfApi(token=HF_TOKEN)

# On Space startup, check if a task is scheduled. If yes, finetune the model. If not,
# display an interface to request a new task.
task = get_task()
if task is None:
    # Start Gradio app
    def gradio_fn(task):
        # On user request, add task and request hardware
        add_task(task)
        api.request_space_hardware(repo_id=TRAINING_SPACE_ID, hardware=SpaceHardware.T4_MEDIUM)

    gr.Interface(fn=gradio_fn, ...).launch()
else:
    runtime = api.get_space_runtime(repo_id=TRAINING_SPACE_ID)
    # Check if Space is loaded with a GPU.
    if runtime.hardware == SpaceHardware.T4_MEDIUM:
        # If yes, finetune base model on dataset !
        train_and_upload(task)

        # Then, mark the task as "DONE"
        mark_as_done(task)

        # DO NOT FORGET: set back CPU hardware
        api.request_space_hardware(repo_id=TRAINING_SPACE_ID, hardware=SpaceHardware.CPU_BASIC)
    else:
        api.request_space_hardware(repo_id=TRAINING_SPACE_ID, hardware=SpaceHardware.T4_MEDIUM)
```

### 任务调度器

可以通过许多方式进行任务调度。以下是使用简单的 CSV 存储为数据集执行此操作的示例。

```py
# Dataset ID in which a `tasks.csv` file contains the tasks to perform.
# Here is a basic example for `tasks.csv` containing inputs (base model and dataset)
# and status (PENDING or DONE).
#     multimodalart/sd-fine-tunable,Wauplin/concept-1,DONE
#     multimodalart/sd-fine-tunable,Wauplin/concept-2,PENDING
TASK_DATASET_ID = "Wauplin/dreambooth-task-scheduler"

def _get_csv_file():
    return hf_hub_download(repo_id=TASK_DATASET_ID, filename="tasks.csv", repo_type="dataset", token=HF_TOKEN)

def get_task():
    with open(_get_csv_file()) as csv_file:
        csv_reader = csv.reader(csv_file, delimiter=',')
        for row in csv_reader:
            if row[2] == "PENDING":
                return row[0], row[1] # model_id, dataset_id

def add_task(task):
    model_id, dataset_id = task
    with open(_get_csv_file()) as csv_file:
        with open(csv_file, "r") as f:
            tasks = f.read()

    api.upload_file(
        repo_id=repo_id,
        repo_type=repo_type,
        path_in_repo="tasks.csv",
        # Quick and dirty way to add a task
        path_or_fileobj=(tasks + f"\n{model_id},{dataset_id},PENDING").encode()
    )

def mark_as_done(task):
    model_id, dataset_id = task
    with open(_get_csv_file()) as csv_file:
        with open(csv_file, "r") as f:
            tasks = f.read()

    api.upload_file(
        repo_id=repo_id,
        repo_type=repo_type,
        path_in_repo="tasks.csv",
        # Quick and dirty way to set the task as DONE
        path_or_fileobj=tasks.replace(
            f"{model_id},{dataset_id},PENDING",
            f"{model_id},{dataset_id},DONE"
        ).encode()
    )
```
