# 参考

> 原始文本：[`huggingface.co/docs/sagemaker/reference`](https://huggingface.co/docs/sagemaker/reference)

## 深度学习容器

下面是当前可用的 Hugging Face DLC 的版本表。该表不包括完整的`image_uri`，如果需要，这里有两个构建的示例。

**手动构建`image_uri`**

`{dlc-aws-account-id}.dkr.ecr.{region}.amazonaws.com/huggingface-{framework}-{(training | inference)}:{framework-version}-transformers{transformers-version}-{device}-{python-version}-{device-tag}`

+   `dlc-aws-account-id`: 拥有 ECR 存储库的帐户的 AWS 帐户 ID。您可以在[这里](https://github.com/aws/sagemaker-python-sdk/blob/e0b9d38e1e3b48647a02af23c4be54980e53dc61/src/sagemaker/image_uri_config/huggingface.json#L21)找到它们

+   `region`: 您想要使用的 AWS 区域。

+   `framework`: 您想要使用的框架，可以是`pytorch`或`tensorflow`。

+   `(training | inference)`: 训练或推理模式。

+   `framework-version`: 您想要使用的框架的版本。

+   `transformers-version`: 您想要使用的 transformers 库的版本。

+   `device`: 您想要使用的设备，可以是`cpu`或`gpu`。

+   `python-version`: DLC 的 Python 版本。

+   `device-tag`: 您想要使用的设备标签。设备标签可以包括操作系统版本和 cuda 版本

**示例 1：PyTorch 训练：** `763104351884.dkr.ecr.us-west-2.amazonaws.com/huggingface-pytorch-training:1.6.0-transformers4.4.2-gpu-py36-cu110-ubuntu18.04` **示例 2：Tensorflow 推理：** `763104351884.dkr.ecr.us-east-1.amazonaws.com/huggingface-tensorflow-inference:2.4.1-transformers4.6.1-cpu-py37-ubuntu18.04`

## 训练 DLC 概述

训练 DLC 概述包括所有发布和可用的 Hugging Face 训练 DLC。它包括用于 GPU 的 PyTorch 和 TensorFlow 版本。

| 🤗 Transformers 版本 | 🤗 数据集版本 | PyTorch/TensorFlow 版本 | 类型 | 设备 | Python 版本 |
| --- | --- | --- | --- | --- | --- |
| 4.4.2 | 1.5.0 | PyTorch 1.6.0 | 训练 | GPU | 3.6 |
| 4.4.2 | 1.5.0 | TensorFlow 2.4.1 | 训练 | GPU | 3.7 |
| 4.5.0 | 1.5.0 | PyTorch 1.6.0 | 训练 | GPU | 3.6 |
| 4.5.0 | 1.5.0 | TensorFlow 2.4.1 | 训练 | GPU | 3.7 |
| 4.6.1 | 1.6.2 | PyTorch 1.6.0 | 训练 | GPU | 3.6 |
| 4.6.1 | 1.6.2 | PyTorch 1.7.1 | 训练 | GPU | 3.6 |
| 4.6.1 | 1.6.2 | TensorFlow 2.4.1 | 训练 | GPU | 3.7 |
| 4.10.2 | 1.11.0 | PyTorch 1.8.1 | 训练 | GPU | 3.6 |
| 4.10.2 | 1.11.0 | PyTorch 1.9.0 | 训练 | GPU | 3.8 |
| 4.10.2 | 1.11.0 | TensorFlow 2.4.1 | 训练 | GPU | 3.7 |
| 4.10.2 | 1.11.0 | TensorFlow 2.5.1 | 训练 | GPU | 3.7 |
| 4.11.0 | 1.12.1 | PyTorch 1.9.0 | 训练 | GPU | 3.8 |
| 4.11.0 | 1.12.1 | TensorFlow 2.5.1 | 训练 | GPU | 3.7 |
| 4.12.3 | 1.15.1 | PyTorch 1.9.1 | 训练 | GPU | 3.8 |
| 4.12.3 | 1.15.1 | TensorFlow 2.5.1 | 训练 | GPU | 3.7 |
| 4.17.0 | 1.18.4 | PyTorch 1.10.2 | 训练 | GPU | 3.8 |
| 4.17.0 | 1.18.4 | TensorFlow 2.6.3 | 训练 | GPU | 3.8 |
| 4.26.0 | 2.9.0 | PyTorch 1.13.1 | 训练 | GPU | 3.9 |

## 推理 DLC 概述

推理 DLC 概述包括所有发布和可用的 Hugging Face 推理 DLC。它包括用于 CPU、GPU 和 AWS Inferentia 的 PyTorch 和 TensorFlow 版本。

| 🤗 Transformers 版本 | PyTorch/TensorFlow 版本 | 类型 | 设备 | Python 版本 |
| --- | --- | --- | --- | --- |
| 4.6.1 | PyTorch 1.7.1 | 推理 | CPU | 3.6 |
| 4.6.1 | PyTorch 1.7.1 | 推理 | GPU | 3.6 |
| 4.6.1 | TensorFlow 2.4.1 | 推理 | CPU | 3.7 |
| 4.6.1 | TensorFlow 2.4.1 | 推理 | GPU | 3.7 |
| 4.10.2 | PyTorch 1.8.1 | 推理 | GPU | 3.6 |
| 4.10.2 | PyTorch 1.9.0 | 推理 | GPU | 3.8 |
| 4.10.2 | TensorFlow 2.4.1 | 推理 | GPU | 3.7 |
| 4.10.2 | TensorFlow 2.5.1 | 推理 | GPU | 3.7 |
| 4.10.2 | PyTorch 1.8.1 | 推理 | CPU | 3.6 |
| 4.10.2 | PyTorch 1.9.0 | 推理 | CPU | 3.8 |
| 4.10.2 | TensorFlow 2.4.1 | 推理 | CPU | 3.7 |
| 4.10.2 | TensorFlow 2.5.1 | 推理 | CPU | 3.7 |
| 4.11.0 | PyTorch 1.9.0 | 推理 | GPU | 3.8 |
| 4.11.0 | TensorFlow 2.5.1 | 推理 | GPU | 3.7 |
| 4.11.0 | PyTorch 1.9.0 | 推理 | CPU | 3.8 |
| 4.11.0 | TensorFlow 2.5.1 | 推理 | CPU | 3.7 |
| 4.12.3 | PyTorch 1.9.1 | 推理 | GPU | 3.8 |
| 4.12.3 | TensorFlow 2.5.1 | 推理 | GPU | 3.7 |
| 4.12.3 | PyTorch 1.9.1 | 推理 | CPU | 3.8 |
| 4.12.3 | TensorFlow 2.5.1 | 推理 | CPU | 3.7 |
| 4.12.3 | PyTorch 1.9.1 | 推理 | Inferentia | 3.7 |
| 4.17.0 | PyTorch 1.10.2 | 推理 | GPU | 3.8 |
| 4.17.0 | TensorFlow 2.6.3 | 推理 | GPU | 3.8 |
| 4.17.0 | PyTorch 1.10.2 | 推理 | CPU | 3.8 |
| 4.17.0 | TensorFlow 2.6.3 | 推理 | CPU | 3.8 |
| 4.26.0 | PyTorch 1.13.1 | 推理 | CPU | 3.9 |
| 4.26.0 | PyTorch 1.13.1 | 推理 | GPU | 3.9 |

## Hugging Face Transformers Amazon SageMaker 示例

展示如何使用 Amazon SageMaker 和 Amazon SageMaker Python SDK 构建、训练和部署 Hugging Face Transformers 的示例 Jupyter 笔记本

| 笔记本 | 类型 | 描述 |
| --- | --- | --- |
| [01 PyTorch 入门](https://github.com/huggingface/notebooks/blob/main/sagemaker/01_getting_started_pytorch/sagemaker-notebook.ipynb) | 训练 | 使用 PyTorch 对预训练的 Hugging Face Transformer 进行文本分类微调的入门端到端示例 |
| [02 TensorFlow 入门](https://github.com/huggingface/notebooks/blob/main/sagemaker/02_getting_started_tensorflow/sagemaker-notebook.ipynb) | 训练 | 使用 TensorFlow 对预训练的 Hugging Face Transformer 进行文本分类微调的入门端到端示例 |
| [03 分布式训练：数据并行](https://github.com/huggingface/notebooks/blob/main/sagemaker/03_distributed_training_data_parallelism/sagemaker-notebook.ipynb) | 训练 | 使用数据并行策略对预训练的 Hugging Face Transformer 进行问答微调的分布式训练端到端示例，使用 Amazon SageMaker 数据并行 |
| [04 分布式训练：模型并行](https://github.com/huggingface/notebooks/blob/main/sagemaker/04_distributed_training_model_parallelism/sagemaker-notebook.ipynb) | 训练 | 使用模型并行策略对预训练的 Hugging Face Transformer 进行分布式训练的端到端示例，使用 Amazon SageMaker 模型并行 |
| [05 如何使用 Spot 实例和检查点](https://github.com/huggingface/notebooks/blob/main/sagemaker/05_spot_instances/sagemaker-notebook.ipynb) | 训练 | 使用 Spot 实例和检查点来降低训练成本的端到端示例 |
| [06 使用 SageMaker Metrics 进行实验跟踪](https://github.com/huggingface/notebooks/blob/main/sagemaker/06_sagemaker_metrics/sagemaker-notebook.ipynb) | 训练 | 使用 SageMaker 指标跟踪实验和训练作业的端到端示例 |
| [07 分布式训练：数据并行](https://github.com/huggingface/notebooks/blob/main/sagemaker/07_tensorflow_distributed_training_data_parallelism/sagemaker-notebook.ipynb) | 训练 | 使用 TensorFlow 和 Amazon SageMaker 数据并行的端到端示例 |
| [08 分布式训练：T5/BART 摘要](https://github.com/huggingface/notebooks/blob/main/sagemaker/08_distributed_summarization_bart_t5/sagemaker-notebook.ipynb) | 训练 | 使用 Amazon SageMaker 数据并行对 BART/T5 进行摘要微调的端到端示例 |
| [09 视觉：微调 ViT](https://github.com/huggingface/notebooks/blob/main/sagemaker/09_image_classification_vision_transformer/sagemaker-notebook.ipynb) | 训练 | 使用 Vision Transformer 进行图像分类的端到端微调示例 |
| [10 从 Amazon S3 部署 HF Transformer](https://github.com/huggingface/notebooks/blob/main/sagemaker/10_deploy_model_from_s3/deploy_transformer_model_from_s3.ipynb) | 推理 | 有关如何从 Amazon S3 部署模型的端到端示例 |
| [11 从 Hugging Face Hub 部署 HF Transformer](https://github.com/huggingface/notebooks/blob/main/sagemaker/11_deploy_model_from_hf_hub/deploy_transformer_model_from_hf_hub.ipynb) | 推理 | 有关如何从 Hugging Face Hub 部署模型的端到端示例 |
| [12 使用 Amazon SageMaker 批量转换进行批处理处理](https://github.com/huggingface/notebooks/blob/main/sagemaker/12_batch_transform_inference/sagemaker-notebook.ipynb) | 推理 | 有关如何使用 Amazon SageMaker 批量转换进行批处理处理的端到端示例 |
| [13 自动缩放 SageMaker 端点](https://github.com/huggingface/notebooks/blob/main/sagemaker/13_deploy_and_autoscaling_transformers/sagemaker-notebook.ipynb) | 推理 | 有关如何为 HF 端点使用自动缩放的端到端示例 |
| [14 微调并推送到 Hub](https://github.com/huggingface/notebooks/blob/main/sagemaker/14_train_and_push_to_hub/sagemaker-notebook.ipynb) | 训练 | 有关如何将 Hugging Face Hub 用作 MLOps 后端以在训练期间保存检查点的端到端示例 |
| [15 训练编译器](https://github.com/huggingface/notebooks/blob/main/sagemaker/15_training_compiler/sagemaker-notebook.ipynb) | 训练 | 有关如何使用 Amazon SageMaker 训练编译器加快训练时间的端到端示例 |
| [16 异步推理](https://github.com/huggingface/notebooks/blob/main/sagemaker/16_async_inference_hf_hub/sagemaker-notebook.ipynb) | 推理 | 有关如何使用 Amazon SageMaker 异步推理端点与 Hugging Face Transformers 的端到端示例 |
| [17 自定义推理.py 脚本](https://github.com/huggingface/notebooks/blob/main/sagemaker/17_custom_inference_script/sagemaker-notebook.ipynb) | 推理 | 有关如何为句子转换器和句子嵌入创建自定义推理.py 的端到端示例 |
| [18 AWS Inferentia](https://github.com/huggingface/notebooks/blob/main/sagemaker/18_inferentia_inference/sagemaker-notebook.ipynb) | 推理 | 有关如何使用 AWS Inferentia 加快推理时间的端到端示例 |

## 推理工具包 API

推理工具包接受`inputs`键中的输入，并在`parameters`键中支持额外的[`pipelines`](https://huggingface.co/docs/transformers/main_classes/pipelines)参数。您可以将`pipelines`中支持的任何`kwargs`作为`parameters`提供。

推理工具包 API 支持的任务包括：

+   **`text-classification`**

+   **`sentiment-analysis`**

+   **`token-classification`**

+   **`feature-extraction`**

+   **`fill-mask`**

+   **`summarization`**

+   **`translation_xx_to_yy`**

+   **`text2text-generation`**

+   **`text-generation`**

+   **`audio-classificatin`**

+   **`automatic-speech-recognition`**

+   **`conversational`**

+   **`image-classification`**

+   **`image-segmentation`**

+   **`object-detection`**

+   **`table-question-answering`**

+   **`zero-shot-classification`**

+   **`zero-shot-image-classification`**

查看以下请求示例，了解一些任务：

**`text-classification`**

```py
{
  "inputs": "This sound track was beautiful! It paints the senery in your mind so well I would recomend it
  even to people who hate vid. game music!"
}
```

**`sentiment-analysis`**

```py
{
  "inputs": "Don't waste your time.  We had two different people come to our house to give us estimates for
a deck (one of them the OWNER).  Both times, we never heard from them.  Not a call, not the estimate, nothing."
}
```

**`token-classification`**

```py
{
  "inputs": "My name is Sylvain and I work at Hugging Face in Brooklyn."
}
```

**`question-answering`**

```py
{
  "inputs": {
    "question": "What is used for inference?",
    "context": "My Name is Philipp and I live in Nuremberg. This model is used with sagemaker for inference."
  }
}
```

**`zero-shot-classification`**

```py
{
  "inputs": "Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!",
  "parameters": {
    "candidate_labels": ["refund", "legal", "faq"]
  }
}
```

**`table-question-answering`**

```py
{
  "inputs": {
    "query": "How many stars does the transformers repository have?",
    "table": {
      "Repository": ["Transformers", "Datasets", "Tokenizers"],
      "Stars": ["36542", "4512", "3934"],
      "Contributors": ["651", "77", "34"],
      "Programming language": ["Python", "Python", "Rust, Python and NodeJS"]
    }
  }
}
```

**`parameterized-request`**

```py
{
  "inputs": "Hugging Face, the winner of VentureBeat’s Innovation in Natural Language Process/Understanding Award for 2021, is looking to level the playing field. The team, launched by Clément Delangue and Julien Chaumond in 2016, was recognized for its work in democratizing NLP, the global market value for which is expected to hit $35.1 billion by 2026\. This week, Google’s former head of Ethical AI Margaret Mitchell joined the team.",
  "parameters": {
    "repetition_penalty": 4.0,
    "length_penalty": 1.5
  }
}
```

## 推理工具包环境变量

推理工具包实现了各种额外的环境变量以简化部署。下面显示了 Hugging Face 特定环境变量的完整列表：

**`HF_TASK`**

`HF_TASK`定义了🤗 Transformers 管道使用的任务。请参阅[此处](https://huggingface.co/docs/transformers/main_classes/pipelines)获取任务的完整列表。

```py
HF_TASK="question-answering"
```

**`HF_MODEL_ID`**

`HF_MODEL_ID` 定义了模型 ID，在创建 SageMaker 端点时会自动从 [hf.co/models](https://huggingface.co/models) 加载。所有 🤗 Hub 的 10,000 多个模型都可以通过这个环境变量获得。

```py
HF_MODEL_ID="distilbert-base-uncased-finetuned-sst-2-english"
```

**`HF_MODEL_REVISION`**

`HF_MODEL_REVISION` 是 `HF_MODEL_ID` 的扩展，允许您定义或固定一个模型修订版本，以确保您始终在 SageMaker 端点上加载相同的模型。

```py
HF_MODEL_REVISION="03b4d196c19d0a73c7e0322684e97db1ec397613"
```

**`HF_API_TOKEN`**

`HF_API_TOKEN` 定义了您的 Hugging Face 授权令牌。`HF_API_TOKEN` 被用作远程文件（如私有模型）的 HTTP bearer 授权。您可以在您的 Hugging Face 账户的 [设置](https://huggingface.co/settings/tokens) 下找到您的令牌。

```py
HF_API_TOKEN="api_XXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
```
