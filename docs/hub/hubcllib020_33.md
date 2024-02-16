# 推理终端

> 原文：[`huggingface.co/docs/huggingface_hub/package_reference/inference_endpoints`](https://huggingface.co/docs/huggingface_hub/package_reference/inference_endpoints)

推理终端提供了一个安全的生产解决方案，可以轻松部署模型到由 Hugging Face 管理的专用和自动缩放基础设施上。推理终端是从[Hub](https://huggingface.co/models)中的模型构建的。本页是关于`huggingface_hub`与推理终端集成的参考。有关推理终端产品的更多信息，请查看其[官方文档](https://huggingface.co/docs/inference-endpoints/index)。

查看相关指南以了解如何使用`huggingface_hub`以编程方式管理您的推理终端。

推理终端可以通过 API 完全管理。终端的文档在[Swagger](https://api.endpoints.huggingface.cloud/)中记录。InferenceEndpoint 类是在此 API 之上构建的简单包装器。

## 方法

推理终端的一部分功能在 HfApi 中实现：

+   get_inference_endpoint()和 list_inference_endpoints()用于获取有关您的推理终端的信息

+   create_inference_endpoint()、update_inference_endpoint()和 delete_inference_endpoint()用于部署和管理推理终端

+   pause_inference_endpoint()和 resume_inference_endpoint()用于暂停和恢复推理终端

+   scale_to_zero_inference_endpoint()用于手动将终端缩放为 0 个副本

## 推理终端

主要的数据类是 InferenceEndpoint。它包含有关部署的`InferenceEndpoint`的信息，包括其配置和当前状态。部署后，您可以使用 InferenceEndpoint.client 和 InferenceEndpoint.async_client 属性在终端上运行推理，分别返回一个 InferenceClient 和一个 AsyncInferenceClient 对象。

### `class huggingface_hub.InferenceEndpoint`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L44)

```py
( namespace: str raw: Dict _token: Optional _api: HfApi )
```

参数

+   `name` (`str`) — 推理终端的唯一名称。

+   `namespace` (`str`) — 推理终端所在的命名空间。

+   `repository` (`str`) — 部署在此推理终端上的模型库的名称。

+   `status` (InferenceEndpointStatus) — 推理端点的当前状态。

+   `url` (`str`, *可选*) — 推理端点的 URL，如果可用。只有已部署的推理端点才会有 URL。

+   `framework` (`str`) — 用于模型的机器学习框架。

+   `revision` (`str`) — 部署在推理端点上的特定模型修订版。

+   `task` (`str`) — 与部署模型相关联的任务。

+   `created_at` (`datetime.datetime`) — 推理端点创建时的时间戳。

+   `updated_at` (`datetime.datetime`) — 推理端点的最后更新时间戳。

+   `type` (InferenceEndpointType) — 推理端点的类型（公共、受保护、私有）。

+   `raw` (`Dict`) — 从 API 返回的原始字典数据。

+   `token` (`str`, *可选*) — 推理端点的身份验证令牌，如果在请求 API 时设置。

包含有关部署的推理端点的信息。

示例：

```py
>>> from huggingface_hub import get_inference_endpoint
>>> endpoint = get_inference_endpoint("my-text-to-image")
>>> endpoint
InferenceEndpoint(name='my-text-to-image', ...)

# Get status
>>> endpoint.status
'running'
>>> endpoint.url
'https://my-text-to-image.region.vendor.endpoints.huggingface.cloud'

# Run inference
>>> endpoint.client.text_to_image(...)

# Pause endpoint to save $$$
>>> endpoint.pause()

# ...
# Resume and wait for deployment
>>> endpoint.resume()
>>> endpoint.wait()
>>> endpoint.client.text_to_image(...)
```

#### `from_raw`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L126)

```py
( raw: Dict namespace: str token: Optional = None api: Optional = None )
```

从原始字典初始化对象。

#### `client`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L145)

```py
( ) → export const metadata = 'undefined';InferenceClient
```

返回

InferenceClient

指向已部署端点的推理客户端。

引发

InferenceEndpointError

+   InferenceEndpointError — 如果推理端点尚未部署。

返回一个客户端，用于对此推理端点进行预测。

#### `async_client`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L162)

```py
( ) → export const metadata = 'undefined';AsyncInferenceClient
```

返回

AsyncInferenceClient

指向已部署端点的与 asyncio 兼容的推理客户端。

引发

InferenceEndpointError

+   InferenceEndpointError — 如果推理端点尚未部署。

返回一个客户端，用于对此推理端点进行预测。

#### `delete`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L346)

```py
( )
```

删除推理端点。

此操作不可逆。如果您不想为推理端点付费，最好使用 InferenceEndpoint.pause()暂停它，或者使用 InferenceEndpoint.scale_to_zero()将其缩减为零。

这是 HfApi.delete_inference_endpoint()的别名。

#### `fetch`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L217)

```py
( ) → export const metadata = 'undefined';InferenceEndpoint
```

返回

InferenceEndpoint

相同的推理端点，在原地变异为最新数据。

获取有关推理端点的最新信息。

#### `pause`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L296)

```py
( ) → export const metadata = 'undefined';InferenceEndpoint
```

返回

InferenceEndpoint

相同的推理端点，在原地改变以获取最新数据。

暂停推理端点。

暂停的推理端点不会收费。可以随时使用 InferenceEndpoint.resume()来恢复。这与使用 InferenceEndpoint.scale_to_zero()将推理端点缩减到零不同，后者在请求到达时会自动重新启动。

这是[HfApi.pause_inference_endpoint()]的别名(/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.pause_inference_endpoint)。当前对象会被就地改变，以获取来自服务器的最新数据。

#### `resume`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L314)

```py
( ) → export const metadata = 'undefined';InferenceEndpoint
```

返回

InferenceEndpoint

相同的推理端点，在原地改变以获取最新数据。

恢复推理端点。

这是[HfApi.resume_inference_endpoint()]的别名(/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.resume_inference_endpoint)。当前对象会被就地改变，以获取来自服务器的最新数据。

#### `scale_to_zero`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L328)

```py
( ) → export const metadata = 'undefined';InferenceEndpoint
```

返回

InferenceEndpoint

相同的推理端点，在原地改变以获取最新数据。

将推理端点缩减到零。

缩减到零的推理端点不会收费。在下一次请求时会恢复，但会有冷启动延迟。这与使用 InferenceEndpoint.pause()暂停推理端点不同，后者需要使用 InferenceEndpoint.resume()手动恢复。

这是[HfApi.scale_to_zero_inference_endpoint()]的别名(/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.scale_to_zero_inference_endpoint)。当前对象会被就地改变，以获取来自服务器的最新数据。

#### `update`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L228)

```py
( accelerator: Optional = None instance_size: Optional = None instance_type: Optional = None min_replica: Optional = None max_replica: Optional = None repository: Optional = None framework: Optional = None revision: Optional = None task: Optional = None ) → export const metadata = 'undefined';InferenceEndpoint
```

参数

+   `accelerator` (`str`, *optional*) — 用于推理的硬件加速器（例如 `"cpu"`）。

+   `instance_size` (`str`, *optional*) — 用于托管模型的实例的大小或类型（例如 `"large"`）。

+   `instance_type` (`str`, *optional*) — 推理端点将部署的云实例类型（例如 `"c6i"`）。

+   `min_replica` (`int`, *optional*) — 推理端点保持运行的最小副本（实例）数量。

+   `max_replica` (`int`, *optional*) — 推理端点扩展到的最大副本（实例）数量。

+   `repository` (`str`, *optional*) — 与推理端点关联的模型存储库的名称（例如 `"gpt2"`）。

+   `framework` (`str`, *optional*) — 用于模型的机器学习框架（例如 `"custom"`）。

+   `revision` (`str`, *optional*) — 部署在推理端点上的特定模型修订版（例如 `"6c0e6080953db56375760c0471a8c5f2929baf11"`）。

+   `task` (`str`, *optional*) — 部署模型的任务（例如 `"text-classification"`）。

返回

InferenceEndpoint

相同的推理端点，就地修改为最新数据。

更新推理端点。

此方法允许更新计算配置、部署的模型或两者。所有参数都是可选的，但至少必须提供一个。

这是 HfApi.update_inference_endpoint() 的别名。当前对象将被就地修改为来自服务器的最新数据。

#### `wait`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L179)

```py
( timeout: Optional = None refresh_every: int = 5 ) → export const metadata = 'undefined';InferenceEndpoint
```

参数

+   `timeout` (`int`, *optional*) — 等待推理端点部署的最长时间，单位为秒。如果为 `None`，将无限期等待。

+   `refresh_every` (`int`, *optional*) — 每次获取推理端点状态之间等待的时间，单位为秒。默认为 5 秒。

返回

InferenceEndpoint

相同的推理端点，就地修改为最新数据。

等待推理端点部署。

从服务器获取的信息将每 1 秒刷新一次。如果在 `timeout` 秒后推理端点未部署，将引发 `InferenceEndpointTimeoutError`。InferenceEndpoint 将被就地修改为最新数据。

## InferenceEndpointStatus

### `class huggingface_hub.InferenceEndpointStatus`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L27)

```py
( value names = None module = None qualname = None type = None start = 1 )
```

一个枚举。

## InferenceEndpointType

### `class huggingface_hub.InferenceEndpointType`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L38)

```py
( value names = None module = None qualname = None type = None start = 1 )
```

一个枚举。

## InferenceEndpointError

### `class huggingface_hub.InferenceEndpointError`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_inference_endpoints.py#L19)

```py
( )
```

处理推理端点时的通用异常。
