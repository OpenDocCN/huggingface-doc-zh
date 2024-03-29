# WordPiece 标记化

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter6/6?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter6/6?fw=pt)

                ![Ask a Question](https://discuss.huggingface.co/t/chapter-6-questions) ![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/zh-CN/chapter6/section6.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/zh-CN/chapter6/section6.ipynb)

WordPiece 是 Google 为预训练 BERT 而开发的标记化算法。此后,它在不少基于 BERT 的 Transformer 模型中得到重用,例如 DistilBERT、MobileBERT、Funnel Transformers 和 MPNET。它在训练方面与 BPE 非常相似,但实际标记化的方式不同。

[`www.youtube-nocookie.com/embed/qpv6ms_t_1A`](https://www.youtube-nocookie.com/embed/qpv6ms_t_1A)

💡 本节深入介绍 WordPiece,甚至展示完整的实现。如果您只想大致了解标记化算法,可以跳到最后。

## 训练算法

⚠️ Google 从未开源 WordPiece 训练算法的实现,因此以下是我们基于已发表文献的最佳猜测。它可能不是 100% 准确的。

与 BPE 一样,WordPiece 从一个小词汇表开始,包括模型使用的特殊标记和初始字母表。因为它通过添加前缀来识别子词 (如同 `##` 对于 BERT),每个单词最初是通过将该前缀添加到单词内的所有字符来拆分的。所以,例如 `"word"` ,像这样拆分:

```py
w ##o ##r ##d
```

因此,初始字母表包含出现在单词开头的所有字符以及出现在单词内部的以 WordPiece 前缀开头的字符。

然后,再次像 BPE 一样,WordPiece 学习合并规则。主要区别在于选择要合并的对的方式。WordPiece 不是选择最频繁的对，而是使用以下公式计算每对的分数: <math display="block"><semantics><mrow><mrow><mi mathvariant="normal">s</mi><mi mathvariant="normal">c</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">e</mi></mrow><mo>=</mo><mo stretchy="false">(</mo><mrow><mi mathvariant="normal">f</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">q</mi><mi mathvariant="normal">_</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">f</mi><mi mathvariant="normal">_</mi><mi mathvariant="normal">p</mi><mi mathvariant="normal">a</mi><mi mathvariant="normal">i</mi><mi mathvariant="normal">r</mi></mrow><mo stretchy="false">)</mo><mi mathvariant="normal">/</mi><mo stretchy="false">(</mo><mrow><mi mathvariant="normal">f</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">q</mi><mi mathvariant="normal">_</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">f</mi><mi mathvariant="normal">_</mi><mi mathvariant="normal">f</mi><mi mathvariant="normal">i</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">s</mi><mi mathvariant="normal">t</mi><mi mathvariant="normal">_</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">l</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">m</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">n</mi><mi mathvariant="normal">t</mi></mrow><mo>×</mo><mrow><mi mathvariant="normal">f</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">q</mi><mi mathvariant="normal">_</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">f</mi><mi mathvariant="normal">_</mi><mi mathvariant="normal">s</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">c</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">n</mi><mi mathvariant="normal">d</mi><mi mathvariant="normal">_</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">l</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">m</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">n</mi><mi mathvariant="normal">t</mi></mrow><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">\mathrm{score} = (\mathrm{freq\_of\_pair}) / (\mathrm{freq\_of\_first\_element} \times \mathrm{freq\_of\_second\_element})</annotation></semantics></math>score=(freq_of_pair)/(freq_of_first_element×freq_of_second_element)

通过将配对的频率除以其每个部分的频率的乘积, 该算法优先合并单个部分在词汇表中频率较低的对。例如,它不一定会合并 `("un", "##able")` 即使这对在词汇表中出现的频率很高,因为 `"un"` 和 `"##able"` 很可能每个词都出现在很多其他词中并且出现频率很高。相比之下,像 `("hu", "##gging")` 可能会更快地合并 (假设 “hugging” 经常出现在词汇表中),因为 `"hu"` 和 `"##gging"` 这两个词单独出现地频率可能较低。

让我们看看我们在 BPE 训练示例中使用的相同词汇:

```py
("hug", 10), ("pug", 5), ("pun", 12), ("bun", 4), ("hugs", 5)
```

这里的拆分将是:

```py
("h" "##u" "##g", 10), ("p" "##u" "##g", 5), ("p" "##u" "##n", 12), ("b" "##u" "##n", 4), ("h" "##u" "##g" "##s", 5)
```

所以最初的词汇将是 `["b", "h", "p", "##g", "##n", "##s", "##u"]` (如果我们暂时忘记特殊标记)。最频繁的一对是 `("##u", "##g")` (目前 20 次),但 `"##u"` 单独出现的频率非常高,所以它的分数不是最高的(它是 1 / 36)。所有带有 `"##u"` 的对实际上都有相同的分数(1 / 36),所以分数最高的对是 `("##g", "##s")` — 唯一没有 `"##u"` 的对— 1 / 20,所以学习的第一个合并是 `("##g", "##s") -> ("##gs")`。

请注意,当我们合并时,我们删除了两个标记之间的 `##`,所以我们添加 `"##gs"` 到词汇表中,并在语料库的单词中应用该合并:

```py
Vocabulary: ["b", "h", "p", "##g", "##n", "##s", "##u", "##gs"]
Corpus: ("h" "##u" "##g", 10), ("p" "##u" "##g", 5), ("p" "##u" "##n", 12), ("b" "##u" "##n", 4), ("h" "##u" "##gs", 5)
```

在这一点中, `"##u"` 是在所有可能的对中,因此它们最终都具有相同的分数。假设在这种情况下,第一对被合并, `("h", "##u") -> "hu"`。这使得我们:

```py
Vocabulary: ["b", "h", "p", "##g", "##n", "##s", "##u", "##gs", "hu"]
Corpus: ("hu" "##g", 10), ("p" "##u" "##g", 5), ("p" "##u" "##n", 12), ("b" "##u" "##n", 4), ("hu" "##gs", 5)
```

然后下一个最高的分数由 `("hu", "##g")` 和 `("hu", "##gs")` 共享(1/15,与其他所有对的 1/21 相比),因此合并得分最高的第一对:

```py
Vocabulary: ["b", "h", "p", "##g", "##n", "##s", "##u", "##gs", "hu", "hug"]
Corpus: ("hug", 10), ("p" "##u" "##g", 5), ("p" "##u" "##n", 12), ("b" "##u" "##n", 4), ("hu" "##gs", 5)
```

我们继续这样处理,直到达到我们所需的词汇量。

✏️ **现在轮到你了!** 下一个合并规则是什么？

## 标记化算法

WordPiece 和 BPE 中的标记化的不同在于 WordPiece 只保存最终词汇,而不是学习的合并规则。从要标记的单词开始,WordPiece 找到词汇表中最长的子词,然后对其进行拆分。例如,如果我们使用上面例子中学到的词汇,对于单词 `"hugs"`,词汇表中从头开始的最长子词是 `"hug"`,所以我们在那里拆分并得到 `["hug", "##s"]`。 然后我们继续使用词汇表中的 `"##s"`,因此 `"hugs"` 的标记化是 `["hug", "##s"]`.

使用 BPE, 我们将按顺序应用学习到的合并并将其标记为 `["hu", "##gs"]`,所以编码不同。

再举一个例子,让我们看看 `"bugs"` 将如何被标记化。 `"b"` 是从词汇表中单词开头开始的最长子词,所以我们在那里拆分并得到 `["b", "##ugs"]`。然后 `"##u"` 是词汇表中从 `"##ugs"` 开始的最长的子词,所以我们在那里拆分并得到 `["b", "##u, "##gs"]`。最后, `"##gs"` 在词汇表中,所以最后一个列表是 `"bugs"` 的标记化。

当分词达到无法在词汇表中找到子词的阶段时, 整个词被标记为未知 — 例如, `"mug"` 将被标记为 `["[UNK]"]`,就像 `"bum"` (即使我们可以以 `"b"` 和 `"##u"` 开始, `"##m"` 不在词汇表中,由此产生的标记将只是 `["[UNK]"]`, 不是 `["b", "##u", "[UNK]"]`)。这是与 BPE 的另一个区别,BPE 只会将不在词汇表中的单个字符分类为未知。

✏️ **现在轮到你了!** `"pugs"` 将被如何标记?

## 实现 WordPiece

现在让我们看一下 WordPiece 算法的实现。与 BPE 一样,这只是教学,你将无法在大型语料库中使用它。

我们将使用与 BPE 示例中相同的语料库:

```py
corpus = [
    "This is the Hugging Face Course.",
    "This chapter is about tokenization.",
    "This section shows several tokenizer algorithms.",
    "Hopefully, you will be able to understand how they are trained and generate tokens.",
]
```

首先,我们需要将语料库预先标记为单词。由于我们正在复制 WordPiece 标记器 (如 BERT),因此我们将使用 `bert-base-cased` 标记器用于预标记化:

```py
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")
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

word_freqs
```

```py
defaultdict(
    int, {'This': 3, 'is': 2, 'the': 1, 'Hugging': 1, 'Face': 1, 'Course': 1, '.': 4, 'chapter': 1, 'about': 1,
    'tokenization': 1, 'section': 1, 'shows': 1, 'several': 1, 'tokenizer': 1, 'algorithms': 1, 'Hopefully': 1,
    ',': 1, 'you': 1, 'will': 1, 'be': 1, 'able': 1, 'to': 1, 'understand': 1, 'how': 1, 'they': 1, 'are': 1,
    'trained': 1, 'and': 1, 'generate': 1, 'tokens': 1})
```

正如我们之前看到的,字母表是由单词的所有第一个字母组成的唯一集合,以及出现在前缀为 `##` 的其他字母:

```py
alphabet = []
for word in word_freqs.keys():
    if word[0] not in alphabet:
        alphabet.append(word[0])
    for letter in word[1:]:
        if f"##{letter}" not in alphabet:
            alphabet.append(f"##{letter}")

alphabet.sort()
alphabet

print(alphabet)
```

```py
['##a', '##b', '##c', '##d', '##e', '##f', '##g', '##h', '##i', '##k', '##l', '##m', '##n', '##o', '##p', '##r', '##s',
 '##t', '##u', '##v', '##w', '##y', '##z', ',', '.', 'C', 'F', 'H', 'T', 'a', 'b', 'c', 'g', 'h', 'i', 's', 't', 'u',
 'w', 'y']
```

我们还在该词汇表的开头添加了模型使用的特殊标记。在使用 BERT 的情况下,它是列表 `["[PAD]", "[UNK]", "[CLS]", "[SEP]", "[MASK]"]`:

```py
vocab = ["[PAD]", "[UNK]", "[CLS]", "[SEP]", "[MASK]"] + alphabet.copy()
```

接下来我们需要拆分每个单词, 所有不是第一个字母的字母都以 `##` 为前缀:

```py
splits = {
    word: [c if i == 0 else f"##{c}" for i, c in enumerate(word)]
    for word in word_freqs.keys()
}
```

现在我们已经准备好训练了,让我们编写一个函数来计算每对的分数。我们需要在训练的每个步骤中使用它:

```py
def compute_pair_scores(splits):
    letter_freqs = defaultdict(int)
    pair_freqs = defaultdict(int)
    for word, freq in word_freqs.items():
        split = splits[word]
        if len(split) == 1:
            letter_freqs[split[0]] += freq
            continue
        for i in range(len(split) - 1):
            pair = (split[i], split[i + 1])
            letter_freqs[split[i]] += freq
            pair_freqs[pair] += freq
        letter_freqs[split[-1]] += freq

    scores = {
        pair: freq / (letter_freqs[pair[0]] * letter_freqs[pair[1]])
        for pair, freq in pair_freqs.items()
    }
    return scores
```

让我们来看看这个字典在初始拆分后的一部分:

```py
pair_scores = compute_pair_scores(splits)
for i, key in enumerate(pair_scores.keys()):
    print(f"{key}: {pair_scores[key]}")
    if i >= 5:
        break
```

```py
('T', '##h'): 0.125
('##h', '##i'): 0.03409090909090909
('##i', '##s'): 0.02727272727272727
('i', '##s'): 0.1
('t', '##h'): 0.03571428571428571
('##h', '##e'): 0.011904761904761904
```

现在,找到得分最高的对只需要一个快速循环:

```py
best_pair = ""
max_score = None
for pair, score in pair_scores.items():
    if max_score is None or max_score < score:
        best_pair = pair
        max_score = score

print(best_pair, max_score)
```

```py
('a', '##b') 0.2
```

所以第一个要学习的合并是 `('a', '##b') -> 'ab'`, 并且我们添加 `'ab'` 到词汇表中:

```py
vocab.append("ab")
```

要继续接下来的步骤,我们需要在我们的 `拆分` 字典中应用该合并。让我们为此编写另一个函数:

```py
def merge_pair(a, b, splits):
    for word in word_freqs:
        split = splits[word]
        if len(split) == 1:
            continue
        i = 0
        while i < len(split) - 1:
            if split[i] == a and split[i + 1] == b:
                merge = a + b[2:] if b.startswith("##") else a + b
                split = split[:i] + [merge] + split[i + 2 :]
            else:
                i += 1
        splits[word] = split
    return splits
```

我们可以看看第一次合并的结果:

```py
splits = merge_pair("a", "##b", splits)
splits["about"]
```

```py
['ab', '##o', '##u', '##t']
```

现在我们有了循环所需的一切,直到我们学会了我们想要的所有合并。我们的目标词汇量为 70:

```py
vocab_size = 70
while len(vocab) < vocab_size:
    scores = compute_pair_scores(splits)
    best_pair, max_score = "", None
    for pair, score in scores.items():
        if max_score is None or max_score < score:
            best_pair = pair
            max_score = score
    splits = merge_pair(*best_pair, splits)
    new_token = (
        best_pair[0] + best_pair[1][2:]
        if best_pair[1].startswith("##")
        else best_pair[0] + best_pair[1]
    )
    vocab.append(new_token)
```

然后我们可以查看生成的词汇表:

```py
print(vocab)
```

```py
['[PAD]', '[UNK]', '[CLS]', '[SEP]', '[MASK]', '##a', '##b', '##c', '##d', '##e', '##f', '##g', '##h', '##i', '##k',
 '##l', '##m', '##n', '##o', '##p', '##r', '##s', '##t', '##u', '##v', '##w', '##y', '##z', ',', '.', 'C', 'F', 'H',
 'T', 'a', 'b', 'c', 'g', 'h', 'i', 's', 't', 'u', 'w', 'y', 'ab','##fu', 'Fa', 'Fac', '##ct', '##ful', '##full', '##fully',
 'Th', 'ch', '##hm', 'cha', 'chap', 'chapt', '##thm', 'Hu', 'Hug', 'Hugg', 'sh', 'th', 'is', '##thms', '##za', '##zat',
 '##ut']
```

正如我们所看到的,与 BPE 相比,这个标记器将单词的一部分作为标记学习得更快一些。

💡 在同一语料库上使用 `train_new_from_iterator()` 不会产生完全相同的词汇表。这是因为 🤗 Tokenizers 库没有为训练实现 WordPiece(因为我们不完全确定它的内部结构),而是使用 BPE。

为了对新文本进行分词,我们对其进行预分词、拆分,然后对每个单词应用分词算法。也就是说,我们从第一个词的开头寻找最大的子词并将其拆分,然后我们在第二部分重复这个过程,对于该词的其余部分和文本中的以下词,依此类推:

```py
def encode_word(word):
    tokens = []
    while len(word) > 0:
        i = len(word)
        while i > 0 and word[:i] not in vocab:
            i -= 1
        if i == 0:
            return ["[UNK]"]
        tokens.append(word[:i])
        word = word[i:]
        if len(word) > 0:
            word = f"##{word}"
    return tokens
```

让我们用词汇表中的一个单词和另一个不在词汇表中的单词进行测试:

```py
print(encode_word("Hugging"))
print(encode_word("HOgging"))
```

```py
['Hugg', '##i', '##n', '##g']
['[UNK]']
```

现在,让我们编写一个标记文本的函数:

```py
def tokenize(text):
    pre_tokenize_result = tokenizer._tokenizer.pre_tokenizer.pre_tokenize_str(text)
    pre_tokenized_text = [word for word, offset in pre_tokenize_result]
    encoded_words = [encode_word(word) for word in pre_tokenized_text]
    return sum(encoded_words, [])
```

我们可以在任何文本上尝试:

```py
tokenize("This is the Hugging Face course!")
```

```py
['Th', '##i', '##s', 'is', 'th', '##e', 'Hugg', '##i', '##n', '##g', 'Fac', '##e', 'c', '##o', '##u', '##r', '##s',
 '##e', '[UNK]']
```

这就是 WordPiece 算法的全部内容!现在让我们来看看 Unigram。