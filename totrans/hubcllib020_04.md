# 安装

> 原始文本：[https://huggingface.co/docs/huggingface_hub/installation](https://huggingface.co/docs/huggingface_hub/installation)

在开始之前，您需要通过安装适当的软件包来设置您的环境。

`huggingface_hub`在**Python 3.8+**上进行了测试。

## 使用pip安装

强烈建议在[虚拟环境](https://docs.python.org/3/library/venv.html)中安装`huggingface_hub`。如果您不熟悉Python虚拟环境，请查看这个[指南](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/)。虚拟环境使得管理不同项目更容易，并避免依赖项之间的兼容性问题。

首先在项目目录中创建一个虚拟环境：

```py
python -m venv .env
```

激活虚拟环境。在Linux和macOS上：

```py
source .env/bin/activate
```

在Windows上激活虚拟环境之前：

```py
.env/Scripts/activate
```

现在您可以准备从PyPi注册表安装`huggingface_hub`：(https://pypi.org/project/huggingface-hub/)

```py
pip install --upgrade huggingface_hub
```

完成后，[检查安装](#check-installation)是否正确运行。

### 安装可选依赖项

`huggingface_hub`的一些依赖项是[可选的](https://setuptools.pypa.io/en/latest/userguide/dependency_management.html#optional-dependencies)，因为它们不是运行`huggingface_hub`核心功能所必需的。但是，如果未安装可选依赖项，则`huggingface_hub`的某些功能可能不可用。

您可以通过`pip`安装可选依赖项：

```py
# Install dependencies for tensorflow-specific features
# /!\ Warning: this is not equivalent to `pip install tensorflow`
pip install 'huggingface_hub[tensorflow]'

# Install dependencies for both torch-specific and CLI-specific features.
pip install 'huggingface_hub[cli,torch]'
```

以下是`huggingface_hub`中的可选依赖项列表：

+   `cli`：为`huggingface_hub`提供更方便的CLI界面。

+   `fastai`，`torch`，`tensorflow`：运行特定框架功能所需的依赖项。

+   `dev`：为了对库做出贡献所需的依赖项。包括`testing`（运行测试），`typing`（运行类型检查器）和`quality`（运行linter）。

### 从源代码安装

在某些情况下，直接从源代码安装`huggingface_hub`是有趣的。这允许您使用最新的`main`版本而不是最新稳定版本。`main`版本对于保持与最新发展同步很有用，例如，如果自上次官方发布以来已修复了一个bug，但尚未发布新版本。

然而，这意味着`main`版本可能并不总是稳定。我们努力保持`main`版本的运行，并且大多数问题通常在几个小时或一天内解决。如果遇到问题，请打开一个Issue，以便我们更快地解决！

```py
pip install git+https://github.com/huggingface/huggingface_hub
```

在安装源代码时，您还可以指定特定分支。如果您想测试一个尚未合并的新功能或新bug修复，这将非常有用：

```py
pip install git+https://github.com/huggingface/huggingface_hub@my-feature-branch
```

完成后，[检查安装](#check-installation)是否正确运行。

### 可编辑安装

从源代码安装允许您设置一个[可编辑安装](https://pip.pypa.io/en/stable/topics/local-project-installs/#editable-installs)。如果您计划为`huggingface_hub`做出贡献并需要测试代码中的更改，则这是一种更高级的安装。您需要在计算机上克隆`huggingface_hub`的本地副本。

```py
# First, clone repo locally
git clone https://github.com/huggingface/huggingface_hub.git

# Then, install with -e flag
cd huggingface_hub
pip install -e .
```

这些命令将链接您克隆存储库的文件夹和您的Python库路径。现在Python将在正常库路径之外查找您克隆到的文件夹。例如，如果您的Python包通常安装在`./.venv/lib/python3.11/site-packages/`中，Python还将搜索您克隆的文件夹`./huggingface_hub/`。

## 使用conda安装

如果您更熟悉conda，可以使用[conda-forge渠道](https://anaconda.org/conda-forge/huggingface_hub)安装`huggingface_hub`：

```py
conda install -c conda-forge huggingface_hub
```

完成后，[检查安装](#check-installation)是否正确运行。

## 检查安装

安装后，请通过运行以下命令检查`huggingface_hub`是否正常工作：

```py
python -c "from huggingface_hub import model_info; print(model_info('gpt2'))"
```

此命令将从Hub获取有关[gpt2](https://huggingface.co/gpt2)模型的信息。输出应该如下所示：

```py
Model Name: gpt2
Tags: ['pytorch', 'tf', 'jax', 'tflite', 'rust', 'safetensors', 'gpt2', 'text-generation', 'en', 'doi:10.57967/hf/0039', 'transformers', 'exbert', 'license:mit', 'has_space']
Task: text-generation
```

## Windows限制

我们的目标是在各处实现机器学习的民主化，我们构建了 `huggingface_hub` 作为一个跨平台库，特别是为了在 Unix 和 Windows 系统上正确运行。然而，在 Windows 上运行时，`huggingface_hub` 有一些限制。以下是已知问题的详尽列表。如果您遇到任何未记录的问题，请通过在 [Github 上开启一个问题](https://github.com/huggingface/huggingface_hub/issues/new/choose) 来告诉我们。

+   `huggingface_hub` 的缓存系统依赖符号链接来高效缓存从 Hub 下载的文件。在 Windows 上，您必须激活开发者模式或以管理员身份运行脚本才能启用符号链接。如果它们未被激活，缓存系统仍然可以工作，但效率不高。请阅读 [缓存限制](./guides/manage-cache#limitations) 部分以获取更多详细信息。

+   Hub 上的文件路径可能包含特殊字符（例如 `"path/to?/my/file"`）。Windows 对 [特殊字符](https://learn.microsoft.com/en-us/windows/win32/intl/character-sets-used-in-file-names) 更加严格，这使得在 Windows 上无法下载这些文件。希望这是一个罕见的情况。如果您认为这是一个错误，请联系存储库所有者或联系我们以找出解决方案。

## 下一步

一旦在您的计算机上正确安装了 `huggingface_hub`，您可能希望 [配置环境变量](package_reference/environment_variables) 或 [查看我们的指南之一](guides/overview) 来开始。
