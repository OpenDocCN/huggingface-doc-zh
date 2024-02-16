# 处理数据

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter3/2?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter3/2?fw=pt)

                 Pytorch TensorFlow![Ask a Question](https://discuss.huggingface.co/t/chapter-3-questions) ![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/zh-CN/chapter3/section2_pt.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/zh-CN/chapter3/section2_pt.ipynb)

这一小节学习第一小节中提到的“如何使用模型中心（hub）大型数据集”，下面是我们用模型中心的数据在 PyTorch 上训练句子分类器的一个例子：

```py
import torch
from transformers import AdamW, AutoTokenizer, AutoModelForSequenceClassification

# Same as before
checkpoint = "bert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)
sequences = [
    "I've been waiting for a HuggingFace course my whole life.",
    "This course is amazing!",
]
batch = tokenizer(sequences, padding=True, truncation=True, return_tensors="pt")

# This is new
batch["labels"] = torch.tensor([1, 1])

optimizer = AdamW(model.parameters())
loss = model(**batch).loss
loss.backward()
optimizer.step()
```

当然，仅仅用两句话训练模型不会产生很好的效果。为了获得更好的结果，您需要准备一个更大的数据集。

在本节中，我们将使用 MRPC（微软研究释义语料库）数据集作为示例，该数据集由威廉·多兰和克里斯·布罗克特在[这篇文章](https://www.aclweb.org/anthology/I05-5002.pdf)发布。该数据集由 5801 对句子组成，每个句子对带有一个标签，指示它们是否为同义（即，如果两个句子的意思相同）。我们在本章中选择了它，因为它是一个小数据集，所以很容易对它进行训练。

### 从模型中心（Hub）加载数据集

[`www.youtube-nocookie.com/embed/_BZearw7f0w`](https://www.youtube-nocookie.com/embed/_BZearw7f0w)

模型中心（hub）不只是包含模型；它也有许多不同语言的多个数据集。点击[数据集](https://huggingface.co/datasets)的链接即可进行浏览。我们建议您在阅读本节后阅读一下[加载和处理新的数据集](https://huggingface.co/docs/datasets/loading)这篇文章，这会让您对 huggingface 的 darasets 更加清晰。但现在，让我们使用 MRPC 数据集中的[GLUE 基准测试数据集](https://gluebenchmark.com/)，它是构成 MRPC 数据集的 10 个数据集之一，这是一个学术基准，用于衡量机器学习模型在 10 个不同文本分类任务中的性能。

🤗 Datasets 库提供了一个非常便捷的命令，可以在模型中心（hub）上下载和缓存数据集。我们可以通过以下的代码下载 MRPC 数据集：

```py
from datasets import load_dataset

raw_datasets = load_dataset("glue", "mrpc")
raw_datasets
```

```py
DatasetDict({
    train: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 3668
    })
    validation: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 408
    })
    test: Dataset({
        features: ['sentence1', 'sentence2', 'label', 'idx'],
        num_rows: 1725
    })
})
```

正如你所看到的，我们获得了一个**DatasetDict**对象，其中包含训练集、验证集和测试集。每一个集合都包含几个列(**sentence1**, **sentence2**, **label**, and **idx**)以及一个代表行数的变量，即每个集合中的行的个数（因此，训练集中有 3668 对句子，验证集中有 408 对，测试集中有 1725 对）。

默认情况下，此命令在下载数据集并缓存到 **~/.cache/huggingface/datasets**. 回想一下第二章，您可以通过设置**HF_HOME**环境变量来自定义缓存的文件夹。

我们可以访问我们数据集中的每一个**raw_train_dataset**对象，如使用字典：

```py
raw_train_dataset = raw_datasets["train"]
raw_train_dataset[0]
```

```py
{'idx': 0,
 'label': 1,
 'sentence1': 'Amrozi accused his brother , whom he called " the witness " , of deliberately distorting his evidence .',
 'sentence2': 'Referring to him as only " the witness " , Amrozi accused his brother of deliberately distorting his evidence .'}
```

我们可以看到标签已经是整数了，所以我们不需要对标签做任何预处理。要知道哪个数字对应于哪个标签，我们可以查看**raw_train_dataset**的**features**. 这将告诉我们每列的类型：

```py
raw_train_dataset.features
```

```py
{'sentence1': Value(dtype='string', id=None),
 'sentence2': Value(dtype='string', id=None),
 'label': ClassLabel(num_classes=2, names=['not_equivalent', 'equivalent'], names_file=None, id=None),
 'idx': Value(dtype='int32', id=None)}
```

在上面的例子之中,**Label（标签）** 是一种**ClassLabel（分类标签）**，使用整数建立起到类别标签的映射关系。**0**对应于**not_equivalent**，**1**对应于**equivalent**。

✏️ **试试看！** 查看训练集的第 15 行元素和验证集的 87 行元素。他们的标签是什么？

### 预处理数据集

[`www.youtube-nocookie.com/embed/0u3ioSwev3s`](https://www.youtube-nocookie.com/embed/0u3ioSwev3s)

为了预处理数据集，我们需要将文本转换为模型能够理解的数字。正如你在第二章上看到的那样

```py
from transformers import AutoTokenizer

checkpoint = "bert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
tokenized_sentences_1 = tokenizer(raw_datasets["train"]["sentence1"])
tokenized_sentences_2 = tokenizer(raw_datasets["train"]["sentence2"])
```

然而，在两句话传递给模型，预测这两句话是否是同义之前。我们需要这两句话依次进行适当的预处理。幸运的是，标记器不仅仅可以输入单个句子还可以输入一组句子，并按照我们的 BERT 模型所期望的输入进行处理：

```py
inputs = tokenizer("This is the first sentence.", "This is the second one.")
inputs
```

```py
{ 
  'input_ids': [101, 2023, 2003, 1996, 2034, 6251, 1012, 102, 2023, 2003, 1996, 2117, 2028, 1012, 102],
  'token_type_ids': [0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1],
  'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
}
```

我们在第二章 讨论了**输入词 id(input_ids)** 和 **注意力遮罩(attention_mask)** ，但我们在那个时候没有讨论**类型标记 ID(token_type_ids)**。在这个例子中，**类型标记 ID(token_type_ids)**的作用就是告诉模型输入的哪一部分是第一句，哪一部分是第二句。

✏️ **试试看！** 选取训练集中的第 15 个元素，将两句话分别标记为一对。结果和上方的例子有什么不同？

如果我们将**input_ids**中的 id 转换回文字:

```py
tokenizer.convert_ids_to_tokens(inputs["input_ids"])
```

我们将得到：

```py
['[CLS]', 'this', 'is', 'the', 'first', 'sentence', '.', '[SEP]', 'this', 'is', 'the', 'second', 'one', '.', '[SEP]']
```

所以我们看到模型需要输入的形式是 **[CLS] sentence1 [SEP] sentence2 [SEP]**。因此，当有两句话的时候。**类型标记 ID(token_type_ids)** 的值是：

```py
['[CLS]', 'this', 'is', 'the', 'first', 'sentence', '.', '[SEP]', 'this', 'is', 'the', 'second', 'one', '.', '[SEP]']
[      0,      0,    0,     0,       0,          0,   0,       0,      1,    1,     1,        1,     1,   1,       1]
```

如您所见，输入中 **[CLS] sentence1 [SEP]** 它们的类型标记 ID 均为**0**，而其他部分，对应于**sentence2 [SEP]**，所有的类型标记 ID 均为**1**.

请注意，如果选择其他的检查点，则不一定具有**类型标记 ID(token_type_ids)**（例如，如果使用 DistilBERT 模型，就不会返回它们）。只有当它在预训练期间使用过这一层，模型在构建时依赖它们，才会返回它们。

用类型标记 ID 对 BERT 进行预训练,并且使用第一章的遮罩语言模型，还有一个额外的应用类型，叫做下一句预测. 这项任务的目标是建立成对句子之间关系的模型。

在下一个句子预测任务中，会给模型输入成对的句子（带有随机遮罩的标记），并被要求预测第二个句子是否紧跟第一个句子。为了提高模型的泛化能力，数据集中一半的两个句子在原始文档中挨在一起，另一半的两个句子来自两个不同的文档。

一般来说，你不需要担心是否有**类型标记 ID(token_type_ids)**。在您的标输入中：只要您对标记器和模型使用相同的检查点，一切都会很好，因为标记器知道向其模型提供什么。

现在我们已经了解了标记器如何处理一对句子，我们可以使用它对整个数据集进行处理：如之前的章节，我们可以给标记器提供一组句子，第一个参数是它第一个句子的列表，第二个参数是第二个句子的列表。这也与我们在第二章中看到的填充和截断选项兼容. 因此，预处理训练数据集的一种方法是：

```py
tokenized_dataset = tokenizer(
    raw_datasets["train"]["sentence1"],
    raw_datasets["train"]["sentence2"],
    padding=True,
    truncation=True,
)
```

这很有效，但它的缺点是返回字典（字典的键是**输入词 id(input_ids)** ， **注意力遮罩(attention_mask)** 和 **类型标记 ID(token_type_ids)**，字典的值是键所对应值的列表）。而且只有当您在转换过程中有足够的内存来存储整个数据集时才不会出错（而🤗数据集库中的数据集是以[Apache Arrow](https://arrow.apache.org/)文件存储在磁盘上，因此您只需将接下来要用的数据加载在内存中，因此会对内存容量的需求要低一些）。

为了将数据保存为数据集，我们将使用[Dataset.map()](https://huggingface.co/docs/datasets/package_reference/main_classes#datasets.Dataset.map)方法，如果我们需要做更多的预处理而不仅仅是标记化，那么这也给了我们一些额外的自定义的方法。这个方法的工作原理是在数据集的每个元素上应用一个函数，因此让我们定义一个标记输入的函数：

```py
def tokenize_function(example):
    return tokenizer(example["sentence1"], example["sentence2"], truncation=True)
```

此函数的输入是一个字典（与数据集的项类似），并返回一个包含**输入词 id(input_ids)** ， **注意力遮罩(attention_mask)** 和 **类型标记 ID(token_type_ids)** 键的新字典。请注意，如果像上面的**示例**一样，如果键所对应的值包含多个句子（每个键作为一个句子列表），那么它依然可以工作，就像前面的例子一样标记器可以处理成对的句子列表。这样的话我们可以在调用**map()**使用该选项 **batched=True** ，这将显著加快标记与标记的速度。这个**标记器**来自[🤗 Tokenizers](https://github.com/huggingface/tokenizers)库由 Rust 编写而成。当我们一次给它大量的输入时，这个标记器可以非常快。

请注意，我们现在在标记函数中省略了**padding**参数。这是因为在标记的时候将所有样本填充到最大长度的效率不高。一个更好的做法：在构建批处理时填充样本更好，因为这样我们只需要填充到该批处理中的最大长度，而不是整个数据集的最大长度。当输入长度变化很大时，这可以节省大量时间和处理能力!

下面是我们如何在所有数据集上同时应用标记函数。我们在调用**map**时使用了**batch =True**，这样函数就可以同时应用到数据集的多个元素上，而不是分别应用到每个元素上。这将使我们的预处理快许多

```py
tokenized_datasets = raw_datasets.map(tokenize_function, batched=True)
tokenized_datasets
```

🤗Datasets 库应用这种处理的方式是向数据集添加新的字段，每个字段对应预处理函数返回的字典中的每个键:

```py
DatasetDict({
    train: Dataset({
        features: ['attention_mask', 'idx', 'input_ids', 'label', 'sentence1', 'sentence2', 'token_type_ids'],
        num_rows: 3668
    })
    validation: Dataset({
        features: ['attention_mask', 'idx', 'input_ids', 'label', 'sentence1', 'sentence2', 'token_type_ids'],
        num_rows: 408
    })
    test: Dataset({
        features: ['attention_mask', 'idx', 'input_ids', 'label', 'sentence1', 'sentence2', 'token_type_ids'],
        num_rows: 1725
    })
})
```

在使用预处理函数**map()**时，甚至可以通过传递**num_proc**参数使用并行处理。我们在这里没有这样做，因为🤗标记器库已经使用多个线程来更快地标记我们的样本，但是如果您没有使用该库支持的快速标记器，使用**num_proc**可能会加快预处理。

我们的**标记函数(tokenize_function)**返回包含**输入词 id(input_ids)** ， **注意力遮罩(attention_mask)** 和 **类型标记 ID(token_type_ids)** 键的字典,所以这三个字段被添加到数据集的标记的结果中。注意，如果预处理函数**map()**为现有键返回一个新值，那将会修改原有键的值。

最后一件我们需要做的事情是，当我们一起批处理元素时，将所有示例填充到最长元素的长度——我们称之为动态填充。

### 动态填充

[`www.youtube-nocookie.com/embed/7q5NyFT8REg`](https://www.youtube-nocookie.com/embed/7q5NyFT8REg)

负责在批处理中将数据整理为一个 batch 的函数称为*collate 函数*。它是你可以在构建**DataLoader**时传递的一个参数，默认是一个函数，它将把你的数据集转换为 PyTorch 张量，并将它们拼接起来(如果你的元素是列表、元组或字典，则会使用递归)。这在我们的这个例子中下是不可行的，因为我们的输入不是都是相同大小的。我们故意在之后每个 batch 上进行填充，避免有太多填充的过长的输入。这将大大加快训练速度，但请注意，如果你在 TPU 上训练，这可能会导致问题——TPU 喜欢固定的形状，即使这需要额外的填充。

为了解决句子长度统一的问题，我们必须定义一个 collate 函数，该函数会将每个 batch 句子填充到正确的长度。幸运的是，🤗transformer 库通过**DataCollatorWithPadding**为我们提供了这样一个函数。当你实例化它时，需要一个标记器(用来知道使用哪个词来填充，以及模型期望填充在左边还是右边)，并将做你需要的一切:

```py
from transformers import DataCollatorWithPadding

data_collator = DataCollatorWithPadding(tokenizer=tokenizer)
```

为了测试这个新玩具，让我们从我们的训练集中抽取几个样本。这里，我们删除列**idx**, **sentence1**和**sentence2**，因为不需要它们，并查看一个 batch 中每个条目的长度:

```py
samples = tokenized_datasets["train"][:8]
samples = {k: v for k, v in samples.items() if k not in ["idx", "sentence1", "sentence2"]}
[len(x) for x in samples["input_ids"]]
```

```py
[50, 59, 47, 67, 59, 50, 62, 32]
```

毫无疑问，我们得到了不同长度的样本，从 32 到 67。动态填充意味着该批中的所有样本都应该填充到长度为 67，这是该批中的最大长度。如果没有动态填充，所有的样本都必须填充到整个数据集中的最大长度，或者模型可以接受的最大长度。让我们再次检查**data_collator**是否正确地动态填充了这批样本：

```py

```py
batch = data_collator(samples)
{k: v.shape for k, v in batch.items()}
```py

```
{'attention_mask': torch.Size([8, 67]),
 'input_ids': torch.Size([8, 67]),
 'token_type_ids': torch.Size([8, 67]),
 'labels': torch.Size([8])}
```

看起来不错！现在，我们已经将原始文本转化为了模型可以处理的数据，我们已准备好对其进行微调！

✏️ **试试看！** 在 GLUE SST-2 数据集上应用预处理。它有点不同，因为它是由单个句子而不是成对的句子组成的，但是我们所做的其他事情看起来应该是一样的。另一个更难的挑战，请尝试编写一个可用于任何 GLUE 任务的预处理函数。