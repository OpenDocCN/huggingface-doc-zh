# 搜索 Hub

> 原文链接：[https://huggingface.co/docs/huggingface_hub/guides/search](https://huggingface.co/docs/huggingface_hub/guides/search)

在本教程中，您将学习如何使用 `huggingface_hub` 在 Hub 上搜索模型、数据集和空间。

## 如何列出存储库？

`huggingface_hub` 库包括一个 HTTP 客户端 [HfApi](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi) 与 Hub 交互。除其他功能外，它可以列出在 Hub 上存储的模型、数据集和空间：

```py
>>> from huggingface_hub import HfApi
>>> api = HfApi()
>>> models = api.list_models()
```

[list_models()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_models) 的输出是在 Hub 上存储的模型的迭代器。

同样，您可以使用 [list_datasets()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_datasets) 列出数据集，使用 [list_spaces()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.list_spaces) 列出空间。

## 如何过滤存储库？

列出存储库很好，但现在您可能想要过滤搜索。列表助手具有多个属性，如：

+   `filter`

+   `author`

+   `search`

+   …

这些参数中有两个是直观的（`author` 和 `search`），但 `filter` 呢？`filter` 接受一个 [ModelFilter](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.ModelFilter) 对象（或 [DatasetFilter](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.DatasetFilter)）。您可以通过指定要过滤的模型来实例化它。

让我们看一个示例，获取在 Hub 上进行图像分类、已在 imagenet 数据集上训练并且使用 PyTorch 运行的所有模型。这可以通过单个 [ModelFilter](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.ModelFilter) 完成。属性组合为“逻辑 AND”。

```py
models = hf_api.list_models(
    filter=ModelFilter(
		task="image-classification",
		library="pytorch",
		trained_dataset="imagenet"
	)
)
```

在过滤时，您还可以对模型进行排序并仅获取前几个结果。例如，以下示例获取了 Hub 上下载量最高的前 5 个数据集：

```py
>>> list(list_datasets(sort="downloads", direction=-1, limit=5))
[DatasetInfo(
	id='argilla/databricks-dolly-15k-curated-en',
	author='argilla',
	sha='4dcd1dedbe148307a833c931b21ca456a1fc4281', 
	last_modified=datetime.datetime(2023, 10, 2, 12, 32, 53, tzinfo=datetime.timezone.utc), 
	private=False,
	downloads=8889377,
	(...)
```

要探索 Hub 上可用的过滤器，请在浏览器中访问 [models](https://huggingface.co/models) 和 [datasets](https://huggingface.co/datasets) 页面，搜索一些参数并查看 URL 中的值。
