# Spaces 更新日志

> 原始文本：[`huggingface.co/docs/hub/spaces-changelog`](https://huggingface.co/docs/hub/spaces-changelog)

## 2023-07-28 - 支持大于等于 1.23.0 的上游 Streamlit 前端

+   Streamlit SDK 使用 PyPI 上发布的上游软件包`>=1.23.0`，因此新发布的版本从发布当天起就可用。

## 2023-05-30 - 添加对 Streamlit 1.23.x 和 1.24.0 的支持

+   添加了对 Streamlit `1.23.0`，`1.23.1`和`1.24.0`的支持。

+   自`1.23.0`以来，Streamlit 前端已从 HF 定制版本更改为上游版本。

## 2023-05-30 - 添加对 Streamlit 1.22.0 的支持

+   添加了对 Streamlit `1.22.0`的支持。

## 2023-05-15 - 默认的 Streamlit 版本

+   默认的 Streamlit 版本设置为`1.21.0`。

## 2023-04-12 - 添加对 Streamlit 1.19.0 的支持

+   添加了对`1.16.0`，`1.17.0`，`1.18.1`和`1.19.0`的支持，并将默认 SDK 版本设置为`1.19.0`。

## 2023-03-28 - Bug 修复

+   修复了一个导致无法在嵌入 iframe 或直接访问的 Streamlit 应用程序上滚动的错误，该错误已在[`discuss.huggingface.co/t/how-to-add-scroll-bars-to-a-streamlit-app-using-space-direct-embed-url/34101`](https://discuss.huggingface.co/t/how-to-add-scroll-bars-to-a-streamlit-app-using-space-direct-embed-url/34101)上报告。该补丁已应用于 Streamlit>=1.18.1。

## 2022-12-15 - Spaces 支持 Docker 容器

+   阅读更多关于：Docker Spaces

## 2022-12-14 - 设置自定义休眠时间的能力

+   阅读更多文档：Spaces 休眠时间

## 2022-12-07 - 添加对 Streamlit 1.15 的支持

+   公告：[`twitter.com/osanseviero/status/1600881584214638592`](https://twitter.com/osanseviero/status/1600881584214638592)。

## 2022-06-07 - 添加对 Streamlit 1.10.0 的支持

+   新的多页面应用功能在 Spaces 上可以直接使用。

+   Streamlit 博文：[`blog.streamlit.io/introducing-multipage-apps`](https://blog.streamlit.io/introducing-multipage-apps)。

## 2022-05-23 - Spaces 加速和响应式系统主题

+   所有使用 Gradio 3+和 Streamlit 1.x.x 的 Spaces 在加载时有显著加速。

+   系统主题现在在应用程序内是响应式的。如果用户切换到深色模式，它会自动更改。

## 2022-05-21 - 默认的 Debian 软件包和工厂重启

+   Spaces 环境现在预装了流行的软件包（`ffmpeg`，`libsndfile1`等）。

    +   这样，大多数情况下，您不需要为 Space 指定任何额外的软件包才能正常工作。

    +   如果需要，仍然可以使用`packages.txt`文件。

+   在 Spaces 中添加了工厂重启按钮，允许用户进行完全重启，避免缓存要求并释放 GPU 内存。

## 2022-05-17 - 添加对 Streamlit 1.9.0 的支持

+   现在支持所有`1.x.0`版本（最高到`1.9.0`）。

## 2022-05-16 - Gradio 3 发布！

+   这是创建新 Space 时的默认版本，请随时[查看](https://huggingface.co/blog/gradio-blocks)。

## 2022-03-04 - SDK 版本锁定

+   在 Space 创建时，`sdk_version`字段现在会自动填充。

    +   确保您的 Space 在更新后保持在相同的 SDK 版本上。

## 2022-03-02 - Gradio 版本固定

+   `sdk_version`配置字段现在与 Gradio SDK 一起工作。

## 2022-02-21 - Python 版本

+   您可以指定要在 Space 上运行的 Python 版本。

+   仅支持 Python 3 版本。

## 2022-01-24 - 从 Spaces 自动链接模型和数据集

+   我们尝试自动提取您的代码中使用的模型和数据集存储库 ID

+   您可以始终在 YAML 中使用`models`和`datasets`手动定义它们。

## 2021-10-20 - 添加对 Streamlit 1.0 的支持

+   我们现在支持 0.79.0 到 1.0.0 之间的所有版本

## 2021-09-07 - Streamlit 版本固定

+   现在您可以选择要在 Space 中安装的 Streamlit 版本

## 2021-09-06 - 将 Streamlit 升级到 0.84.2

+   支持会话状态 API

+   [Streamlit 更新日志](https://github.com/streamlit/streamlit/releases/tag/0.84.0)

## 2021-08-10 - 将 Streamlit 升级到 0.83.0

+   [Streamlit 更新日志](https://github.com/streamlit/streamlit/releases/tag/0.83.0)

## 2021-08-04 - Debian 软件包

+   现在您可以将您的 `apt-get` 依赖项添加到 `packages.txt` 文件中

## 2021-08-03 - Streamlit 组件

+   为[Streamlit 组件](https://streamlit.io/components)添加支持

## 2021-08-03 - Flax/Jax GPU 改进

+   对于启用 GPU 的空间，请确保 Flax / Jax 在 GPU 上运行顺畅

## 2021-08-02 - 将 Streamlit 升级到 0.82.0

+   [Streamlit 更新日志](https://github.com/streamlit/streamlit/releases/tag/0.82.0)

## 2021-08-01 - 可用的原始日志

+   在空间存储库中添加原始日志（构建和容器）的链接（可供具有对空间的写访问权限的用户查看）
