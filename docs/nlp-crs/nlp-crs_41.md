# 大数据? 🤗 Datasets 来救援!

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter5/4?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter5/4?fw=pt)

                ![Ask a Question](https://discuss.huggingface.co/t/chapter-5-questions) ![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/zh-CN/chapter5/section4.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/zh-CN/chapter5/section4.ipynb)

如今,不难发现我们经常使用数 GB 的数据集, 特别是如果你打算从头开始预训练像 BERT 或者 GPT-2 这样的转换器。 在这种情况下, *加载* 数据集就是一个挑战。例如, 用于预训练 GPT-2 的 WebText 语料库包含超过 800 万个文档和 40 GB 的文本 — 将其加载到笔记本电脑的 RAM 中可能会让它抓狂！

幸运的是, 🤗 Datasets 旨在克服这些限制。它通过将数据集作为内存映射文件来处理，并通过在语料库中流化条目来摆脱硬盘限制, 从而使你避免内存管理问题。

[`www.youtube-nocookie.com/embed/JwISwTCPPWo`](https://www.youtube-nocookie.com/embed/JwISwTCPPWo)

在本节中, 我们将探索🤗 Datasets 的特性。它有一个称为 [the Pile](https://pile.eleuther.ai)的 825 GB 的语料库。 让我们开始吧!

## 什么是 Pile?

The Pile 是由[EleutherAI](https://www.eleuther.ai)创建的一个英语文本语料库, 用于训练大规模语言模型。它包含各种各样的数据集, 涵盖科学文章, GitHub 代码库以及过滤的 Web 文本。训练语料库在[14 GB chunks](https://the-eye.eu/public/AI/pile/), 并且你也可以下载几个[单独的组件](https://the-eye.eu/public/AI/pile_preliminary_components/)。 让我们先来看看 PubMed Abstracts 数据集, 它是[PubMed](https://pubmed.ncbi.nlm.nih.gov/)上的 1500 万篇生物医学出版物的摘要的语料库。 数据集采用[JSON 行格式](https://jsonlines.org) 并使用`zstandard`库进行压缩, 所以我们首先需要先安装`zstandard`库:

```py
!pip install zstandard
```

接下来, 我们可以使用第二节中所学的加载远程数据集的方法加载数据集:

```py
from datasets import load_dataset

# This takes a few minutes to run, so go grab a tea or coffee while you wait :)
data_files = "https://the-eye.eu/public/AI/pile_preliminary_components/PUBMED_title_abstracts_2019_baseline.jsonl.zst"
pubmed_dataset = load_dataset("json", data_files=data_files, split="train")
pubmed_dataset
```

```py
Dataset({
    features: ['meta', 'text'],
    num_rows: 15518009
})
```

我们可以看到我们的数据集中有 15,518,009 行和 2 列 — 这是非常多的!

✎ 默认情况下, 🤗 Datasets 会自动解压加载数据集所需的文件。 如果你想保留硬盘空间, 你可以传递 `DownloadConfig(delete_extracted=True)` 到 `download_config` 的 `load_dataset()`参数. 有关更多详细信息, 请参阅文档]([`huggingface.co/docs/datasets/package_reference/builder_classes#datasets.DownloadConfig)。`](https://huggingface.co/docs/datasets/package_reference/builder_classes#datasets.DownloadConfig)%E3%80%82)

让我们看看数据集的第一个元素的内容:

```py
pubmed_dataset[0]
```

```py
{'meta': {'pmid': 11409574, 'language': 'eng'},
 'text': 'Epidemiology of hypoxaemia in children with acute lower respiratory infection.\nTo determine the prevalence of hypoxaemia in children aged under 5 years suffering acute lower respiratory infections (ALRI), the risk factors for hypoxaemia in children under 5 years of age with ALRI, and the association of hypoxaemia with an increased risk of dying in children of the same age ...'}
```

可以看到, 这看起来像是医学文章的摘要。 现在让我们看看我们使用了 RAM 的多少存储空间来加载数据集!

## 内存映射的魔力

在 Python 中测量内存使用情况的一个简单的方法是使用[`psutil`](https://psutil.readthedocs.io/en/latest/)库,它可以使用 `pip`安装, 如下所示:

```py
!pip install psutil
```

它提供了一个 `Process` 类,这个类允许我们检查当前进程的内存使用情况, 如下所示:

```py
import psutil

# Process.memory_info is expressed in bytes, so convert to megabytes
print(f"RAM used: {psutil.Process().memory_info().rss / (1024 * 1024):.2f} MB")
```

```py
RAM used: 5678.33 MB
```

这里的`rss`属性是指 *常驻集* 的大小, 它是进程在 RAM 中占用的内存比例。 这个测量结果也包括了 Python 编译器和我们加载的库所使用的内存, 所以实际上用于加载数据集的内存会更小一些。为了比较, 让我们使用 `dataset_size` 属性看看数据集在磁盘上有多大。 由于结果像之前一样用字节表示, 我们需要手动将其转换为 GB:

```py
print(f"Number of files in dataset : {pubmed_dataset.dataset_size}")
size_gb = pubmed_dataset.dataset_size / (1024**3)
print(f"Dataset size (cache file) : {size_gb:.2f} GB")
```

```py
Number of files in dataset : 20979437051
Dataset size (cache file) : 19.54 GB
```

非常棒 — 尽管它将近 20GB, 但我们能够占用很少的 RAM 空间加载和访问数据集!

✏️ **试试看!** 从[subsets](https://the-eye.eu/public/AI/pile_preliminary_components/)中选择一个大于你的笔记本或者台式机的 RAM 大小的子集, 用 🤗 Datasets 加载这个数据集, 并且测量 RAM 的使用量。 请注意, 要获得准确的测量结果, 你需要在另一个进程中执行这个操作。你可以在 [the Pile paper](https://arxiv.org/abs/2101.00027)的表一中找到每个子集解压后的大小。

如果你熟悉 Pandas, 这个结果可能会让人感到很意外。因为 Wes Kinney 的著名的[经验法则](https://wesmckinney.com/blog/apache-arrow-pandas-internals/) 是你需要的 RAM 应该是数据集的大小的 5 倍到 10 倍。 那么 🤗 Datasets 是如何解决这个内存管理问题的呢? 🤗 Datasets 将每一个数据集看作一个[内存映射文件](https://en.wikipedia.org/wiki/Memory-mapped_file), 它提供了 RAM 和文件系统存储之间的映射, 该映射允许库访问和操作数据集的元素, 而且无需将其完全加载到内存中。

内存映射文件也一个在多个进程之间共享, 这使得像 `Dataset.map()`之类的方法可以并行化, 并且无需移动或者赋值数据集。在底层, 这些功能都是由[Apache Arrow](https://arrow.apache.org)内存格式和[`pyarrow`](https://arrow.apache.org/docs/python/index.html)库提供的支持, 使得数据加载和处理速度快如闪电。 (更多有关 Apache Arrow 的详细信息以及与 Pandas 的比较, 请查看[Dejan Simic’s blog post](https://towardsdatascience.com/apache-arrow-read-dataframe-with-zero-memory-69634092b1a).) 为了更清晰地看到这个过程, 让我们通过迭代 PubMed Abstracts 数据集中的所有元素来运行一个速度测试小程序:

```py
import timeit

code_snippet = """batch_size = 1000

for idx in range(0, len(pubmed_dataset), batch_size):
    _ = pubmed_dataset[idx:idx + batch_size]
"""

time = timeit.timeit(stmt=code_snippet, number=1, globals=globals())
print(
    f"Iterated over {len(pubmed_dataset)} examples (about {size_gb:.1f} GB) in "
    f"{time:.1f}s, i.e. {size_gb/time:.3f} GB/s"
)
```

```py
'Iterated over 15518009 examples (about 19.5 GB) in 64.2s, i.e. 0.304 GB/s'
```

这里我们使用了 Python 的 `timeit` 模块来测量执行 `code_snippet`所耗的时间。 你通常能以十分之几 GB/s 到几 GB/s 的速度迭代数据集。通过上述的方法就已经能够解决大多数大数据集加载的限制, 但是有时候你不得不使用一个很大的数据集, 它甚至都不能存储在笔记本电脑的硬盘上。例如, 如果我们尝试下载整个 Pile, 我们需要 825GB 的可用磁盘空间! 为了处理这种情况, 🤗 Datasets 提供了一个流式功能, 这个功能允许我们动态下载和访问元素, 并且不需要下载整个数据集。让我们来看看这个功能是如何工作的。

💡在 Jupyter 笔记中你还可以使用[`%%timeit` magic function](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit)为单元格计时。

## 流式数据集

要使用数据集流, 你只需要将 `streaming=True` 参数传递给 `load_dataset()` 函数。接下来, 让我们再次加载 PubMed Abstracts 数据集, 但是采用流模式:

```py
pubmed_dataset_streamed = load_dataset(
    "json", data_files=data_files, split="train", streaming=True
)
```

与我们在本章其他地方遇到的熟悉的 `Dataset` 不同, `streaming=True` 返回的对象是一个 `IterableDataset`。 顾名思义, 要访问 `IterableDataset` , 我们需要迭代它。我们可以按照如下方式访问流式数据集的第一个元素:

```py
next(iter(pubmed_dataset_streamed))
```

```py
{'meta': {'pmid': 11409574, 'language': 'eng'},
 'text': 'Epidemiology of hypoxaemia in children with acute lower respiratory infection.\nTo determine the prevalence of hypoxaemia in children aged under 5 years suffering acute lower respiratory infections (ALRI), the risk factors for hypoxaemia in children under 5 years of age with ALRI, and the association of hypoxaemia with an increased risk of dying in children of the same age ...'}
```

如果您需要在训练期间标记流式数据集中的元素可以使用 `IterableDataset.map()`进行动态处理。该过程与我们在第三章中标记数据集的过程完全相同, 唯一的区别是输出是逐个返回的:

```py
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased")
tokenized_dataset = pubmed_dataset_streamed.map(lambda x: tokenizer(x["text"]))
next(iter(tokenized_dataset))
```

```py
{'input_ids': [101, 4958, 5178, 4328, 6779, ...], 'attention_mask': [1, 1, 1, 1, 1, ...]}
```

💡 你可以传递 `batched=True` 来通过流式加速标记化, 如同我们在上一节看到的那样。它将逐批处理示例; 默认的批量大小为 1,000, 可以使用 `batch_size` 参数指定批量大小。

你还可以使用 `IterableDataset.shuffle()` 打乱流式数据集, 但与 `Dataset.shuffle()` 不同的是这只会打乱预定义 `buffer_size` 中的元素:

```py
shuffled_dataset = pubmed_dataset_streamed.shuffle(buffer_size=10_000, seed=42)
next(iter(shuffled_dataset))
```

```py
{'meta': {'pmid': 11410799, 'language': 'eng'},
 'text': 'Randomized study of dose or schedule modification of granulocyte colony-stimulating factor in platinum-based chemotherapy for elderly patients with lung cancer ...'}
```

在这个示例中, 我们从缓冲区的前 10,000 个示例中随机选择了一个示例。一旦访问了一个示例, 它在缓冲区中的位置就会被语料库中的下一个示例填充 (即, 上述案例中的第 10,001 个示例)。你还可以使用 `IterableDataset.take()` 和 `IterableDataset.skip()` 函数从流式数据集中选择元素, 它的作用类似于 `Dataset.select()`。例如, 要选择 PubMed Abstracts 数据集的前 5 个示例, 我们可以执行以下操作:

```py
dataset_head = pubmed_dataset_streamed.take(5)
list(dataset_head)
```

```py
[{'meta': {'pmid': 11409574, 'language': 'eng'},
  'text': 'Epidemiology of hypoxaemia in children with acute lower respiratory infection ...'},
 {'meta': {'pmid': 11409575, 'language': 'eng'},
  'text': 'Clinical signs of hypoxaemia in children with acute lower respiratory infection: indicators of oxygen therapy ...'},
 {'meta': {'pmid': 11409576, 'language': 'eng'},
  'text': "Hypoxaemia in children with severe pneumonia in Papua New Guinea ..."},
 {'meta': {'pmid': 11409577, 'language': 'eng'},
  'text': 'Oxygen concentrators and cylinders ...'},
 {'meta': {'pmid': 11409578, 'language': 'eng'},
  'text': 'Oxygen supply in rural africa: a personal experience ...'}]
```

同样, 你可以使用 `IterableDataset.skip()` 函数将打乱的数据集拆分为训练集和验证集, 如下所示:

```py
# Skip the first 1,000 examples and include the rest in the training set
train_dataset = shuffled_dataset.skip(1000)
# Take the first 1,000 examples for the validation set
validation_dataset = shuffled_dataset.take(1000)
```

让我们用一个常见的任务来进行我们对数据集流的最后探索: 将多个数据集组合在一起创建一个心得语料库。 🤗 Datasets 提供了一个 `interleave_datasets()` 函数, 它将一个 `IterableDataset` 对象列表组合为单个的 `IterableDataset`, 其中新数据集的元素是通过在列表中的对象交替获得的。当你试图组合大型数据集时, 这个函数特别有用, 让我们通过下面这个例子来试着组合 Pile 的自由法律数据集,它是来自美国法院的 51 GB 的法律意见数据集:

```py
law_dataset_streamed = load_dataset(
    "json",
    data_files="https://the-eye.eu/public/AI/pile_preliminary_components/FreeLaw_Opinions.jsonl.zst",
    split="train",
    streaming=True,
)
next(iter(law_dataset_streamed))
```

```py
{'meta': {'case_ID': '110921.json',
  'case_jurisdiction': 'scotus.tar.gz',
  'date_created': '2010-04-28T17:12:49Z'},
 'text': '\n461 U.S. 238 (1983)\nOLIM ET AL.\nv.\nWAKINEKONA\nNo. 81-1581.\nSupreme Court of United States.\nArgued January 19, 1983.\nDecided April 26, 1983.\nCERTIORARI TO THE UNITED STATES COURT OF APPEALS FOR THE NINTH CIRCUIT\n*239 Michael A. Lilly, First Deputy Attorney General of Hawaii, argued the cause for petitioners. With him on the brief was James H. Dannenberg, Deputy Attorney General...'}
```

这个数据集足够大, 可以对大多数笔记本电脑的 RAM 有足够的压力, 但是我们已经能够毫不费力地加载和访问它! 现在我们使用 `interleave_datasets()` 函数加载来自 FreeLaw 和 PubMed Abstracts 的数据集:

```py
from itertools import islice
from datasets import interleave_datasets

combined_dataset = interleave_datasets([pubmed_dataset_streamed, law_dataset_streamed])
list(islice(combined_dataset, 2))
```

```py
[{'meta': {'pmid': 11409574, 'language': 'eng'},
  'text': 'Epidemiology of hypoxaemia in children with acute lower respiratory infection ...'},
 {'meta': {'case_ID': '110921.json',
   'case_jurisdiction': 'scotus.tar.gz',
   'date_created': '2010-04-28T17:12:49Z'},
  'text': '\n461 U.S. 238 (1983)\nOLIM ET AL.\nv.\nWAKINEKONA\nNo. 81-1581.\nSupreme Court of United States.\nArgued January 19, 1983.\nDecided April 26, 1983.\nCERTIORARI TO THE UNITED STATES COURT OF APPEALS FOR THE NINTH CIRCUIT\n*239 Michael A. Lilly, First Deputy Attorney General of Hawaii, argued the cause for petitioners. With him on the brief was James H. Dannenberg, Deputy Attorney General...'}]
```

这里我们使用了来自 Python 的 `itertools` 模块的 `islice()` 函数从合并的数据集中选择前两个示例, 并且我们可以看到它们实际上就是两个源数据集中的前两个示例拼在一起形成的:

最后, 如果你想流式传输整个 825GB 的 Pile, 你可以按照如下方式获取所有准备好的文件:

```py
base_url = "https://the-eye.eu/public/AI/pile/"
data_files = {
    "train": [base_url + "train/" + f"{idx:02d}.jsonl.zst" for idx in range(30)],
    "validation": base_url + "val.jsonl.zst",
    "test": base_url + "test.jsonl.zst",
}
pile_dataset = load_dataset("json", data_files=data_files, streaming=True)
next(iter(pile_dataset["train"]))
```

```py
{'meta': {'pile_set_name': 'Pile-CC'},
 'text': 'It is done, and submitted. You can play “Survival of the Tastiest” on Android, and on the web...'}
```

✏️ **试试看!** 使用像[`mc4`](https://huggingface.co/datasets/mc4) 或者 [`oscar`](https://huggingface.co/datasets/oscar)这样的大型 Common Crawl 语料库来创建一个流式多语言数据集, 该数据集代表你选择的国家/地区语言的口语比例。例如, 瑞士的四种民族语言分别是德语、法语、意大利语和罗曼什语, 因此你可以尝试根据根据口语比例对 Oscar 子集进行采用来创建瑞士语料库。

你现在拥有加载和处理各种类型和大小的数据集的所需的所有工具 — 但是除非你非常幸运, 否则在你的 NLP 之旅中会有一个难题, 你将不得不创建一个数据集来解决手头的问题。这就是下一节的主题!