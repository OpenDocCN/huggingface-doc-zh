# 在 Amazon SageMaker 上训练和部署 Hugging Face

> 原文链接：[`huggingface.co/docs/sagemaker/getting-started`](https://huggingface.co/docs/sagemaker/getting-started)

入门指南将向您展示如何在 Amazon SageMaker 上快速使用 Hugging Face。学习如何在 SageMaker 上为二进制文本分类任务微调和部署预训练的🤗 Transformers 模型。

💡 如果您是 Hugging Face 的新手，我们建议先阅读🤗 Transformers 的[快速入门](https://huggingface.co/docs/transformers/quicktour)。

[`www.youtube.com/embed/pYqjCzoyWyo`](https://www.youtube.com/embed/pYqjCzoyWyo)

📓 打开[notebook](https://github.com/huggingface/notebooks/blob/main/sagemaker/01_getting_started_pytorch/sagemaker-notebook.ipynb)进行跟随！

## 安装和设置

通过安装必要的 Hugging Face 库和 SageMaker 来开始。如果您尚未安装，还需要安装[PyTorch](https://pytorch.org/get-started/locally/)和[TensorFlow](https://www.tensorflow.org/install/pip#tensorflow-2-packages-are-available)。

```py
pip install "sagemaker>=2.140.0" "transformers==4.26.1" "datasets[s3]==2.10.1" --upgrade
```

如果您想在[SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio.html)中运行此示例，请升级[ipywidgets](https://ipywidgets.readthedocs.io/en/latest/)以配合🤗 Datasets 库，并重新启动内核：

```py
%%capture
import IPython
!conda install -c conda-forge ipywidgets -y
IPython.Application.instance().kernel.do_shutdown(True)
```

接下来，您应该设置您的环境：一个 SageMaker 会话和一个 S3 存储桶。S3 存储桶将存储数据、模型和日志。您需要访问一个具有所需权限的[IAM 执行角色](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)。

如果您计划在本地环境中使用 SageMaker，您需要自己提供`role`。了解如何设置请查看[这里](https://huggingface.co/docs/sagemaker/train#installation-and-setup)。

⚠️ 只有在 SageMaker 内运行笔记本时才可用执行角色。如果您尝试在不在 SageMaker 上运行的笔记本中运行`get_execution_role`，您将收到区域错误。

```py
import sagemaker

sess = sagemaker.Session()
sagemaker_session_bucket = None
if sagemaker_session_bucket is None and sess is not None:
    sagemaker_session_bucket = sess.default_bucket()

role = sagemaker.get_execution_role()
sess = sagemaker.Session(default_bucket=sagemaker_session_bucket)
```

## 预处理

🤗 Datasets 库使下载和预处理数据集变得容易。下载和标记[IMDb](https://huggingface.co/datasets/imdb)数据集：

```py
from datasets import load_dataset
from transformers import AutoTokenizer

# load dataset
train_dataset, test_dataset = load_dataset("imdb", split=["train", "test"])

# load tokenizer
tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased")

# create tokenization function
def tokenize(batch):
    return tokenizer(batch["text"], padding="max_length", truncation=True)

# tokenize train and test datasets
train_dataset = train_dataset.map(tokenize, batched=True)
test_dataset = test_dataset.map(tokenize, batched=True)

# set dataset format for PyTorch
train_dataset =  train_dataset.rename_column("label", "labels")
train_dataset.set_format("torch", columns=["input_ids", "attention_mask", "labels"])
test_dataset = test_dataset.rename_column("label", "labels")
test_dataset.set_format("torch", columns=["input_ids", "attention_mask", "labels"])
```

## 将数据集上传到 S3 存储桶

接下来，使用🤗 Datasets S3 [文件系统](https://huggingface.co/docs/datasets/filesystems.html)实现将预处理的数据集上传到您的 S3 会话存储桶中：

```py
# save train_dataset to s3
training_input_path = f's3://{sess.default_bucket()}/{s3_prefix}/train'
train_dataset.save_to_disk(training_input_path)

# save test_dataset to s3
test_input_path = f's3://{sess.default_bucket()}/{s3_prefix}/test'
test_dataset.save_to_disk(test_input_path)
```

## 开始训练作业

创建一个 Hugging Face Estimator 来处理端到端的 SageMaker 训练和部署。需要注意的最重要的参数是：

+   `entry_point`指的是您可以在[这里](https://github.com/huggingface/notebooks/blob/main/sagemaker/01_getting_started_pytorch/scripts/train.py)找到的微调脚本。

+   `instance_type`指的是将启动的 SageMaker 实例。查看[这里](https://aws.amazon.com/sagemaker/pricing/)以获取完整的实例类型列表。

+   `超参数`指的是模型将进行微调的训练超参数。

```py
from sagemaker.huggingface import HuggingFace

hyperparameters={
    "epochs": 1,                            # number of training epochs
    "train_batch_size": 32,                 # training batch size
    "model_name":"distilbert-base-uncased"  # name of pretrained model
}

huggingface_estimator = HuggingFace(
    entry_point="train.py",                 # fine-tuning script to use in training job
    source_dir="./scripts",                 # directory where fine-tuning script is stored
    instance_type="ml.p3.2xlarge",          # instance type
    instance_count=1,                       # number of instances
    role=role,                              # IAM role used in training job to acccess AWS resources (S3)
    transformers_version="4.26",             # Transformers version
    pytorch_version="1.13",                  # PyTorch version
    py_version="py39",                      # Python version
    hyperparameters=hyperparameters         # hyperparameters to use in training job
)
```

一行代码开始训练：

```py
huggingface_estimator.fit({"train": training_input_path, "test": test_input_path})
```

## 部署模型

一旦训练作业完成，通过调用`deploy()`部署您的微调模型，指定实例数量和实例类型：

```py
predictor = huggingface_estimator.deploy(initial_instance_count=1,"ml.g4dn.xlarge")
```

在您的数据上调用`predict()`：

```py
sentiment_input = {"inputs": "It feels like a curtain closing...there was an elegance in the way they moved toward conclusion. No fan is going to watch and feel short-changed."}

predictor.predict(sentiment_input)
```

运行完您的请求后，删除端点：

```py
predictor.delete_endpoint()
```

## 接下来做什么？

恭喜，您刚刚在 SageMaker 上微调和部署了一个预训练的🤗 Transformers 模型！🎉

接下来，继续阅读我们的文档，了解更多关于训练和部署的细节。有许多有趣的功能，如分布式训练和 Spot 实例。
