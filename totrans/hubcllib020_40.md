# 管理集合

> 原始文本：[https://huggingface.co/docs/huggingface_hub/package_reference/collections](https://huggingface.co/docs/huggingface_hub/package_reference/collections)

查看 [HfApi](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi) 文档页面，了解如何管理 Hub 上的空间的方法参考。

+   获取集合内容：[get_collection()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.get_collection)

+   创建新集合：[create_collection()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.create_collection)

+   更新集合：[update_collection_metadata()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.update_collection_metadata)

+   删除集合：[delete_collection()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.delete_collection)

+   将项目添加到集合中：[add_collection_item()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.add_collection_item)

+   更新集合中的项目：[update_collection_item()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.update_collection_item)

+   从集合中删除项目：[delete_collection_item()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.delete_collection_item)

### Collection

### `class huggingface_hub.Collection`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L992)

```py
( **kwargs )
```

参数

+   `slug` (`str`) — 集合的 slug。例如 `"TheBloke/recent-models-64f9a55bb3115b4f513ec026"`。

+   `title` (`str`) — 集合的标题。例如 `"Recent models"`。

+   `owner` (`str`) — 集合的所有者。例如 `"TheBloke"`。

+   `items` (`List[CollectionItem]`) — 集合中项目的列表。

+   `last_updated` (`datetime`) — 集合的最后更新日期。

+   `position` (`int`) — 集合在所有者的集合列表中的位置。

+   `private` (`bool`) — 集合是否为私有。

+   `theme` (`str`) — 集合的主题。例如 `"green"`。

+   `upvotes` (`int`) — 集合的点赞数。

+   `description` (`str`, *optional*) — 集合的描述，以纯文本形式。

+   `url` (`str`) — （属性）Hub 上集合的 URL。

包含有关 Hub 上集合的信息。

### CollectionItem

### `class huggingface_hub.CollectionItem`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/hf_api.py#L957)

```py
( _id: str id: str type: CollectionItemType_T position: int note: Optional[Dict] = None **kwargs )
```

参数

+   `item_object_id` (`str`) — 集合中项目的唯一 ID。

+   `item_id` (`str`) — Hub 上底层对象的 ID。可以是 repo_id 或 paper id，例如 `"jbilcke-hf/ai-comic-factory"`、`"2307.09288"`。

+   `item_type` (`str`) — 底层对象的类型。可以是 `"model"`、`"dataset"`、`"space"` 或 `"paper"` 中的一个。

+   `position` (`int`) — 项目在集合中的位置。

+   `note` (`str`, *optional*) — 与项目相关的注释，以纯文本形式。

包含有关集合中项目（模型、数据集、空间或论文）的信息。
