# 将任何 ML 框架与 Hub 集成

> 原文链接：[https://huggingface.co/docs/huggingface_hub/guides/integrations](https://huggingface.co/docs/huggingface_hub/guides/integrations)

Hugging Face Hub 使得向社区托管和共享模型变得容易。它支持开源生态系统中的[数十个库](https://huggingface.co/docs/hub/models-libraries)。我们一直在努力扩大这种支持，推动协作机器学习的发展。`huggingface_hub` 库在这个过程中发挥着关键作用，使任何 Python 脚本都能轻松地推送和加载文件。

有四种主要方法可以将库与 Hub 集成：

1.  **推送到 Hub：** 实现一个方法将模型上传到 Hub。这包括模型权重，以及[模型卡片](https://huggingface.co/docs/huggingface_hub/how-to-model-cards)和运行模型所需的任何其他相关信息或数据（例如，训练日志）。这种方法通常被称为 `push_to_hub()`。

1.  **从 Hub 下载：** 实现一个方法从 Hub 加载模型。该方法应该下载模型配置/权重并加载模型。这种方法通常被称为 `from_pretrained` 或 `load_from_hub()`。

1.  **推理 API：** 使用我们的服务器免费运行由您的库支持的模型的推理。

1.  **小部件：** 在 Hub 上模型的主页上显示一个小部件。它允许用户快速从浏览器中尝试模型。

在本指南中，我们将专注于前两个主题。我们将介绍您可以使用的两种主要集成库的方法，以及它们的优点和缺点。在指南结束时，将总结所有内容，以帮助您在这两者之间做出选择。请记住，这些只是指导方针，您可以根据自己的需求自由调整。

如果您对推理和小部件感兴趣，可以查看[此指南](https://huggingface.co/docs/hub/models-adding-libraries#set-up-the-inference-api)。在这两种情况下，如果您正在将库与 Hub 集成并希望在[我们的文档](https://huggingface.co/docs/hub/models-libraries)中列出，请随时与我们联系。

## 灵活的方法：助手

将库集成到 Hub 的第一种方法是实际上通过自己实现 `push_to_hub` 和 `from_pretrained` 方法。这样可以完全灵活地确定需要上传/下载的文件以及如何处理特定于您的框架的输入。您可以参考两个[上传文件](./upload)和[下载文件](./download)指南，了解更多相关信息。例如，FastAI 集成是如何实现的（请参见 [push_to_hub_fastai()](/docs/huggingface_hub/v0.20.3/en/package_reference/mixins#huggingface_hub.push_to_hub_fastai) 和 [from_pretrained_fastai()](/docs/huggingface_hub/v0.20.3/en/package_reference/mixins#huggingface_hub.from_pretrained_fastai)）。

不同库之间的实现可能有所不同，但工作流程通常是相似的。

### from_pretrained

这是一个 `from_pretrained` 方法通常的样子：

```py
def from_pretrained(model_id: str) -> MyModelClass:
   # Download model from Hub
   cached_model = hf_hub_download(
      repo_id=repo_id,
      filename="model.pkl",
      library_name="fastai",
      library_version=get_fastai_version(),
   )

   # Load model
    return load_model(cached_model)
```

### push_to_hub

`push_to_hub` 方法通常需要更复杂的处理来处理仓库创建，生成模型卡片和保存权重。一个常见的方法是将所有这些文件保存在临时文件夹中，然后上传并删除它们。

```py
def push_to_hub(model: MyModelClass, repo_name: str) -> None:
   api = HfApi()

   # Create repo if not existing yet and get the associated repo_id
   repo_id = api.create_repo(repo_name, exist_ok=True)

   # Save all files in a temporary directory and push them in a single commit
   with TemporaryDirectory() as tmpdir:
      tmpdir = Path(tmpdir)

      # Save weights
      save_model(model, tmpdir / "model.safetensors")

      # Generate model card
      card = generate_model_card(model)
      (tmpdir / "README.md").write_text(card)

      # Save logs
      # Save figures
      # Save evaluation metrics
      # ...

      # Push to hub
      return api.upload_folder(repo_id=repo_id, folder_path=tmpdir)
```

当然，这只是一个例子。如果您对更复杂的操作感兴趣（删除远程文件，动态上传权重，将权重持久保存在本地等），请参考[上传文件](./upload)指南。

### 限制

虽然灵活，但这种方法也有一些缺点，特别是在维护方面。当使用 `huggingface_hub` 时，Hugging Face 用户通常习惯于在工作中使用额外的功能。例如，从 Hub 加载文件时，通常会提供参数，比如：

+   `token`：从私有仓库下载

+   `revision`：从特定分支下载

+   `cache_dir`：将文件缓存到特定目录

+   `force_download`/`resume_download`/`local_files_only`：重用缓存与否

+   `api_endpoint`/`proxies`: 配置HTTP会话

在推送模型时，支持类似的参数：

+   `commit_message`: 自定义提交消息

+   `private`: 如果缺失则创建私有仓库

+   `create_pr`: 创建PR而不是推送到`main`

+   `branch`: 推送到分支而不是`main`分支

+   `allow_patterns`/`ignore_patterns`: 过滤要上传的文件

+   `token`

+   `api_endpoint`

+   …

所有这些参数都可以添加到我们上面看到的实现中，并传递给`huggingface_hub`方法。但是，如果参数发生更改或添加了新功能，您将需要更新您的包。支持这些参数还意味着您需要在您的一侧维护更多的文档。要了解如何缓解这些限制，请跳转到我们的下一节**类继承**。

## 一个更复杂的方法：类继承

正如我们上面看到的，要将库与Hub集成，有两种主要方法：上传文件（`push_to_hub`）和下载文件（`from_pretrained`）。您可以自己实现这些方法，但会有一些注意事项。为了解决这个问题，`huggingface_hub`提供了一个使用类继承的工具。让我们看看它是如何工作的！

在许多情况下，一个库已经使用Python类实现了其模型。该类包含模型的属性和用于加载、运行、训练和评估的方法。我们的方法是通过使用mixin扩展此类来包含上传和下载功能。[Mixin](https://stackoverflow.com/a/547714)是一个旨在通过多重继承为现有类添加一组特定功能的类。`huggingface_hub`提供了自己的mixin，[ModelHubMixin](/docs/huggingface_hub/v0.20.3/en/package_reference/mixins#huggingface_hub.ModelHubMixin)。关键在于理解其行为以及如何自定义它。

[ModelHubMixin](/docs/huggingface_hub/v0.20.3/en/package_reference/mixins#huggingface_hub.ModelHubMixin)类实现了3个*公共*方法（`push_to_hub`、`save_pretrained`和`from_pretrained`）。这些是您的用户将调用的方法，用于使用您的库加载/保存模型。[ModelHubMixin](/docs/huggingface_hub/v0.20.3/en/package_reference/mixins#huggingface_hub.ModelHubMixin)还定义了2个*私有*方法（`_save_pretrained`和`_from_pretrained`）。这些是您必须实现的方法。因此，要集成您的库，您应该：

1.  使您的模型类从[ModelHubMixin](/docs/huggingface_hub/v0.20.3/en/package_reference/mixins#huggingface_hub.ModelHubMixin)继承。

1.  实现私有方法：

    +   [_save_pretrained()](/docs/huggingface_hub/v0.20.3/en/package_reference/mixins#huggingface_hub.ModelHubMixin._save_pretrained)：接受目录路径作为输入并将模型保存到其中的方法。您必须在此方法中编写所有将模型转储的逻辑：模型卡片、模型权重、配置文件、训练日志和图表。此模型的任何相关信息都必须由此方法处理。[模型卡片](https://huggingface.co/docs/hub/model-cards)特别重要，用于描述您的模型。查看[我们的实施指南](./model-cards)以获取更多详细信息。

    +   [_from_pretrained()](/docs/huggingface_hub/v0.20.3/en/package_reference/mixins#huggingface_hub.ModelHubMixin._from_pretrained)：**类方法**，接受`model_id`作为输入并返回一个实例化的模型。该方法必须下载相关文件并加载它们。

1.  您已经完成了！

使用[ModelHubMixin](/docs/huggingface_hub/v0.20.3/en/package_reference/mixins#huggingface_hub.ModelHubMixin)的优势在于一旦处理了文件的序列化/加载，您就可以立即开始。您无需担心仓库创建、提交、PR或修订等问题。所有这些都由mixin处理，并提供给您的用户。Mixin还确保公共方法有很好的文档和类型注释。

### 一个具体的例子：PyTorch

一个很好的例子是我们上面看到的[PyTorchModelHubMixin](/docs/huggingface_hub/v0.20.3/en/package_reference/mixins#huggingface_hub.PyTorchModelHubMixin)，这是针对PyTorch框架的集成。这是一个可直接使用的集成。

#### 如何使用？

以下是任何用户如何从/向Hub加载/保存PyTorch模型的方法：

```py
>>> import torch
>>> import torch.nn as nn
>>> from huggingface_hub import PyTorchModelHubMixin

# 1\. Define your Pytorch model exactly the same way you are used to
>>> class MyModel(nn.Module, PyTorchModelHubMixin): # multiple inheritance
...     def __init__(self):
...         super().__init__() 
...         self.param = nn.Parameter(torch.rand(3, 4))
...         self.linear = nn.Linear(4, 5)

...     def forward(self, x):
...         return self.linear(x + self.param)
>>> model = MyModel()

# 2\. (optional) Save model to local directory
>>> model.save_pretrained("path/to/my-awesome-model")

# 3\. Push model weights to the Hub
>>> model.push_to_hub("my-awesome-model")

# 4\. Initialize model from the Hub
>>> model = MyModel.from_pretrained("username/my-awesome-model")
```

#### 实施

实际上，这个实现非常简单，完整的实现可以在[这里](https://github.com/huggingface/huggingface_hub/blob/main/src/huggingface_hub/hub_mixin.py)找到。

1.  首先，从`ModelHubMixin`继承您的类：

```py
from huggingface_hub import ModelHubMixin

class PyTorchModelHubMixin(ModelHubMixin):
   (...)
```

1.  实现`_save_pretrained`方法：

```py
from huggingface_hub import ModelCard, ModelCardData

class PyTorchModelHubMixin(ModelHubMixin):
   (...)

   def _save_pretrained(self, save_directory: Path):
      """Generate Model Card and save weights from a Pytorch model to a local directory."""
      model_card = ModelCard.from_template(
         card_data=ModelCardData(
            license='mit',
            library_name="pytorch",
            ...
         ),
         model_summary=...,
         model_type=...,
         ...
      )
      (save_directory / "README.md").write_text(str(model))
      torch.save(obj=self.module.state_dict(), f=save_directory / "pytorch_model.bin")
```

1.  实现`_from_pretrained`方法：

```py
class PyTorchModelHubMixin(ModelHubMixin):
   (...)

 @classmethod # Must be a classmethod!
   def _from_pretrained( cls,
      *,
      model_id: str,
      revision: str,
      cache_dir: str,
      force_download: bool,
      proxies: Optional[Dict],
      resume_download: bool,
      local_files_only: bool,
      token: Union[str, bool, None],
      map_location: str = "cpu", # additional argument
      strict: bool = False, # additional argument
      **model_kwargs, ):
      """Load Pytorch pretrained weights and return the loaded model."""
      if os.path.isdir(model_id): # Can either be a local directory
         print("Loading weights from local directory")
         model_file = os.path.join(model_id, "pytorch_model.bin")
      else: # Or a model on the Hub
         model_file = hf_hub_download( # Download from the hub, passing same input args
            repo_id=model_id,
            filename="pytorch_model.bin",
            revision=revision,
            cache_dir=cache_dir,
            force_download=force_download,
            proxies=proxies,
            resume_download=resume_download,
            token=token,
            local_files_only=local_files_only,
         )

      # Load model and return - custom logic depending on your framework
      model = cls(**model_kwargs)
      state_dict = torch.load(model_file, map_location=torch.device(map_location))
      model.load_state_dict(state_dict, strict=strict)
      model.eval()
      return model
```

就是这样！您的库现在可以让用户上传和下载文件到Hub。

## 快速比较

让我们快速总结一下我们看到的两种方法及其优缺点。下表仅供参考。您的框架可能有一些特殊情况需要处理。本指南仅提供集成处理的指导和想法。无论如何，如果您有任何问题，请随时与我们联系！

| 集成 | 使用助手 | 使用[ModelHubMixin](/docs/huggingface_hub/v0.20.3/en/package_reference/mixins#huggingface_hub.ModelHubMixin) |
| :-: | :-: | :-: |
| 用户体验 | `model = load_from_hub(...)` `push_to_hub(model, ...)` | `model = MyModel.from_pretrained(...)` `model.push_to_hub(...)` |
| 灵活性 | 非常灵活。您完全控制实现。 | 不太灵活。您的框架必须有一个模型类。 |
| 维护 | 需要更多维护来添加对配置和新功能的支持。可能还需要修复用户报告的问题。 | 维护较少，因为大部分与Hub的交互已在`huggingface_hub`中实现。 |
| 文档/类型注释 | 需要手动编写。 | 部分由`huggingface_hub`处理。 |
