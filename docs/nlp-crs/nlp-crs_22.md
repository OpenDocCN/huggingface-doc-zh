# 章末小测试

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter2/8?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter2/8?fw=pt)

                Pytorch TensorFlow![Ask a Question](https://discuss.huggingface.co/t/chapter-2-questions)

### 1\. 语言建模 Pipeline 的顺序是什么？

### 2\. Transformer 模型的输出有多少个维度，每个维度分别是什么？

### 3.下列哪一个是 Subword 标记(Tokenization)的例子（从分词的颗粒度来划分）？

### 4.什么是模型的 Head 层？

### 5.什么是 AutoModel？

### 6.当将不同长度的序列批处理在一起时，需要进行哪些处理？

### 7.将 SoftMax 激活函数应用于序列分类(Sequence Classification)模型的 logits 输出有什么意义？

### 8.大多数标记器(Tokenizer)的 API 以什么方法为核心？

### 9.这个代码示例中的 result 变量包含什么？

```py
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")
result = tokenizer.tokenize("Hello!")
```

### 10.下面的代码有什么错误吗？

```py
from transformers import AutoTokenizer, AutoModel

tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")
model = AutoModel.from_pretrained("gpt2")

encoded = tokenizer("Hey!", return_tensors="pt")
result = model(**encoded)
```