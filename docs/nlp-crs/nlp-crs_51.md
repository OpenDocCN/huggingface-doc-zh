# 标准化和预标记化

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter6/4?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter6/4?fw=pt)

                ![Ask a Question](https://discuss.huggingface.co/t/chapter-6-questions) ![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/zh-CN/chapter6/section4.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/zh-CN/chapter6/section4.ipynb)

在我们更深入地研究与 Transformer 模型（字节对编码 [BPE]、WordPiece 和 Unigram）一起使用的三种最常见的子词标记化算法之前，我们将首先看一下每个标记器应用于文本的预处理。以下是标记化管道中步骤的高级概述：

![The tokenization pipeline.](img/648d9f2ea149d92346d8e5b52684a09c.png) ![The tokenization pipeline.](img/84d45b2e06b08900ef9e1ab86334bfe0.png)

在将文本拆分为子标记之前（根据其模型），分词器执行两个步骤： *normalization* 和 *pre-tokenization*.

## 正常化

[`www.youtube-nocookie.com/embed/4IIC2jI9CaU`](https://www.youtube-nocookie.com/embed/4IIC2jI9CaU)

标准化步骤涉及一些常规清理，例如删除不必要的空格、小写和/或删除重音符号。如果你熟悉[Unicode normalization](http://www.unicode.org/reports/tr15/)（例如 NFC 或 NFKC），这也是 tokenizer 可能应用的东西。

🤗Transformers **tokenizer** 有一个属性叫做 **backend_tokenizer** 它提供了对 🤗 Tokenizers 库中底层标记器的访问：

```py
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")
print(type(tokenizer.backend_tokenizer))
```

```py
<class 'tokenizers.Tokenizer'>
```

**normalizer** 的属性 **tokenizer** 对象有一个 **normalize_str()** 我们可以用来查看标准化是如何执行的方法：

```py
print(tokenizer.backend_tokenizer.normalizer.normalize_str("Héllò hôw are ü?"))
```

```py
'hello how are u?'
```

在这个例子中，因为我们选择了 **bert-base-uncased** 检查点，标准化应用小写并删除重音。

✏️ **试试看!** 从检查点加载标记器并将相同的示例传递给它。您可以看到分词器的带壳和无壳版本之间的主要区别是什么？

## 预标记化

[`www.youtube-nocookie.com/embed/grlLV8AIXug`](https://www.youtube-nocookie.com/embed/grlLV8AIXug)

正如我们将在下一节中看到的，分词器不能单独在原始文本上进行训练。相反，我们首先需要将文本拆分为小实体，例如单词。这就是预标记化步骤的用武之地。 正如我们在 Chapter 2, 基于单词的标记器可以简单地将原始文本拆分为空白和标点符号的单词。这些词将是分词器在训练期间可以学习的子标记的边界。

要查看快速分词器如何执行预分词，我们可以使用 **pre_tokenize_str()** 的方法 **pre_tokenizer** 的属性 **tokenizer** 目的：

```py
tokenizer.backend_tokenizer.pre_tokenizer.pre_tokenize_str("Hello, how are  you?")
```

```py
[('Hello', (0, 5)), (',', (5, 6)), ('how', (7, 10)), ('are', (11, 14)), ('you', (16, 19)), ('?', (19, 20))]
```

请注意分词器如何已经跟踪偏移量，这就是它如何为我们提供上一节中使用的偏移量映射。这里分词器忽略了这两个空格，只用一个替换它们，但偏移量在 **are** 和 **you** 考虑到这一点。

由于我们使用的是 BERT 分词器，预分词涉及对空格和标点符号进行拆分。对于这一步，其他标记器可以有不同的规则。例如，如果我们使用 GPT-2 标记器：

```py
tokenizer = AutoTokenizer.from_pretrained("gpt2")
tokenizer.backend_tokenizer.pre_tokenizer.pre_tokenize_str("Hello, how are  you?")
```

它也会在空格和标点符号上拆分，但它会保留空格并将它们替换为 **Ġ** 符号，如果我们解码令牌，则使其能够恢复原始空格：

```py
[('Hello', (0, 5)), (',', (5, 6)), ('Ġhow', (6, 10)), ('Ġare', (10, 14)), ('Ġ', (14, 15)), ('Ġyou', (15, 19)),
 ('?', (19, 20))]
```

另请注意，与 BERT 分词器不同，此分词器不会忽略双空格

最后一个例子，让我们看一下基于 SentencePiece 算法的 T5 分词器：

```py
tokenizer = AutoTokenizer.from_pretrained("t5-small")
tokenizer.backend_tokenizer.pre_tokenizer.pre_tokenize_str("Hello, how are  you?")
```

```py
[('▁Hello,', (0, 6)), ('▁how', (7, 10)), ('▁are', (11, 14)), ('▁you?', (16, 20))]
```

与 GPT-2 标记器一样，这个标记器保留空格并用特定标记替换它们（ **_** )，但 T5 分词器只在空格上拆分，而不是标点符号。还要注意，它默认在句子的开头添加了一个空格（之前 **Hello** ) 并忽略了之间的双空格 **are** 和 **you** .

现在我们已经了解了一些不同的标记器如何处理文本，我们可以开始探索底层算法本身。我们首先快速浏览一下广泛适用的 SentencePiece；然后，在接下来的三个部分中，我们将研究用于子词标记化的三种主要算法是如何工作的。

## 句子

[SentencePiece](https://github.com/google/sentencepiece) 是一种用于文本预处理的标记化算法，您可以将其与我们将在接下来的三个部分中看到的任何模型一起使用。它将文本视为 Unicode 字符序列，并用特殊字符替换空格， **▁** .与 Unigram 算法结合使用（参见 section 7), 它甚至不需要预标记化步骤，这对于不使用空格字符的语言（如中文或日语）非常有用。

SentencePiece 的另一个主要特点是可逆标记化：由于没有对空格进行特殊处理，因此只需通过将它们连接起来并替换 **_** s 带空格——这会导致标准化的文本。正如我们之前看到的，BERT 分词器删除了重复的空格，因此它的分词是不可逆的。

## 算法概述

在下面的部分中，我们将深入研究三种主要的子词标记化算法：BPE（由 GPT-2 和其他人使用）、WordPiece（例如由 BERT 使用）和 Unigram（由 T5 和其他人使用）。在我们开始之前，这里是它们各自工作原理的快速概述。如果您还没有理解，请在阅读下一节后立即回到此表。

| Model | BPE | WordPiece | Unigram |
| :-: | :-: | :-: | :-: |
| Training | Starts from a small vocabulary and learns rules to merge tokens | Starts from a small vocabulary and learns rules to merge tokens | Starts from a large vocabulary and learns rules to remove tokens |
| Training step | Merges the tokens corresponding to the most common pair | Merges the tokens corresponding to the pair with the best score based on the frequency of the pair, privileging pairs where each individual token is less frequent | Removes all the tokens in the vocabulary that will minimize the loss computed on the whole corpus |
| Learns | Merge rules and a vocabulary | Just a vocabulary | A vocabulary with a score for each token |
| Encoding | Splits a word into characters and applies the merges learned during training | Finds the longest subword starting from the beginning that is in the vocabulary, then does the same for the rest of the word | Finds the most likely split into tokens, using the scores learned during training |

现在让我们深入了解 BPE！