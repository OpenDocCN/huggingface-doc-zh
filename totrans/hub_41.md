# Docker Spaces

> 原始文本：[https://huggingface.co/docs/hub/spaces-sdks-docker](https://huggingface.co/docs/hub/spaces-sdks-docker)

Spaces可以容纳用于Streamlit和Gradio范围之外的应用程序的自定义[Docker容器](https://docs.docker.com/get-started/)。Docker Spaces允许用户超越标准SDK之前可能的限制。从FastAPI和Go端点到Phoenix应用程序和ML Ops工具，Docker Spaces可以帮助在许多不同的设置中。

## 设置Docker Spaces

选择**Docker**作为SDK，当[创建一个新的Space](https://huggingface.co/new-space)时，将通过在`README.md`文件的YAML块中将`sdk`属性设置为`docker`来初始化您的Space。或者，对于现有的Space存储库，可以在Spaces的**README.md**文件顶部的YAML块中设置`sdk: docker`。您还可以通过设置`app_port: 7860`来更改默认暴露的端口`7860`。之后，您可以创建一个通常的`Dockerfile`。

```py
---
title: Basic Docker SDK Space
emoji: 🐳
colorFrom: purple
colorTo: gray
sdk: docker
app_port: 7860
---
```

在内部，您可以拥有任意数量的开放端口。例如，您可以在Space内部安装Elasticsearch，并在其默认端口9200上内部调用它。

如果您想将在多个端口上提供的应用程序暴露给外部世界，一个解决方法是使用反向代理，如Nginx，将来自更广泛互联网的请求（在单个端口上）分发到不同的内部端口。

## 秘密和变量管理

您可以在Space设置中管理Space的环境变量。在[这里](./spaces-overview#managing-secrets)阅读更多。

### 变量

#### 构建时

在构建Docker Space时，变量作为`build-arg`传递。阅读[Docker的专用文档](https://docs.docker.com/engine/reference/builder/#arg)以获取有关如何在Dockerfile中使用此功能的完整指南。

```py
	# Declare your environment variables with the ARG directive
	ARG MODEL_REPO_NAME

	FROM python:latest
	# [...]
	# You can use them like environment variables
	RUN predict.py $MODEL_REPO_NAME
```

#### 运行时

变量在容器的运行时环境中注入。

### 秘密

#### 构建时

在Docker Spaces中，出于安全原因，秘密管理方式有所不同。一旦在[设置选项卡](./spaces-overview#managing-secrets)中创建了一个秘密，您可以通过在Dockerfile中添加以下行来公开该秘密：

例如，如果`SECRET_EXAMPLE`是您在设置选项卡中创建的秘密的名称，您可以通过将其挂载到文件中，然后使用`$(cat /run/secrets/SECRET_EXAMPLE)`在构建时读取它。

查看下面的示例：

```py
# Expose the secret SECRET_EXAMPLE at buildtime and use its value as git remote URL
RUN --mount=type=secret,id=SECRET_EXAMPLE,mode=0444,required=true \
 git init && \
 git remote add origin $(cat /run/secrets/SECRET_EXAMPLE)
```

```py
# Expose the secret SECRET_EXAMPLE at buildtime and use its value as a Bearer token for a curl request
RUN --mount=type=secret,id=SECRET_EXAMPLE,mode=0444,required=true \
	curl test -H 'Authorization: Bearer $(cat /run/secrets/SECRET_EXAMPLE)'
```

#### 运行时

与公共变量一样，在运行时，您可以将秘密作为环境变量访问。例如，在Python中，您可以使用`os.environ.get("SECRET_EXAMPLE")`。查看使用秘密的Docker Space的[示例](https://huggingface.co/spaces/DockerTemplates/secret-example)。

## 权限

容器以用户ID 1000运行。为了避免权限问题，您应该在任何`COPY`或下载之前创建一个用户并设置其`WORKDIR`。

```py
# Set up a new user named "user" with user ID 1000
RUN useradd -m -u 1000 user

# Switch to the "user" user
USER user

# Set home to the user's home directory
ENV HOME=/home/user \
	PATH=/home/user/.local/bin:$PATH

# Set the working directory to the user's home directory
WORKDIR $HOME/app

# Try and run pip command after setting the user with `USER user` to avoid permission issues with Python
RUN pip install --no-cache-dir --upgrade pip

# Copy the current directory contents into the container at $HOME/app setting the owner to the user
COPY --chown=user . $HOME/app

# Download a checkpoint
RUN mkdir content
ADD --chown=user https://<SOME_ASSET_URL> content/<SOME_ASSET_NAME>
```

始终使用`--chown=user`与`ADD`和`COPY`一起，以确保新文件由您的用户拥有。

如果您仍然遇到权限问题，您可能需要在`Dockerfile`中使用`chmod`或`chown`来授予正确的权限。例如，如果您想使用目录`/data`，您可以执行：

```py
RUN mkdir -p /data
RUN chmod 777 /data
```

您应该始终避免不必要的chowns。

更新文件的元数据会创建一个存储在新层中的新副本。因此，递归的chown可能会导致镜像非常大，因为所有受影响的文件都会重复。

而不是通过运行`chown`来修复权限：

```py
COPY checkpoint .
RUN chown -R user checkpoint
```

您应该始终执行：

```py
COPY --chown=user checkpoint .
```

（`ADD`命令也是如此）

## 数据持久性

在磁盘上写入的数据在您的Docker Space重新启动时会丢失，除非您选择进行[持久存储](./spaces-storage)升级。

如果您选择进行持久存储升级，您可以使用`/data`目录来存储数据。该目录被挂载在持久卷上，这意味着在该目录中写入的数据将在重新启动时保留。

目前，`/data`卷仅在运行时可用，即您不能在Dockerfile的构建步骤中使用`/data`。

您还可以针对特定情况使用我们的数据集中心，您可以将状态和数据存储在git LFS存储库中。您可以在[这里](https://huggingface.co/spaces/Wauplin/space_to_dataset_saver)找到一个使用[`huggingface_hub`库](https://huggingface.co/docs/huggingface_hub/index)来以编程方式上传文件到数据集存储库的持久性示例。这个Space示例以及[这个指南](https://huggingface.co/docs/huggingface_hub/main/en/guides/upload#scheduled-uploads)将帮助您确定哪种解决方案最适合您的数据类型。

最后，在某些情况下，您可能希望从您的Space代码中使用外部存储解决方案，比如外部托管的数据库，S3等。

### 带有GPU的Docker容器

您可以通过使用我们的GPU-flavored [Spaces Hardware](./spaces-gpus)之一来运行带有GPU支持的Docker容器。

我们建议在Docker Hub上使用[`nvidia/cuda`](https://hub.docker.com/r/nvidia/cuda)作为基础镜像，该镜像预先安装了CUDA和cuDNN。

在Docker构建过程中，您无法访问GPU硬件。因此，在Dockerfile的构建步骤中不应尝试运行任何与GPU相关的命令。例如，您不能在构建镜像时运行`nvidia-smi`或`torch.cuda.is_available()`。更多信息请阅读[这里](https://github.com/NVIDIA/nvidia-docker/wiki/nvidia-docker#description)。

## 阅读更多

+   [完整的Docker演示示例](spaces-sdks-docker-first-demo)

+   [Docker Spaces示例列表](spaces-sdks-docker-examples)

+   [Spaces示例](https://huggingface.co/SpacesExamples)
