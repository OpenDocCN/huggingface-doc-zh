# 实用工具

> 原文：[https://huggingface.co/docs/huggingface_hub/package_reference/utilities](https://huggingface.co/docs/huggingface_hub/package_reference/utilities)

## 配置日志记录

`huggingface_hub`包提供了一个`logging`工具，用于控制包本身的日志级别。您可以这样导入它：

```py
from huggingface_hub import logging
```

然后，您可以定义详细程度以更新您将看到的日志量：

```py
from huggingface_hub import logging

logging.set_verbosity_error()
logging.set_verbosity_warning()
logging.set_verbosity_info()
logging.set_verbosity_debug()

logging.set_verbosity(...)
```

这些级别应该理解如下：

+   `error`：仅显示关于使用的关键日志，可能会导致错误或意外行为。

+   `warning`：显示不是关键的日志，但使用可能导致意外行为。此外，可能会显示重要的信息性日志。

+   `info`：显示大多数日志，包括一些关于底层发生情况的冗长日志。如果某些行为表现出意外行为，我们建议将详细程度级别切换到此级别以获取更多信息。

+   `debug`：显示所有日志，包括一些内部日志，可用于跟踪底层发生的情况。

#### `huggingface_hub.utils.logging.get_verbosity`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/logging.py#L103)

```py
( )
```

返回HuggingFace Hub的根记录器的当前级别。

HuggingFace Hub具有以下日志级别：

+   `huggingface_hub.logging.CRITICAL`，`huggingface_hub.logging.FATAL`

+   `huggingface_hub.logging.ERROR`

+   `huggingface_hub.logging.WARNING`，`huggingface_hub.logging.WARN`

+   `huggingface_hub.logging.INFO`

+   `huggingface_hub.logging.DEBUG`

#### `huggingface_hub.utils.logging.set_verbosity`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/logging.py#L125)

```py
( verbosity: int )
```

参数

+   `verbosity`（`int`）— 日志级别，例如，`huggingface_hub.logging.DEBUG`和`huggingface_hub.logging.INFO`。

设置HuggingFace Hub的根记录器级别。

#### `huggingface_hub.utils.logging.set_verbosity_info`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/logging.py#L137)

```py
( )
```

将详细程度设置为`logging.INFO`。

#### `huggingface_hub.utils.logging.set_verbosity_debug`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/logging.py#L151)

```py
( )
```

将详细程度设置为`logging.DEBUG`。

#### `huggingface_hub.utils.logging.set_verbosity_warning`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/logging.py#L144)

```py
( )
```

将详细程度设置为`logging.WARNING`。

#### `huggingface_hub.utils.logging.set_verbosity_error`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/logging.py#L158)

```py
( )
```

将详细程度设置为`logging.ERROR`。

#### `huggingface_hub.utils.logging.disable_propagation`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/logging.py#L165)

```py
( )
```

禁用库日志输出的传播。请注意，默认情况下已禁用日志传播。

#### `huggingface_hub.utils.logging.enable_propagation`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/logging.py#L173)

```py
( )
```

启用库日志输出的传播。如果根记录器已配置，请禁用HuggingFace Hub的默认处理程序以防止双重记录。

### 特定于存储库的辅助方法

下面公开的方法在修改`huggingface_hub`库本身的模块时是相关的。如果您使用`huggingface_hub`并且不修改它们，则不应该使用这些方法。

#### `huggingface_hub.utils.logging.get_logger`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/logging.py#L78)

```py
( name: Optional = None )
```

参数

+   `name`（`str`，*可选*）— 要获取的记录器的名称，通常是文件名

返回具有指定名称的记录器。此函数不应直接由库用户访问。

示例：

```py
>>> from huggingface_hub import get_logger

>>> logger = get_logger(__file__)
>>> logger.set_verbosity_info()
```

## 配置进度条

进度条是一种有用的工具，用于在执行长时间任务时向用户显示信息（例如下载或上传文件时）。`huggingface_hub`提供了一个`tqdm`包装器，以一致的方式在整个库中显示进度条。

默认情况下，进度条是启用的。您可以通过设置`HF_HUB_DISABLE_PROGRESS_BARS`环境变量来全局禁用它们。您还可以使用[enable_progress_bars()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.enable_progress_bars)和[disable_progress_bars()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.disable_progress_bars)来启用/禁用它们。如果设置了环境变量，则优先考虑助手。

```py
>>> from huggingface_hub import snapshot_download
>>> from huggingface_hub.utils import are_progress_bars_disabled, disable_progress_bars, enable_progress_bars

>>> # Disable progress bars globally
>>> disable_progress_bars()

>>> # Progress bar will not be shown !
>>> snapshot_download("gpt2")

>>> are_progress_bars_disabled()
True

>>> # Re-enable progress bars globally
>>> enable_progress_bars()
```

### are_progress_bars_disabled

`huggingface_hub.utils.are_progress_bars_disabled`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/tqdm.py#L112)

```py
( )
```

返回全局进度条是否已禁用。

`huggingface_hub`中使用的进度条可以通过[enable_progress_bars()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.enable_progress_bars)和[disable_progress_bars()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.disable_progress_bars)全局启用或禁用，或者通过将`HF_HUB_DISABLE_PROGRESS_BARS`设置为环境变量。

### disable_progress_bars

`huggingface_hub.utils.disable_progress_bars`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/tqdm.py#L78)

```py
( )
```

除非设置了`HF_HUB_DISABLE_PROGRESS_BARS`环境变量，否则禁用`huggingface_hub`中使用的全局进度条。

使用[enable_progress_bars()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.enable_progress_bars)重新启用它们。

### enable_progress_bars

`huggingface_hub.utils.enable_progress_bars`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/tqdm.py#L95)

```py
( )
```

在`huggingface_hub`中全局启用进度条，除非设置了`HF_HUB_DISABLE_PROGRESS_BARS`环境变量。

使用[disable_progress_bars()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.disable_progress_bars)来禁用它们。

## 配置HTTP后端

在某些环境中，您可能希望配置如何进行HTTP调用，例如如果您使用代理。`huggingface_hub`允许您使用[configure_http_backend()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.configure_http_backend)在全局范围内进行配置。然后，所有发送到Hub的请求将使用您的设置。在幕后，`huggingface_hub`使用`requests.Session`，因此您可能希望参考[`requests`文档](https://requests.readthedocs.io/en/latest/user/advanced)以了解更多可用参数。

由于`requests.Session`不能保证是线程安全的，`huggingface_hub`为每个线程创建一个会话实例。使用会话允许我们在HTTP调用之间保持连接打开，并最终节省时间。如果您正在将`huggingface_hub`集成到第三方库中，并希望对Hub进行自定义调用，请使用[get_session()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.get_session)获取由用户配置的会话（即用`get_session().get(...)`替换任何`requests.get(...)`调用）。

`huggingface_hub.configure_http_backend`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_http.py#L98)

```py
( backend_factory: Callable = <function _default_backend_factory at 0x7fafc132f910> )
```

通过提供`backend_factory`来配置HTTP后端。`huggingface_hub`发出的任何HTTP调用都将使用此工厂实例化的会话对象。如果您在特定环境中运行脚本需要自定义配置（例如自定义代理或证书），这可能很有用。

使用[get_session()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.get_session)获取配置的Session。由于`requests.Session`不能保证是线程安全的，`huggingface_hub`为每个线程创建一个Session实例。它们都是使用[configure_http_backend()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.configure_http_backend)中设置的相同的`backend_factory`实例化的。LRU缓存用于在调用之间缓存创建的会话（和连接）。最大大小为128，以避免内存泄漏，如果生成了成千上万个线程。

查看[此问题](https://github.com/psf/requests/issues/2766)以了解更多关于`requests`中线程安全性的信息。

示例：

```py
import requests
from huggingface_hub import configure_http_backend, get_session

# Create a factory function that returns a Session with configured proxies
def backend_factory() -> requests.Session:
    session = requests.Session()
    session.proxies = {"http": "http://10.10.1.10:3128", "https": "https://10.10.1.11:1080"}
    return session

# Set it as the default session factory
configure_http_backend(backend_factory=backend_factory)

# In practice, this is mostly done internally in `huggingface_hub`
session = get_session()
```

#### `huggingface_hub.get_session`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_http.py#L134)

```py
( )
```

获取一个`requests.Session`对象，使用用户的会话工厂。

使用[get_session()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.get_session)获取配置的Session。由于`requests.Session`不能保证是线程安全的，`huggingface_hub`为每个线程创建一个Session实例。它们都是使用[configure_http_backend()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.configure_http_backend)中设置的相同的`backend_factory`实例化的。LRU缓存用于在调用之间缓存创建的会话（和连接）。最大大小为128，以避免内存泄漏，如果生成了成千上万个线程。

查看[此问题](https://github.com/psf/requests/issues/2766)以了解更多关于`requests`中线程安全性的信息。

示例：

```py
import requests
from huggingface_hub import configure_http_backend, get_session

# Create a factory function that returns a Session with configured proxies
def backend_factory() -> requests.Session:
    session = requests.Session()
    session.proxies = {"http": "http://10.10.1.10:3128", "https": "https://10.10.1.11:1080"}
    return session

# Set it as the default session factory
configure_http_backend(backend_factory=backend_factory)

# In practice, this is mostly done internally in `huggingface_hub`
session = get_session()
```

## 处理HTTP错误

`huggingface_hub`定义了自己的HTTP错误，以细化`requests`引发的`HTTPError`，并附加服务器返回的额外信息。

### 提升状态

[hf_raise_for_status()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.hf_raise_for_status)旨在成为从Hub发出的任何请求的“raise for status”的中心方法。它包装了基本的`requests.raise_for_status`以提供额外信息。抛出的任何`HTTPError`都会转换为`HfHubHTTPError`。

```py
import requests
from huggingface_hub.utils import hf_raise_for_status, HfHubHTTPError

response = requests.post(...)
try:
    hf_raise_for_status(response)
except HfHubHTTPError as e:
    print(str(e)) # formatted message
    e.request_id, e.server_message # details returned by server

    # Complete the error message with additional information once it's raised
    e.append_to_message("\n`create_commit` expects the repository to exist.")
    raise
```

#### `huggingface_hub.utils.hf_raise_for_status`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_errors.py#L230)

```py
( response: Response endpoint_name: Optional = None )
```

参数

+   `response`（`Response`）— 服务器返回的响应。

+   `endpoint_name`（`str`，*可选*）— 被调用的端点的名称。如果提供，错误消息将更完整。

`response.raise_for_status()`的内部版本，将细化潜在的HTTPError。引发的异常将是`HfHubHTTPError`的实例。

这个辅助程序旨在成为调用Hugging Face Hub时唯一的`raise_for_status`方法。

示例：

```py
    import requests
    from huggingface_hub.utils import get_session, hf_raise_for_status, HfHubHTTPError

    response = get_session().post(...)
    try:
        hf_raise_for_status(response)
    except HfHubHTTPError as e:
        print(str(e)) # formatted message
        e.request_id, e.server_message # details returned by server

        # Complete the error message with additional information once it's raised
        e.append_to_message("
ate_commit` expects the repository to exist.")
        raise
```

请求失败时引发：

+   [RepositoryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RepositoryNotFoundError) 如果无法找到要下载的仓库。这可能是因为它不存在，因为`repo_type`没有正确设置，或者因为仓库是`private`，而您没有访问权限。

+   [GatedRepoError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.GatedRepoError) 如果仓库存在但是被限制，用户不在授权列表中。

+   [RevisionNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.RevisionNotFoundError) 如果仓库存在但是找不到修订版本。

+   [EntryNotFoundError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.EntryNotFoundError) 如果仓库存在但是条目（例如请求的文件）找不到。

+   [BadRequestError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.BadRequestError) 如果请求失败并出现HTTP 400 BadRequest错误。

+   [HfHubHTTPError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HfHubHTTPError) 如果请求失败原因未列出。

### HTTP错误

这是在`huggingface_hub`中抛出的HTTP错误列表。

#### HfHubHTTPError

`HfHubHTTPError`是任何HF Hub HTTP错误的父类。它负责解析服务器响应并格式化错误消息，以尽可能向用户提供尽可能多的信息。

### `class huggingface_hub.utils.HfHubHTTPError`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_errors.py#L32)

```py
( message: str response: Optional = None )
```

用于在HF Hub中引发任何自定义HTTP错误的HTTPError继承。

任何HTTP错误至少转换为`HfHubHTTPError`。如果服务器发送了一些信息，它将被添加到错误消息中。

添加细节：

+   如果存在，则从“X-Request-Id”标题中获取请求ID。

+   来自标题“X-Error-Message”的服务器错误消息。

+   如果在响应正文中找到错误消息，则显示服务器错误消息。

示例：

```py
    import requests
    from huggingface_hub.utils import get_session, hf_raise_for_status, HfHubHTTPError

    response = get_session().post(...)
    try:
        hf_raise_for_status(response)
    except HfHubHTTPError as e:
        print(str(e)) # formatted message
        e.request_id, e.server_message # details returned by server

        # Complete the error message with additional information once it's raised
        e.append_to_message("
ate_commit` expects the repository to exist.")
        raise
```

#### `append_to_message`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_errors.py#L109)

```py
( additional_message: str )
```

将附加信息添加到`HfHubHTTPError`初始消息中。

#### RepositoryNotFoundError

### `class huggingface_hub.utils.RepositoryNotFoundError`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_errors.py#L114)

```py
( message: str response: Optional = None )
```

尝试访问具有无效存储库名称或用户无权访问的私有存储库名称的hf.co URL时引发。

示例：

```py
>>> from huggingface_hub import model_info
>>> model_info("<non_existent_repository>")
(...)
huggingface_hub.utils._errors.RepositoryNotFoundError: 401 Client Error. (Request ID: PvMw_VjBMjVdMz53WKIzP)

Repository Not Found for url: https://huggingface.co/api/models/%3Cnon_existent_repository%3E.
Please make sure you specified the correct `repo_id` and `repo_type`.
If the repo is private, make sure you are authenticated.
Invalid username or password.
```

#### GatedRepoError

### `class huggingface_hub.utils.GatedRepoError`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_errors.py#L135)

```py
( message: str response: Optional = None )
```

尝试访问未授权用户的受限存储库时引发。

注意：派生自`RepositoryNotFoundError`以确保向后兼容性。

示例：

```py
>>> from huggingface_hub import model_info
>>> model_info("<gated_repository>")
(...)
huggingface_hub.utils._errors.GatedRepoError: 403 Client Error. (Request ID: ViT1Bf7O_026LGSQuVqfa)

Cannot access gated repo for url https://huggingface.co/api/models/ardent-figment/gated-model.
Access to model ardent-figment/gated-model is restricted and you are not in the authorized list.
Visit https://huggingface.co/ardent-figment/gated-model to ask for access.
```

#### RevisionNotFoundError

### `class huggingface_hub.utils.RevisionNotFoundError`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_errors.py#L157)

```py
( message: str response: Optional = None )
```

尝试访问具有有效存储库但无效修订版本的hf.co URL时引发。

示例：

```py
>>> from huggingface_hub import hf_hub_download
>>> hf_hub_download('bert-base-cased', 'config.json', revision='<non-existent-revision>')
(...)
huggingface_hub.utils._errors.RevisionNotFoundError: 404 Client Error. (Request ID: Mwhe_c3Kt650GcdKEFomX)

Revision Not Found for url: https://huggingface.co/bert-base-cased/resolve/%3Cnon-existent-revision%3E/config.json.
```

#### EntryNotFoundError

### `class huggingface_hub.utils.EntryNotFoundError`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_errors.py#L175)

```py
( message: str response: Optional = None )
```

尝试访问具有有效存储库和修订版本但无效文件名的hf.co URL时引发。

示例：

```py
>>> from huggingface_hub import hf_hub_download
>>> hf_hub_download('bert-base-cased', '<non-existent-file>')
(...)
huggingface_hub.utils._errors.EntryNotFoundError: 404 Client Error. (Request ID: 53pNl6M0MxsnG5Sw8JA6x)

Entry Not Found for url: https://huggingface.co/bert-base-cased/resolve/main/%3Cnon-existent-file%3E.
```

#### BadRequestError

### `class huggingface_hub.utils.BadRequestError`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_errors.py#L216)

```py
( message: str response: Optional = None )
```

当服务器返回HTTP 400错误时，由`hf_raise_for_status`引发。

示例：

```py
>>> resp = requests.post("hf.co/api/check", ...)
>>> hf_raise_for_status(resp, endpoint_name="check")
huggingface_hub.utils._errors.BadRequestError: Bad request for check endpoint: {details} (Request ID: XXX)
```

#### LocalEntryNotFoundError

### `class huggingface_hub.utils.LocalEntryNotFoundError`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_errors.py#L193)

```py
( message: str )
```

尝试在网络不可用或不可用时（连接问题）访问未在磁盘上的文件或快照时引发。该条目可能存在于Hub上。

注意：`ValueError`类型用于确保向后兼容性。注意：`LocalEntryNotFoundError`派生自`HTTPError`，因为`EntryNotFoundError`即使不是网络问题也是如此。

示例：

```py
>>> from huggingface_hub import hf_hub_download
>>> hf_hub_download('bert-base-cased', '<non-cached-file>',  local_files_only=True)
(...)
huggingface_hub.utils._errors.LocalEntryNotFoundError: Cannot find the requested files in the disk cache and outgoing traffic has been disabled. To enable hf.co look-ups and downloads online, set 'local_files_only' to False.
```

#### OfflineModeIsEnabled

### `class huggingface_hub.utils.OfflineModeIsEnabled`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_http.py#L44)

```py
( )
```

当设置`HF_HUB_OFFLINE=1`作为环境变量时发出请求时引发。

## 遥测

`huggingface_hub`包括一个帮助程序来发送遥测数据。这些信息帮助我们调试问题并优先考虑新功能。用户可以通过设置`HF_HUB_DISABLE_TELEMETRY=1`环境变量随时禁用遥测收集。在离线模式下（即设置HF_HUB_OFFLINE=1时），遥测也会被禁用。

如果您是第三方库的维护者，发送遥测数据就像调用 `send_telemetry` 一样简单。数据将在单独的线程中发送，以尽可能减少对用户的影响。

#### `huggingface_hub.utils.send_telemetry`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_telemetry.py#L20)

```py
( topic: str library_name: Optional = None library_version: Optional = None user_agent: Union = None )
```

参数

+   `topic` (`str`) — 要监视的主题名称。主题直接用于构建 URL。如果要监视子主题，只需使用“/”分隔。示例：“gradio”, “transformers/examples”,…

+   `library_name` (`str`, *可选*) — 发起 HTTP 请求的库的名称。将添加到用户代理标头中。

+   `library_version` (`str`, *可选*) — 发起 HTTP 请求的库的版本。将添加到用户代理标头中。

+   `user_agent` (`str`, `dict`, *可选*) — 用户代理信息，可以是字典或单个字符串形式。它将包含有关已安装包的信息。

发送有助于跟踪不同 HF 库使用情况的遥测数据。

这些使用数据帮助我们调试问题并优先考虑新功能。但是，我们理解并尊重不希望分享额外信息的每个人的隐私。您可以通过将环境变量 `HF_HUB_DISABLE_TELEMETRY=1` 设置为禁用遥测收集。在离线模式下（即设置 `HF_HUB_OFFLINE=1` 时），遥测也会被禁用。

遥测收集在单独的线程中运行，以最小化对用户的影响。

示例：

```py
>>> from huggingface_hub.utils import send_telemetry

# Send telemetry without library information
>>> send_telemetry("ping")

# Send telemetry to subtopic with library information
>>> send_telemetry("gradio/local_link", library_name="gradio", library_version="3.22.1")

# Send telemetry with additional data
>>> send_telemetry(
...     topic="examples",
...     library_name="transformers",
...     library_version="4.26.0",
...     user_agent={"pipeline": "text_classification", "framework": "flax"},
... )
```

## 验证器

`huggingface_hub` 包含自定义验证器，可以自动验证方法参数。验证受到 [Pydantic](https://pydantic-docs.helpmanual.io/) 中对类型提示进行验证的启发，但功能更有限。

### 通用装饰器

[validate_hf_hub_args()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.validate_hf_hub_args) 是一个通用装饰器，用于封装遵循 `huggingface_hub` 命名规范的方法的参数。默认情况下，所有实现了验证器的参数都将被验证。

如果输入无效，将抛出 [HFValidationError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HFValidationError)。只有第一个无效值会抛出错误并停止验证过程。

用法：

```py
>>> from huggingface_hub.utils import validate_hf_hub_args

>>> @validate_hf_hub_args
... def my_cool_method(repo_id: str):
...     print(repo_id)

>>> my_cool_method(repo_id="valid_repo_id")
valid_repo_id

>>> my_cool_method("other..repo..id")
huggingface_hub.utils._validators.HFValidationError: Cannot have -- or .. in repo_id: 'other..repo..id'.

>>> my_cool_method(repo_id="other..repo..id")
huggingface_hub.utils._validators.HFValidationError: Cannot have -- or .. in repo_id: 'other..repo..id'.

>>> @validate_hf_hub_args
... def my_cool_auth_method(token: str):
...     print(token)

>>> my_cool_auth_method(token="a token")
"a token"

>>> my_cool_auth_method(use_auth_token="a use_auth_token")
"a use_auth_token"

>>> my_cool_auth_method(token="a token", use_auth_token="a use_auth_token")
UserWarning: Both `token` and `use_auth_token` are passed (...). `use_auth_token` value will be ignored.
"a token"
```

#### validate_hf_hub_args

#### `huggingface_hub.utils.validate_hf_hub_args`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_validators.py#L46)

```py
( fn: CallableT )
```

抛出

[HFValidationError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HFValidationError)

+   [HFValidationError](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.HFValidationError) — 如果输入无效。

验证 `huggingface_hub` 的任何公共方法接收的参数值。

此装饰器的目标是在各处重复使用的参数验证。默认情况下，所有定义的验证器都会被测试。

验证器：

+   [validate_repo_id()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.validate_repo_id): `repo_id` 必须是 `"repo_name"` 或 `"namespace/repo_name"`。命名空间是用户名或组织。

+   [smoothly_deprecate_use_auth_token()](/docs/huggingface_hub/v0.20.3/en/package_reference/utilities#huggingface_hub.utils.smoothly_deprecate_use_auth_token): 使用 `token` 替代 `use_auth_token`（仅当装饰函数不需要 `use_auth_token` 时 - 实际上，在 `huggingface_hub` 中始终如此）。

示例:

```py
>>> from huggingface_hub.utils import validate_hf_hub_args

>>> @validate_hf_hub_args
... def my_cool_method(repo_id: str):
...     print(repo_id)

>>> my_cool_method(repo_id="valid_repo_id")
valid_repo_id

>>> my_cool_method("other..repo..id")
huggingface_hub.utils._validators.HFValidationError: Cannot have -- or .. in repo_id: 'other..repo..id'.

>>> my_cool_method(repo_id="other..repo..id")
huggingface_hub.utils._validators.HFValidationError: Cannot have -- or .. in repo_id: 'other..repo..id'.

>>> @validate_hf_hub_args
... def my_cool_auth_method(token: str):
...     print(token)

>>> my_cool_auth_method(token="a token")
"a token"

>>> my_cool_auth_method(use_auth_token="a use_auth_token")
"a use_auth_token"

>>> my_cool_auth_method(token="a token", use_auth_token="a use_auth_token")
UserWarning: Both `token` and `use_auth_token` are passed (...)
"a token"
```

#### HFValidationError

### `class huggingface_hub.utils.HFValidationError`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_validators.py#L39)

```py
( )
```

`huggingface_hub` 验证器抛出的通用异常。

继承自 [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError)。

### 参数验证器

验证器也可以单独使用。以下是可以验证的所有参数列表。

#### repo_id

#### `huggingface_hub.utils.validate_repo_id`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_validators.py#L123)

```py
( repo_id: str )
```

验证`repo_id`是否有效。

这并不意味着取代Hub上进行的适当验证，而是尽可能避免本地不一致（例如：在`repo_id`中传递`repo_type`是被禁止的）。

规则：

+   1到96个字符之间。

+   要么“repo_name”，要么“namespace/repo_name”

+   [a-zA-Z0-9]或"-","_","."

+   "-"和".."是被禁止的

有效的："foo"，"foo/bar"，"123"，"Foo-BAR_foo.bar123"

无效的："datasets/foo/bar"，".repo_id"，"foo--bar"，"foo.git"

示例：

```py
>>> from huggingface_hub.utils import validate_repo_id
>>> validate_repo_id(repo_id="valid_repo_id")
>>> validate_repo_id(repo_id="other..repo..id")
huggingface_hub.utils._validators.HFValidationError: Cannot have -- or .. in repo_id: 'other..repo..id'.
```

在[https://github.com/huggingface/huggingface_hub/issues/1008](https://github.com/huggingface/huggingface_hub/issues/1008)中讨论。在moon-landing（内部存储库）：

+   [https://github.com/huggingface/moon-landing/blob/main/server/lib/Names.ts#L27](https://github.com/huggingface/moon-landing/blob/main/server/lib/Names.ts#L27)

+   [https://github.com/huggingface/moon-landing/blob/main/server/views/components/NewRepoForm/NewRepoForm.svelte#L138](https://github.com/huggingface/moon-landing/blob/main/server/views/components/NewRepoForm/NewRepoForm.svelte#L138)

#### smoothly_deprecate_use_auth_token

并不是一个验证器，但也可以运行。

#### `huggingface_hub.utils.smoothly_deprecate_use_auth_token`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/utils/_validators.py#L177)

```py
( fn_name: str has_token: bool kwargs: Dict )
```

在`huggingface_hub`代码库中平滑弃用`use_auth_token`。

长期目标是在代码库中删除对`use_auth_token`的任何提及，而是使用唯一且不那么冗长的`token`参数。这将通过几个步骤完成：

1.  步骤0：需要对Hub进行读访问的方法使用`use_auth_token`参数（`str`，`bool`或`None`）。需要写访问权限的方法具有`token`参数（`str`，`None`）。这个隐式规则存在是为了在不必要时不发送令牌（`use_auth_token=False`）即使已登录。

1.  步骤1：我们希望使一切和使用`token`（支持`token=False`用于只读方法）。为了不破坏现有代码，如果将`use_auth_token`传递给函数，则`use_auth_token`值将作为`token`传递，而不会有任何警告。a. 特殊情况：如果同时传递了`use_auth_token`和`token`值，则会发出警告，并且将忽略`use_auth_token`值。

1.  步骤2：一旦发布，我们应该尽可能推动下游库从`use_auth_token`切换到`token`，但不会发出警告（例如：在相应的存储库上手动创建问题）。

1.  步骤3：经过过渡期（例如6个月，直到2023年4月？），我们更新`huggingface_hub`以在`use_auth_token`上发出警告。希望很少有用户会受到影响，因为问题已经被解决。此外，`huggingface_hub`中的单元测试必须适应预期会发出警告（但仍然像以前一样使用`use_auth_token`）。

1.  步骤4：经过正常的弃用周期（3个发布？），删除此验证器。`use_auth_token`绝对不会得到支持。此外，我们在`huggingface_hub`中更新单元测试，以在所有地方使用`token`。

这已经在讨论中：

+   [https://github.com/huggingface/huggingface_hub/issues/1094](https://github.com/huggingface/huggingface_hub/issues/1094)。

+   [https://github.com/huggingface/huggingface_hub/pull/928](https://github.com/huggingface/huggingface_hub/pull/928)

+   (相关) [https://github.com/huggingface/huggingface_hub/pull/1064](https://github.com/huggingface/huggingface_hub/pull/1064)
