# Hub API 端点

> 原始文本：[`huggingface.co/docs/hub/api`](https://huggingface.co/docs/hub/api)

我们有开放的端点，您可以使用它们从 Hub 中检索信息，以及执行某些操作，如创建模型、数据集或 Space 仓库。我们提供一个包装的 Python 库，[`huggingface_hub`](https://github.com/huggingface/huggingface_hub)，可以轻松访问这些端点。我们还提供 webhooks 以接收有关仓库的实时增量信息。享受吧！

以下端点的基本 URL 是`https://huggingface.co`。例如，要构建下面的`/api/models`调用，可以调用 URL [`huggingface.co/api/models`](https://huggingface.co/api/models)。

## Hub API 游乐场

想尝试我们的 API 吗？现在在我们的[游乐场](https://huggingface.co/spaces/enzostvs/hub-api-playground)上试试吧！

![](https://huggingface.co/spaces/enzostvs/hub-api-playground)

## 仓库列表 API

以下端点帮助获取有关存储在 Hub 上的模型、数据集、Spaces 和指标的信息。

在进行 API 调用以检索有关仓库的信息时，`createdAt`属性指示了相应仓库创建的时间。重要的是要注意，对于在我们开始存储创建日期之前创建的所有仓库，都分配了一个唯一值`2022-03-02T23:29:04.000Z`。

### GET /api/models

从 Hub 中的所有模型获取信息。响应是分页的，使用[`Link`头](https://docs.github.com/en/rest/guides/using-pagination-in-the-rest-api?apiVersion=2022-11-28#link-header)来获取下一页。您可以指定额外的参数以获得更具体的结果。

+   `search`: 根据仓库和用户名的子字符串进行过滤，例如`resnet`或`microsoft`。

+   `author`: 根据作者或组织过滤模型，例如`huggingface`或`microsoft`。

+   `filter`: 根据标签进行过滤，例如`text-classification`或`spacy`。

+   `sort`: 用于排序的属性，例如`downloads`或`author`。

+   `direction`: 排序的方向，例如`-1`表示降序，其他任何值表示升序。

+   `limit`: 限制获取的模型数量。

+   `full`: 是否获取大部分模型数据，例如所有标签、文件等。

+   `config`: 是否也获取仓库配置。

负载：

```py
params = {
    "search":"search",
    "author":"author",
    "filter":"filter",
    "sort":"sort",
    "direction":"direction",
    "limit":"limit",
    "full":"full",
    "config":"config"
}
```

这相当于`huggingface_hub.list_models()`。

### GET /api/models/{repo_id} 或 /api/models/{repo_id}/revision/{revision}

获取特定模型的所有信息。

这相当于`huggingface_hub.model_info(repo_id, revision)`。

### GET /api/models-tags-by-type

获取托管在 Hub 上的所有可用模型标签。

这相当于`huggingface_hub.get_model_tags()`。

### GET /api/datasets

从 Hub 中的所有数据集获取信息。响应是分页的，使用[`Link`头](https://docs.github.com/en/rest/guides/using-pagination-in-the-rest-api?apiVersion=2022-11-28#link-header)来获取下一页。您可以指定额外的参数以获得更具体的结果。

+   `search`: 根据仓库和用户名的子字符串进行过滤，例如`pets`或`microsoft`。

+   `author`: 根据作者或组织过滤数据集，例如`huggingface`或`microsoft`。

+   `filter`: 根据标签进行过滤，例如`task_categories:text-classification`或`languages:en`。

+   `sort`: 用于排序的属性，例如`downloads`或`author`。

+   `direction`: 排序的方向，例如`-1`表示降序，其他任何值表示升序。

+   `limit`: 限制获取的数据集数量。

+   `full`: 是否获取大部分数据集数据，例如所有标签、文件等。

负载：

```py
params = {
    "search":"search",
    "author":"author",
    "filter":"filter",
    "sort":"sort",
    "direction":"direction",
    "limit":"limit",
    "full":"full",
    "config":"config"
}
```

这相当于`huggingface_hub.list_datasets()`。

### GET /api/datasets/{repo_id} 或 /api/datasets/{repo_id}/revision/{revision}

获取特定数据集的所有信息。

+   `full`: 是否获取大部分数据集数据，例如所有标签、文件等。

负载：

```py
params = {"full": "full"}
```

这相当于`huggingface_hub.dataset_info(repo_id, revision)`。

### GET /api/datasets/{repo_id}/parquet

获取自动转换的 parquet 文件列表。

### GET /api/datasets/{repo_id}/parquet/{config}/{split}/{n}.parquet

获取自动转换的 parquet 文件的第 n 个分片。

### GET /api/datasets-tags-by-type

获取 Hub 中托管的所有可用数据集标签。

这相当于`huggingface_hub.get_dataset_tags()`。

### GET /api/spaces

从 Hub 中获取所有空间的信息。响应进行了分页处理，使用[`Link`标头](https://docs.github.com/en/rest/guides/using-pagination-in-the-rest-api?apiVersion=2022-11-28#link-header)获取下一页。您可以指定其他参数以获得更具体的结果。

+   `search`：基于存储库和用户名的子字符串进行筛选，例如`resnet`或`microsoft`

+   `author`：按作者或组织筛选模型，例如`huggingface`或`microsoft`

+   `filter`：基于标签进行筛选，例如`text-classification`或`spacy`。

+   `sort`：用于排序的属性，例如`downloads`或`author`。

+   `direction`：排序方向，例如`-1`表示降序，其他任何值表示升序。

+   `limit`：限制获取的模型数量。

+   `full`：是否获取大部分模型数据，例如所有标签、文件等。

负载：

```py
params = {
    "search":"search",
    "author":"author",
    "filter":"filter",
    "sort":"sort",
    "direction":"direction",
    "limit":"limit",
    "full":"full",
    "config":"config"
}
```

这相当于`huggingface_hub.list_spaces()`。

### GET /api/spaces/{repo_id}或/api/spaces/{repo_id}/revision/{revision}

获取特定模型的所有信息。

这相当于`huggingface_hub.space_info(repo_id, revision)`。

## 存储库 API

以下端点管理存储库设置，如创建和删除存储库。

### POST /api/repos/create

创建一个存储库。默认情况下是一个模型存储库。

参数：

+   `type`：存储库类型（数据集或空间；默认为模型）。

+   `name`：存储库名称。

+   `organization`：组织名称（可选）。

+   `private`：存储库是否为私有。

+   `sdk`：当类型为`space`（streamlit、gradio、docker 或静态）时。

负载：

```py
payload = {
    "type":"model",
    "name":"name",
    "organization": "organization",
    "private":"private",
    "sdk": "sdk"
}
```

这相当于`huggingface_hub.create_repo()`。

### DELETE /api/repos/delete

删除一个存储库。默认情况下是一个模型存储库。

参数：

+   `type`：存储库类型（数据集或空间；默认为模型）。

+   `name`：存储库名称。

+   `organization`：组织名称（可选）。

负载：

```py
payload = {
    "type": "model",
    "name": "name",
    "organization": "organization",
}
```

这相当于`huggingface_hub.delete_repo()`。

### PUT /api/repos/{repo_type}/{repo_id}/settings

更新存储库可见性。

负载：

```py
payload = {
    "private": "private",
}
```

这相当于`huggingface_hub.update_repo_visibility()`。

### POST /api/repos/move

移动存储库（在同一命名空间内重命名或从用户转移到组织）。

参数：

+   `fromRepo`：要重命名的存储库。

+   `toRepo`：存储库的新名称。

+   `type`：存储库类型（数据集或空间；默认为模型）。

负载：

```py
payload = {
    "fromRepo" : "namespace/repo_name",
    "toRepo" : "namespace2/repo_name2",
    "type": "model",
}
```

这相当于`huggingface_hub.move_repo()`。

## 用户 API

以下端点获取有关用户的信息。

### GET /api/whoami-v2

获取用户名和用户所属的组织。

负载：

```py
headers = { "authorization" :  "Bearer $token" }
```

这相当于`huggingface_hub.whoami()`。

## 集合 API

使用集合将 Hub 上的存储库（模型、数据集、空间和论文）分组到专用页面上。

您可以在集合指南中了解更多信息。集合也可以使用 Python 客户端进行管理（参见[指南](https://huggingface.co/docs/huggingface_hub/main/en/guides/collections)）。

### POST /api/collections

在 Hub 上创建一个新的集合，包括标题、描述（可选）和第一个项目（可选）。项目由类型（`model`、`dataset`、`space`或`paper`）和 ID（Hub 上的 repo_id 或 paper_id）定义。

负载：

```py
payload = {
    "title": "My cool models",
    "namespace": "username_or_org",
    "description": "Here is a shortlist of models I've trained.",
    "item" : {
        "type": "model",
        "id": "username/cool-model",
    }
    "private": false,

}
```

这相当于`huggingface_hub.create_collection()`。

### GET /api/collections/{namespace}/{slug}-{id}

返回有关集合的信息。

这相当于`huggingface_hub.get_collection()`。

### GET /api/collections

根据一些条件从 Hub 中列出集合。支持的参数包括：

+   `owner`（字符串）：按特定用户或组织创建的集合进行筛选。

+   `item`（字符串）：过滤包含特定项目的集合。值必须是 item_type 和 item_id 连接的。示例：`"models/teknium/OpenHermes-2.5-Mistral-7B"`，`"datasets/squad"`或`"papers/2311.12983"`。

+   `sort`（字符串）：对返回的集合进行排序。支持的值为`"lastModified"`，`"trending"`（默认）和`"upvotes"`。

+   `limit`（整数）：每页最多的集合数量（100）。

+   `q`（字符串）：基于标题和描述的子字符串进行过滤。

如果未设置任何参数，则返回所有集合。

响应是分页的。要获取所有集合，必须遵循[`Link` header](https://docs.github.com/en/rest/guides/using-pagination-in-the-rest-api?apiVersion=2022-11-28#link-header)。

在列出集合时，每个集合的项目列表被截断为最多 4 个项目。要检索集合中的所有项目，您需要使用其集合 slug 进行额外调用。

负载：

```py
params = {
    "owner": "TheBloke",
    "item": "models/teknium/OpenHermes-2.5-Mistral-7B",
    "sort": "lastModified",
    "limit" : 1,
}
```

这相当于`huggingface_hub.list_collections()`。

### PATCH /api/collections/{namespace}/{slug}-{id}

更新 Hub 上集合的元数据。您不能使用此方法添加或修改集合中的项目。负载的所有字段都是可选的。

负载：

```py
payload = {
    "title": "My cool models",
    "description": "Here is a shortlist of models I've trained.",
    "private": false,
    "position": 0, // position of the collection on your profile
    "theme": "green",
}
```

这相当于`huggingface_hub.update_collection_metadata()`。

### DELETE /api/collections/{namespace}/{slug}-{id}

返回一个集合。这是一个不可逆的操作。删除的集合无法恢复。

这相当于`huggingface_hub.delete_collection()`。

### POST /api/collections/{namespace}/{slug}-{id}/item

向集合添加项目。项目由类型（`model`，`dataset`，`space`或`paper`）和 id（Hub 上的 repo_id 或 paper_id）定义。还可以附加一个注释到项目（可选）。

负载：

```py
payload = {
    "item" : {
        "type": "model",
        "id": "username/cool-model",
    }
    "note": "Here is the model I trained on ...",
}
```

这相当于`huggingface_hub.add_collection_item()`。

### PATCH /api/collections/{namespace}/{slug}-{id}/items/{item_id}

更新集合中的项目。您必须知道项目对象的 id，该 id 与将项目添加到集合时提供的 repo_id/paper_id 不同。`item_id`可以通过获取集合来检索。

您可以更新附加到项目的注释或项目在集合中的位置。这两个字段都是可选的。

```py
payload = {
    "position": 0,
    "note": "Here is the model I trained on ...",
}
```

这相当于`huggingface_hub.update_collection_item()`。

### DELETE /api/collections/{namespace}/{slug}-{id}/items/{item_id}

从集合中删除项目。您必须知道项目对象的 id，该 id 与将项目添加到集合时提供的 repo_id/paper_id 不同。`item_id`可以通过获取集合来检索。

这相当于`huggingface_hub.delete_collection_item()`。
