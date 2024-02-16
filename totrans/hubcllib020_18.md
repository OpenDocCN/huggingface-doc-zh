# 创建和分享模型卡

> 原始文本：[https://huggingface.co/docs/huggingface_hub/guides/model-cards](https://huggingface.co/docs/huggingface_hub/guides/model-cards)

`huggingface_hub`库提供了一个Python接口，用于创建、分享和更新模型卡。访问[专门的文档页面](https://huggingface.co/docs/hub/models-cards)以深入了解Hub上的模型卡是什么，以及它们在幕后是如何工作的。

[新（测试版）！尝试我们的实验性模型卡创建应用](https://huggingface.co/spaces/huggingface/Model_Cards_Writing_Tool)

## 从Hub加载一个模型卡

要从Hub加载现有卡片，您可以使用[ModelCard.load()](/docs/huggingface_hub/v0.20.3/en/package_reference/cards#huggingface_hub.RepoCard.load)函数。在这里，我们将从[`nateraw/vit-base-beans`](https://huggingface.co/nateraw/vit-base-beans)加载卡片。

```py
from huggingface_hub import ModelCard

card = ModelCard.load('nateraw/vit-base-beans')
```

这张卡片有一些有用的属性，您可能希望访问/利用：

+   `card.data`：返回一个[ModelCardData](/docs/huggingface_hub/v0.20.3/en/package_reference/cards#huggingface_hub.ModelCardData)实例，其中包含模型卡的元数据。在此实例上调用`.to_dict()`以获得表示为字典的表示。

+   `card.text`：返回卡片的文本，*不包括元数据头*。

+   `card.content`：返回卡片的文本内容，*包括元数据头*。

## 创建模型卡

### 从文本

要从文本初始化一个模型卡，只需将卡片的文本内容传递给`ModelCard`的init。

```py
content = """
---
language: en
license: mit
---

# My Model Card
"""

card = ModelCard(content)
card.data.to_dict() == {'language': 'en', 'license': 'mit'}  # True
```

您可能想要以f-strings的方式来做这件事。在以下示例中，我们：

+   使用[ModelCardData.to_yaml()](/docs/huggingface_hub/v0.20.3/en/package_reference/cards#huggingface_hub.CardData.to_yaml)将我们定义的元数据转换为YAML，以便我们可以将其用于在模型卡中插入YAML块。

+   展示了如何通过Python f-strings使用模板变量。

```py
card_data = ModelCardData(language='en', license='mit', library='timm')

example_template_var = 'nateraw'
content = f"""
---
{ card_data.to_yaml() }
---

# My Model Card

This model created by [@{example_template_var}](https://github.com/{example_template_var})
"""

card = ModelCard(content)
print(card)
```

上面的示例会让我们得到一个看起来像这样的卡片：

```py
---
language: en
license: mit
library: timm
--- 
# My Model Card

This model created by [@nateraw](https://github.com/nateraw)
```

### 从Jinja模板

如果您已安装`Jinja2`，您可以从一个Jinja模板文件创建模型卡。让我们看一个基本示例：

```py
from pathlib import Path

from huggingface_hub import ModelCard, ModelCardData

# Define your jinja template
template_text = """
---
{{ card_data }}
---

# Model Card for MyCoolModel

This model does this and that.

This model was created by [@{{ author }}](https://hf.co/{{author}}).
""".strip()

# Write the template to a file
Path('custom_template.md').write_text(template_text)

# Define card metadata
card_data = ModelCardData(language='en', license='mit', library_name='keras')

# Create card from template, passing it any jinja template variables you want.
# In our case, we'll pass author
card = ModelCard.from_template(card_data, template_path='custom_template.md', author='nateraw')
card.save('my_model_card_1.md')
print(card)
```

生成的卡片的markdown如下：

```py
---
language: en
license: mit
library_name: keras
--- 
# Model Card for MyCoolModel

This model does this and that.

This model was created by [@nateraw](https://hf.co/nateraw).
```

如果您更新任何card.data，它将反映在卡片本身上。

```py
card.data.library_name = 'timm'
card.data.language = 'fr'
card.data.license = 'apache-2.0'
print(card)
```

现在，您可以看到元数据头已更新：

```py
---
language: fr
license: apache-2.0
library_name: timm
--- 
# Model Card for MyCoolModel

This model does this and that.

This model was created by [@nateraw](https://hf.co/nateraw).
```

当您更新卡片数据时，您可以通过调用[ModelCard.validate()](/docs/huggingface_hub/v0.20.3/en/package_reference/cards#huggingface_hub.RepoCard.validate)来验证卡片仍然有效，并且符合在Hugging Face Hub上设置的任何验证规则。

### 从默认模板

而不是使用您自己的模板，您也可以使用[默认模板](https://github.com/huggingface/huggingface_hub/blob/main/src/huggingface_hub/templates/modelcard_template.md)，这是一个功能齐全的模型卡，有很多部分您可能想要填写。在幕后，它使用[Jinja2](https://jinja.palletsprojects.com/en/3.1.x/)来填写模板文件。

请注意，您必须安装Jinja2才能使用`from_template`。您可以使用`pip install Jinja2`来安装。

```py
card_data = ModelCardData(language='en', license='mit', library_name='keras')
card = ModelCard.from_template(
    card_data,
    model_id='my-cool-model',
    model_description="this model does this and that",
    developers="Nate Raw",
    repo="https://github.com/huggingface/huggingface_hub",
)
card.save('my_model_card_2.md')
print(card)
```

## 分享模型卡

如果您已经通过`huggingface-cli login`或[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)进行了Hugging Face Hub的身份验证，您可以通过简单调用[ModelCard.push_to_hub()](/docs/huggingface_hub/v0.20.3/en/package_reference/cards#huggingface_hub.RepoCard.push_to_hub)将卡片推送到Hub。让我们看看如何做到这一点...

首先，我们将在经过身份验证的用户命名空间下创建一个名为‘hf-hub-modelcards-pr-test’的新存储库：

```py
from huggingface_hub import whoami, create_repo

user = whoami()['name']
repo_id = f'{user}/hf-hub-modelcards-pr-test'
url = create_repo(repo_id, exist_ok=True)
```

然后，我们将从默认模板创建一个卡片（与上面定义的相同）：

```py
card_data = ModelCardData(language='en', license='mit', library_name='keras')
card = ModelCard.from_template(
    card_data,
    model_id='my-cool-model',
    model_description="this model does this and that",
    developers="Nate Raw",
    repo="https://github.com/huggingface/huggingface_hub",
)
```

最后，我们将把它推送到hub

```py
card.push_to_hub(repo_id)
```

您可以在[这里](https://huggingface.co/nateraw/hf-hub-modelcards-pr-test/blob/main/README.md)查看生成的卡片。

如果您希望将卡片作为拉取请求推送，只需在调用`push_to_hub`时说`create_pr=True`：

```py
card.push_to_hub(repo_id, create_pr=True)
```

从此命令创建的结果PR可以在[这里](https://huggingface.co/nateraw/hf-hub-modelcards-pr-test/discussions/3)看到。

## 更新元数据

在本节中，我们将看到存储库卡片中的元数据以及如何更新它们。

`metadata`指的是一个哈希映射（或键值）上下文，提供有关模型、数据集或空间的一些高级信息。这些信息可以包括模型的`pipeline类型`、`model_id`或`model_description`等详细信息。有关更多详细信息，您可以查看这些指南：[模型卡片](https://huggingface.co/docs/hub/model-cards#model-card-metadata)、[数据集卡片](https://huggingface.co/docs/hub/datasets-cards#dataset-card-metadata)和[空间设置](https://huggingface.co/docs/hub/spaces-settings#spaces-settings)。现在让我们看一些关于如何更新这些元数据的示例。

让我们从第一个示例开始：

```py
>>> from huggingface_hub import metadata_update
>>> metadata_update("username/my-cool-model", {"pipeline_tag": "image-classification"})
```

使用这两行代码，您将更新元数据以设置一个新的`pipeline_tag`。

默认情况下，您不能更新卡片上已经存在的键。如果要这样做，必须显式传递`overwrite=True`：

```py
>>> from huggingface_hub import metadata_update
>>> metadata_update("username/my-cool-model", {"pipeline_tag": "text-generation"}, overwrite=True)
```

经常发生的情况是，您想要对一个您没有写入权限的存储库提出一些更改建议。您可以通过在该存储库上创建一个PR来实现这一点，这将允许所有者审查并合并您的建议。

```py
>>> from huggingface_hub import metadata_update
>>> metadata_update("someone/model", {"pipeline_tag": "text-classification"}, create_pr=True)
```

## 包括评估结果

要在元数据`model-index`中包含评估结果，可以传递一个[EvalResult](/docs/huggingface_hub/v0.20.3/en/package_reference/cards#huggingface_hub.EvalResult)或一个带有相关评估结果的`EvalResult`列表。在调用`card.data.to_dict()`时，它将创建`model-index`。有关此操作原理的更多信息，请查看[Hub文档的此部分](https://huggingface.co/docs/hub/models-cards#evaluation-results)。

请注意，使用此函数需要在[ModelCardData](/docs/huggingface_hub/v0.20.3/en/package_reference/cards#huggingface_hub.ModelCardData)中包含`model_name`属性。

```py
card_data = ModelCardData(
    language='en',
    license='mit',
    model_name='my-cool-model',
    eval_results = EvalResult(
        task_type='image-classification',
        dataset_type='beans',
        dataset_name='Beans',
        metric_type='accuracy',
        metric_value=0.7
    )
)

card = ModelCard.from_template(card_data)
print(card.data)
```

生成的`card.data`应该如下所示：

```py
language: en
license: mit
model-index:
- name: my-cool-model
  results:
  - task:
      type: image-classification
    dataset:
      name: Beans
      type: beans
    metrics:
    - type: accuracy
      value: 0.7
```

如果您有多个评估结果想要共享，只需传递一个`EvalResult`列表：

```py
card_data = ModelCardData(
    language='en',
    license='mit',
    model_name='my-cool-model',
    eval_results = [
        EvalResult(
            task_type='image-classification',
            dataset_type='beans',
            dataset_name='Beans',
            metric_type='accuracy',
            metric_value=0.7
        ),
        EvalResult(
            task_type='image-classification',
            dataset_type='beans',
            dataset_name='Beans',
            metric_type='f1',
            metric_value=0.65
        )
    ]
)
card = ModelCard.from_template(card_data)
card.data
```

这将使您得到以下`card.data`：

```py
language: en
license: mit
model-index:
- name: my-cool-model
  results:
  - task:
      type: image-classification
    dataset:
      name: Beans
      type: beans
    metrics:
    - type: accuracy
      value: 0.7
    - type: f1
      value: 0.65
```
