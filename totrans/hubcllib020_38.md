# 仓库卡片

> 原文：[`huggingface.co/docs/huggingface_hub/package_reference/cards`](https://huggingface.co/docs/huggingface_hub/package_reference/cards)

huggingface_hub 库提供了一个 Python 接口，用于创建、共享和更新模型/数据集卡片。访问[专门的文档页面](https://huggingface.co/docs/hub/models-cards)深入了解 Hub 上的模型卡片是什么，以及它们在幕后是如何工作的。您还可以查看我们的模型卡片指南了解如何在自己的项目中使用这些实用程序。

## 仓库卡片

`RepoCard` 对象是 ModelCard、DatasetCard 和 `SpaceCard` 的父类。

### `class huggingface_hub.RepoCard`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L35)

```py
( content: str ignore_metadata_errors: bool = False )
```

#### `__init__`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L40)

```py
( content: str ignore_metadata_errors: bool = False )
```

参数

+   `content` (`str`) — Markdown 文件的内容。

从字符串内容初始化 RepoCard。内容应为带有 YAML 块开头和 Markdown 主体的 Markdown 文件。

示例：

```py
>>> from huggingface_hub.repocard import RepoCard
>>> text = '''
... ---
... language: en
... license: mit
... ---
...
... # My repo
... '''
>>> card = RepoCard(text)
>>> card.data.to_dict()
{'language': 'en', 'license': 'mit'}
>>> card.text
'\n# My repo\n'

```

引发以下错误：

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) 当仓库卡片元数据的内容不是字典时。

#### `from_template`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L290)

```py
( card_data: CardData template_path: Optional = None **template_kwargs ) → export const metadata = 'undefined';huggingface_hub.repocard.RepoCard
```

参数

+   `card_data` (`huggingface_hub.CardData`) — 包含要包含在 Hugging Face Hub 上仓库卡片 YAML 头部中的元数据的 huggingface_hub.CardData 实例。

+   `template_path` (`str`, *可选*) — 一个带有可选 Jinja 模板变量的 markdown 文件路径，可以使用 `template_kwargs` 填充。默认为默认模板。

返回

huggingface_hub.repocard.RepoCard

具有指定卡片数据和模板内容的 RepoCard 实例。

从模板初始化 RepoCard。默认情况下，它使用默认模板。

模板是可以通过传递关键字参数进行自定义的 Jinja2 模板。

#### `load`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L134)

```py
( repo_id_or_path: Union repo_type: Optional = None token: Optional = None ignore_metadata_errors: bool = False ) → export const metadata = 'undefined';huggingface_hub.repocard.RepoCard
```

参数

+   `repo_id_or_path` (`Union[str, Path]`) — 与 Hugging Face Hub 仓库关联的仓库 ID 或本地文件路径。

+   `repo_type` (`str`, *可选*) — 要推送到的 Hugging Face 仓库的类型。默认为 None，将使用“model”。其他选项是“dataset”和“space”。在从本地文件路径加载时不使用。如果从子类调用此方法，则默认值将是子类的 `repo_type`。

+   `token` (`str`, *可选*) — 认证令牌，使用 `huggingface_hub.HfApi.login` 方法获取。将默认为存储的令牌。

+   `ignore_metadata_errors` (`str`) — 如果为 True，在解析元数据部分时出现错误将被忽略。在此过程中可能会丢失一些信息。请自行承担风险使用。

返回

huggingface_hub.repocard.RepoCard

从仓库的 README.md 文件或文件路径初始化的 RepoCard（或子类）。

从 Hugging Face Hub 仓库的 README.md 或本地文件路径初始化 RepoCard。

示例：

```py
>>> from huggingface_hub.repocard import RepoCard
>>> card = RepoCard.load("nateraw/food")
>>> assert card.data.tags == ["generated_from_trainer", "image-classification", "pytorch"]

```

#### `push_to_hub`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L227)

```py
( repo_id: str token: Optional = None repo_type: Optional = None commit_message: Optional = None commit_description: Optional = None revision: Optional = None create_pr: Optional = None parent_commit: Optional = None ) → export const metadata = 'undefined';str
```

参数

+   `repo_id` (`str`) — 要推送到的 Hugging Face Hub 仓库的仓库 ID。示例：“nateraw/food”。

+   `token` (`str`, *可选*) — 认证令牌，使用 `huggingface_hub.HfApi.login` 方法获取。将默认为存储的令牌。

+   `repo_type` (`str`, *optional*，默认为 “model”) — 要推送到的 Hugging Face 仓库的类型。选项为 “model”、“dataset” 和 “space”。如果此函数由子类调用，则默认为子类的 `repo_type`。

+   `commit_message` (`str`, *optional*) — 生成的提交的摘要/标题/第一行。

+   `commit_description` (`str`, *optional*) — 生成的提交的描述。

+   `revision` (`str`, *optional*) — 要提交的 git 修订版本。默认为 `"main"` 分支的头部。

+   `create_pr` (`bool`, *optional*) — 是否创建一个带有此提交的拉取请求。默认为 `False`。

+   `parent_commit` (`str`, *optional*) — 父提交的 OID / SHA，以十六进制字符串表示。也支持缩写（前 7 个字符）。如果指定并且 `create_pr` 为 `False`，则如果 `revision` 没有指向 `parent_commit`，提交将失败。如果指定并且 `create_pr` 为 `True`，将从 `parent_commit` 创建拉取请求。指定 `parent_commit` 可以确保在提交更改之前仓库没有发生变化，并且如果仓库同时更新/提交，这可能特别有用。

返回

`str`

更新卡片元数据的提交的 URL。

将 RepoCard 推送到 Hugging Face Hub 仓库。

#### `save`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L114)

```py
( filepath: Union )
```

参数

+   `filepath` (`Union[Path, str]`) — 要保存的 markdown 文件的文件路径。

将 RepoCard 保存到文件。

示例：

```py
>>> from huggingface_hub.repocard import RepoCard
>>> card = RepoCard("---\nlanguage: en\n---\n# This is a test repo card")
>>> card.save("/tmp/test.md")

```

#### `validate`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L188)

```py
( repo_type: Optional = None )
```

参数

+   `repo_type` (`str`, *optional*，默认为 “model”) — 要推送到的 Hugging Face 仓库的类型。选项为 “model”、“dataset” 和 “space”。如果从子类调用此函数，则默认为子类的 `repo_type`。

根据 Hugging Face Hub 的卡片验证逻辑验证卡片。使用此函数需要访问互联网，因此仅由 huggingface_hub.repocard.RepoCard.push_to_hub() 内部调用。

引发以下错误：

+   如果卡片未通过验证检查，则引发 [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError)。

+   如果由于任何其他原因请求到 Hub API 失败，则引发 [`HTTPError`](https://requests.readthedocs.io/en/latest/api/#requests.HTTPError)。

## 卡片数据

CardData 对象是 ModelCardData 和 DatasetCardData 的父类。

### `class huggingface_hub.CardData`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard_data.py#L162)

```py
( ignore_metadata_errors: bool = False **kwargs )
```

包含来自 RepoCard 的元数据的结构。

CardData 是 ModelCardData 和 DatasetCardData 的父类。

元数据可以导出为字典或 YAML。导出可以定制以更改数据的表示方式（例如：展平评估结果）。`CardData` 表现为字典（可以获取、弹出、设置值），但不继承自 `dict`，以允许此导出步骤。

#### `get`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard_data.py#L214)

```py
( key: str default: Any = None )
```

为给定的元数据键获取值。

#### `pop`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard_data.py#L218)

```py
( key: str default: Any = None )
```

为给定的元数据键弹出值。

#### `to_dict`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard_data.py#L176)

```py
( ) → export const metadata = 'undefined';dict
```

返回

`dict`

以字典形式表示的 CardData，准备转储到 YAML 块中，以便包含在 README.md 文件中。

将 CardData 转换为字典。

#### `to_yaml`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard_data.py#L196)

```py
( line_break = None ) → export const metadata = 'undefined';str
```

参数

+   `line_break` (str, *optional*) — 转储到 yaml 时要使用的换行符。

返回

`str`

以 YAML 块表示的 CardData。

将 CardData 转储为适合包含在 README.md 文件中的 YAML 块。

## 模型卡片

### ModelCard

### `class huggingface_hub.ModelCard`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L328)

```py
( content: str ignore_metadata_errors: bool = False )
```

#### `from_template`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L333)

```py
( card_data: ModelCardData template_path: Optional = None **template_kwargs ) → export const metadata = 'undefined';huggingface_hub.ModelCard
```

参数

+   `card_data` (`huggingface_hub.ModelCardData`) — 包含要包含在 Hugging Face Hub 模型卡片 YAML 标头中的元数据的 huggingface_hub.ModelCardData 实例。

+   `template_path` (`str`, *optional*) — 具有可选 Jinja 模板变量的 markdown 文件的路径，可以使用 `template_kwargs` 填充。默认为默认模板。

返回

huggingface_hub.ModelCard

具有指定卡片数据和模板内容的 ModelCard 实例。

从模板初始化 ModelCard。默认情况下，它使用默认模板，可以在此处找到：[`github.com/huggingface/huggingface_hub/blob/main/src/huggingface_hub/templates/modelcard_template.md`](https://github.com/huggingface/huggingface_hub/blob/main/src/huggingface_hub/templates/modelcard_template.md)

模板是可以通过传递关键字参数进行自定义的 Jinja2 模板。

示例：

```py
>>> from huggingface_hub import ModelCard, ModelCardData, EvalResult

>>> # Using the Default Template
>>> card_data = ModelCardData(
...     language='en',
...     license='mit',
...     library_name='timm',
...     tags=['image-classification', 'resnet'],
...     datasets=['beans'],
...     metrics=['accuracy'],
... )
>>> card = ModelCard.from_template(
...     card_data,
...     model_description='This model does x + y...'
... )

>>> # Including Evaluation Results
>>> card_data = ModelCardData(
...     language='en',
...     tags=['image-classification', 'resnet'],
...     eval_results=[
...         EvalResult(
...             task_type='image-classification',
...             dataset_type='beans',
...             dataset_name='Beans',
...             metric_type='accuracy',
...             metric_value=0.9,
...         ),
...     ],
...     model_name='my-cool-model',
... )
>>> card = ModelCard.from_template(card_data)

>>> # Using a Custom Template
>>> card_data = ModelCardData(
...     language='en',
...     tags=['image-classification', 'resnet']
... )
>>> card = ModelCard.from_template(
...     card_data=card_data,
...     template_path='./src/huggingface_hub/templates/modelcard_template.md',
...     custom_template_var='custom value',  # will be replaced in template if it exists
... )

```

### ModelCardData

### `class huggingface_hub.ModelCardData`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard_data.py#L239)

```py
( language: Union = None license: Optional = None library_name: Optional = None tags: Optional = None datasets: Optional = None metrics: Optional = None eval_results: Optional = None model_name: Optional = None ignore_metadata_errors: bool = False **kwargs )
```

参数

+   `language` (`Union[str, List[str]]`, *optional*) — 模型训练数据或元数据的语言。必须是 ISO 639-1、639-2 或 639-3 代码（两/三个字母），或者像“code”、“multilingual”这样的特殊值。默认为 `None`。

+   `license` (`str`, *optional*) — 此模型的许可证。示例：apache-2.0 或来自 [`huggingface.co/docs/hub/repositories-licenses`](https://huggingface.co/docs/hub/repositories-licenses) 的任何许可证。默认为 None。

+   `library_name` (`str`, *optional*) — 此模型使用的库的名称。示例：keras 或来自 [`github.com/huggingface/huggingface.js/blob/main/packages/tasks/src/model-libraries.ts`](https://github.com/huggingface/huggingface.js/blob/main/packages/tasks/src/model-libraries.ts) 的任何库。默认为 None。

+   `tags` (`List[str]`, *optional*) — 要添加到您的模型的标签列表，可在 Hugging Face Hub 上进行过滤时使用。默认为 None。

+   `datasets` (`List[str]`, *optional*) — 用于训练此模型的数据集列表。应该是在 [`hf.co/datasets`](https://hf.co/datasets) 上找到的数据集 ID。默认为 None。

+   `metrics` (`List[str]`, *optional*) — 用于评估此模型的指标列表。应该是可以在 [`hf.co/metrics`](https://hf.co/metrics) 找到的指标名称。示例：‘accuracy’。默认为 None。

+   `eval_results` (`Union[List[EvalResult], EvalResult]`, *optional*) — 定义模型评估结果的 `huggingface_hub.EvalResult` 列表。如果提供了，`model_name` 将用作 PapersWithCode 排行榜上的名称。默认为 `None`。

+   `model_name` (`str`, *optional*) — 此模型的名称。与 `eval_results` 一起用于构建卡片元数据中的 `model-index`。您在此处提供的名称将用于 PapersWithCode 排行榜。如果未提供，则将使用存储库名称作为默认值。默认为 None。

+   `ignore_metadata_errors` (`str`) — 如果为 True，则在解析元数据部分时将忽略错误。在此过程中可能会丢失一些信息。请自行承担风险使用。

+   `kwargs` (`dict`, *optional*) — 将添加到模型卡片中的其他元数据。默认为 None。

模型卡片元数据，当包含在您的 README.md 顶部时，将被 Hugging Face Hub 使用

示例：

```py
>>> from huggingface_hub import ModelCardData
>>> card_data = ModelCardData(
...     language="en",
...     license="mit",
...     library_name="timm",
...     tags=['image-classification', 'resnet'],
... )
>>> card_data.to_dict()
{'language': 'en', 'license': 'mit', 'library_name': 'timm', 'tags': ['image-classification', 'resnet']}

```

## 数据集卡片

数据集卡片在 ML 社区中也被称为数据卡片。

### DatasetCard

### `class huggingface_hub.DatasetCard`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L408)

```py
( content: str ignore_metadata_errors: bool = False )
```

#### `from_template`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L413)

```py
( card_data: DatasetCardData template_path: Optional = None **template_kwargs ) → export const metadata = 'undefined';huggingface_hub.DatasetCard
```

参数

+   `card_data` (`huggingface_hub.DatasetCardData`) — 一个包含要包含在 Hugging Face Hub 数据集卡片的 YAML 头部中的元数据的 huggingface_hub.DatasetCardData 实例。

+   `template_path` (`str`, *optional*) — 指向一个带有可选 Jinja 模板变量的 markdown 文件的路径，这些变量可以用`template_kwargs`填充。默认为默认模板。

返回

huggingface_hub.DatasetCard

一个包含指定卡片数据和模板内容的 DatasetCard 实例。

从模板初始化一个 DatasetCard。默认情况下，它使用默认模板，可以在这里找到：[`github.com/huggingface/huggingface_hub/blob/main/src/huggingface_hub/templates/datasetcard_template.md`](https://github.com/huggingface/huggingface_hub/blob/main/src/huggingface_hub/templates/datasetcard_template.md)

模板是可以通过传递关键字参数进行自定义的 Jinja2 模板。

示例：

```py
>>> from huggingface_hub import DatasetCard, DatasetCardData

>>> # Using the Default Template
>>> card_data = DatasetCardData(
...     language='en',
...     license='mit',
...     annotations_creators='crowdsourced',
...     task_categories=['text-classification'],
...     task_ids=['sentiment-classification', 'text-scoring'],
...     multilinguality='monolingual',
...     pretty_name='My Text Classification Dataset',
... )
>>> card = DatasetCard.from_template(
...     card_data,
...     pretty_name=card_data.pretty_name,
... )

>>> # Using a Custom Template
>>> card_data = DatasetCardData(
...     language='en',
...     license='mit',
... )
>>> card = DatasetCard.from_template(
...     card_data=card_data,
...     template_path='./src/huggingface_hub/templates/datasetcard_template.md',
...     custom_template_var='custom value',  # will be replaced in template if it exists
... )

```

### DatasetCardData

### `class huggingface_hub.DatasetCardData`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard_data.py#L345)

```py
( language: Union = None license: Union = None annotations_creators: Union = None language_creators: Union = None multilinguality: Union = None size_categories: Union = None source_datasets: Optional = None task_categories: Union = None task_ids: Union = None paperswithcode_id: Optional = None pretty_name: Optional = None train_eval_index: Optional = None config_names: Union = None ignore_metadata_errors: bool = False **kwargs )
```

参数

+   `language` (`List[str]`, *optional*) — 数据集的数据或元数据的语言。它必须是 ISO 639-1、639-2 或 639-3 代码（两/三个字母），或者像“code”、“multilingual”这样的特殊值。

+   `license` (`Union[str, List[str]]`, *optional*) — 此数据集的许可证。示例：apache-2.0 或来自[`huggingface.co/docs/hub/repositories-licenses`](https://huggingface.co/docs/hub/repositories-licenses)的任何许可证。

+   `annotations_creators` (`Union[str, List[str]]`, *optional*) — 数据集的注释是如何创建的。选项有：‘found’、‘crowdsourced’、‘expert-generated’、‘machine-generated’、‘no-annotation’、‘other’。

+   `language_creators` (`Union[str, List[str]]`, *optional*) — 数据集中基于文本的数据是如何创建的。选项有：‘found’、‘crowdsourced’、‘expert-generated’、‘machine-generated’、‘other’

+   `multilinguality` (`Union[str, List[str]]`, *optional*) — 数据集是否是多语言的。选项有：‘monolingual’、‘multilingual’、‘translation’、‘other’。

+   `size_categories` (`Union[str, List[str]]`, *optional*) — 数据集中示例的数量。选项有：‘n<1K’、‘1K<n>1T’和‘other’。</n>

+   `source_datasets` (`List[str]]`, *optional*) — 指示数据集是原始数据集还是从另一个现有数据集扩展而来。选项有：‘original’和‘extended’。

+   `task_categories` (`Union[str, List[str]]`, *optional*) — 数据集支持哪些任务类别？

+   `task_ids` (`Union[str, List[str]]`, *optional*) — 数据集支持哪些具体任务？

+   `paperswithcode_id` (`str`, *optional*) — PapersWithCode 上数据集的 ID。

+   `pretty_name` (`str`, *optional*) — 数据集的更易读名称。 (例如 “猫与狗”)

+   `train_eval_index` (`Dict`, *optional*) — 描述在 Hub 上进行评估所需规范的字典。如果未提供，将从 kwargs 的‘train-eval-index’键中获取。

+   `config_names` (`Union[str, List[str]]`, *optional*) — 数据集的可用配置列表。

Dataset Card Metadata 是 Hugging Face Hub 在您的 README.md 顶部包含时使用的。

## Space Cards

### SpaceCard

### `class huggingface_hub.SpaceCard`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L472)

```py
( content: str ignore_metadata_errors: bool = False )
```

### SpaceCardData

### `class huggingface_hub.SpaceCardData`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard_data.py#L425)

```py
( title: Optional = None sdk: Optional = None sdk_version: Optional = None python_version: Optional = None app_file: Optional = None app_port: Optional = None license: Optional = None duplicated_from: Optional = None models: Optional = None datasets: Optional = None tags: Optional = None ignore_metadata_errors: bool = False **kwargs )
```

参数

+   `title` (`str`, *optional*) — Space 的标题。

+   `sdk` (`str`, *optional*) — Space 的 SDK（`gradio`、`streamlit`、`docker`或`static`之一）。

+   `sdk_version` (`str`, *optional*) — 使用的 SDK 的版本（如果是 Gradio/Streamlit sdk）。

+   `python_version` (`str`, *optional*) — Space 中使用的 Python 版本（如果是 Gradio/Streamlit sdk）。

+   `app_file` (`str`, *optional*) — 您的主应用程序文件的路径（其中包含 gradio 或 streamlit Python 代码，或静态 html 代码）。路径相对于存储库的根目录。

+   `app_port` (`str`, *optional*) — 应用程序运行的端口。仅在 sdk 为`docker`时使用。

+   `license` (`str`, *optional*) — 此模型的许可证。示例：apache-2.0 或来自[`huggingface.co/docs/hub/repositories-licenses`](https://huggingface.co/docs/hub/repositories-licenses)的任何许可证。

+   `duplicated_from` (`str`, *optional*) — 如果这是一个重复的 Space，则为原始 Space 的 ID。

+   `models`（List`str`，*optional*） — 与此 Space 相关的模型列表。应该是在[`hf.co/models`](https://hf.co/models)上找到的数据集 ID。

+   `datasets` (`List[str]`, *optional*) — 与此 Space 相关的数据集列表。应该是在[`hf.co/datasets`](https://hf.co/datasets)上找到的数据集 ID。

+   `tags` (`List[str]`, *optional*) — 要添加到 Space 的标签列表，可在 Hub 上进行过滤时使用。

+   `ignore_metadata_errors` (`str`) — 如果为 True，则在解析元数据部分时将忽略错误。在此过程中可能会丢失一些信息。请自行承担风险使用。

+   `kwargs` (`dict`, *optional*) — 将添加到 Space 卡中的其他元数据。

Space Card Metadata 是 Hugging Face Hub 在您的 README.md 顶部包含时使用的。

要获取 Spaces 配置的详尽参考，请访问[`huggingface.co/docs/hub/spaces-config-reference#spaces-configuration-reference`](https://huggingface.co/docs/hub/spaces-config-reference#spaces-configuration-reference)。

示例：

```py
>>> from huggingface_hub import SpaceCardData
>>> card_data = SpaceCardData(
...     title="Dreambooth Training",
...     license="mit",
...     sdk="gradio",
...     duplicated_from="multimodalart/dreambooth-training"
... )
>>> card_data.to_dict()
{'title': 'Dreambooth Training', 'sdk': 'gradio', 'license': 'mit', 'duplicated_from': 'multimodalart/dreambooth-training'}
```

## Utilities

### EvalResult

### `class huggingface_hub.EvalResult`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard_data.py#L10)

```py
( task_type: str dataset_type: str dataset_name: str metric_type: str metric_value: Any task_name: Optional = None dataset_config: Optional = None dataset_split: Optional = None dataset_revision: Optional = None dataset_args: Optional = None metric_name: Optional = None metric_config: Optional = None metric_args: Optional = None verified: Optional = None verify_token: Optional = None source_name: Optional = None source_url: Optional = None )
```

参数

+   `task_type` (`str`) — 任务标识符。示例：“图像分类”。

+   `dataset_type` (`str`) — 数据集标识符。示例：“common_voice”。使用数据集 id 来自[`hf.co/datasets`](https://hf.co/datasets)。

+   `dataset_name` (`str`) — 数据集的漂亮名称。示例：“Common Voice（法语）”。

+   `metric_type` (`str`) — 指标标识符。示例：“wer”。使用来自[`hf.co/metrics`](https://hf.co/metrics)的指标 id。

+   `metric_value` (`Any`) — 指标值。示例：0.9 或“20.0 ± 1.2”。

+   `task_name` (`str`, *optional*) — 任务的漂亮名称。示例：“语音识别”。

+   `dataset_config` (`str`, *optional*) — 在`load_dataset()`中使用的数据集配置的名称。示例：`load_dataset("common_voice", "fr")`中的 fr。有关更多信息，请参阅`datasets`文档：[`hf.co/docs/datasets/package_reference/loading_methods#datasets.load_dataset.name`](https://hf.co/docs/datasets/package_reference/loading_methods#datasets.load_dataset.name)

+   `dataset_split` (`str`, *optional*) — 在`load_dataset()`中使用的拆分。示例：“test”。

+   `dataset_revision` (`str`, *optional*) — 在`load_dataset()`中使用的数据集的修订版（也称为 Git Sha）。示例：5503434ddd753f426f4b38109466949a1217c2bb

+   `dataset_args` (`Dict[str, Any]`, *可选*) — 在`Metric.compute()`期间传递的参数。例如对于`bleu`: `{"max_order": 4}`

+   `metric_name` (`str`, *可选*) — 指标的漂亮名称。示例: “Test WER”。

+   `metric_config` (`str`, *可选*) — 在`load_metric()`中使用的指标配置的名称。例如: 在`load_metric("bleurt", "bleurt-large-512")`中的 bleurt-large-512。有关更多信息，请参阅`datasets`文档: [`huggingface.co/docs/datasets/v2.1.0/en/loading#load-configurations`](https://huggingface.co/docs/datasets/v2.1.0/en/loading#load-configurations)

+   `metric_args` (`Dict[str, Any]`, *可选*) — 在`Metric.compute()`期间传递的参数。例如对于`bleu`: max_order: 4

+   `verified` (`bool`, *可选*) — 指示指标是否来自 Hugging Face 的[评估服务](https://huggingface.co/spaces/autoevaluate/model-evaluator)。由 Hugging Face 自动计算，不要设置。

+   `verify_token` (`str`, *可选*) — 用于验证指标是否来自 Hugging Face 的[评估服务](https://huggingface.co/spaces/autoevaluate/model-evaluator)的 JSON Web Token。

+   `source_name` (`str`, *可选*) — 评估结果来源的名称。示例: “Open LLM Leaderboard”。

+   `source_url` (`str`, *可选*) — 评估结果来源的 URL。示例: "[`huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard`](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard%22)"。

在 Model Cards 的模型索引中找到的单个评估结果的扁平表示。

有关模型索引规范的更多信息，请参见[`github.com/huggingface/hub-docs/blob/main/modelcard.md?plain=1`](https://github.com/huggingface/hub-docs/blob/main/modelcard.md?plain=1)。

#### `is_equal_except_value`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard_data.py#L143)

```py
( other: EvalResult )
```

如果`self`和`other`描述完全相同的指标但值不同，则返回 True。

### model_index_to_eval_results

#### `huggingface_hub.repocard_data.model_index_to_eval_results`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard_data.py#L506)

```py
( model_index: List ) → export const metadata = 'undefined';model_name (str)
```

参数

+   `model_index` (`List[Dict[str, Any]]`) — 一个模型索引数据结构，可能来自 Hugging Face Hub 上的 README.md 文件。

返回

model_name (`str`)

在模型索引中找到的模型名称。这将用作模型在像 PapersWithCode 这样的排行榜上的标识符。eval_results (`List[EvalResult]`): 包含在提供的 model_index 中报告的指标的`huggingface_hub.EvalResult`对象列表。

接受一个模型索引并返回模型名称以及一个包含`huggingface_hub.EvalResult`对象的列表。

可以在此处找到模型索引的详细规范: [`github.com/huggingface/hub-docs/blob/main/modelcard.md?plain=1`](https://github.com/huggingface/hub-docs/blob/main/modelcard.md?plain=1)

示例：

```py
>>> from huggingface_hub.repocard_data import model_index_to_eval_results
>>> # Define a minimal model index
>>> model_index = [
...     {
...         "name": "my-cool-model",
...         "results": [
...             {
...                 "task": {
...                     "type": "image-classification"
...                 },
...                 "dataset": {
...                     "type": "beans",
...                     "name": "Beans"
...                 },
...                 "metrics": [
...                     {
...                         "type": "accuracy",
...                         "value": 0.9
...                     }
...                 ]
...             }
...         ]
...     }
... ]
>>> model_name, eval_results = model_index_to_eval_results(model_index)
>>> model_name
'my-cool-model'
>>> eval_results[0].task_type
'image-classification'
>>> eval_results[0].metric_type
'accuracy'

```

### eval_results_to_model_index

#### `huggingface_hub.repocard_data.eval_results_to_model_index`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard_data.py#L622)

```py
( model_name: str eval_results: List ) → export const metadata = 'undefined';model_index (List[Dict[str, Any]])
```

参数

+   `model_name` (`str`) — 模型的名称（例如“my-cool-model”）。这将用作模型在像 PapersWithCode 这样的排行榜上的标识符。

+   `eval_results` (`List[EvalResult]`) — 包含要在模型索引中报告的指标的`huggingface_hub.EvalResult`对象列表。

返回

model_index (`List[Dict[str, Any]]`)

将 eval_results 转换为模型索引。

接受给定的模型名称和`huggingface_hub.EvalResult`列表，并返回一个有效的模型索引，该索引将与 Hugging Face Hub 期望的格式兼容。

示例：

```py
>>> from huggingface_hub.repocard_data import eval_results_to_model_index, EvalResult
>>> # Define minimal eval_results
>>> eval_results = [
...     EvalResult(
...         task_type="image-classification",  # Required
...         dataset_type="beans",  # Required
...         dataset_name="Beans",  # Required
...         metric_type="accuracy",  # Required
...         metric_value=0.9,  # Required
...     )
... ]
>>> eval_results_to_model_index("my-cool-model", eval_results)
[{'name': 'my-cool-model', 'results': [{'task': {'type': 'image-classification'}, 'dataset': {'name': 'Beans', 'type': 'beans'}, 'metrics': [{'type': 'accuracy', 'value': 0.9}]}]}]

```

### metadata_eval_result

#### `huggingface_hub.metadata_eval_result`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L544)

```py
( model_pretty_name: str task_pretty_name: str task_id: str metrics_pretty_name: str metrics_id: str metrics_value: Any dataset_pretty_name: str dataset_id: str metrics_config: Optional = None metrics_verified: bool = False dataset_config: Optional = None dataset_split: Optional = None dataset_revision: Optional = None metrics_verification_token: Optional = None ) → export const metadata = 'undefined';dict
```

参数

+   `model_pretty_name` (`str`) — 以自然语言命名的模型名称。

+   `task_pretty_name` (`str`) — 以自然语言命名的任务名称。

+   `task_id` (`str`) — 示例：automatic-speech-recognition。任务 ID。

+   `metrics_pretty_name` (`str`) — 以自然语言命名的指标名称。示例：测试 WER。

+   `metrics_id` (`str`) — 示例：wer。来自[`hf.co/metrics`](https://hf.co/metrics)的指标 ID。

+   `metrics_value` (`Any`) — 指标的值。示例：20.0 或“20.0 ± 1.2”。

+   `dataset_pretty_name` (`str`) — 以自然语言命名的数据集名称。

+   `dataset_id` (`str`) — 示例：common_voice。来自[`hf.co/datasets`](https://hf.co/datasets)的数据集 ID。

+   `metrics_config` (`str`, *可选*) — 在`load_metric()`中使用的指标配置的名称。示例：在`load_metric("bleurt", "bleurt-large-512")`中的 bleurt-large-512。

+   `metrics_verified` (`bool`, *可选*, 默认为`False`) — 指示指标是否来自 Hugging Face 的[评估服务](https://huggingface.co/spaces/autoevaluate/model-evaluator)。由 Hugging Face 自动计算，不要设置。

+   `dataset_config` (`str`, *可选*) — 示例：fr。在`load_dataset()`中使用的数据集配置的名称。

+   `dataset_split` (`str`, *可选*) — 示例：test。在`load_dataset()`中使用的数据集拆分的名称。

+   `dataset_revision` (`str`, *可选*) — 示例：5503434ddd753f426f4b38109466949a1217c2bb。在`load_dataset()`中使用的数据集修订的名称。

+   `metrics_verification_token` (`bool`, *可选*) — 用于验证指标是否来自 Hugging Face 的[评估服务](https://huggingface.co/spaces/autoevaluate/model-evaluator)的 JSON Web 令牌。

返回值

`dict`

一个包含模型在数据集上评估结果的元数据字典。

创建一个包含模型在数据集上评估结果的元数据字典。

示例：

```py
>>> from huggingface_hub import metadata_eval_result
>>> results = metadata_eval_result(
...         model_pretty_name="RoBERTa fine-tuned on ReactionGIF",
...         task_pretty_name="Text Classification",
...         task_id="text-classification",
...         metrics_pretty_name="Accuracy",
...         metrics_id="accuracy",
...         metrics_value=0.2662102282047272,
...         dataset_pretty_name="ReactionJPEG",
...         dataset_id="julien-c/reactionjpeg",
...         dataset_config="default",
...         dataset_split="test",
... )
>>> results == {
...     'model-index': [
...         {
...             'name': 'RoBERTa fine-tuned on ReactionGIF',
...             'results': [
...                 {
...                     'task': {
...                         'type': 'text-classification',
...                         'name': 'Text Classification'
...                     },
...                     'dataset': {
...                         'name': 'ReactionJPEG',
...                         'type': 'julien-c/reactionjpeg',
...                         'config': 'default',
...                         'split': 'test'
...                     },
...                     'metrics': [
...                         {
...                             'type': 'accuracy',
...                             'value': 0.2662102282047272,
...                             'name': 'Accuracy',
...                             'verified': False
...                         }
...                     ]
...                 }
...             ]
...         }
...     ]
... }
True

```

### metadata_update

#### `huggingface_hub.metadata_update`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/repocard.py#L672)

```py
( repo_id: str metadata: Dict repo_type: Optional = None overwrite: bool = False token: Optional = None commit_message: Optional = None commit_description: Optional = None revision: Optional = None create_pr: bool = False parent_commit: Optional = None ) → export const metadata = 'undefined';str
```

参数

+   `repo_id` (`str`) — 仓库的名称。

+   `metadata` (`dict`) — 包含要更新的元数据的字典。

+   `repo_type` (`str`, *可选*) — 如果更新为数据集或空间，则设置为`"dataset"`或`"space"`，如果更新为模型，则设置为`None`或`"model"`。默认为`None`。

+   `overwrite` (`bool`, *可选*, 默认为`False`) — 如果设置为`True`，则可以覆盖现有字段，否则尝试覆盖现有字段将导致错误。

+   `token` (`str`, *可选*) — Hugging Face 认证令牌。

+   `commit_message` (`str`, *可选*) — 生成提交的摘要/标题/第一行。默认为`f"Update metadata with huggingface_hub"`

+   `commit_description` (`str` *可选*) — 生成提交的描述

+   `revision` (`str`, *可选*) — 要提交的 git 修订。默认为`"main"`分支的头部。

+   `create_pr` (`boolean`, *可选*) — 是否从`revision`创建一个拉取请求并提交该提交。默认为`False`。

+   `parent_commit` (`str`, *可选*) — 父提交的 OID/SHA，以十六进制字符串表示。也支持缩写（前 7 个字符）。如果指定且`create_pr`为`False`，则如果`revision`不指向`parent_commit`，提交将失败。如果指定且`create_pr`为`True`，将从`parent_commit`创建拉取请求。指定`parent_commit`可确保在提交更改之前仓库未更改，并且在仓库同时更新/提交时特别有用。

返回值

`str`

更新卡片元数据的提交 URL。

更新 Hugging Face Hub 上仓库的 README.md 中的元数据。如果 README.md 文件尚不存在，则将创建一个新文件，并包含元数据和默认的 ModelCard 或 DatasetCard 模板。对于`space`仓库，如果没有`README.md`文件，将抛出错误，因为没有`README.md`文件的空间无法存在。

示例：

```py
>>> from huggingface_hub import metadata_update
>>> metadata = {'model-index': [{'name': 'RoBERTa fine-tuned on ReactionGIF',
...             'results': [{'dataset': {'name': 'ReactionGIF',
...                                      'type': 'julien-c/reactiongif'},
...                           'metrics': [{'name': 'Recall',
...                                        'type': 'recall',
...                                        'value': 0.7762102282047272}],
...                          'task': {'name': 'Text Classification',
...                                   'type': 'text-classification'}}]}]}
>>> url = metadata_update("hf-internal-testing/reactiongif-roberta-card", metadata)

```
