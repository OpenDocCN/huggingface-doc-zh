# 将模型部署到 Amazon SageMaker

> 原始文本：[`huggingface.co/docs/sagemaker/inference`](https://huggingface.co/docs/sagemaker/inference)

在 SageMaker 中部署🤗 Transformers 模型进行推理就像这样简单：

```py
from sagemaker.huggingface import HuggingFaceModel

# create Hugging Face Model Class and deploy it as SageMaker endpoint
huggingface_model = HuggingFaceModel(...).deploy()
```

本指南将向您展示如何使用[推理工具包](https://github.com/aws/sagemaker-huggingface-inference-toolkit)零代码部署模型。推理工具包建立在🤗 Transformers 的[`pipeline`功能](https://huggingface.co/docs/transformers/main_classes/pipelines)之上。学习如何：

+   安装和设置推理工具包。

+   在 SageMaker 中部署经过训练的🤗 Transformers 模型。

+   [从 Hugging Face [模型 Hub](https://huggingface.co/models)部署🤗 Transformers 模型](#deploy-a-model-from-the-hub)。

+   使用🤗 Transformers 和 Amazon SageMaker 运行批量转换作业。

+   创建自定义推理模块。

## 安装和设置

在将🤗 Transformers 模型部署到 SageMaker 之前，您需要注册 AWS 账户。如果您还没有 AWS 账户，请在[此处](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-set-up.html)了解更多。

一旦您拥有 AWS 账户，可以通过以下方式之一开始使用：

+   [SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-onboard.html)

+   [SageMaker 笔记本实例](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-console.html)

+   本地环境

要开始本地训练，您需要设置适当的[IAM 角色](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)。

升级到最新的`sagemaker`版本。

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

*注意：执行角色仅在 SageMaker 内运行笔记本时可用。如果在不在 SageMaker 上运行的笔记本中运行`get_execution_role`，则会出现`region`错误。*

**本地环境**

按照下面所示设置您的本地环境：

```py
import sagemaker
import boto3

iam_client = boto3.client('iam')
role = iam_client.get_role(RoleName='role-name-of-your-iam-role-with-right-permissions')['Role']['Arn']
sess = sagemaker.Session()
```

## 在 SageMaker 中部署经过训练的🤗 Transformers 模型

[`www.youtube.com/embed/pfBGgSGnYLs`](https://www.youtube.com/embed/pfBGgSGnYLs)

有两种方法可以部署在 SageMaker 中训练的 Hugging Face 模型：

+   在训练完成后部署它。

+   稍后可以使用`model_data`从 S3 部署保存的模型。

📓打开[笔记本](https://github.com/huggingface/notebooks/blob/main/sagemaker/10_deploy_model_from_s3/deploy_transformer_model_from_s3.ipynb)查看如何将模型从 S3 部署到 SageMaker 进行推理的示例。

### 训练后部署

在训练后直接部署您的模型，请确保所有必需的文件都保存在您的训练脚本中，包括分词器和模型。

如果使用 Hugging Face 的`Trainer`，您可以将分词器作为参数传递给`Trainer`。当您调用`trainer.save_model()`时，它将自动保存。

```py
from sagemaker.huggingface import HuggingFace

############ pseudo code start ############

# create Hugging Face Estimator for training
huggingface_estimator = HuggingFace(....)

# start the train job with our uploaded datasets as input
huggingface_estimator.fit(...)

############ pseudo code end ############

# deploy model to SageMaker Inference
predictor = hf_estimator.deploy(initial_instance_count=1, instance_type="ml.m5.xlarge")

# example request: you always need to define "inputs"
data = {
   "inputs": "Camera - You are awarded a SiPix Digital Camera! call 09061221066 fromm landline. Delivery within 28 days."
}

# request
predictor.predict(data)
```

运行请求后，您可以按照以下方式删除端点：

```py
# delete endpoint
predictor.delete_endpoint()
```

### 使用 model_data 部署

如果您已经训练了模型并希望在以后部署它，请使用`model_data`参数指定您的分词器和模型权重的位置。

```py
from sagemaker.huggingface.model import HuggingFaceModel

# create Hugging Face Model Class
huggingface_model = HuggingFaceModel(
   model_data="s3://models/my-bert-model/model.tar.gz",  # path to your trained SageMaker model
   role=role,                                            # IAM role with permissions to create an endpoint
   transformers_version="4.26",                           # Transformers version used
   pytorch_version="1.13",                                # PyTorch version used
   py_version='py39',                                    # Python version used
)

# deploy model to SageMaker Inference
predictor = huggingface_model.deploy(
   initial_instance_count=1,
   instance_type="ml.m5.xlarge"
)

# example request: you always need to define "inputs"
data = {
   "inputs": "Camera - You are awarded a SiPix Digital Camera! call 09061221066 fromm landline. Delivery within 28 days."
}

# request
predictor.predict(data)
```

运行我们的请求后，您可以使用以下链接再次删除端点：

```py
# delete endpoint
predictor.delete_endpoint()
```

### 为部署创建模型工件

稍后部署时，可以创建一个包含所有必需文件的`model.tar.gz`文件，例如：

+   `pytorch_model.bin`

+   `tf_model.h5`

+   `tokenizer.json`

+   `tokenizer_config.json`

例如，您的文件应该如下所示：

```py
model.tar.gz/
|- pytorch_model.bin
|- vocab.txt
|- tokenizer_config.json
|- config.json
|- special_tokens_map.json
```

从🤗 Hub 创建自己的`model.tar.gz`文件：

1.  下载模型：

```py
git lfs install
git clone git@hf.co:{repository}
```

1.  创建一个`tar`文件：

```py
cd {repository}
tar zcvf model.tar.gz *
```

1.  将`model.tar.gz`上传到 S3：

```py
aws s3 cp model.tar.gz <s3://{my-s3-path}>
```

现在您可以提供 S3 URI 给`model_data`参数，以便稍后部署您的模型。

## 从🤗 Hub 部署模型

[`www.youtube.com/embed/l9QZuazbzWM`](https://www.youtube.com/embed/l9QZuazbzWM)

要直接从🤗 Hub 部署模型到 SageMaker，创建`HuggingFaceModel`时定义两个环境变量：

+   `HF_MODEL_ID` 定义了模型 ID，当您创建 SageMaker 端点时，它会自动从[huggingface.co/models](http://huggingface.co/models)加载。通过这个环境变量可以访问 🤗 Hub 上的 10,000 多个模型。

+   `HF_TASK` 定义了 🤗 Transformers `pipeline` 的任务。完整的任务列表可以在[这里](https://huggingface.co/docs/transformers/main_classes/pipelines)找到。

```py
from sagemaker.huggingface.model import HuggingFaceModel

# Hub model configuration <https://huggingface.co/models>
hub = {
  'HF_MODEL_ID':'distilbert-base-uncased-distilled-squad', # model_id from hf.co/models
  'HF_TASK':'question-answering'                           # NLP task you want to use for predictions
}

# create Hugging Face Model Class
huggingface_model = HuggingFaceModel(
   env=hub,                                                # configuration for loading model from Hub
   role=role,                                              # IAM role with permissions to create an endpoint
   transformers_version="4.26",                             # Transformers version used
   pytorch_version="1.13",                                  # PyTorch version used
   py_version='py39',                                      # Python version used
)

# deploy model to SageMaker Inference
predictor = huggingface_model.deploy(
   initial_instance_count=1,
   instance_type="ml.m5.xlarge"
)

# example request: you always need to define "inputs"
data = {
"inputs": {
	"question": "What is used for inference?",
	"context": "My Name is Philipp and I live in Nuremberg. This model is used with sagemaker for inference."
	}
}

# request
predictor.predict(data)
```

在您运行我们的请求后，您可以使用以下命令再次删除端点：

```py
# delete endpoint
predictor.delete_endpoint()
```

📓 打开[笔记本](https://github.com/huggingface/notebooks/blob/main/sagemaker/11_deploy_model_from_hf_hub/deploy_transformer_model_from_hf_hub.ipynb)查看如何将模型从 🤗 Hub 部署到 SageMaker 进行推断的示例。

## 使用 🤗 Transformers 和 SageMaker 运行批量转换

[`www.youtube.com/embed/lnTixz0tUBg`](https://www.youtube.com/embed/lnTixz0tUBg)

训练模型后，您可以使用[SageMaker 批量转换](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-batch.html)来执行模型推断。批量转换接受您的推断数据作为 S3 URI，然后 SageMaker 将负责下载数据，运行预测，并将结果上传到 S3。有关批量转换的更多详细信息，请查看[这里](https://docs.aws.amazon.com/sagemaker/latest/dg/batch-transform.html)。

⚠️ 目前 Hugging Face 推断 DLC 仅支持 `.jsonl` 用于批量转换，因为文本数据的结构复杂。

*注意：确保您的 `inputs` 在预处理期间符合模型的 `max_length`。*

如果您使用 Hugging Face Estimator 训练了一个模型，请调用 `transformer()` 方法为基于训练作业的模型创建一个转换作业（有关更多详细信息，请参见[这里](https://sagemaker.readthedocs.io/en/stable/overview.html#sagemaker-batch-transform)）：

```py
batch_job = huggingface_estimator.transformer(
    instance_count=1,
    instance_type='ml.p3.2xlarge',
    strategy='SingleRecord')

batch_job.transform(
    data='s3://s3-uri-to-batch-data',
    content_type='application/json',    
    split_type='Line')
```

如果您想稍后运行批量转换作业或使用 🤗 Hub 中的模型，创建一个 `HuggingFaceModel` 实例，然后调用 `transformer()` 方法：

```py
from sagemaker.huggingface.model import HuggingFaceModel

# Hub model configuration <https://huggingface.co/models>
hub = {
	'HF_MODEL_ID':'distilbert-base-uncased-finetuned-sst-2-english',
	'HF_TASK':'text-classification'
}

# create Hugging Face Model Class
huggingface_model = HuggingFaceModel(
   env=hub,                                                # configuration for loading model from Hub
   role=role,                                              # IAM role with permissions to create an endpoint
   transformers_version="4.26",                             # Transformers version used
   pytorch_version="1.13",                                  # PyTorch version used
   py_version='py39',                                      # Python version used
)

# create transformer to run a batch job
batch_job = huggingface_model.transformer(
    instance_count=1,
    instance_type='ml.p3.2xlarge',
    strategy='SingleRecord'
)

# starts batch transform job and uses S3 data as input
batch_job.transform(
    data='s3://sagemaker-s3-demo-test/samples/input.jsonl',
    content_type='application/json',    
    split_type='Line'
)
```

`input.jsonl` 如下所示：

```py
{"inputs":"this movie is terrible"}
{"inputs":"this movie is amazing"}
{"inputs":"SageMaker is pretty cool"}
{"inputs":"SageMaker is pretty cool"}
{"inputs":"this movie is terrible"}
{"inputs":"this movie is amazing"}
```

📓 打开[笔记本](https://github.com/huggingface/notebooks/blob/main/sagemaker/12_batch_transform_inference/sagemaker-notebook.ipynb)查看如何运行用于推断的批量转换作业的示例。

## 用户定义的代码和模块

Hugging Face 推断工具包允许用户覆盖 `HuggingFaceHandlerService` 的默认方法。您需要创建一个名为 `code/` 的文件夹，并在其中放置一个 `inference.py` 文件。有关如何归档模型工件的更多详细信息，请参阅这里。例如：

```py
model.tar.gz/
|- pytorch_model.bin
|- ....
|- code/
  |- inference.py
  |- requirements.txt 
```

`inference.py` 文件包含您的自定义推断模块，`requirements.txt` 文件包含应添加的附加依赖项。自定义模块可以覆盖以下方法：

+   `model_fn(model_dir)` 覆盖了加载模型的默认方法。返回值 `model` 将在 `predict` 中用于预测。`predict` 接收参数 `model_dir`，即您解压后的 `model.tar.gz` 的路径。

+   `transform_fn(model, data, content_type, accept_type)` 覆盖了默认的转换函数，使用您自定义的实现。您需要在 `transform_fn` 中实现自己的 `preprocess`、`predict` 和 `postprocess` 步骤。此方法不能与下面提到的 `input_fn`、`predict_fn` 或 `output_fn` 结合使用。

+   `input_fn(input_data, content_type)` 覆盖了预处理的默认方法。返回值 `data` 将在 `predict` 中用于预测。输入是：

    +   `input_data` 是您请求的原始主体。

    +   `content_type` 是请求头中的内容类型。

+   `predict_fn(processed_data, model)` 覆盖了预测的默认方法。返回值 `predictions` 将在 `postprocess` 中使用。输入是 `processed_data`，是从 `preprocess` 得到的结果。

+   `output_fn(prediction, accept)` 覆盖了后处理的默认方法。返回值 `result` 将是您请求的响应（例如 `JSON`）。输入是：

    +   `predictions`是来自`predict`的结果。

    +   `accept`是来自 HTTP 请求的返回接受类型，例如`application/json`。

这里是一个带有`model_fn`、`input_fn`、`predict_fn`和`output_fn`的自定义推理模块示例：

```py
from sagemaker_huggingface_inference_toolkit import decoder_encoder

def model_fn(model_dir):
    # implement custom code to load the model
    loaded_model = ...

    return loaded_model 

def input_fn(input_data, content_type):
    # decode the input data  (e.g. JSON string -> dict)
    data = decoder_encoder.decode(input_data, content_type)
    return data

def predict_fn(data, model):
    # call your custom model with the data
    outputs = model(data , ... )
    return predictions

def output_fn(prediction, accept):
    # convert the model output to the desired output format (e.g. dict -> JSON string)
    response = decoder_encoder.encode(prediction, accept)
    return response
```

只使用`model_fn`和`transform_fn`自定义您的推理模块：

```py
from sagemaker_huggingface_inference_toolkit import decoder_encoder

def model_fn(model_dir):
    # implement custom code to load the model
    loaded_model = ...

    return loaded_model 

def transform_fn(model, input_data, content_type, accept):
     # decode the input data (e.g. JSON string -> dict)
    data = decoder_encoder.decode(input_data, content_type)

    # call your custom model with the data
    outputs = model(data , ... ) 

    # convert the model output to the desired output format (e.g. dict -> JSON string)
    response = decoder_encoder.encode(output, accept)

    return response
```
