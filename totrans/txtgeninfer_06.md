# 消息 API

> 原始文本：[`huggingface.co/docs/text-generation-inference/messages_api`](https://huggingface.co/docs/text-generation-inference/messages_api)

文本生成推理（TGI）现在支持消息 API，与 OpenAI 聊天完成 API 完全兼容。此功能从版本 1.4.0 开始提供。您可以使用 OpenAI 的客户端库或期望 OpenAI 模式的第三方库与 TGI 的消息 API 进行交互。以下是一些如何利用此兼容性的示例。

> **注意：**消息 API 支持 TGI 版本 1.4.0 及以上。确保您使用兼容版本以访问此功能。

#### 目录

+   发出请求

+   流式传输

+   同步

+   拥抱脸推理端点

+   云服务提供商

    +   亚马逊 SageMaker

## 发出请求

您可以使用`curl`向 TGI 的消息 API 发出请求。以下是一个示例：

```py
curl localhost:3000/v1/chat/completions \
    -X POST \
    -d '{
  "model": "tgi",
  "messages": [
    {
      "role": "system",
      "content": "You are a helpful assistant."
    },
    {
      "role": "user",
      "content": "What is deep learning?"
    }
  ],
  "stream": true,
  "max_tokens": 20
}' \
    -H 'Content-Type: application/json'
```

## 流式传输

您还可以使用 OpenAI 的 Python 客户端库发出流式请求。以下是如何操作的示例：

```py
from openai import OpenAI

# init the client but point it to TGI
client = OpenAI(
    base_url="http://localhost:3000/v1",
    api_key="-"
)

chat_completion = client.chat.completions.create(
    model="tgi",
    messages=[
        {"role": "system", "content": "You are a helpful assistant." },
        {"role": "user", "content": "What is deep learning?"}
    ],
    stream=True
)

# iterate and print stream
for message in chat_completion:
    print(message)
```

## 同步

如果您更喜欢发出同步请求，可以这样做：

```py
from openai import OpenAI

# init the client but point it to TGI
client = OpenAI(
    base_url="http://localhost:3000/v1",
    api_key="-"
)

chat_completion = client.chat.completions.create(
    model="tgi",
    messages=[
        {"role": "system", "content": "You are a helpful assistant." },
        {"role": "user", "content": "What is deep learning?"}
    ],
    stream=False
)

print(chat_completion)
```

## 拥抱脸推理端点

消息 API 已与[推理端点](https://huggingface.co/inference-endpoints/dedicated)集成。

现在可以使用具有聊天模板的 LLM 的“文本生成推理”来使用每个端点。以下是如何使用 OpenAI 的 Python 客户端库与 TGI 一起使用 IE 的示例：

> **注意：**确保将`base_url`替换为您的端点 URL，并在 URL 末尾包含`v1/`。`api_key`应替换为您的拥抱脸 API 密钥。

```py
from openai import OpenAI

# init the client but point it to TGI
client = OpenAI(
    # replace with your endpoint url, make sure to include "v1/" at the end
    base_url="https://vlzz10eq3fol3429.us-east-1.aws.endpoints.huggingface.cloud/v1/",
    # replace with your API key
    api_key="hf_XXX"
)

chat_completion = client.chat.completions.create(
    model="tgi",
    messages=[
        {"role": "system", "content": "You are a helpful assistant." },
        {"role": "user", "content": "What is deep learning?"}
    ],
    stream=True
)

# iterate and print stream
for message in chat_completion:
    print(message.choices[0].delta.content, end="")
```

## 云服务提供商

TGI 可以部署在各种云提供商上，用于可扩展和强大的文本生成。亚马逊 SageMaker 是其中之一，最近已添加对 TGI 的支持。以下是如何在亚马逊 SageMaker 上部署 TGI 的方法：

## 亚马逊 SageMaker

要在亚马逊 SageMaker 中启用消息 API，您需要设置环境变量`MESSAGES_API_ENABLED=true`。

这将修改`/invocations`路由以接受由角色和内容组成的消息字典。请参考下面的示例，了解如何使用新的消息 API 部署 Llama。

```py
import json
import sagemaker
import boto3
from sagemaker.huggingface import HuggingFaceModel, get_huggingface_llm_image_uri

try:
 role = sagemaker.get_execution_role()
except ValueError:
 iam = boto3.client('iam')
 role = iam.get_role(RoleName='sagemaker_execution_role')['Role']['Arn']

# Hub Model configuration. https://huggingface.co/models
hub = {
 'HF_MODEL_ID':'HuggingFaceH4/zephyr-7b-beta',
 'SM_NUM_GPUS': json.dumps(1),
 'MESSAGES_API_ENABLED': True
}

# create Hugging Face Model Class
huggingface_model = HuggingFaceModel(
 image_uri=get_huggingface_llm_image_uri("huggingface",version="1.4.0"),
 env=hub,
 role=role,
)

# deploy model to SageMaker Inference
predictor = huggingface_model.deploy(
 initial_instance_count=1,
 instance_type="ml.g5.2xlarge",
 container_startup_health_check_timeout=300,
  )

# send request
predictor.predict({
"messages": [
        {"role": "system", "content": "You are a helpful assistant." },
        {"role": "user", "content": "What is deep learning?"}
    ]
})
```
