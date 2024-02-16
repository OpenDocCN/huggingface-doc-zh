# 在AWS Inferentia上使用Optimum Neuron的句子转换器

> 原文链接：[https://huggingface.co/docs/optimum-neuron/tutorials/sentence_transformers](https://huggingface.co/docs/optimum-neuron/tutorials/sentence_transformers)

*这个教程有一个笔记本版本[在这里](https://github.com/huggingface/optimum-neuron/blob/main/notebooks/sentence-transformers/getting-started.ipynb)。*

本指南解释了如何在AWS Inferentia2上使用Optimum Neuron编译、加载和使用[Sentence Transformers（SBERT）](https://www.sbert.net/)模型，实现嵌入的高效计算。句子转换器是用于生成句子嵌入的强大模型。您可以使用这个句子转换器为100多种语言计算句子/文本嵌入。然后可以将这些嵌入与余弦相似性进行比较，以找到具有相似含义的句子。这对于语义文本相似性、语义搜索或释义挖掘可能很有用。

*注意：目前仅支持文本模型，我们正在为CLIP添加视觉支持。*

## 将句子转换器模型转换为AWS Inferentia2

首先，您需要将您的句子转换器模型转换为与AWS Inferentia2兼容的格式。您可以使用`optimum-cli`或`NeuronModelForSentenceTransformers`类使用Optimum Neuron编译句子转换器模型。下面您将找到两种方法的示例。我们必须确保安装了`sentence-transformers`。这仅在导出模型时需要。

```py
pip install sentence-transformers
```

在这里，我们将使用`NeuronModelForSentenceTransformers`，它可以用于将任何Sntence Transformers模型转换为与AWS Inferentia2兼容的格式或加载已转换的模型。在使用`NeuronModelForSentenceTransformers`导出模型时，您需要设置`export=True`并定义输入形状和批量大小。输入形状由`sequence_length`定义，批量大小由`batch_size`定义。

```py
from optimum.neuron import NeuronModelForSentenceTransformers

# Sentence Transformers model from HuggingFace
model_id = "BAAI/bge-small-en-v1.5"
input_shapes = {"batch_size": 1, "sequence_length": 384}  # mandatory shapes

# Load Transformers model and export it to AWS Inferentia2
model = NeuronModelForSentenceTransformers.from_pretrained(model_id, export=True, **input_shapes)

# Save model to disk
model.save_pretrained("bge_emb_inf2/")
```

在这里，我们将使用`optimum-cli`来转换模型。与`NeuronModelForSentenceTransformers`类似，我们需要定义输入形状和批量大小。输入形状由`sequence_length`定义，批量大小由`batch_size`定义。`optimum-cli`将自动将模型转换为与AWS Inferentia2兼容的格式，并将其保存到指定的输出目录。

```py
optimum-cli export neuron -m BAAI/bge-small-en-v1.5 --library-name sentence_transformers --sequence_length 384 --batch_size 1 --task feature-extraction bge_emb_inf2/
```

## 加载编译的句子转换器模型并运行推理

一旦我们有了编译的句子转换器模型，我们可以导出自己或在Hugging Face Hub上可用，我们可以加载它并运行推理。为了加载模型，我们可以使用`NeuronModelForSentenceTransformers`类，这是`SentenceTransformer`类的抽象层。`NeuronModelForSentenceTransformers`类将自动填充输入到指定的`sequence_length`并在AWS Inferentia2上运行推理。

```py
from optimum.neuron import NeuronModelForSentenceTransformers
from transformers import AutoTokenizer

model_id_or_path = "bge_emb_inf2/"
tokenizer_id = "BAAI/bge-small-en-v1.5"

# Load model and tokenizer
model = NeuronModelForSentenceTransformers.from_pretrained(model_id_or_path)
tokenizer = AutoTokenizer.from_pretrained(tokenizer_id)

# Run inference
prompt = "I like to eat apples"
encoded_input = tokenizer(prompt, return_tensors='pt')
outputs = model(**encoded_input)

token_embeddings = outputs.token_embeddings
sentence_embedding = outputs.sentence_embedding

print(f"token embeddings: {token_embeddings.shape}") # torch.Size([1, 7, 384])
print(f"sentence_embedding: {sentence_embedding.shape}") # torch.Size([1, 384])
```

## 生产用途

要在生产环境中部署这些模型，请参考[Amazon SageMaker博客](https://www.philschmid.de/inferentia2-embeddings)。
