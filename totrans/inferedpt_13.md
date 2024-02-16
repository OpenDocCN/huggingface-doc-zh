# 发送请求到端点

> 原始文本：[`huggingface.co/docs/inference-endpoints/guides/test_endpoint`](https://huggingface.co/docs/inference-endpoints/guides/test_endpoint)

您可以使用 UI 发送请求到推理端点，利用推理小部件或以编程方式，例如使用 cURL、`@huggingface/inference`、`huggingface_hub`或任何 REST 客户端。端点概述不仅为您提供了一个交互式小部件来测试端点，还会为`python`、`javascript`和`curl`生成代码。您可以使用此代码快速开始使用您喜欢的编程语言的端点。

以下还是如何使用`@huggingface/inference`库调用推理端点的示例。

## 使用 UI 发送请求

端点概述提供了访问推理小部件的功能，可以用于发送请求（请参阅创建端点的第 6 步）。这使您可以快速测试不同输入的端点，并与团队成员共享。

## 使用 cURL 发送请求

上述请求的 cURL 命令应如下所示。您需要提供您的用户令牌，该令牌可以在您的 Hugging Face[账户设置](https://huggingface.co/settings/tokens)中找到：

示例请求：

```py
curl https://uu149rez6gw9ehej.eu-west-1.aws.endpoints.huggingface.cloud/distilbert-sentiment \
	-X POST \
	-d '{"inputs": "Deploying my first endpoint was an amazing experience."}' \
	-H "Authorization: Bearer <Token>"
```

端点 API 提供与[推理 API](https://huggingface.co/docs/api-inference/detailed_parameters)和[SageMaker 推理工具包](https://huggingface.co/docs/sagemaker/reference#inference-toolkit-api)相同的 API 定义。所有请求有效负载都在支持的任务部分中有文档记录。

这意味着对于 NLP 任务，有效负载表示为`inputs`键，并且其他管道参数包含在`parameters`键中。您可以提供来自[pipelines](https://huggingface.co/docs/transformers/main_classes/pipelines)支持的任何`kwargs`作为参数。对于图像或音频任务，您应将数据作为具有相应 MIME 类型的二进制请求发送。以下是音频有效负载的示例 cURL：

```py
curl --request POST \
  --url https://uu149rez6gw9ehej.eu-west-1.aws.endpoints.huggingface.cloud/wav2vec-asr \
  --header 'Authorization: Bearer <Token>' \
  --header 'Content-Type: audio/x-flac' \
  --data-binary '@sample1.flac'
```

要将您的 cURL 命令用作代码，请使用[cURL 转换器](https://curlconverter.com/)工具，快速开始使用您选择的编程语言。

## 使用 javascript 库@huggingface/inference

您可以使用 javascript 库调用推理端点：

```py
const inference = new HfInference('hf_...') // your user token

const gpt2 = inference.endpoint('https://xyz.eu-west-1.aws.endpoints.huggingface.cloud/gpt2-endpoint')
const { generated_text } = await gpt2.textGeneration({ inputs: 'The answer to the universe is' })
```

### 自定义处理程序

`@huggingface/inference`支持来自[`huggingface.co/tasks`](https://huggingface.co/tasks)的任务，并相应地进行了类型化。

如果您的模型具有额外的输入，甚至是自定义输入/输出，您可以使用更通用的`.request` / `streamingRequest`：

```py
const output = await inference.request({
  inputs: "blablabla",
  parameters: {
    custom_parameter_1: ...,
    ...
  }
});
```
