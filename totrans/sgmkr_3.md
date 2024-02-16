# 在 Amazon SageMaker 上运行训练

> 原始文本：[`huggingface.co/docs/sagemaker/train`](https://huggingface.co/docs/sagemaker/train)

[`www.youtube.com/embed/ok3hetb42gU`](https://www.youtube.com/embed/ok3hetb42gU)

本指南将向您展示如何使用`HuggingFace` SageMaker Python SDK 训练一个🤗 Transformers 模型。学习如何：

+   安装和设置您的训练环境。

+   准备一个训练脚本。

+   创建一个 Hugging Face Estimator。

+   使用`fit`方法运行训练。

+   访问您训练的模型。

+   进行分布式训练。

+   创建一个 spot 实例。

+   从 GitHub 存储库加载训练脚本。

+   收集训练指标。

## 安装和设置

在您可以使用 SageMaker 训练🤗 Transformers 模型之前，您需要注册 AWS 账户。如果您还没有 AWS 账户，请在[这里](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-set-up.html)了解更多信息。

一旦您拥有 AWS 账户，可以通过以下方式之一开始使用：

+   [SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-onboard.html)

+   [SageMaker 笔记本实例](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-console.html)

+   本地环境

要在本地开始训练，您需要设置适当的[IAM 角色](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)。

升级到最新的`sagemaker`版本：

```py
pip install sagemaker --upgrade
```

**SageMaker 环境**

按照下面所示设置您的 SageMaker 环境：

```py
import sagemaker
sess = sagemaker.Session()
role = sagemaker.get_execution_role()
```

*注意：执行角色仅在 SageMaker 内运行笔记本时可用。如果在非 SageMaker 笔记本中运行`get_execution_role`，则会出现`region`错误。*

**本地环境**

按照下面所示设置您的本地环境：

```py
import sagemaker
import boto3

iam_client = boto3.client('iam')
role = iam_client.get_role(RoleName='role-name-of-your-iam-role-with-right-permissions')['Role']['Arn']
sess = sagemaker.Session()
```

## 准备一个🤗 Transformers 微调脚本

我们的训练脚本与您可能在 SageMaker 之外运行的训练脚本非常相似。但是，您可以通过各种环境变量访问有关训练环境的有用属性（请参阅[这里](https://github.com/aws/sagemaker-training-toolkit/blob/master/ENVIRONMENT_VARIABLES.md)获取完整列表），例如：

+   `SM_MODEL_DIR`: 一个表示训练作业写入模型工件的路径的字符串。训练后，此目录中的工件将上传到 S3 以进行模型托管。`SM_MODEL_DIR`始终设置为`/opt/ml/model`。

+   `SM_NUM_GPUS`: 一个表示主机可用 GPU 数量的整数。

+   `SM_CHANNEL_XXXX:` 一个表示包含指定通道输入数据的目录路径的字符串。例如，当您在 Hugging Face Estimator 的`fit`方法中指定`train`和`test`时，环境变量设置为`SM_CHANNEL_TRAIN`和`SM_CHANNEL_TEST`。

在 Hugging Face Estimator 中定义的`hyperparameters`作为命名参数传递，并由`ArgumentParser()`处理。

```py
import transformers
import datasets
import argparse
import os

if __name__ == "__main__":

    parser = argparse.ArgumentParser()

    # hyperparameters sent by the client are passed as command-line arguments to the script
    parser.add_argument("--epochs", type=int, default=3)
    parser.add_argument("--per_device_train_batch_size", type=int, default=32)
    parser.add_argument("--model_name_or_path", type=str)

    # data, model, and output directories
    parser.add_argument("--model-dir", type=str, default=os.environ["SM_MODEL_DIR"])
    parser.add_argument("--training_dir", type=str, default=os.environ["SM_CHANNEL_TRAIN"])
    parser.add_argument("--test_dir", type=str, default=os.environ["SM_CHANNEL_TEST"])
```

*请注意，SageMaker 不支持 argparse 操作。例如，如果您想使用布尔超参数，请在脚本中将`type`指定为`bool`并提供明确的`True`或`False`值。*

查看[这里](https://github.com/huggingface/notebooks/blob/main/sagemaker/01_getting_started_pytorch/scripts/train.py)完整的🤗 Transformers 训练脚本示例。

## 训练输出管理

如果 `TrainingArguments` 中的 `output_dir` 设置为 ‘/opt/ml/model’，则 Trainer 会保存所有训练工件，包括日志、检查点和模型。Amazon SageMaker 将整个 ‘/opt/ml/model’ 目录存档为 `model.tar.gz`，并在训练作业结束时将其上传到 Amazon S3。根据您的超参数和 `TrainingArguments`，这可能导致一个大的工件（> 5GB），这可能会减慢 Amazon SageMaker 推理的部署速度。您可以通过自定义 [TrainingArguments](https://huggingface.co/docs/transformers/main/en/main_classes/trainer#transformers.TrainingArguments) 控制检查点、日志和工件的保存方式。例如，通过提供 `save_total_limit` 作为 `TrainingArgument`，您可以控制检查点的总量限制。如果保存了新的检查点并且达到了最大限制，则删除 `output_dir` 中的旧检查点。

除了上面已经提到的选项之外，还有另一个选项可以在训练会话期间保存训练工件。Amazon SageMaker 支持[Checkpointing](https://docs.aws.amazon.com/sagemaker/latest/dg/model-checkpoints.html)，允许您在训练期间持续将您的工件保存到 Amazon S3，而不是在训练结束时保存。要启用[Checkpointing](https://docs.aws.amazon.com/sagemaker/latest/dg/model-checkpoints.html)，您需要提供指向 Amazon S3 位置的 `checkpoint_s3_uri` 参数，该参数位于 `HuggingFace` estimator 中，并将 `output_dir` 设置为 `/opt/ml/checkpoints`。*注意：如果将 `output_dir` 设置为 `/opt/ml/checkpoints`，请确保在训练结束时调用 `trainer.save_model("/opt/ml/model")` 或 model.save_pretrained(“/opt/ml/model”)/`tokenizer.save_pretrained("/opt/ml/model")`，以便能够无缝地将您的模型部署到 Amazon SageMaker 进行推理。*

## 创建一个 Hugging Face Estimator

通过创建[Hugging Face Estimator](https://sagemaker.readthedocs.io/en/stable/frameworks/huggingface/sagemaker.huggingface.html#huggingface-estimator)在 SageMaker 上运行 🤗 Transformers 训练脚本。Estimator 处理端到端的 SageMaker 训练。您应该在 Estimator 中定义几个参数：

1.  `entry_point` 指定要使用的微调脚本。

1.  `instance_type` 指定要启动的 Amazon 实例。请参考[这里](https://aws.amazon.com/sagemaker/pricing/)获取完整的实例类型列表。

1.  `hyperparameters` 指定训练超参数。查看其他可用的超参数[这里](https://github.com/huggingface/notebooks/blob/main/sagemaker/01_getting_started_pytorch/scripts/train.py)。

以下代码示例显示如何使用自定义脚本 `train.py` 进行训练，其中包含三个超参数（`epochs`、`per_device_train_batch_size` 和 `model_name_or_path`）：

```py
from sagemaker.huggingface import HuggingFace

# hyperparameters which are passed to the training job
hyperparameters={'epochs': 1,
                 'per_device_train_batch_size': 32,
                 'model_name_or_path': 'distilbert-base-uncased'
                 }

# create the Estimator
huggingface_estimator = HuggingFace(
        entry_point='train.py',
        source_dir='./scripts',
        instance_type='ml.p3.2xlarge',
        instance_count=1,
        role=role,
        transformers_version='4.26',
        pytorch_version='1.13',
        py_version='py39',
        hyperparameters = hyperparameters
)
```

如果您在本地运行 `TrainingJob`，请定义 `instance_type='local'` 或 `instance_type='local_gpu'` 以使用 GPU。请注意，这在 SageMaker Studio 中不起作用。

## 执行训练

通过在 Hugging Face Estimator 上调用 `fit` 来启动您的 `TrainingJob`。在 `fit` 中指定您的输入训练数据。输入训练数据可以是：

+   S3 URI，例如 `s3://my-bucket/my-training-data`。

+   对于 Amazon 弹性文件系统或 FSx for Lustre，运行 `FileSystemInput`。请参考[这里](https://sagemaker.readthedocs.io/en/stable/overview.html?highlight=FileSystemInput#use-file-systems-as-training-inputs)获取有关将这些文件系统用作输入的更多详细信息。

调用 `fit` 开始训练：

```py
huggingface_estimator.fit(
  {'train': 's3://sagemaker-us-east-1-558105141721/samples/datasets/imdb/train',
   'test': 's3://sagemaker-us-east-1-558105141721/samples/datasets/imdb/test'}
)
```

SageMaker 启动并管理所有必需的 EC2 实例，并通过运行以下内容来启动 `TrainingJob`：

```py
/opt/conda/bin/python train.py --epochs 1 --model_name_or_path distilbert-base-uncased --per_device_train_batch_size 32
```

## 访问训练模型

训练完成后，您可以通过[AWS 控制台](https://console.aws.amazon.com/console/home?nc2=h_ct&src=header-signin)访问您的模型，或直接从 S3 下载。

```py
from sagemaker.s3 import S3Downloader

S3Downloader.download(
    s3_uri=huggingface_estimator.model_data, # S3 URI where the trained model is located
    local_path='.',                          # local path where *.targ.gz is saved
    sagemaker_session=sess                   # SageMaker session used for training the model
)
```

## 分布式训练

SageMaker 提供了两种分布式训练策略：数据并行 ism 和模型并行 ism。数据并行 ism 将训练集分割到多个 GPU 上，而模型并行 ism 将模型分割到多个 GPU 上。

### 数据并行 ism

Hugging Face 的[Trainer](https://huggingface.co/docs/transformers/main_classes/trainer)支持 SageMaker 的数据并行 ism 库。如果您的训练脚本使用 Trainer API，您只需要在 Hugging Face Estimator 中定义分布参数：

```py
# configuration for running training on smdistributed data parallel
distribution = {'smdistributed':{'dataparallel':{ 'enabled': True }}}

# create the Estimator
huggingface_estimator = HuggingFace(
        entry_point='train.py',
        source_dir='./scripts',
        instance_type='ml.p3dn.24xlarge',
        instance_count=2,
        role=role,
        transformers_version='4.26.0',
        pytorch_version='1.13.1',
        py_version='py39',
        hyperparameters = hyperparameters,
        distribution = distribution
)
```

📓 打开[notebook](https://github.com/huggingface/notebooks/blob/main/sagemaker/07_tensorflow_distributed_training_data_parallelism/sagemaker-notebook.ipynb)以查看如何使用 TensorFlow 运行数据并行 ism 库的示例。

### 模型并行 ism

Hugging Face 的[Trainer]还支持 SageMaker 的模型并行 ism 库。如果您的训练脚本使用 Trainer API，您只需要在 Hugging Face Estimator 中定义分布参数（有关使用模型并行 ism 的更详细信息，请参见[此处](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_general.html?highlight=modelparallel#required-sagemaker-python-sdk-parameters)）：

```py
# configuration for running training on smdistributed model parallel
mpi_options = {
    "enabled" : True,
    "processes_per_host" : 8
}

smp_options = {
    "enabled":True,
    "parameters": {
        "microbatches": 4,
        "placement_strategy": "spread",
        "pipeline": "interleaved",
        "optimize": "speed",
        "partitions": 4,
        "ddp": True,
    }
}

distribution={
    "smdistributed": {"modelparallel": smp_options},
    "mpi": mpi_options
}

 # create the Estimator
huggingface_estimator = HuggingFace(
        entry_point='train.py',
        source_dir='./scripts',
        instance_type='ml.p3dn.24xlarge',
        instance_count=2,
        role=role,
        transformers_version='4.26.0',
        pytorch_version='1.13.1',
        py_version='py39',
        hyperparameters = hyperparameters,
        distribution = distribution
)
```

📓 打开[notebook](https://github.com/huggingface/notebooks/blob/main/sagemaker/04_distributed_training_model_parallelism/sagemaker-notebook.ipynb)以查看如何运行模型并行 ism 库的示例。

## Spot 实例

Hugging Face 扩展了 SageMaker Python SDK，这意味着我们可以从[完全托管的 EC2 spot 实例](https://docs.aws.amazon.com/sagemaker/latest/dg/model-managed-spot-training.html)中受益。这可以帮助您节省高达 90%的训练成本！

*注意：除非您的训练作业完成得很快，我们建议您使用[checkpointing](https://docs.aws.amazon.com/sagemaker/latest/dg/model-checkpoints.html)与托管的 spot 训练。在这种情况下，您需要定义`checkpoint_s3_uri`。*

设置`use_spot_instances=True`并在 Estimator 中定义您的`max_wait`和`max_run`时间以使用 spot 实例：

```py
# hyperparameters which are passed to the training job
hyperparameters={'epochs': 1,
                 'train_batch_size': 32,
                 'model_name':'distilbert-base-uncased',
                 'output_dir':'/opt/ml/checkpoints'
                 }

# create the Estimator
huggingface_estimator = HuggingFace(
        entry_point='train.py',
        source_dir='./scripts',
        instance_type='ml.p3.2xlarge',
        instance_count=1,
	    checkpoint_s3_uri=f's3://{sess.default_bucket()}/checkpoints'
        use_spot_instances=True,
        # max_wait should be equal to or greater than max_run in seconds
        max_wait=3600,
        max_run=1000,
        role=role,
        transformers_version='4.26',
        pytorch_version='1.13',
        py_version='py39',
        hyperparameters = hyperparameters
)

# Training seconds: 874
# Billable seconds: 262
# Managed Spot Training savings: 70.0%
```

📓 打开[notebook](https://github.com/huggingface/notebooks/blob/main/sagemaker/05_spot_instances/sagemaker-notebook.ipynb)以查看如何使用 spot 实例的示例。

## Git 存储库

Hugging Face Estimator 可以加载存储在 GitHub 存储库中的训练脚本（https://sagemaker.readthedocs.io/en/stable/overview.html#use-scripts-stored-in-a-git-repository）。在`entry_point`中提供训练脚本的相对路径，在`source_dir`中提供目录的相对路径。

如果您正在使用`git_config`来运行[🤗 Transformers 示例脚本](https://github.com/huggingface/transformers/tree/main/examples)，您需要在`transformers_version`中配置正确的`'branch'`（例如，如果您使用`transformers_version='4.4.2'`，您必须使用`'branch':'v4.4.2'`）。

*提示：通过在训练脚本的超参数中设置`output_dir=/opt/ml/model`将您的模型保存到 S3 中。*

```py
# configure git settings
git_config = {'repo': 'https://github.com/huggingface/transformers.git','branch': 'v4.4.2'} # v4.4.2 refers to the transformers_version you use in the estimator

 # create the Estimator
huggingface_estimator = HuggingFace(
        entry_point='run_glue.py',
        source_dir='./examples/pytorch/text-classification',
        git_config=git_config,
        instance_type='ml.p3.2xlarge',
        instance_count=1,
        role=role,
        transformers_version='4.26',
        pytorch_version='1.13',
        py_version='py39',
        hyperparameters=hyperparameters
)
```

## SageMaker 指标

[SageMaker 指标](https://docs.aws.amazon.com/sagemaker/latest/dg/training-metrics.html#define-train-metrics)自动解析训练作业日志以获取指标并将其发送到 CloudWatch。如果您希望 SageMaker 解析日志，您必须指定指标的名称和 SageMaker 用于查找指标的正则表达式。

```py
# define metrics definitions
metric_definitions = [
    {"Name": "train_runtime", "Regex": "train_runtime.*=\D*(.*?)$"},
    {"Name": "eval_accuracy", "Regex": "eval_accuracy.*=\D*(.*?)$"},
    {"Name": "eval_loss", "Regex": "eval_loss.*=\D*(.*?)$"},
]

# create the Estimator
huggingface_estimator = HuggingFace(
        entry_point='train.py',
        source_dir='./scripts',
        instance_type='ml.p3.2xlarge',
        instance_count=1,
        role=role,
        transformers_version='4.26',
        pytorch_version='1.13',
        py_version='py39',
        metric_definitions=metric_definitions,
        hyperparameters = hyperparameters)
```

📓 打开[notebook](https://github.com/huggingface/notebooks/blob/main/sagemaker/06_sagemaker_metrics/sagemaker-notebook.ipynb)以查看如何在 SageMaker 中捕获指标的示例。
