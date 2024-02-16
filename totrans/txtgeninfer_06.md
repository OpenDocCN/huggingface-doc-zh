# 消息API

> 原始文本：[https://huggingface.co/docs/text-generation-inference/messages_api](https://huggingface.co/docs/text-generation-inference/messages_api)

文本生成推理（TGI）现在支持消息API，与OpenAI聊天完成API完全兼容。此功能从版本1.4.0开始提供。您可以使用OpenAI的客户端库或期望OpenAI模式的第三方库与TGI的消息API进行交互。以下是一些如何利用此兼容性的示例。

> **注意：**消息API支持TGI版本1.4.0及以上。确保您使用兼容版本以访问此功能。

#### 目录

+   [发出请求](#making-a-request)

+   [流式传输](#streaming)

+   [同步](#synchronous)

+   [拥抱脸推理端点](#hugging-face-inference-endpoints)

+   [云服务提供商](#cloud-providers)

    +   [亚马逊SageMaker](#amazon-sagemaker)

## 发出请求

您可以使用`curl`向TGI的消息API发出请求。以下是一个示例：

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

您还可以使用OpenAI的Python客户端库发出流式请求。以下是如何操作的示例：

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

消息API已与[推理端点](https://huggingface.co/inference-endpoints/dedicated)集成。

现在可以使用具有聊天模板的LLM的“文本生成推理”来使用每个端点。以下是如何使用OpenAI的Python客户端库与TGI一起使用IE的示例：

> **注意：**确保将`base_url`替换为您的端点URL，并在URL末尾包含`v1/`。`api_key`应替换为您的拥抱脸API密钥。

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

TGI可以部署在各种云提供商上，用于可扩展和强大的文本生成。亚马逊SageMaker是其中之一，最近已添加对TGI的支持。以下是如何在亚马逊SageMaker上部署TGI的方法：

## 亚马逊SageMaker

要在亚马逊SageMaker中启用消息API，您需要设置环境变量`MESSAGES_API_ENABLED=true`。

这将修改`/invocations`路由以接受由角色和内容组成的消息字典。请参考下面的示例，了解如何使用新的消息API部署Llama。

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
