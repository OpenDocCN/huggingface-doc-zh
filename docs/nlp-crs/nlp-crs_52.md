# 字节对编码标记化

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter6/5?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter6/5?fw=pt)

                ![Ask a Question](https://discuss.huggingface.co/t/chapter-6-questions) ![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/zh-CN/chapter6/section5.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/zh-CN/chapter6/section5.ipynb)

字节对编码(BPE)最初被开发为一种压缩文本的算法,然后在预训练 GPT 模型时被 OpenAI 用于标记化。许多 Transformer 模型都使用它,包括 GPT、GPT-2、RoBERTa、BART 和 DeBERTa。

[`www.youtube-nocookie.com/embed/HEikzVL-lZU`](https://www.youtube-nocookie.com/embed/HEikzVL-lZU)

💡 本节深入介绍了 BPE,甚至展示了一个完整的实现。如果你只想大致了解标记化算法,可以跳到最后。

## 训练算法

BPE 训练首先计算语料库中使用的唯一单词集(在完成标准化和预标记化步骤之后),然后通过获取用于编写这些单词的所有符号来构建词汇表。一个非常简单的例子,假设我们的语料库使用了这五个词:

```py
"hug", "pug", "pun", "bun", "hugs"
```

基础词汇将是 `["b", "g", "h", "n", "p", "s", "u"]`。对于实际情况,基本词汇表将包含所有 ASCII 字符,至少,可能还包含一些 Unicode 字符。如果您正在标记的示例使用不在训练语料库中的字符,则该字符将转换为未知标记。这就是为什么许多 NLP 模型在分析带有表情符号的内容方面非常糟糕的原因之一。

TGPT-2 和 RoBERTa 标记器(非常相似)有一个聪明的方法来处理这个问题: 他们不把单词看成是用 Unicode 字符写的，而是用字节写的。这样,基本词汇表的大小很小(256),但你能想到的每个字符仍将被包含在内,而不会最终转换为未知标记。这个技巧被称为 *字节级 BPE*。

获得这个基本词汇后,我们添加新的标记,直到通过学习*合并*达到所需的词汇量,这是将现有词汇表的两个元素合并为一个新元素的规则。因此,在开始时,这些合并将创建具有两个字符的标记,然后,随着训练的进行,会创建更长的子词。

在分词器训练期间的任何一步,BPE 算法都会搜索最常见的现有标记对 (“对”,这里我们指的是单词中的两个连续标记)。最频繁的一对将被合并,我们冲洗并重复下一步。

回到我们之前的例子,让我们假设单词具有以下频率:

```py
("hug", 10), ("pug", 5), ("pun", 12), ("bun", 4), ("hugs", 5)
```

意味着 `"hug"` 在语料库中出现了 10 次, `"pug"` 5 次, `"pun"` 12 次, `"bun"` 4 次, 以及 `"hugs"` 5 次。我们通过将每个单词拆分为字符(形成我们初始词汇表的字符)来开始训练,这样我们就可以将每个单词视为一个标记列表:

```py
("h" "u" "g", 10), ("p" "u" "g", 5), ("p" "u" "n", 12), ("b" "u" "n", 4), ("h" "u" "g" "s", 5)
```

然后我们看成对。这对 `("h", "u")` 出现在单词 `"hug"` 和 `"hugs"`中,所以语料库中总共有 15 次。不过,这并不是最频繁的一对:这个荣誉属于 `("u", "g")`,它出现在 `"hug"`, `"pug"`, 以及 `"hugs"`中,在词汇表中总共 20 次。

因此,标记器学习的第一个合并规则是 `("u", "g") -> "ug"`,意思就是 `"ug"` 将被添加到词汇表中,并且这对应该合并到语料库的所有单词中。在这个阶段结束时,词汇表和语料库看起来像这样:

```py
Vocabulary: ["b", "g", "h", "n", "p", "s", "u", "ug"]
Corpus: ("h" "ug", 10), ("p" "ug", 5), ("p" "u" "n", 12), ("b" "u" "n", 4), ("h" "ug" "s", 5)
```

现在我们有一些导致标记长于两个字符的对: 例如 `("h", "ug")`, 在语料库中出现 15 次。然而,这个阶段最频繁的对是 `("u", "n")`,在语料库中出现 16 次,所以学到的第二个合并规则是 `("u", "n") -> "un"`。将其添加到词汇表并合并所有现有的这个对,将出现:

```py
Vocabulary: ["b", "g", "h", "n", "p", "s", "u", "ug", "un"]
Corpus: ("h" "ug", 10), ("p" "ug", 5), ("p" "un", 12), ("b" "un", 4), ("h" "ug" "s", 5)
```

现在最频繁的一对是 `("h", "ug")`,所以我们学习了合并规则 `("h", "ug") -> "hug"`,这给了我们第一个三个字母的标记。合并后,语料库如下所示:

```py
Vocabulary: ["b", "g", "h", "n", "p", "s", "u", "ug", "un", "hug"]
Corpus: ("hug", 10), ("p" "ug", 5), ("p" "un", 12), ("b" "un", 4), ("hug" "s", 5)
```

我们继续这样合并,直到达到我们所需的词汇量。

✏️ **现在轮到你了!**你认为下一个合并规则是什么？

## 标记化算法

标记化紧跟训练过程,从某种意义上说,通过应用以下步骤对新输入进行标记:

1.  规范化
2.  预标记化
3.  将单词拆分为单个字符
4.  将学习的合并规则按顺序应用于这些拆分

让我们以我们在训练期间使用的示例为例,学习三个合并规则:

```py
("u", "g") -> "ug"
("u", "n") -> "un"
("h", "ug") -> "hug"
```

这个单词 `"bug"` 将被标记为 `["b", "ug"]`。然而 `"mug"`,将被标记为 `["[UNK]", "ug"]`,因为字母 `"m"` 不再基本词汇表中。同样,单词`"thug"` 会被标记为 `["[UNK]", "hug"]`: 字母 `"t"` 不在基本词汇表中,应用合并规则首先导致 `"u"` 和 `"g"` 被合并,然后是 `"hu"` 和 `"g"` 被合并。

✏️ **现在轮到你了!** 你认为这个词 `"unhug"` 将如何被标记？

## 实现 BPE

现在让我们看一下 BPE 算法的实现。这不会是你可以在大型语料库上实际使用的优化版本;我们只是想向你展示代码,以便你可以更好地理解算法

首先我们需要一个语料库,所以让我们用几句话创建一个简单的语料库:

```py
corpus = [
    "This is the Hugging Face Course.",
    "This chapter is about tokenization.",
    "This section shows several tokenizer algorithms.",
    "Hopefully, you will be able to understand how they are trained and generate tokens.",
]
```

接下来,我们需要将该语料库预先标记为单词。由于我们正在复制 BPE 标记器(如 GPT-2),我们将使用 `gpt2` 标记器作为预标记化的标记器:

```py
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("gpt2")
```

然后我们在进行预标记化时计算语料库中每个单词的频率:

```py
from collections import defaultdict

word_freqs = defaultdict(int)

for text in corpus:
    words_with_offsets = tokenizer.backend_tokenizer.pre_tokenizer.pre_tokenize_str(text)
    new_words = [word for word, offset in words_with_offsets]
    for word in new_words:
        word_freqs[word] += 1

print(word_freqs)
```

```py
defaultdict(int, {'This': 3, 'Ġis': 2, 'Ġthe': 1, 'ĠHugging': 1, 'ĠFace': 1, 'ĠCourse': 1, '.': 4, 'Ġchapter': 1,
    'Ġabout': 1, 'Ġtokenization': 1, 'Ġsection': 1, 'Ġshows': 1, 'Ġseveral': 1, 'Ġtokenizer': 1, 'Ġalgorithms': 1,
    'Hopefully': 1, ',': 1, 'Ġyou': 1, 'Ġwill': 1, 'Ġbe': 1, 'Ġable': 1, 'Ġto': 1, 'Ġunderstand': 1, 'Ġhow': 1,
    'Ġthey': 1, 'Ġare': 1, 'Ġtrained': 1, 'Ġand': 1, 'Ġgenerate': 1, 'Ġtokens': 1})
```

下一步是计算基本词汇,由语料库中使用的所有字符组成:

```py
alphabet = []

for word in word_freqs.keys():
    for letter in word:
        if letter not in alphabet:
            alphabet.append(letter)
alphabet.sort()

print(alphabet)
```

```py
[ ',', '.', 'C', 'F', 'H', 'T', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'k', 'l', 'm', 'n', 'o', 'p', 'r', 's',
  't', 'u', 'v', 'w', 'y', 'z', 'Ġ']
```

我们还在该词汇表的开头添加了模型使用的特殊标记。对于 GPT-2,唯一的特殊标记是 `"<|endoftext|>"`:

```py
vocab = ["<|endoftext|>"] + alphabet.copy()
```

我们现在需要将每个单词拆分为单独的字符,以便能够开始训练:

```py
splits = {word: [c for c in word] for word in word_freqs.keys()}
```

现在我们已准备好进行训练,让我们编写一个函数来计算每对的频率。我们需要在训练的每个步骤中使用它:

```py
def compute_pair_freqs(splits):
    pair_freqs = defaultdict(int)
    for word, freq in word_freqs.items():
        split = splits[word]
        if len(split) == 1:
            continue
        for i in range(len(split) - 1):
            pair = (split[i], split[i + 1])
            pair_freqs[pair] += freq
    return pair_freqs
```

让我们来看看这个字典在初始拆分后的一部分:

```py
pair_freqs = compute_pair_freqs(splits)

for i, key in enumerate(pair_freqs.keys()):
    print(f"{key}: {pair_freqs[key]}")
    if i >= 5:
        break
```

```py
('T', 'h'): 3
('h', 'i'): 3
('i', 's'): 5
('Ġ', 'i'): 2
('Ġ', 't'): 7
('t', 'h'): 3
```

现在, 找到最频繁的对只需要一个快速的循环:

```py
best_pair = ""
max_freq = None

for pair, freq in pair_freqs.items():
    if max_freq is None or max_freq < freq:
        best_pair = pair
        max_freq = freq

print(best_pair, max_freq)
```

```py
('Ġ', 't') 7
```

所以第一个要学习的合并是 `('Ġ', 't') -> 'Ġt'`, 我们添加 `'Ġt'` 到词汇表:

```py
merges = {("Ġ", "t"): "Ġt"}
vocab.append("Ġt")
```

要继续接下来的步骤,我们需要在我们的`分词`字典中应用该合并。让我们为此编写另一个函数:

```py
def merge_pair(a, b, splits):
    for word in word_freqs:
        split = splits[word]
        if len(split) == 1:
            continue

        i = 0
        while i < len(split) - 1:
            if split[i] == a and split[i + 1] == b:
                split = split[:i] + [a + b] + split[i + 2 :]
            else:
                i += 1
        splits[word] = split
    return splits
```

我们可以看看第一次合并的结果:

```py
splits = merge_pair("Ġ", "t", splits)
print(splits["Ġtrained"])
```

```py
['Ġt', 'r', 'a', 'i', 'n', 'e', 'd']
```

现在我们有了循环所需的一切,直到我们学会了我们想要的所有合并。我们的目标是词汇量达到 50:

```py
vocab_size = 50

while len(vocab) < vocab_size:
    pair_freqs = compute_pair_freqs(splits)
    best_pair = ""
    max_freq = None
    for pair, freq in pair_freqs.items():
        if max_freq is None or max_freq < freq:
            best_pair = pair
            max_freq = freq
    splits = merge_pair(*best_pair, splits)
    merges[best_pair] = best_pair[0] + best_pair[1]
    vocab.append(best_pair[0] + best_pair[1])
```

结果,我们学习了 19 条合并规则(初始词汇表的大小 31 — 30 字母字符,加上特殊标记):

```py
print(merges)
```

```py
{('Ġ', 't'): 'Ġt', ('i', 's'): 'is', ('e', 'r'): 'er', ('Ġ', 'a'): 'Ġa', ('Ġt', 'o'): 'Ġto', ('e', 'n'): 'en',
 ('T', 'h'): 'Th', ('Th', 'is'): 'This', ('o', 'u'): 'ou', ('s', 'e'): 'se', ('Ġto', 'k'): 'Ġtok',
 ('Ġtok', 'en'): 'Ġtoken', ('n', 'd'): 'nd', ('Ġ', 'is'): 'Ġis', ('Ġt', 'h'): 'Ġth', ('Ġth', 'e'): 'Ġthe',
 ('i', 'n'): 'in', ('Ġa', 'b'): 'Ġab', ('Ġtoken', 'i'): 'Ġtokeni'}
```

词汇表由特殊标记、初始字母和所有合并结果组成:

```py
print(vocab)
```

```py
['<|endoftext|>', ',', '.', 'C', 'F', 'H', 'T', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'k', 'l', 'm', 'n', 'o',
 'p', 'r', 's', 't', 'u', 'v', 'w', 'y', 'z', 'Ġ', 'Ġt', 'is', 'er', 'Ġa', 'Ġto', 'en', 'Th', 'This', 'ou', 'se',
 'Ġtok', 'Ġtoken', 'nd', 'Ġis', 'Ġth', 'Ġthe', 'in', 'Ġab', 'Ġtokeni']
```

💡 在同一语料库上使用 `train_new_from_iterator()` 不会产生完全相同的词汇表。这是因为当有最频繁对的选择时,我们选择遇到的第一个, 而 🤗 Tokenizers 库根据内部 ID 选择第一个。

为了对新文本进行分词,我们对其进行预分词、拆分，然后应用学到的所有合并规则:

```py
def tokenize(text):
    pre_tokenize_result = tokenizer._tokenizer.pre_tokenizer.pre_tokenize_str(text)
    pre_tokenized_text = [word for word, offset in pre_tokenize_result]
    splits = [[l for l in word] for word in pre_tokenized_text]
    for pair, merge in merges.items():
        for idx, split in enumerate(splits):
            i = 0
            while i < len(split) - 1:
                if split[i] == pair[0] and split[i + 1] == pair[1]:
                    split = split[:i] + [merge] + split[i + 2 :]
                else:
                    i += 1
            splits[idx] = split

    return sum(splits, [])
```

我们可以在任何由字母表中的字符组成的文本上尝试这个:

```py
tokenize("This is not a token.")
```

```py
['This', 'Ġis', 'Ġ', 'n', 'o', 't', 'Ġa', 'Ġtoken', '.']
```

⚠️ 如果存在未知字符,我们的实现将抛出错误,因为我们没有做任何处理它们。GPT-2 实际上没有未知标记(使用字节级 BPE 时不可能得到未知字符),但这可能发生在这里,因为我们没有在初始词汇表中包含所有可能的字节。 BPE 的这方面超出了本节的范围,因此我们忽略了细节。

这就是 BPE 算法！接下来,我们将看看 WordPiece。