# 登录和注销

> [https://huggingface.co/docs/huggingface_hub/package_reference/login](https://huggingface.co/docs/huggingface_hub/package_reference/login)

`huggingface_hub` 库允许用户以编程方式登录和注销机器到 Hub。

有关身份验证的更多详细信息，请查看[此部分](../quick-start#authentication)。

## login

#### `huggingface_hub.login`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_login.py#L49)

```py
( token: Optional = None add_to_git_credential: bool = False new_session: bool = True write_permission: bool = False )
```

参数

+   `token`（`str`，*可选*）— 用户访问令牌，可从[https://huggingface.co/settings/token](https://huggingface.co/settings/token)生成。

+   `add_to_git_credential`（`bool`，默认为 `False`）— 如果为 `True`，将 `token` 设置为 git 凭据。如果未配置 git 凭据助手，将向用户显示警告。如果 `token` 为 `None`，则 `add_to_git_credential` 的值将被忽略，并将再次提示最终用户。

+   `new_session`（`bool`，默认为 `True`）— 如果为 `True`，即使机器上已保存了一个令牌，也会请求一个新的令牌。

+   `write_permission`（`bool`，默认为 `False`）— 如果为 `True`，需要具有写权限的令牌。

引发

`ValueError` 或 `ImportError`

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) — 如果传递了组织令牌。只有个人帐户令牌才有效用于登录。

+   [`ValueError`](https://docs.python.org/3/library/exceptions.html#ValueError) — 如果令牌无效。

+   [`ImportError`](https://docs.python.org/3/library/exceptions.html#ImportError) — 如果在笔记本中运行但未安装 `ipywidgets`。

登录机器以访问 Hub。

`token` 被持久化在缓存中，并设置为 git 凭据。完成后，机器将登录，并访问令牌将在所有 `huggingface_hub` 组件中可用。如果未提供 `token`，将提示用户使用小部件（在笔记本中）或通过终端进行输入。

要从脚本外部登录，也可以使用 `huggingface-cli login`，这是一个包装[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)的 cli 命令。

[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login) 是[notebook_login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.notebook_login)的一个可替换方法，因为它包装并扩展了其功能。

当未传递令牌时，[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login) 将自动检测脚本是否在笔记本中运行。但是，由于现在存在各种笔记本，此检测可能不准确。如果是这种情况，您可以始终通过使用[notebook_login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.notebook_login)或[interpreter_login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.interpreter_login)来强制使用 UI。

## interpreter_login

#### `huggingface_hub.interpreter_login`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_login.py#L154)

```py
( new_session: bool = True write_permission: bool = False )
```

参数

+   `new_session`（`bool`，默认为 `True`）— 如果为 `True`，即使机器上已保存了一个令牌，也会请求一个新的令牌。

+   `write_permission`（`bool`，默认为 `False`）— 如果为 `True`，需要具有写权限的令牌。

显示一个提示登录到 HF 网站并存储令牌。

这相当于在笔记本中未运行时不传递令牌的[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)。[interpreter_login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.interpreter_login) 在您想要强制使用终端提示而不是笔记本小部件时非常有用。

有关更多详细信息，请参阅[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)。

## notebook_login

#### `huggingface_hub.notebook_login`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_login.py#L219)

```py
( new_session: bool = True write_permission: bool = False )
```

参数

+   `new_session` (`bool`，默认为`True`) — 如果为`True`，即使在机器上已经保存了一个令牌，也会请求一个令牌。

+   `write_permission` (`bool`，默认为`False`) — 如果为`True`，则需要具有写权限的令牌。

显示一个小部件，用于登录到HF网站并存储令牌。

这相当于在笔记本中运行时不传递令牌的[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)。如果您想强制使用笔记本小部件而不是终端中的提示，则[notebook_login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.notebook_login)很有用。

有关更多详细信息，请参见[login()](/docs/huggingface_hub/v0.20.3/en/package_reference/login#huggingface_hub.login)。

## 注销

#### `huggingface_hub.logout`

[<来源>](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_login.py#L116)

```py
( )
```

从Hub注销机器。

令牌从机器中删除并从git凭据中删除。
