# 支持的变压器和扩散器任务

> 原始文本: [`huggingface.co/docs/inference-endpoints/supported_tasks`](https://huggingface.co/docs/inference-endpoints/supported_tasks)

推理端点为来自变压器、句子变压器和扩散器库的机器学习任务提供开箱即用的支持。以下是 Hugging Face 管理的推理端点支持的任务表。这些任务不需要任何形式的代码或“自定义容器”来部署端点。如果您想要自定义以下任何任务，或者想要编写自己的自定义任务，请查看“创建自己的推理处理程序”部分以获取更多信息。

| 任务 | 框架 | 开箱即用支持 |
| --- | --- | --- |
| 文本到图像 | 扩散器 | ✅ |
| 文本分类 | 变压器 | ✅ |
| 零射击分类 | 变压器 | ✅ |
| 标记分类 | 变压器 | ✅ |
| 问答 | 变压器 | ✅ |
| 填充掩码 | 变压器 | ✅ |
| 摘要 | 变压器 | ✅ |
| 翻译 | 变压器 | ✅ |
| 文本到文本生成 | 变压器 | ✅ |
| 文本生成 | 变压器 | ✅ |
| 特征提取 | 变压器 | ✅ |
| 句子嵌入 | 句子变压器 | ✅ |
| 句子相似度 | 句子变压器 | ✅ |
| 排名 | 句子变压器 | ✅ |
| 图像分类 | 变压器 | ✅ |
| 自动语音识别 | 变压器 | ✅ |
| 音频分类 | 变压器 | ✅ |
| 目标检测 | 变压器 | ✅ |
| 图像分割 | 变压器 | ✅ |
| 表格问答 | 变压器 | ✅ |
| 对话 | 变压器 | ✅ |
| 自定义 | 自定义 | ✅ |
| 视觉问答 | 变压器 | ❌ |
| 零射击图像分类 | 变压器 | ❌ |

## 示例请求负载

查看以下请求示例，了解一些任务：

### 自定义处理程序

```py
{
  "inputs": "This is a sample input",
  "moreData": 1,
  "customTask": true
}
```

### 文本分类

```py
{
  "inputs": "This sound track was beautiful! It paints the senery in your mind so well I would recomend it
  even to people who hate vid. game music!"
}
```

### 零射击分类

```py
{
  "inputs": "Hi, I recently bought a device from your company but it is not working as advertised and I would like to get reimbursed!",
  "parameters": {
    "candidate_labels": ["refund", "legal", "faq"]
  }
}
```

### 标记分类

```py
{
  "inputs": "This sound track was beautiful! It paints the senery in your mind so well I would recomend it
  even to people who hate vid. game music!"
}
```

### 问答

```py
{
  "inputs": {
    "question": "What is used for inference?",
    "context": "My Name is Philipp and I live in Nuremberg. This model is used with sagemaker for inference."
  }
}
```

### 填充掩码

```py
{
  "inputs": "This sound track was <mask>! It paints the senery in your mind so well I would recomend it
  even to people who hate vid. game music!"
}
```

### 摘要

```py
{
  "inputs": "This sound track was beautiful! It paints the senery in your mind so well I would recomend it
  even to people who hate vid. game music!"
}
```

### 翻译

```py
{
  "inputs": "This sound track was beautiful! It paints the senery in your mind so well I would recomend it
  even to people who hate vid. game music!"
}
```

### 文本到文本生成

```py
{
  "inputs": "This sound track was beautiful! It paints the senery in your mind so well I would recomend it
  even to people who hate vid. game music!"
}
```

### 文本生成

```py
{
  "inputs": "This sound track was beautiful! It paints the senery in your mind so well I would recomend it
  even to people who hate vid. game music!"
}
```

### 特征提取

```py
{
  "inputs": "This sound track was beautiful! It paints the senery in your mind so well I would recomend it
  even to people who hate vid. game music!"
}
```

### 句子嵌入

```py
{
  "inputs": "This sound track was beautiful! It paints the senery in your mind so well I would recomend it
  even to people who hate vid. game music!"
}
```

### 句子相似度

```py
{
  "inputs": ["This sound track was beautiful!", "It paints the senery in your mind so well"]
}
```

### 排名

```py
{
  "inputs": ["This sound track was beautiful!", "It paints the senery in your mind so well"]
}
```

### 图像分类

图像分类可以直接从`图像`接收`json`负载或二进制数据。

**JSON**

```py
{
  "inputs": "/9j/4AAQSkZJRgABAQEBLAEsAAD/2wBDAAMCAgI"
}
```

**二进制**

```py
curl --request POST \
  --url https://{ENDPOINT}/ \
  --header 'Content-Type: image/jpg' \
  --header 'Authorization: Bearer {HF_TOKEN}' \
  --data-binary '@test.jpg'
```

### 自动语音识别

自动语音识别可以直接从`音频`接收`json`负载或二进制数据。

**JSON**

```py
{
  "inputs": "/9j/4AAQSkZJRgABAQEBLAEsAAD/2wBDAAMCAgI"
}
```

**二进制**

```py
curl --request POST \
  --url https://{ENDPOINT}/ \
  --header 'Content-Type: audio/x-flac' \
  --header 'Authorization: Bearer {HF_TOKEN}' \
  --data-binary '@sample.flac'
```

### 音频分类

音频分类可以直接从`音频`接收`json`负载或二进制数据。

**JSON**

```py
{
  "inputs": "/9j/4AAQSkZJRgABAQEBLAEsAAD/2wBDAAMCAgI"
}
```

**二进制**

```py
curl --request POST \
  --url https://{ENDPOINT}/ \
  --header 'Content-Type: audio/x-flac' \
  --header 'Authorization: Bearer {HF_TOKEN}' \
  --data-binary '@sample.flac'
```

### 目标检测

目标检测可以直接从`图像`接收`json`负载或二进制数据。

**JSON**

```py
{
  "inputs": "/9j/4AAQSkZJRgABAQEBLAEsAAD/2wBDAAMCAgI"
}
```

**二进制**

```py
curl --request POST \
  --url https://{ENDPOINT}/ \
  --header 'Content-Type: image/jpg' \
  --header 'Authorization: Bearer {HF_TOKEN}' \
  --data-binary '@test.jpg'
```

### 图像分割

图像分割可以直接从`图像`接收`json`负载或二进制数据。

**JSON**

```py
{
  "inputs": "/9j/4AAQSkZJRgABAQEBLAEsAAD/2wBDAAMCAgI"
}
```

**二进制**

```py
curl --request POST \
  --url https://{ENDPOINT}/ \
  --header 'Content-Type: image/jpg' \
  --header 'Authorization: Bearer {HF_TOKEN}' \
  --data-binary '@test.jpg'
```

### 表格问答

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

### 对话

```py
{        
  "inputs": {
    "past_user_inputs": ["Which movie is the best ?"],
    "generated_responses": ["It's Die Hard for sure."],
    "text": "Can you explain why?",
  }
}
```

### 文本到图像

```py
{        
  "inputs": "realistic render portrait realistic render portrait of group of flying blue whales towards the moon, intricate, toy, sci - fi, extremely detailed, digital painting, sculpted in zbrush, artstation, concept art, smooth, sharp focus, illustration, chiaroscuro lighting, golden ratio, incredible art by artgerm and greg rutkowski and alphonse mucha and simon stalenhag",
}
```

### 附加参数

您可以添加由变压器的`pipelines` api 支持的附加参数。

例如，如果您有一个`文本生成`流水线，您可以为`重复惩罚`或`最大长度`提供`generation_kwargs`

```py
{
  "inputs": "Hugging Face, the winner of VentureBeat’s Innovation in Natural Language Process/Understanding Award for 2021, is looking to level the playing field. The team, launched by Clément Delangue and Julien Chaumond in 2016, was recognized for its work in democratizing NLP, the global market value for which is expected to hit $35.1 billion by 2026\. This week, Google’s former head of Ethical AI Margaret Mitchell joined the team.",
  "parameters": {
    "repetition_penalty": 4.0,
    "max_length": 128
  }
}
```
