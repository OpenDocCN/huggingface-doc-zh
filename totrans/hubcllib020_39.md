# 管理您的Space运行时

> 原文：[https://huggingface.co/docs/huggingface_hub/package_reference/space_runtime](https://huggingface.co/docs/huggingface_hub/package_reference/space_runtime)

查看[HfApi](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi)文档页面，了解如何管理Hub上的Space的方法。

+   复制一个Space：[duplicate_space()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.duplicate_space)

+   获取当前运行时：[get_space_runtime()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.get_space_runtime)

+   管理秘密：[add_space_secret()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.add_space_secret) 和 [delete_space_secret()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.delete_space_secret)

+   管理硬件：[request_space_hardware()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.request_space_hardware)

+   管理状态：[pause_space()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.pause_space), [restart_space()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.restart_space), [set_space_sleep_time()](/docs/huggingface_hub/v0.20.3/en/package_reference/hf_api#huggingface_hub.HfApi.set_space_sleep_time)

## 数据结构

### SpaceRuntime

### `class huggingface_hub.SpaceRuntime`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_space_api.py#L89)

```py
( data: Dict )
```

参数

+   `stage` (`str`) — Space的当前阶段。示例：RUNNING。

+   `hardware` (`str` 或 `None`) — Space的当前硬件。示例：“cpu-basic”。如果Space第一次处于 `BUILDING` 状态，则可以为 `None`。

+   `requested_hardware` (`str` 或 `None`) — 请求的硬件。可以与 `hardware` 不同，特别是如果刚刚发出请求。示例：“t4-medium”。如果尚未请求硬件，则可以为 `None`。

+   `sleep_time` (`int` 或 `None`) — Space在最后一次请求后保持活动的秒数。默认情况下（如果值为 `None`），如果在升级后的硬件上运行，则Space永远不会进入休眠状态，而在免费的‘cpu-basic’硬件上运行时，将在48小时后进入休眠状态。更多详情请参阅 [https://huggingface.co/docs/hub/spaces-gpus#sleep-time](https://huggingface.co/docs/hub/spaces-gpus#sleep-time)。

+   `raw` (`dict`) — 从服务器返回的原始响应。包含有关Space运行时的更多信息，如副本数量、cpu数量、内存大小等。

包含有关Space当前运行时的信息。

### SpaceHardware

### `class huggingface_hub.SpaceHardware`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_space_api.py#L48)

```py
( value names = None module = None qualname = None type = None start = 1 )
```

在Hub上可用于运行Space的硬件的枚举。

值可以与字符串进行比较：

```py
assert SpaceHardware.CPU_BASIC == "cpu-basic"
```

取自 [https://github.com/huggingface/moon-landing/blob/main/server/repo_types/SpaceInfo.ts#L73](https://github.com/huggingface/moon-landing/blob/main/server/repo_types/SpaceInfo.ts#L73)（私有链接）。

### SpaceStage

### `class huggingface_hub.SpaceStage`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_space_api.py#L23)

```py
( value names = None module = None qualname = None type = None start = 1 )
```

在Hub上对Space可能的阶段进行枚举。

值可以与字符串进行比较：

```py
assert SpaceStage.BUILDING == "BUILDING"
```

取自 [https://github.com/huggingface/moon-landing/blob/main/server/repo_types/SpaceInfo.ts#L61](https://github.com/huggingface/moon-landing/blob/main/server/repo_types/SpaceInfo.ts#L61)（私有链接）。

### SpaceStorage

### `class huggingface_hub.SpaceStorage`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_space_api.py#L72)

```py
( value names = None module = None qualname = None type = None start = 1 )
```

在Hub上为您的Space提供的持久存储的枚举。

值可以与字符串进行比较：

```py
assert SpaceStorage.SMALL == "small"
```

取自[https://github.com/huggingface/moon-landing/blob/main/server/repo_types/SpaceHardwareFlavor.ts#L24](https://github.com/huggingface/moon-landing/blob/main/server/repo_types/SpaceHardwareFlavor.ts#L24)（私有链接）。

### SpaceVariable

### `class huggingface_hub.SpaceVariable`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_space_api.py#L129)

```py
( key: str values: Dict )
```

参数

+   `key` (`str`) — 变量键。示例："MODEL_REPO_ID"

+   `value` (`str`) — 变量值。示例："the_model_repo_id"。

+   `description` (`str` 或 None) — 变量描述。示例："实现模型的模型仓库ID"。

+   `updatedAt` (`datetime`) — 变量的最后更新时间。

包含有关Space当前变量的信息。
