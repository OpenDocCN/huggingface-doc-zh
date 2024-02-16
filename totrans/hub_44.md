# 空间配置参考

> 原始文本：[https://huggingface.co/docs/hub/spaces-config-reference](https://huggingface.co/docs/hub/spaces-config-reference)

空间通过存储库根目录中**README.md**文件顶部的`YAML`块进行配置。下面列出了所有接受的参数。

**`title`**：*字符串*

空间的显示标题。

**`emoji`**：*字符串*

空格表情符号（仅允许表情符号字符）。

**`colorFrom`**：*字符串*

缩略图渐变色（红色、黄色、绿色、蓝色、靛蓝色、紫色、粉色、灰色）。

**`colorTo`**：*字符串*

缩略图渐变色（红色、黄色、绿色、蓝色、靛蓝色、紫色、粉色、灰色）。

**`sdk`**：*字符串*

可以是`gradio`、`streamlit`、`docker`或`static`。

**`python_version`**：*字符串*

任何有效的Python`3.x`或`3.x.x`版本。

默认为`3.10`。

**`sdk_version`**：*字符串*

指定所选SDK的版本（Streamlit或Gradio）。

支持所有版本的Gradio。

支持所有版本的Streamlit从`0.79.0`开始。

**`suggested_hardware`**：*字符串*

指定此空间必须运行的建议[硬件](https://huggingface.co/docs/hub/spaces-gpus)。

适用于其他用户可能复制的空间。

设置此值不会自动为此空间分配硬件。

值必须是有效的硬件类型（例如`"cpu-upgrade"`、`"t4-small"`、`"t4-medium"`、`"a10g-small"`、`"a10g-large"`、`"a10g-largex2"`、`"a10g-largex4"`或`"a100-large"`）。

**`suggested_storage`**：*字符串*

指定此空间必须运行的建议[永久存储](https://huggingface.co/docs/hub/spaces-storage)。

适用于其他用户可能复制的空间。

设置此值不会自动为此空间分配永久存储。

值必须是`"small"`、`"medium"`或`"large"`中的一个。

**`app_file`**：*字符串*

主应用程序文件的路径（其中包含`gradio`或`streamlit` Python代码，或`static` html代码）。

路径相对于存储库的根目录。

**`app_port`**：*整数*

应用程序运行的端口。仅在`sdk`为`docker`时使用。默认端口为`7860`。

**`base_path`**：*字符串* 对于非静态空间，要呈现的初始url。需要以`/`开头。对于静态空间，请改用`app_file`。

**`fullWidth`**：*布尔值*

您的空间是否在iframe内部呈现为全宽度（当`true`时）或固定宽度列（即“容器”CSS）。在`gradio`中默认为false，在其他sdk中默认为true。

**`models`**：*List[string]*

在空间中使用的HF模型ID（如`gpt2`或`deepset/roberta-base-squad2`）。

如果此处未指定，将自动从您的代码中解析。

**`datasets`**：*List[string]*

在空间中使用的HF数据集ID（如`common_voice`或`oscar-corpus/OSCAR-2109`）。

如果此处未指定，将自动从您的代码中解析。

**`tags`**：*List[string]*

描述您的空间任务或范围的术语列表。

**`pinned`**：*布尔值*

空间是否保持在您的个人资料顶部。如果您有很多空间，这可能很有用，这样您和其他人可以快速看到您最好的空间。

**`hf_oauth`**：*布尔值*

此空间是否关联到连接的OAuth应用程序。有关更多详细信息，请参见[向您的空间添加一个带有HF按钮的登录](https://huggingface.co/docs/hub/spaces-oauth)。

**`hf_oauth_scopes`**：*List[string]* 连接的OAuth应用程序的授权范围。`openid`和`profile`默认已授权，不需要此参数。有关更多详细信息，请参见[向您的空间添加一个带有HF按钮的登录](https://huggingface.co/docs/hub/spaces-oauth)。

**`disable_embedding`**：*布尔值*

空间iframe是否可以嵌入到其他网站中。默认为false，即空间*可以*被嵌入。

**`startup_duration_timeout`**：*字符串*

为您的空间设置自定义启动持续时间超时。这是您的空间允许启动的最长时间，超过这个时间将被标记为不健康。默认为30分钟，但任何有效的持续时间（如`1h`、`30m`）都是可以接受的。

**`custom_headers`**：*Dict[string, string]*

设置自定义HTTP标头，这些标头将在为您的空间提供服务时添加到所有HTTP响应中。

目前，只允许使用[cross-origin-embedder-policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Embedder-Policy) (COEP)、[cross-origin-opener-policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Opener-Policy) (COOP)和[cross-origin-resource-policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Resource-Policy) (CORP)标头。这些标头可用于设置跨源隔离环境并启用强大的功能，例如`SharedArrayBuffer`：

```py
custom_headers:
  cross-origin-embedder-policy: require-corp
  cross-origin-opener-policy: same-origin
  cross-origin-resource-policy: cross-origin
```

*注意：* 所有标头和值必须为小写。

**`preload_from_hub`**：*List[string]* 指定在构建空间时预加载的Hugging Face Hub模型或其他大文件列表。通过在应用程序启动时准备好文件，可以优化启动时间。这对于依赖于大型模型或数据集的空间特别有用，否则这些模型或数据集需要在运行时下载。

每个项目的格式是`"repository_name"`，以从存储库下载所有文件，或者`"repository_name file1,file2"`，以下载该存储库中的特定文件。您还可以使用格式`"repository_name file1,file2 commit_sha256"`指定要下载的特定提交。

示例用法：

```py
preload_from_hub:
  - warp-ai/wuerstchen-prior text_encoder/model.safetensors,prior/diffusion_pytorch_model.safetensors
  - coqui/XTTS-v1
  - gpt2 config.json 11c5a3d5811f50298f278a704980280950aedb10
```

在此示例中，空间将在构建时从Hugging Face Hub预加载特定的`.safetensors`文件，完整的`coqui/XTTS-v1`存储库，以及`gpt2`存储库中`config.json`文件的特定修订版本。

文件保存在默认的`huggingface_hub`磁盘缓存`~/.cache/huggingface/hub`中。如果您的应用程序希望将它们保存在其他位置，或者您已更改了`HF_HOME`变量，则此预加载目前不会遵循那样做。
