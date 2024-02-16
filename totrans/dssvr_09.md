# 预览数据集

> 原文链接：[https://huggingface.co/docs/datasets-server/first_rows](https://huggingface.co/docs/datasets-server/first_rows)

Datasets Server提供了一个`/first-rows`端点，用于可视化数据集的前100行。这将让您对数据集中包含的数据类型和示例数据有一个很好的了解。

![dataset-viewer](../Images/97b9f61671054d67cf8cb0949a1195e9.png)

本指南向您展示如何使用Datasets Server的`/first-rows`端点预览数据集。也可以尝试使用[Postman](https://www.postman.com/huggingface/workspace/hugging-face-apis/request/23242779-32d6a8be-b800-446a-8cee-f6b5ca1710df)，[RapidAPI](https://rapidapi.com/hugging-face-hugging-face-default/api/hugging-face-datasets-api)，或[ReDoc](https://redocly.github.io/redoc/?url=https://datasets-server.huggingface.co/openapi.json#operation/listFirstRows)。

`/first-rows`端点接受三个查询参数：

+   `dataset`：数据集名称，例如`glue`或`mozilla-foundation/common_voice_10_0`

+   `config`：配置名称，例如`cola`

+   `split`：分割名称，例如`train`

PythonJavaScriptcURL

```py
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://datasets-server.huggingface.co/first-rows?dataset=ibm/duorc&config=SelfRC&split=train"
def query():
    response = requests.get(API_URL, headers=headers)
    return response.json()
data = query()
```

端点响应是一个包含两个键的JSON：

+   数据集的[`features`](https://huggingface.co/docs/datasets/about_dataset_features)，包括列名和数据类型。

+   数据集的前100个`rows`以及特定行中每列包含的内容。

例如，这里是`ibm/duorc`/`SelfRC`训练集分割的`features`和前100个`rows`：

```py
{
  "dataset": "ibm/duorc",
  "config": "SelfRC",
  "split": "train",
  "features": [
    {
      "feature_idx": 0,
      "name": "plot_id",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 1,
      "name": "plot",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 2,
      "name": "title",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 3,
      "name": "question_id",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 4,
      "name": "question",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 5,
      "name": "answers",
      "type": {
        "feature": { "dtype": "string", "_type": "Value" },
        "_type": "Sequence"
      }
    },
    {
      "feature_idx": 6,
      "name": "no_answer",
      "type": { "dtype": "bool", "_type": "Value" }
    }
  ],
  "rows": [
    {
      "row_idx": 0,
      "row": {
        "plot_id": "/m/03vyhn",
        "plot": "200 years in the future, Mars has been colonized by a high-tech company.\nMelanie Ballard (Natasha Henstridge) arrives by train to a Mars mining camp which has cut all communication links with the company headquarters. She's not alone, as she is with a group of fellow police officers. They find the mining camp deserted except for a person in the prison, Desolation Williams (Ice Cube), who seems to laugh about them because they are all going to die. They were supposed to take Desolation to headquarters, but decide to explore first to find out what happened.They find a man inside an encapsulated mining car, who tells them not to open it. However, they do and he tries to kill them. One of the cops witnesses strange men with deep scarred and heavily tattooed faces killing the remaining survivors. The cops realise they need to leave the place fast.Desolation explains that the miners opened a kind of Martian construction in the soil which unleashed red dust. Those who breathed that dust became violent psychopaths who started to build weapons and kill the uninfected. They changed genetically, becoming distorted but much stronger.The cops and Desolation leave the prison with difficulty, and devise a plan to kill all the genetically modified ex-miners on the way out. However, the plan goes awry, and only Melanie and Desolation reach headquarters alive. Melanie realises that her bosses won't ever believe her. However, the red dust eventually arrives to headquarters, and Melanie and Desolation need to fight once again.",
        "title": "Ghosts of Mars",
        "question_id": "b440de7d-9c3f-841c-eaec-a14bdff950d1",
        "question": "How did the police arrive at the Mars mining camp?",
        "answers": ["They arrived by train."],
        "no_answer": false
      },
      "truncated_cells": []
    },
    {
      "row_idx": 1,
      "row": {
        "plot_id": "/m/03vyhn",
        "plot": "200 years in the future, Mars has been colonized by a high-tech company.\nMelanie Ballard (Natasha Henstridge) arrives by train to a Mars mining camp which has cut all communication links with the company headquarters. She's not alone, as she is with a group of fellow police officers. They find the mining camp deserted except for a person in the prison, Desolation Williams (Ice Cube), who seems to laugh about them because they are all going to die. They were supposed to take Desolation to headquarters, but decide to explore first to find out what happened.They find a man inside an encapsulated mining car, who tells them not to open it. However, they do and he tries to kill them. One of the cops witnesses strange men with deep scarred and heavily tattooed faces killing the remaining survivors. The cops realise they need to leave the place fast.Desolation explains that the miners opened a kind of Martian construction in the soil which unleashed red dust. Those who breathed that dust became violent psychopaths who started to build weapons and kill the uninfected. They changed genetically, becoming distorted but much stronger.The cops and Desolation leave the prison with difficulty, and devise a plan to kill all the genetically modified ex-miners on the way out. However, the plan goes awry, and only Melanie and Desolation reach headquarters alive. Melanie realises that her bosses won't ever believe her. However, the red dust eventually arrives to headquarters, and Melanie and Desolation need to fight once again.",
        "title": "Ghosts of Mars",
        "question_id": "a9f95c0d-121f-3ca9-1595-d497dc8bc56c",
        "question": "Who has colonized Mars 200 years in the future?",
        "answers": [
          "A high-tech company has colonized Mars 200 years in the future."
        ],
        "no_answer": false
      },
      "truncated_cells": []
    }
    ...
  ],
  "truncated": false
}
```

## 截断的响应

对于一些数据集，从`/first-rows`的响应大小可能超过1MB，这种情况下响应会被截断，直到大小小于1MB。这意味着您可能无法在响应中获得100行，因为行被截断，这种情况下`truncated`字段将为`true`。

在某些情况下，即使前几行生成的响应超过1MB，一些列也会被截断并转换为字符串。您将在`truncated_cells`字段中看到这些列。

例如，[`ett`](https://datasets-server.huggingface.co/first-rows?dataset=ett&config=m2&split=test)数据集只返回10行，`target`和`feat_dynamic_real`列被截断：

```py
  ...
  "rows": [
    {
      "row_idx": 0,
      "row": {
        "start": "2016-07-01T00:00:00",
        "target": "[38.6619987487793,38.222999572753906,37.34400177001953,37.124000549316406,37.124000549316406,36.9039",
        "feat_static_cat": [0],
        "feat_dynamic_real": "[[41.130001068115234,39.62200164794922,38.86800003051758,35.518001556396484,37.52799987792969,37.611",
        "item_id": "OT"
      },
      "truncated_cells": ["target", "feat_dynamic_real"]
    },
    {
      "row_idx": 1,
      "row": {
        "start": "2016-07-01T00:00:00",
        "target": "[38.6619987487793,38.222999572753906,37.34400177001953,37.124000549316406,37.124000549316406,36.9039",
        "feat_static_cat": [0],
        "feat_dynamic_real": "[[41.130001068115234,39.62200164794922,38.86800003051758,35.518001556396484,37.52799987792969,37.611",
        "item_id": "OT"
      },
      "truncated_cells": ["target", "feat_dynamic_real"]
    },
  ...
  ],
  truncated: true
```
