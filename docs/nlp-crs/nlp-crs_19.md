# 处理多个序列

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter2/5?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter2/5?fw=pt)

                 Pytorch TensorFlow![Ask a Question](https://discuss.huggingface.co/t/chapter-2-questions) ![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/zh-CN/chapter2/section5_pt.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/zh-CN/chapter2/section5_pt.ipynb)

[`www.youtube-nocookie.com/embed/M6adb1j2jPI`](https://www.youtube-nocookie.com/embed/M6adb1j2jPI)

在上一节中，我们探讨了最简单的用例：对一个小长度的序列进行推理。然而，一些问题已经出现：

*   我们如何处理多个序列？

*   我们如何处理多个序列不同长度?

*   词汇索引是让模型正常工作的唯一输入吗？

*   是否存在序列太长的问题？

让我们看看这些问题会带来什么样的问题，以及如何使用🤗 Transformers API 解决它们

## 模型需要一批输入

在上一个练习中，您看到了序列如何转换为数字列表。让我们将此数字列表转换为张量，并将其发送到模型：

```py
import torch
from transformers import AutoTokenizer, AutoModelForSequenceClassification

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

sequence = "I've been waiting for a HuggingFace course my whole life."

tokens = tokenizer.tokenize(sequence)
ids = tokenizer.convert_tokens_to_ids(tokens)
input_ids = torch.tensor(ids)
# This line will fail.
model(input_ids)
```

```py
IndexError: Dimension out of range (expected to be in range of [-1, 0], but got 1)
```

哦，不！为什么失败了？“我们遵循了第 2 节中管道的步骤。

问题是我们向模型发送了一个序列，而🤗 Transformers 模型默认情况下需要多个句子。在这里，当我们将分词器应用于一个应用程序时，我们尝试在幕后完成分词器所做的一切，但如果仔细观察，您会发现 tokenizer 不仅将输入 ID 列表转换为张量，还在其顶部添加了一个维度：

```py
tokenized_inputs = tokenizer(sequence, return_tensors="pt")
print(tokenized_inputs["input_ids"])
```

```py
tensor([[  101,  1045,  1005,  2310,  2042,  3403,  2005,  1037, 17662, 12172,
          2607,  2026,  2878,  2166,  1012,   102]])
```

让我们重试并添加一个新维度：

```py
import torch
from transformers import AutoTokenizer, AutoModelForSequenceClassification

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

sequence = "I've been waiting for a HuggingFace course my whole life."

tokens = tokenizer.tokenize(sequence)
ids = tokenizer.convert_tokens_to_ids(tokens)

input_ids = torch.tensor([ids])
print("Input IDs:", input_ids)

output = model(input_ids)
print("Logits:", output.logits)
```

我们打印输入 ID 以及生成的 logits-以下是输出：

```py
Input IDs: [[ 1045,  1005,  2310,  2042,  3403,  2005,  1037, 17662, 12172,  2607, 2026,  2878,  2166,  1012]]
Logits: [[-2.7276,  2.8789]]
```

*Batching* 是一次通过模型发送多个句子的行为。如果你只有一句话，你可以用一个序列构建一个批次：

```py
batched_ids = [ids, ids]
```

这是一批两个相同的序列！

✏️ **Try it out!** 试试看！将此列表转换为张量并通过模型传递。检查您是否获得与之前相同的登录（但是只有两次）

批处理允许模型在输入多个句子时工作。使用多个序列就像使用单个序列构建批一样简单。不过，还有第二个问题。当你试图将两个（或更多）句子组合在一起时，它们的长度可能不同。如果您以前使用过张量，那么您知道它们必须是矩形，因此无法将输入 ID 列表直接转换为张量。为了解决这个问题，我们通常填充输入。

## 填充输入

以下列表不能转换为张量：

```py
batched_ids = [
    [200, 200, 200],
    [200, 200]
]
```

为了解决这个问题，我们将使用填充使张量具有矩形。Padding 通过在值较少的句子中添加一个名为 Padding token 的特殊单词来确保我们所有的句子长度相同。例如，如果你有 10 个包含 10 个单词的句子和 1 个包含 20 个单词的句子，填充将确保所有句子都包含 20 个单词。在我们的示例中，生成的张量如下所示：

```py
padding_id = 100

batched_ids = [
    [200, 200, 200],
    [200, 200, padding_id],
]
```

可以在 tokenizer.pad_token_id 中找到填充令牌 ID. 让我们使用它，将我们的两句话分别发送到模型中，并分批发送到一起：

```py
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

sequence1_ids = [[200, 200, 200]]
sequence2_ids = [[200, 200]]
batched_ids = [
    [200, 200, 200],
    [200, 200, tokenizer.pad_token_id],
]

print(model(torch.tensor(sequence1_ids)).logits)
print(model(torch.tensor(sequence2_ids)).logits)
print(model(torch.tensor(batched_ids)).logits)
```

```py
tensor([[ 1.5694, -1.3895]], grad_fn=<AddmmBackward>)
tensor([[ 0.5803, -0.4125]], grad_fn=<AddmmBackward>)
tensor([[ 1.5694, -1.3895],
        [ 1.3373, -1.2163]], grad_fn=<AddmmBackward>)
```

我们批处理预测中的 logits 有点问题：第二行应该与第二句的 logits 相同，但我们得到了完全不同的值！

这是因为 Transformer 模型的关键特性是关注层，它将每个标记上下文化。这些将考虑填充标记，因为它们涉及序列中的所有标记。为了在通过模型传递不同长度的单个句子时，或者在传递一批应用了相同句子和填充的句子时获得相同的结果，我们需要告诉这些注意层忽略填充标记。这是通过使用 attention mask 来实现的。

## 注意力面具

*Attention masks*是与输入 ID 张量形状完全相同的张量，用 0 和 1 填充：1s 表示应注意相应的标记，0s 表示不应注意相应的标记（即，模型的注意力层应忽略它们）。

让我们用 attention mask 完成上一个示例：

```py
batched_ids = [
    [200, 200, 200],
    [200, 200, tokenizer.pad_token_id],
]

attention_mask = [
    [1, 1, 1],
    [1, 1, 0],
]

outputs = model(torch.tensor(batched_ids), attention_mask=torch.tensor(attention_mask))
print(outputs.logits)
```

```py
tensor([[ 1.5694, -1.3895],
        [ 0.5803, -0.4125]], grad_fn=<AddmmBackward>)
```

现在我们得到了该批中第二个句子的相同登录。

请注意，第二个序列的最后一个值是一个填充 ID，它在 attention mask 中是一个 0 值。

✏️ 试试看！在第 2 节中使用的两个句子上手动应用标记化（“我一生都在等待拥抱课程。”和“我非常讨厌这个！”）。通过模型传递它们，并检查您是否获得与第 2 节中相同的登录。现在使用填充标记将它们批处理在一起，然后创建适当的注意掩码。检查通过模型时是否获得相同的结果！

## 长序列

对于 Transformers 模型，我们可以通过模型的序列长度是有限的。大多数模型处理多达 512 或 1024 个令牌的序列，当要求处理更长的序列时，会崩溃。此问题有两种解决方案：

*   使用支持的序列长度较长的模型。

*   截断序列。

模型有不同的支持序列长度，有些模型专门处理很长的序列。 [Longformer](https://huggingface.co/transformers/model_doc/longformer.html) 这是一个例子，另一个是 [LED](https://huggingface.co/transformers/model_doc/led.html) . 如果您正在处理一项需要很长序列的任务，我们建议您查看这些模型。

否则，我们建议您通过指定 max_sequence_length 参数：

```py
sequence = sequence[:max_sequence_length]
```