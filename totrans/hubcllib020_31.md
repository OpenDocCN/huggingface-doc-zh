# 混合和序列化方法

> 原始文本: [`huggingface.co/docs/huggingface_hub/package_reference/mixins`](https://huggingface.co/docs/huggingface_hub/package_reference/mixins)

## 混合

`huggingface_hub` 库提供了一系列可以用作对象父类的混合，以提供简单的上传和下载功能。查看我们的 集成指南 了解如何将任何 ML 框架与 Hub 集成。

### 通用

### `class huggingface_hub.ModelHubMixin`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hub_mixin.py#L21)

```py
( )
```

一个通用的混合，用于将任何机器学习框架与 Hub 集成。

要集成您的框架，您的模型类必须继承自此类。自定义保存/加载模型的逻辑必须在 `_from_pretrained` 和 `_save_pretrained` 中进行重写。PyTorchModelHubMixin 是与 Hub 集成的混合示例。查看我们的 集成指南 获取更多说明。

#### `_save_pretrained`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hub_mixin.py#L74)

```py
( save_directory: Path )
```

参数

+   `save_directory` (`str` 或 `Path`) — 模型权重和配置将保存在的目录路径。

在子类中重写此方法以定义如何保存模型。查看我们的 集成指南 获取说明。

`_from_pretrained`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hub_mixin.py#L169)

```py
( model_id: str revision: Optional cache_dir: Union force_download: bool proxies: Optional resume_download: bool local_files_only: bool token: Union **model_kwargs )
```

参数

+   `model_id` (`str`) — 从 Huggingface Hub 加载的模型的 ID（例如 `bigscience/bloom`）。

+   `revision` (`str`, *optional*) — Hub 上模型的修订版本。可以是分支名称、git 标签或任何提交 ID。默认为 `main` 分支上的最新提交。

+   `force_download` (`bool`, *optional*, 默认为 `False`) — 是否强制（重新）从 Hub 下载模型权重和配置文件，覆盖现有缓存。

+   `resume_download` (`bool`, *optional*, 默认为 `False`) — 是否删除接收不完整的文件。如果存在这样的文件，将尝试恢复下载。

+   `proxies` (`Dict[str, str]`, *optional*) — 用于按协议或端点使用的代理服务器字典（例如，`{'http': 'foo.bar:3128', 'http://hostname': 'foo.bar:4012'}`）。

+   `token` (`str` 或 `bool`, *optional*) — 用作远程文件的 HTTP bearer 授权的令牌。默认情况下，将使用运行 `huggingface-cli login` 时缓存的令牌。

+   `cache_dir` (`str`, `Path`, *optional*) — 存储缓存文件的文件夹路径。

+   `local_files_only` (`bool`, *optional*, 默认为 `False`) — 如果为 `True`，则避免下载文件，并返回本地缓存文件的路径（如果存在）。model_kwargs — 传递给 _from_pretrained() 方法的额外关键字参数。

在子类中重写此方法以定义如何从预训练加载模型。

使用 hf_hub_download() 或 snapshot_download() 在加载之前从 Hub 下载文件。大多数输入参数可以直接传递给这两种方法。如果需要，可以使用“model_kwargs”向此方法添加更多参数。例如 `PyTorchModelHubMixin._from_pretrained()` 接受一个 `map_location` 参数，用于设置模型应加载到哪个设备上。

查看我们的 集成指南 获取更多说明。

#### `from_pretrained`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hub_mixin.py#L85)

```py
( pretrained_model_name_or_path: Union force_download: bool = False resume_download: bool = False proxies: Optional = None token: Union = None cache_dir: Union = None local_files_only: bool = False revision: Optional = None **model_kwargs )
```

参数

+   `pretrained_model_name_or_path` (`str`, `Path`) —

    +   `model_id` (字符串) — 在 Hub 上托管的模型的 `model_id`，例如 `bigscience/bloom`。

    +   或者是指向包含使用 [save_pretrained](https://huggingface.co/docs/transformers/v4.36.2/en/main_classes/model#transformers.PreTrainedModel.save_pretrained) 保存的模型权重的 `directory` 的路径，例如 `../path/to/my_model_directory/`。

+   `revision` (`str`, *optional*) — 模型在 Hub 上的修订版本。可以是分支名称、git 标签或任何提交 ID。默认为 `main` 分支上的最新提交。

+   `force_download` (`bool`, *optional*, 默认为 `False`) — 是否强制（重新）从 Hub 下载模型权重和配置文件，覆盖现有缓存。

+   `resume_download` (`bool`, *optional*, 默认为 `False`) — 是否删除接收不完整的文件。如果存在这样的文件，将尝试恢复下载。

+   `proxies` (`Dict[str, str]`, *optional*) — 要在每个请求上使用的代理服务器的协议或端点的字典，例如 `{'http': 'foo.bar:3128', 'http://hostname': 'foo.bar:4012'}`。代理将用于每个请求。

+   `token` (`str` 或 `bool`, *optional*) — 用作远程文件的 HTTP bearer 授权的令牌。默认情况下，它将使用运行 `huggingface-cli login` 时缓存的令牌。

+   `cache_dir` (`str`, `Path`, *optional*) — 存储缓存文件的文件夹路径。

+   `local_files_only` (`bool`, *optional*, 默认为 `False`) — 如果为 `True`，则避免下载文件并返回本地缓存文件的路径（如果存在）。

+   `model_kwargs` (`Dict`, *optional*) — 传递给模型在初始化期间的额外 kwargs。

从 Huggingface Hub 下载模型并实例化。

#### `push_to_hub`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hub_mixin.py#L218)

```py
( repo_id: str config: Optional = None commit_message: str = 'Push model using huggingface_hub.' private: bool = False api_endpoint: Optional = None token: Optional = None branch: Optional = None create_pr: Optional = None allow_patterns: Union = None ignore_patterns: Union = None delete_patterns: Union = None )
```

参数

+   `repo_id` (`str`) — 要推送到的存储库的 ID（示例："username/my-model"）。

+   `config` (`dict`, *optional*) — 要与模型权重一起保存的配置对象。

+   `commit_message` (`str`, *optional*) — 推送时要提交的消息。

+   `private` (`bool`, *optional*, 默认为 `False`) — 创建的存储库是否应为私有。

+   `api_endpoint` (`str`, *optional*) — 推送模型到 Hub 时要使用的 API 端点。

+   `token` (`str`, *optional*) — 用作远程文件的 HTTP bearer 授权的令牌。默认情况下，它将使用运行 `huggingface-cli login` 时缓存的令牌。

+   `branch` (`str`, *optional*) — 要推送模型的 git 分支。默认为 `"main"`。

+   `create_pr` (`boolean`, *optional*) — 是否从 `branch` 创建一个 Pull Request。默认为 `False`。

+   `allow_patterns` (`List[str]` 或 `str`, *optional*) — 如果提供，只有至少匹配一个模式的文件才会被推送。

+   `ignore_patterns` (`List[str]` 或 `str`, *optional*) — 如果提供，与任何模式匹配的文件将不会被推送。

+   `delete_patterns` (`List[str]` 或 `str`, *optional*) — 如果提供，与任何模式匹配的远程文件将从存储库中删除。

上传模型检查点到 Hub。

使用 `allow_patterns` 和 `ignore_patterns` 来精确过滤应推送到 Hub 的文件。使用 `delete_patterns` 来删除同一提交中的现有远程文件。有关更多详细信息，请参阅 upload_folder() 参考。

#### `save_pretrained`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hub_mixin.py#L30)

```py
( save_directory: Union config: Optional = None repo_id: Optional = None push_to_hub: bool = False **kwargs )
```

参数

+   `save_directory` (`str` 或 `Path`) — 将保存模型权重和配置的目录路径。

+   `config` (`dict`, *optional*) — 指定为键/值字典的模型配置。

+   `push_to_hub`（`bool`，*可选*，默认为`False`）— 是否在保存后将模型推送到 Huggingface Hub。

+   `repo_id`（`str`，*可选*）— Hub 上您的存储库的 ID。仅在`push_to_hub=True`时使用。如果未提供，将默认为文件夹名称。kwargs — 传递给 push_to_hub()方法的其他关键字参数。

在本地目录中保存权重。

### PyTorch

### `class huggingface_hub.PyTorchModelHubMixin`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hub_mixin.py#L290)

```py
( )
```

实现了 ModelHubMixin，为 PyTorch 模型提供模型 Hub 上传/下载功能。默认情况下，模型处于评估模式，使用`model.eval()`（关闭了 dropout 模块）。要训练模型，您应该首先使用`model.train()`将其设置回训练模式。

示例：

```py
>>> import torch
>>> import torch.nn as nn
>>> from huggingface_hub import PyTorchModelHubMixin

>>> class MyModel(nn.Module, PyTorchModelHubMixin):
...     def __init__(self):
...         super().__init__()
...         self.param = nn.Parameter(torch.rand(3, 4))
...         self.linear = nn.Linear(4, 5)

...     def forward(self, x):
...         return self.linear(x + self.param)
>>> model = MyModel()

# Save model weights to local directory
>>> model.save_pretrained("my-awesome-model")

# Push model weights to the Hub
>>> model.push_to_hub("my-awesome-model")

# Download and initialize weights from the Hub
>>> model = MyModel.from_pretrained("username/my-awesome-model")
```

### Keras

### `class huggingface_hub.KerasModelHubMixin`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/keras_mixin.py#L397)

```py
( )
```

实现了 ModelHubMixin，为 Keras 模型提供模型 Hub 上传/下载功能。

```py
>>> import tensorflow as tf
>>> from huggingface_hub import KerasModelHubMixin

>>> class MyModel(tf.keras.Model, KerasModelHubMixin):
...     def __init__(self, **kwargs):
...         super().__init__()
...         self.config = kwargs.pop("config", None)
...         self.dummy_inputs = ...
...         self.layer = ...

...     def call(self, *args):
...         return ...

>>> # Initialize and compile the model as you normally would
>>> model = MyModel()
>>> model.compile(...)
>>> # Build the graph by training it or passing dummy inputs
>>> _ = model(model.dummy_inputs)
>>> # Save model weights to local directory
>>> model.save_pretrained("my-awesome-model")
>>> # Push model weights to the Hub
>>> model.push_to_hub("my-awesome-model")
>>> # Download and initialize weights from the Hub
>>> model = MyModel.from_pretrained("username/super-cool-model")
```

#### `huggingface_hub.from_pretrained_keras`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/keras_mixin.py#L216)

```py
( *args **kwargs )
```

参数

+   `pretrained_model_name_or_path`（`str`或`os.PathLike`）— 可以是：

    +   一个字符串，预训练模型的`模型 id`，托管在 huggingface.co 上的模型仓库内。有效的模型 id 可以位于根级别，如`bert-base-uncased`，或者在用户或组织名称下命名空间，如`dbmdz/bert-base-german-cased`。

    +   您可以通过简单地在模型 id 末尾添加`@`来添加`revision`，如此：`dbmdz/bert-base-german-cased@main`。修订版是要使用的特定模型版本。它可以是分支名称、标签名称或提交 ID，因为我们在 huggingface.co 上使用基于 git 的系统存储模型和其他工件，所以`revision`可以是 git 允许的任何标识符。

    +   一个包含使用[save_pretrained](https://huggingface.co/docs/transformers/v4.36.2/en/main_classes/model#transformers.PreTrainedModel.save_pretrained)保存的模型权重的`目录`路径，例如，`./my_model_directory/`。

    +   如果您同时提供配置和状态字典（分别使用关键字参数`config`和`state_dict`），则为`None`。

+   `force_download`（`bool`，*可选*，默认为`False`）— 是否强制下载模型权重和配置文件，覆盖缓存版本（如果存在）。

+   `resume_download`（`bool`，*可选*，默认为`False`）— 是否删除接收不完整的文件。如果存在这样的文件，将尝试恢复下载。

+   `proxies`（`Dict[str, str]`，*可选*）— 一个代理服务器字典，按协议或端点使用，例如，`{'http': 'foo.bar:3128', 'http://hostname': 'foo.bar:4012'}`。这些代理在每个请求中使用。

+   `token`（`str`或`bool`，*可选*）— 用作远程文件 HTTP bearer 授权的令牌。如果为`True`，将使用运行`transformers-cli login`时生成的令牌（存储在`~/.huggingface`）。

+   `cache_dir`（`Union[str, os.PathLike]`，*可选*）— 预训练模型配置应该缓存在其中的目录路径，如果不使用标准缓存。

+   `local_files_only(bool,` *可选*，默认为`False`) — 是否仅查看本地文件（即，不尝试下载模型）。

+   `model_kwargs`（`Dict`，*可选*）— model_kwargs 将在初始化期间传递给模型

从 Hub 中实例化一个预训练的 Keras 模型。预期模型以`SavedModel`格式存在。

当您想使用私有模型时，需要传递`token=True`。

#### `huggingface_hub.push_to_hub_keras`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/keras_mixin.py#L275)

```py
( model repo_id: str config: Optional = None commit_message: str = 'Push Keras model using huggingface_hub.' private: bool = False api_endpoint: Optional = None token: Optional = None branch: Optional = None create_pr: Optional = None allow_patterns: Union = None ignore_patterns: Union = None delete_patterns: Union = None log_dir: Optional = None include_optimizer: bool = False tags: Union = None plot_model: bool = True **model_save_kwargs )
```

参数

+   `model` (`Keras.Model`) — 您想要推送到 Hub 的 Keras 模型。模型必须已编译和构建。

+   `repo_id` (`str`) — 要推送到的存储库的 ID（示例："username/my-model"）。

+   `commit_message` (`str`, *optional*, defaults to “Add Keras model”) — 推送时要提交的消息。

+   `private` (`bool`, *optional*, defaults to `False`) — 是否应该创建私有存储库。

+   `api_endpoint` (`str`, *optional*) — 推送模型到 Hub 时要使用的 API 端点。

+   `token` (`str`, *optional*) — 用作远程文件 HTTP bearer 授权的令牌。如果未设置，将使用使用`huggingface-cli login`登录时设置的令牌（存储在`~/.huggingface`中）。

+   `branch` (`str`, *optional*) — 要推送模型的 git 分支。这默认为存储库中指定的默认分支，即`"main"`。

+   `create_pr` (`boolean`, *optional*) — 是否从`branch`创建一个 Pull Request。默认为`False`。

+   `config` (`dict`, *optional*) — 要与模型权重一起保存的配置对象。

+   `allow_patterns` (`List[str]` or `str`, *optional*) — 如果提供，只有匹配至少一个模式的文件才会被推送。

+   `ignore_patterns` (`List[str]` or `str`, *optional*) — 如果提供，不推送与任何模式匹配的文件。

+   `delete_patterns` (`List[str]` or `str`, *optional*) — 如果提供，与任何模式匹配的远程文件将从存储库中删除。

+   `log_dir` (`str`, *optional*) — 要推送的 TensorBoard 日志目录。如果存储库中包含日志文件，则 Hub 将自动托管和显示 TensorBoard 实例。

+   `include_optimizer` (`bool`, *optional*, defaults to `False`) — 是否在序列化过程中包含优化器。

+   `tags` (Union[`list`, `str`], *optional*) — 与模型相关的标签列表或单个标签的字符串。请参见[此处](https://github.com/huggingface/hub-docs/blob/main/modelcard.md?plain=1)的示例标签。

+   `plot_model` (`bool`, *optional*, defaults to `True`) — 将此设置为`True`将绘制模型并将其放入模型卡中。需要安装 graphviz 和 pydot。

+   `model_save_kwargs(dict,` *optional*) — model_save_kwargs 将传递给[`tf.keras.models.save_model()`](https://www.tensorflow.org/api_docs/python/tf/keras/models/save_model)。

将模型检查点上传到 Hub。

使用`allow_patterns`和`ignore_patterns`来精确过滤应推送到 Hub 的文件。使用`delete_patterns`在同一次提交中删除现有的远程文件。有关更多详细信息，请参阅 upload_folder()参考。

#### `huggingface_hub.save_pretrained_keras`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/keras_mixin.py#L131)

```py
( model save_directory: Union config: Optional = None include_optimizer: bool = False plot_model: bool = True tags: Union = None **model_save_kwargs )
```

参数

+   `model` (`Keras.Model`) — 您想要保存的[Keras 模型](https://www.tensorflow.org/api_docs/python/tf/keras/Model)。模型必须已编译和构建。

+   `save_directory` (`str` or `Path`) — 指定要保存 Keras 模型的目录。

+   `config` (`dict`, *optional*) — 要与模型权重一起保存的配置对象。

+   `include_optimizer(bool,` *optional*, defaults to `False`) — 是否在序列化过程中包含优化器。

+   `plot_model` (`bool`, *optional*, defaults to `True`) — 将此设置为`True`将绘制模型并将其放入模型卡中。需要安装 graphviz 和 pydot。

+   `tags` (Union[`str`,`list`], *optional*) — 与模型相关的标签列表或单个标签的字符串。请参见[此处](https://github.com/huggingface/hub-docs/blob/main/modelcard.md?plain=1)的示例标签。

+   `model_save_kwargs(dict,` *optional*) — model_save_kwargs 将传递给[`tf.keras.models.save_model()`](https://www.tensorflow.org/api_docs/python/tf/keras/models/save_model)。

将 Keras 模型保存到 SavedModel 格式的 save_directory 中。如果您使用 Functional 或 Sequential API，则使用此选项。

### Fastai

#### `huggingface_hub.from_pretrained_fastai`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/fastai_utils.py#L298)

```py
( repo_id: str revision: Optional = None )
```

参数

+   `repo_id` (`str`) — pickled fastai.Learner 所在的位置。可以是以下两者之一：

    +   托管在 Hugging Face Hub 上。例如：'espejelomar/fatai-pet-breeds-classification'或'distilgpt2'。您可以通过在`repo_id`末尾添加`@`来添加`revision`。例如：`dbmdz/bert-base-german-cased@main`。Revision 是要使用的特定模型版本。由于我们在 Hugging Face Hub 上使用基于 git 的系统来存储模型和其他工件，因此它可以是分支名称、标签名称或提交 ID。

    +   本地托管。`repo_id`将是包含 pickle 文件和指示用于构建`fastai.Learner`的 fastai 和 fastcore 版本的 pyproject.toml 的目录。例如：`./my_model_directory/`。

+   `revision` (`str`, *optional*) — 下载存储库文件的修订版本。请参阅`snapshot_download`的文档。

从 Hub 或本地目录加载预训练的 fastai 模型。

#### `huggingface_hub.push_to_hub_fastai`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/fastai_utils.py#L343)

```py
( learner repo_id: str commit_message: str = 'Push FastAI model using huggingface_hub.' private: bool = False token: Optional = None config: Optional = None branch: Optional = None create_pr: Optional = None allow_patterns: Union = None ignore_patterns: Union = None delete_patterns: Union = None api_endpoint: Optional = None )
```

参数

+   `learner` (*Learner*) — 您想要推送到 Hub 的*fastai.Learner*。

+   `repo_id` (*str*) — Hub 中您模型的存储库 ID 的格式为“namespace/repo_name”。命名空间可以是您的个人帐户或您具有写入访问权限的组织（例如，'stanfordnlp/stanza-de'）。

+   `commit_message` (*str`,* optional*) — 推送时要提交的消息。默认为`"add model"`。

+   `private` (*bool*, *optional*, 默认为*False*) — 创建的存储库是否应为私有。

+   `token` (*str*, *optional*) — 用作远程文件的 HTTP bearer 授权的 Hugging Face 帐户令牌。如果为`None`，则将通过提示询问令牌。

+   `config` (*dict*, *optional*) — 要与模型权重一起保存的配置对象。

+   `branch` (*str*, *optional*) — 推送模型的 git 分支。默认为存储库中指定的默认分支，默认为*“main”*。

+   `create_pr` (*boolean*, *optional*) — 是否从*branch*创建具有该提交的 Pull Request。默认为*False*。

+   `api_endpoint` (*str*, *optional*) — 用于将模型推送到 hub 时使用的 API 端点。

+   `allow_patterns` (*List[str]* or *str*, *optional*) — 如果提供，只有至少匹配一个模式的文件才会被推送。

+   `ignore_patterns` (*List[str]* or *str*, *optional*) — 如果提供，与任何模式匹配的文件将不会被推送。

+   `delete_patterns` (*List[str]* or *str*, *optional*) — 如果提供，与任何模式匹配的远程文件将从存储库中删除。

将学习器检查点文件上传到 Hub。

使用*allow_patterns*和*ignore_patterns*来精确过滤应推送到 hub 的文件。使用*delete_patterns*来删除同一提交中的现有远程文件。有关更多详细信息，请参阅[*upload_folder*]参考。

引发以下错误：

+   [*ValueError*](https://docs.python.org/3/library/exceptions.html#ValueError) 如果用户未登录到 Hugging Face Hub。
