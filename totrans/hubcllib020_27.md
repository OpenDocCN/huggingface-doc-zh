# 环境变量

> 原始文本：[`huggingface.co/docs/huggingface_hub/package_reference/environment_variables`](https://huggingface.co/docs/huggingface_hub/package_reference/environment_variables)

可以使用环境变量配置`huggingface_hub`。

如果您对环境变量不熟悉，这里有关于它们的通用文章[在 macOS 和 Linux 上](https://linuxize.com/post/how-to-set-and-list-environment-variables-in-linux/)以及在[Windows 上](https://phoenixnap.com/kb/windows-set-environment-variable)。

本页面将指导您了解所有特定于`huggingface_hub`的环境变量及其含义。

## 通用

### HF_INFERENCE_ENDPOINT

用于配置推理 API 的基本 URL。如果您的组织指向 API 网关而不是直接指向推理 API，则可能需要设置此变量。

默认为`"https://api-inference.huggingface.com"`。

### HF_HOME

用于配置`huggingface_hub`将本地存储数据的位置。特别是，您的令牌和缓存将存储在此文件夹中。

默认为`"~/.cache/huggingface"`，除非设置了 XDG_CACHE_HOME。

### HF_HUB_CACHE

配置 Hub 存储库（模型、数据集和空间）在本地缓存的位置。

默认为`"$HF_HOME/hub"`（例如，默认情况下为`"~/.cache/huggingface/hub"`）。

### HF_ASSETS_CACHE

用于配置由下游库创建的资产在本地缓存的位置。这些资产可以是预处理数据、从 GitHub 下载的文件、日志等。

默认为`"$HF_HOME/assets"`（例如，默认情况下为`"~/.cache/huggingface/assets"`）。

### HF_TOKEN

用于配置用户访问令牌以对 Hub 进行身份验证。如果设置，此值将覆盖存储在机器上的令牌（在`"$HF_HOME/token"`中）。

有关身份验证的更多详细信息，请查看此部分。

### HF_HUB_VERBOSITY

设置`huggingface_hub`的日志记录器的详细程度。必须是`{"debug", "info", "warning", "error", "critical"}`中的一个。

默认为`"warning"`。

有关更多详细信息，请参阅日志参考。

### HF_HUB_LOCAL_DIR_AUTO_SYMLINK_THRESHOLD

用于定义文件被视为“小”文件的大小的整数值。在将文件下载到本地目录时，小文件将被复制以便于用户体验，而较大的文件将被创建符号链接以节省磁盘使用空间。

有关更多详细信息，请参阅下载指南。

### HF_HUB_ETAG_TIMEOUT

用于定义在从存储库获取最新元数据之前等待服务器响应的秒数的整数值。如果请求超时，`huggingface_hub`将默认使用本地缓存的文件。设置较低的值可以加快对已缓存文件的机器工作流程。较高的值可以确保元数据调用在更多情况下成功。默认为 10 秒。

### HF_HUB_DOWNLOAD_TIMEOUT

用于定义下载文件时等待服务器响应的秒数的整数值。如果请求超时，将引发 TimeoutError。在连接速度较慢的机器上设置较高的值是有益的。较小的值会在完全网络中断的情况下更快地使进程失败。默认为 10 秒。

## 布尔值

以下环境变量期望一个布尔值。如果其值为`{"1", "ON", "YES", "TRUE"}`中的一个（不区分大小写），则该变量将被视为`True`。任何其他值（或未定义）将被视为`False`。

### HF_HUB_OFFLINE

如果设置，将不会向 Hugging Face Hub 发出任何 HTTP 调用。如果尝试下载文件，只会访问缓存文件。如果未检测到缓存文件，则会引发错误。这在您的网络速度较慢且您不关心文件的最新版本时非常有用。

如果设置了环境变量 `HF_HUB_OFFLINE=1` 并调用 HfApi 的任何方法，将引发 OfflineModeIsEnabled 异常。

**注意：** 即使文件的最新版本已被缓存，调用 `hf_hub_download` 仍会触发 HTTP 请求以检查是否有新版本可用。设置 `HF_HUB_OFFLINE=1` 将跳过此调用，加快加载时间。

### HF_HUB_DISABLE_IMPLICIT_TOKEN

对于 Hub 的每个请求，身份验证并非是强制性的。例如，请求关于 `"gpt2"` 模型的详细信息不需要进行身份验证。但是，如果用户已经登录，默认行为将始终发送令牌，以便在访问私有或受限制的存储库时简化用户体验（永远不会收到 HTTP 401 未经授权）。出于隐私考虑，您可以通过设置 `HF_HUB_DISABLE_IMPLICIT_TOKEN=1` 来禁用此行为。在这种情况下，令牌仅在“写访问”调用时发送（例如：创建提交）。

**注意：** 禁用隐式发送令牌可能会产生奇怪的副作用。例如，如果您想在 Hub 上列出所有模型，您的私有模型将不会被列出。您需要在脚本中显式传递 `token=True` 参数。

### HF_HUB_DISABLE_PROGRESS_BARS

对于耗时的任务，默认情况下 `huggingface_hub` 显示一个进度条（使用 tqdm）。您可以通过设置 `HF_HUB_DISABLE_PROGRESS_BARS=1` 一次性禁用所有进度条。

### HF_HUB_DISABLE_SYMLINKS_WARNING

如果您使用的是 Windows 机器，建议启用开发者模式或以管理员模式运行 `huggingface_hub`。否则，`huggingface_hub` 将无法在缓存系统中创建符号链接。您仍然可以执行任何脚本，但您的用户体验会降低，因为一些巨大的文件可能会在您的硬盘上重复出现。会触发警告消息来警告您这种行为。设置 `HF_HUB_DISABLE_SYMLINKS_WARNING=1`，以禁用此警告。

有关更多详细信息，请参阅 缓存限制。

### HF_HUB_DISABLE_EXPERIMENTAL_WARNING

`huggingface_hub` 的一些功能是实验性的。这意味着您可以使用它们，但我们不能保证它们将来会得到维护。特别是，我们可能会在没有任何弃用周期的情况下更新这些功能的 API 或行为。在使用实验性功能时会触发警告消息来警告您。如果您愿意调试可能出现的问题，可以设置 `HF_HUB_DISABLE_EXPERIMENTAL_WARNING=1` 来禁用警告。

如果您正在使用实验性功能，请告诉我们！您的反馈可以帮助我们设计和改进它。

### HF_HUB_DISABLE_TELEMETRY

默认情况下，HF 库（`transformers`、`datasets`、`gradio` 等）会收集一些数据以监控使用情况、调试问题并帮助优先考虑功能。每个库都定义自己的策略（即要监控的使用情况），但核心实现发生在 `huggingface_hub` 中（参见 `send_telemetry`）。

您可以将 `HF_HUB_DISABLE_TELEMETRY=1` 设置为环境变量以全局禁用遥测。

### HF_HUB_ENABLE_HF_TRANSFER

设置为 `True` 可以通过 `hf_transfer` 从 Hub 更快地上传和下载。

默认情况下，`huggingface_hub`使用基于 Python 的`requests.get`和`requests.post`函数。虽然这些函数可靠且多功能，但对于带宽较高的机器来说可能不是最有效的选择。[`hf_transfer`](https://github.com/huggingface/hf_transfer)是一个基于 Rust 开发的软件包，旨在通过将大文件分成较小的部分并使用多个线程同时传输它们来最大化使用的带宽。这种方法可能会将传输速度提高一倍。要使用`hf_transfer`，您需要单独安装它[from PyPI](https://pypi.org/project/hf-transfer/)并将`HF_HUB_ENABLE_HF_TRANSFER=1`设置为环境变量。

请注意，使用`hf_transfer`会带来一些限制。由于它不是纯粹基于 Python 的，因此调试错误可能会具有挑战性。此外，`hf_transfer`缺少一些用户友好的功能，如可恢复下载和代理。这些遗漏是有意为之，以保持 Rust 逻辑的简单性和速度。因此，在`huggingface_hub`中，默认情况下不启用`hf_transfer`。

## 已废弃的环境变量

为了使 Hugging Face 生态系统中的所有环境变量标准化，一些变量已被标记为废弃。尽管它们仍然可用，但不再优先于它们的替代项。以下表格概述了废弃变量及其对应的替代项：

| 废弃变量 | 替代 |
| --- | --- |
| `HUGGINGFACE_HUB_CACHE` | `HF_HUB_CACHE` |
| `HUGGINGFACE_ASSETS_CACHE` | `HF_ASSETS_CACHE` |
| `HUGGING_FACE_HUB_TOKEN` | `HF_TOKEN` |
| `HUGGINGFACE_HUB_VERBOSITY` | `HF_HUB_VERBOSITY` |

## 来自外部工具

一些环境变量不是特定于`huggingface_hub`，但在设置时仍会被考虑。

### DO_NOT_TRACK

布尔值。等同于`HF_HUB_DISABLE_TELEMETRY`。当设置为 true 时，在 Hugging Face Python 生态系统（`transformers`，`diffusers`，`gradio`等）中全局禁用遥测。有关更多详细信息，请参见[`consoledonottrack.com/`](https://consoledonottrack.com/)。

### NO_COLOR

布尔值。当设置时，`huggingface-cli`工具将不打印任何 ANSI 颜色。请参见[no-color.org](https://no-color.org/)。

### XDG_CACHE_HOME

仅在未设置`HF_HOME`时使用！

这是在 Linux 机器上配置[用户特定的非必要（缓存）数据应写入的默认方式](https://wiki.archlinux.org/title/XDG_Base_Directory)。

如果未设置`HF_HOME`，默认主目录将是`"$XDG_CACHE_HOME/huggingface"`，而不是`"~/.cache/huggingface"`。
