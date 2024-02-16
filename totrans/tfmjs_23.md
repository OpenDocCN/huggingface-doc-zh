# env

> 原文：[`huggingface.co/docs/transformers.js/api/env`](https://huggingface.co/docs/transformers.js/api/env)

用于配置 Transformers.js 的模块。

**示例：** 禁用远程模型。

```py
import { env } from '@xenova/transformers';
env.allowRemoteModels = false;
```

**示例：** 设置本地模型路径。

```py
import { env } from '@xenova/transformers';
env.localModelPath = '/path/to/local/models/';
```

**示例：** 设置缓存目录。

```py
import { env } from '@xenova/transformers';
env.cacheDir = '/path/to/cache/directory/';
```

* * *

## env.env

用于控制执行的全局变量。这为用户提供了一种简单的方法来配置 Transformers.js。

**种类**：`env` 的静态常量

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| backends | `Object` | 公开不同后端的环境变量，允许用户设置这些变量。 |
| __dirname | `string` | 模块的目录名称。用于解析本地路径。 |
| version | `string` | Transformers.js 的版本。 |
| allowRemoteModels | `boolean` | 是否允许加载远程文件，默认为 `true`。如果设置为 `false`，则在加载管道、模型、分词器、处理器等时将具有与设置 `local_files_only=true` 相同的效果。 |
| remoteHost | `string` | 用于加载模型的主机 URL。默认为 Hugging Face Hub。 |
| remotePathTemplate | `string` | 加载模型时要填充并附加到 `remoteHost` 的路径模板。 |
| allowLocalModels | `boolean` | 是否允许加载本地文件，默认为 `true`。如果设置为 `false`，则将跳过本地文件检查，并尝试从远程主机加载模型。 |
| localModelPath | `string` | 从中加载本地模型的路径。默认为 `/models/`。 |
| useFS | `boolean` | 是否使用文件系统加载文件。默认情况下，如果可用，则为 `true`。 |
| useBrowserCache | `boolean` | 是否使用缓存 API 缓存模型。默认情况下，如果可用，则为 `true`。 |
| useFSCache | `boolean` | 是否使用文件系统缓存文件。默认情况下，如果可用，则为 `true`。 |
| cacheDir | `string` | 用于使用文件系统缓存文件的目录。默认为 `./.cache`。 |
| useCustomCache | `boolean` | 是否使用自定义缓存系统（由 `customCache` 定义），默认为 `false`。 |
| customCache | `Object` | 要使用的自定义缓存。默认为 `null`。注意：这必须是一个实现 Web 缓存 API 的 `match` 和 `put` 函数的对象。有关更多信息，请参见 [`developer.mozilla.org/en-US/docs/Web/API/Cache`](https://developer.mozilla.org/en-US/docs/Web/API/Cache) |

* * *
