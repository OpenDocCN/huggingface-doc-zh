# TensorBoard 记录器

> 原文链接：[`huggingface.co/docs/huggingface_hub/package_reference/tensorboard`](https://huggingface.co/docs/huggingface_hub/package_reference/tensorboard)

TensorBoard 是一个用于机器学习实验的可视化工具包。TensorBoard 允许跟踪和可视化指标，如损失和准确性，可视化模型图，查看直方图，显示图像等等。TensorBoard 与 Hugging Face Hub 集成良好。当推送到 Hub 时，Hub 会自动检测 TensorBoard 跟踪（如 `tfevents`），并启动一个实例来可视化它们。要获取有关 Hub 上的 TensorBoard 集成的更多信息，请查看[此指南](https://huggingface.co/docs/hub/tensorboard)。

为了从这个集成中受益，`huggingface_hub` 提供了一个自定义记录器，用于将日志推送到 Hub。它可以作为 [SummaryWriter](https://tensorboardx.readthedocs.io/en/latest/tensorboard.html) 的替代品，无需额外的代码。跟踪仍然会在本地保存，并且后台作业会定期将它们推送到 Hub。

## HFSummaryWriter

### `class huggingface_hub.HFSummaryWriter`

[< source >](https://github.com/huggingface/huggingface_hub/blob/v0.20.3/src/huggingface_hub/_tensorboard_logger.py#L35)

```py
( *args **kwargs )
```

参数

+   `repo_id` (`str`) — 将要推送日志的存储库的 ID。

+   `logdir` (`str`, *optional*) — 将写入日志的目录。如果未指定，则基础 `SummaryWriter` 对象将创建一个本地目录。

+   `commit_every` (`int` or `float`, *optional*) — 将日志推送到 Hub 的频率（以分钟为单位）。默认为 5 分钟。

+   `squash_history` (`bool`, *optional*) — 每次提交后是否压缩存储库的历史记录。默认为`False`。在存储库变得过大时，压缩提交对于避免性能下降是有用的。

+   `repo_type` (`str`, *optional*) — 将要推送日志的存储库类型。默认为“model”。

+   `repo_revision` (`str`, *optional*) — 将要推送日志的存储库的修订版本。默认为“main”。

+   `repo_private` (`bool`, *optional*) — 是否创建私有存储库。默认为 False。如果存储库已经存在，则此参数将被忽略。

+   `path_in_repo` (`str`, *optional*) — 将日志推送到存储库中的文件夹的路径。默认为“tensorboard/”。

+   `repo_allow_patterns` (`List[str]` or `str`, *optional*) — 要包含在上传中的模式列表。默认为`"*.tfevents.*"`。查看[上传指南](https://huggingface.co/docs/huggingface_hub/guides/upload#upload-a-folder)获取更多详细信息。

+   `repo_ignore_patterns` (`List[str]` or `str`, *optional*) — 要在上传中排除的模式列表。查看[上传指南](https://huggingface.co/docs/huggingface_hub/guides/upload#upload-a-folder)获取更多详细信息。

+   `token` (`str`, *optional*) — 认证令牌。将默认使用存储的令牌。有关更多详细信息，请参阅[`huggingface.co/settings/token`](https://huggingface.co/settings/token) kwargs — 传递给 `SummaryWriter` 的额外关键字参数。

将训练日志推送到 Hub 的 tensorboard 的 `SummaryWriter` 包装器。

数据首先在本地记录，然后异步推送到 Hub。将数据推送到 Hub 是在单独的线程中进行的，以避免阻塞训练脚本。特别是，如果由于任何原因（例如连接问题）上传失败，主脚本将不会被中断。数据将每隔 `commit_every` 分钟自动推送到 Hub（默认为每 5 分钟一次）。

`HFSummaryWriter` 是实验性的。其 API 可能会在未来更改而不事先通知。

示例：

```py
>>> from huggingface_hub import HFSummaryWriter

# Logs are automatically pushed every 15 minutes
>>> logger = HFSummaryWriter(repo_id="test_hf_logger", commit_every=15)
>>> logger.add_scalar("a", 1)
>>> logger.add_scalar("b", 2)
...

# You can also trigger a push manually
>>> logger.scheduler.trigger()
```

```py
>>> from huggingface_hub import HFSummaryWriter

# Logs are automatically pushed every 5 minutes (default) + when exiting the context manager
>>> with HFSummaryWriter(repo_id="test_hf_logger") as logger:
...     logger.add_scalar("a", 1)
...     logger.add_scalar("b", 2)
```
