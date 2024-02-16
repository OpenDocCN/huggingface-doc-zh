# 提取文本摘要

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter7/5?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter7/5?fw=pt)

                Pytorch TensorFlow![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/chapter7/section5_pt.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/chapter7/section5_pt.ipynb)

在本节中，我们将看看如何使用 Transformer 模型将长文档压缩为摘要，这项任务称为文本摘要.这是最具挑战性的 NLP 任务之一，因为它需要一系列能力，例如理解长篇文章和生成能够捕捉文档中主要主题的连贯文本。但是，如果做得好，文本摘要是一种强大的工具，可以减轻领域专家详细阅读长文档的负担，从而加快各种业务流程。

[`www.youtube-nocookie.com/embed/yHnr5Dk2zCI`](https://www.youtube-nocookie.com/embed/yHnr5Dk2zCI)

尽管在[Hugging Face Hub](https://huggingface.co/models?pipeline_tag=summarization=downloads)上已经存在各种微调模型用于文本摘要，几乎所有这些都只适用于英文文档。因此，为了在本节中添加一些变化，我们将为英语和西班牙语训练一个双语模型。在本节结束时，您将有一个可以总结客户评论的[模型](https://huggingface.co/huggingface-course/mt5-small-finetuned-amazon-en-es)。

[`course-demos-mt5-small-finetuned-amazon-en-es.hf.space`](https://course-demos-mt5-small-finetuned-amazon-en-es.hf.space)

如下所示：正如我们将看到的，这些摘要很简洁，因为它们是从客户在产品评论中提供的标题中学到的。让我们首先为这项任务准备一个合适的双语语料库。

## 准备多语言语料库

我们将使用[多语言亚马逊评论语料库](https://huggingface.co/datasets/amazon_reviews_multi)创建我们的双语摘要器。该语料库由六种语言的亚马逊产品评论组成，通常用于对多语言分类器进行基准测试。然而，由于每条评论都附有一个简短的标题，我们可以使用标题作为我们模型学习的目标摘要！首先，让我们从 Hugging Face Hub 下载英语和西班牙语子集：

```py
from datasets import load_dataset

spanish_dataset = load_dataset("amazon_reviews_multi", "es")
english_dataset = load_dataset("amazon_reviews_multi", "en")
english_dataset
```

```py
DatasetDict({
    train: Dataset({
        features: ['review_id', 'product_id', 'reviewer_id', 'stars', 'review_body', 'review_title', 'language', 'product_category'],
        num_rows: 200000
    })
    validation: Dataset({
        features: ['review_id', 'product_id', 'reviewer_id', 'stars', 'review_body', 'review_title', 'language', 'product_category'],
        num_rows: 5000
    })
    test: Dataset({
        features: ['review_id', 'product_id', 'reviewer_id', 'stars', 'review_body', 'review_title', 'language', 'product_category'],
        num_rows: 5000
    })
})
```

如您所见，对于每种语言，都有 200,000 条评论 **train** 拆分，每个评论有 5,000 条评论 **validation** 和 **test** 分裂。我们感兴趣的评论信息包含在 **review_body** 和 **review_title** 列。让我们通过创建一个简单的函数来查看一些示例，该函数使用我们在第五章学到过：

```py
def show_samples(dataset, num_samples=3, seed=42):
    sample = dataset["train"].shuffle(seed=seed).select(range(num_samples))
    for example in sample:
        print(f"\n'>> Title: {example['review_title']}'")
        print(f"'>> Review: {example['review_body']}'")

show_samples(english_dataset)
```

```py
'>> Title: Worked in front position, not rear'
'>> Review: 3 stars because these are not rear brakes as stated in the item description. At least the mount adapter only worked on the front fork of the bike that I got it for.'

'>> Title: meh'
'>> Review: Does it’s job and it’s gorgeous but mine is falling apart, I had to basically put it together again with hot glue'

'>> Title: Can\'t beat these for the money'
'>> Review: Bought this for handling miscellaneous aircraft parts and hanger "stuff" that I needed to organize; it really fit the bill. The unit arrived quickly, was well packaged and arrived intact (always a good sign). There are five wall mounts-- three on the top and two on the bottom. I wanted to mount it on the wall, so all I had to do was to remove the top two layers of plastic drawers, as well as the bottom corner drawers, place it when I wanted and mark it; I then used some of the new plastic screw in wall anchors (the 50 pound variety) and it easily mounted to the wall. Some have remarked that they wanted dividers for the drawers, and that they made those. Good idea. My application was that I needed something that I can see the contents at about eye level, so I wanted the fuller-sized drawers. I also like that these are the new plastic that doesn\'t get brittle and split like my older plastic drawers did. I like the all-plastic construction. It\'s heavy duty enough to hold metal parts, but being made of plastic it\'s not as heavy as a metal frame, so you can easily mount it to the wall and still load it up with heavy stuff, or light stuff. No problem there. For the money, you can\'t beat it. Best one of these I\'ve bought to date-- and I\'ve been using some version of these for over forty years.'
```

✏️ **试试看！** 更改 `Dataset.shuffle()` 命令中的随机种子以探索语料库中的其他评论。 如果您是说西班牙语的人，请查看 `spanish_dataset` 中的一些评论，看看标题是否也像合理的摘要。

此示例显示了人们通常在网上找到的评论的多样性，从正面到负面（以及介于两者之间的所有内容！）。尽管标题为“meh”的示例信息量不大，但其他标题看起来像是对评论本身的体面总结。在单个 GPU 上训练所有 400,000 条评论的摘要模型将花费太长时间，因此我们将专注于为单个产品领域生成摘要。为了了解我们可以选择哪些域，让我们将 **english_dataset** 转换到 **pandas.DataFrame** 并计算每个产品类别的评论数量：

```py
english_dataset.set_format("pandas")
english_df = english_dataset["train"][:]
# Show counts for top 20 products
english_df["product_category"].value_counts()[:20]
```

```py
home                      17679
apparel                   15951
wireless                  15717
other                     13418
beauty                    12091
drugstore                 11730
kitchen                   10382
toy                        8745
sports                     8277
automotive                 7506
lawn_and_garden            7327
home_improvement           7136
pet_products               7082
digital_ebook_purchase     6749
pc                         6401
electronics                6186
office_product             5521
shoes                      5197
grocery                    4730
book                       3756
Name: product_category, dtype: int64
```

英语数据集中最受欢迎的产品是家居用品、服装和无线电子产品。不过，为了坚持亚马逊的主题，让我们专注于总结书籍的评论——毕竟，这是亚马逊这家公司成立的基础！我们可以看到两个符合要求的产品类别（ **book** 和 **digital_ebook_purchase** )，所以让我们为这些产品过滤两种语言的数据集。正如我们在第五章学到的， 这 **Dataset.filter()** 函数允许我们非常有效地对数据集进行切片，因此我们可以定义一个简单的函数来执行此操作：

```py
def filter_books(example):
    return (
        example["product_category"] == "book"
        or example["product_category"] == "digital_ebook_purchase"
    )
```

现在，当我们将此函数应用于 **english_dataset** 和 **spanish_dataset** ，结果将只包含涉及书籍类别的那些行。在应用过滤器之前，让我们将**english_dataset**的格式从 **pandas** 切换回到 **arrow** ：

```py
english_dataset.reset_format()
```

然后我们可以应用过滤器功能，作为健全性检查，让我们检查评论样本，看看它们是否确实与书籍有关：

```py
spanish_books = spanish_dataset.filter(filter_books)
english_books = english_dataset.filter(filter_books)
show_samples(english_books)
```

```py
'>> Title: I\'m dissapointed.'
'>> Review: I guess I had higher expectations for this book from the reviews. I really thought I\'d at least like it. The plot idea was great. I loved Ash but, it just didnt go anywhere. Most of the book was about their radio show and talking to callers. I wanted the author to dig deeper so we could really get to know the characters. All we know about Grace is that she is attractive looking, Latino and is kind of a brat. I\'m dissapointed.'

'>> Title: Good art, good price, poor design'
'>> Review: I had gotten the DC Vintage calendar the past two years, but it was on backorder forever this year and I saw they had shrunk the dimensions for no good reason. This one has good art choices but the design has the fold going through the picture, so it\'s less aesthetically pleasing, especially if you want to keep a picture to hang. For the price, a good calendar'

'>> Title: Helpful'
'>> Review: Nearly all the tips useful and. I consider myself an intermediate to advanced user of OneNote. I would highly recommend.'
```

好的，我们可以看到评论并不是严格意义上的书籍，可能是指日历和 OneNote 等电子应用程序等内容。尽管如此，该领域似乎适合训练摘要模型。在我们查看适合此任务的各种模型之前，我们还有最后一点数据准备要做：将英语和西班牙语评论合并为一个 **DatasetDict** 目的。 🤗 Datasets 提供了一个方便的 **concatenate_datasets()** 函数（顾名思义）合并 **Dataset** 对象。因此，为了创建我们的双语数据集，我们将遍历每个拆分，连接该拆分的数据集，并打乱结果以确保我们的模型不会过度拟合单一语言：

```py
from datasets import concatenate_datasets, DatasetDict

books_dataset = DatasetDict()

for split in english_books.keys():
    books_dataset[split] = concatenate_datasets(
        [english_books[split], spanish_books[split]]
    )
    books_dataset[split] = books_dataset[split].shuffle(seed=42)

# Peek at a few examples
show_samples(books_dataset)
```

```py
'>> Title: Easy to follow!!!!'
'>> Review: I loved The dash diet weight loss Solution. Never hungry. I would recommend this diet. Also the menus are well rounded. Try it. Has lots of the information need thanks.'

'>> Title: PARCIALMENTE DAÑADO'
'>> Review: Me llegó el día que tocaba, junto a otros libros que pedí, pero la caja llegó en mal estado lo cual dañó las esquinas de los libros porque venían sin protección (forro).'

'>> Title: no lo he podido descargar'
'>> Review: igual que el anterior'
```

这当然看起来像是英语和西班牙语评论的混合！现在我们有了一个训练语料库，最后要检查的一件事是评论中单词的分布及其标题。这对于摘要任务尤其重要，其中数据中的简短参考摘要会使模型偏向于仅在生成的摘要中输出一两个单词。下面的图显示了单词分布，我们可以看到有些标题严重偏向于 1-2 个单词：

![Word count distributions for the review titles and texts.](img/4c336a3ddf7ed03d13fd26950df52cf9.png) ![Word count distributions for the review titles and texts.](img/5ad8673270e8c77f177b224347921ac7.png)

为了解决这个问题，我们将过滤掉标题非常短的示例，以便我们的模型可以生成更有趣的摘要。由于我们正在处理英文和西班牙文文本，因此我们可以使用粗略的启发式方法在空白处拆分标题，然后使用我们可信赖的 **Dataset.filter()** 方法如下：

```py
books_dataset = books_dataset.filter(lambda x: len(x["review_title"].split()) > 2)
```

现在我们已经准备好了我们的语料库，让我们来看看一些可以对其进行微调的可能的 Transformer 模型！

## 文本摘要模型

如果你仔细想想，文本摘要是一种类似于机器翻译的任务：我们有一个像评论这样的文本正文，我们希望将其“翻译”成一个较短的版本，以捕捉输入的显着特征。因此，大多数用于文本摘要的 Transformer 模型采用了我们在第一章遇到的编码器-解码器架构。尽管有一些例外，例如 GPT 系列模型，它们在 few-shot（少量微调）之后也可以提取摘要。下表列出了一些流行的预训练模型，可以对其进行微调以进行汇总。

| Transformer 模型 | 描述 | 多种语言? |
| :-: | --- | :-: |
| [GPT-2](https://huggingface.co/gpt2-xl) | 虽然训练为自回归语言模型，但您可以通过在输入文本末尾附加“TL;DR”来使 GPT-2 生成摘要。 | ❌ |
| [PEGASUS](https://huggingface.co/google/pegasus-large) | 在预训练是的目标是来预测多句子文本中的屏蔽句子。 这个预训练目标比普通语言建模更接近文本摘要，并且在流行的基准测试中得分很高。 | ❌ |
| [T5](https://huggingface.co/t5-base) | 通用的 Transformer 架构，在文本到文本的框架中制定所有任务； 例如，模型文本摘要的输入格式是`summarize: ARTICLE`。 | ❌ |
| [mT5](https://huggingface.co/google/mt5-base) | T5 的多语言版本，在多语言 Common Crawl 语料库 (mC4) 上进行预训练，涵盖 101 种语言。 | ✅ |
| [BART](https://huggingface.co/facebook/bart-base) | 一种新颖的 Transformer 架构，其中包含经过训练的编码器和解码器堆栈，以重建被破坏的输入，结合了 BERT 和 GPT-2 的预训练方案。 | ❌ |
| [mBART-50](https://huggingface.co/facebook/mbart-large-50) | BART 的多语言版本，预训练了 50 种语言。 | ✅ |

从此表中可以看出，大多数用于摘要的 Transformer 模型（以及大多数 NLP 任务）都是单语的。如果您的任务是使用“有大量语料库”的语言（如英语或德语），这很好，但对于世界各地正在使用的数千种其他语言，则不然。幸运的是，有一类多语言 Transformer 模型，如 mT5 和 mBART，可以解决问题。这些模型是使用语言建模进行预训练的，但有一点不同：它们不是在一种语言的语料库上训练，而是同时在 50 多种语言的文本上进行联合训练！

我们将使用 mT5，这是一种基于 T5 的有趣架构，在文本到文本框架中进行了预训练。在 T5 中，每个 NLP 任务都是根据提示前缀来制定的，例如 **summarize:** 这使模型使生成的文本适应提示。如下图所示，这让 T5 变得非常通用，因为你可以用一个模型解决很多任务！

![Different tasks performed by the T5 architecture.](img/19e5917b8990000666f27dc3b84ed471.png) ![Different tasks performed by the T5 architecture.](img/d01eef5637189e725a5554c44db13f7e.png)

mT5 不使用前缀，但具有 T5 的大部分功能，并且具有多语言的优势。现在我们已经选择了一个模型，让我们来看看准备我们的训练数据。

✏️ **试试看！** 完成本节后，通过使用相同的技术对 mBART 进行微调，看看 mT5 与 mBART 相比有多好。 对于奖励积分，您还可以尝试仅在英文评论上微调 T5。 由于 T5 需要一个特殊的前缀提示，因此您需要在下面的预处理步骤中将“summarize:”添加到输入示例中。

## 预处理数据

[`www.youtube-nocookie.com/embed/1m7BerpSq8A`](https://www.youtube-nocookie.com/embed/1m7BerpSq8A)

我们的下一个任务是对我们的评论及其标题进行标记和编码。像往常一样，我们首先加载与预训练模型检查点相关的标记器。我们将使用 **mt5-small** 作为我们的检查点，以便我们可以在合理的时间内微调模型：

```py
from transformers import AutoTokenizer

model_checkpoint = "google/mt5-small"
tokenizer = AutoTokenizer.from_pretrained(model_checkpoint)
```

💡在 NLP 项目的早期阶段，一个好的做法是在小样本数据上训练一类“小”模型。这使您可以更快地调试和迭代端到端工作流。一旦您对结果充满信心，您始终可以通过简单地更改模型检查点来在大规模数据上训练模型！

让我们在一个小例子上测试 mT5 标记器：

```py
inputs = tokenizer("I loved reading the Hunger Games!")
inputs
```

```py
{'input_ids': [336, 259, 28387, 11807, 287, 62893, 295, 12507, 1], 'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1]}
```

在这里我们可以看到我们在第三章第一次微调实验中遇到的熟悉的 **input_ids** 和 **attention_mask** .让我们用分词器解码这些输入 ID ，可以**convert_ids_to_tokens()** 函数来查看我们正在处理什么样的标记器：

```py
tokenizer.convert_ids_to_tokens(inputs.input_ids)
```

```py
['▁I', '▁', 'loved', '▁reading', '▁the', '▁Hung', 'er', '▁Games', '</s>']
```

特殊的 Unicode 字符 `▁` 和序列结束标记 `</s>` 表明我们正在处理 SentencePiece 分词器，它基于在第六章中讨论的 Unigram 分词算法. Unigram 对多语言语料库特别有用，因为它允许 SentencePiece 不知道重音、标点符号以及许多语言（如日语）没有空格字符。

为了标记我们的语料库，我们必须处理与摘要相关的微妙之处：因为我们的标签也是文本，所以它们可能超过模型的最大上下文大小。 这意味着我们需要对评论及其标题进行截断，以确保我们不会将过长的输入传递给我们的模型。 🤗 Transformers 中的分词器提供了一个绝妙的 `text_target` 参数，允许您将标签与输入并行分词。 以下是如何为 mT5 处理输入和目标的示例：

```py
max_input_length = 512
max_target_length = 30

def preprocess_function(examples):
    model_inputs = tokenizer(
        examples["review_body"],
        max_length=max_input_length,
        truncation=True,
    )
    labels = tokenizer(
        examples["review_title"], max_length=max_target_length, truncation=True
    )
    model_inputs["labels"] = labels["input_ids"]
    return model_inputs
```

让我们通过这段代码来了解发生了什么。我们做的第一件事是定义值 **max_input_length** 和 **max_target_length** ，它为我们的评论和标题的长度设置了上限。由于评论正文通常比标题大得多，我们相应地调整了这些值。

有了 `preprocess_function()`，我们在整个课程中广泛使用的方便的 `Dataset.map()` 函数来标记整个语料库是一件简单的事情：

```py
tokenized_datasets = books_dataset.map(preprocess_function, batched=True)
```

既然语料库已经预处理完毕，我们来看看一些常用的摘要指标。正如我们将看到的，在衡量机器生成的文本的质量方面没有灵丹妙药。

💡 你可能已经注意到我们在上面的 `Dataset.map()` 函数中使用了 `batched=True`。 这会以 1,000 个（默认）为单位对示例进行编码，并允许您利用 🤗 Transformers 中快速标记器的多线程功能。 在可能的情况下，尝试使用 `batched=True` 来加速您的预处理！

## 文本摘要的指标

[`www.youtube-nocookie.com/embed/TMshhnrEXlg`](https://www.youtube-nocookie.com/embed/TMshhnrEXlg)

与我们在本课程中涵盖的大多数其他任务相比，衡量文本生成任务（如摘要或翻译）的性能并不那么简单。例如，对于“我喜欢阅读饥饿游戏”这样的评论，有多个有效摘要，例如“我喜欢饥饿游戏”或“饥饿游戏是一本好书”。显然，在生成的摘要和标签之间应用某种精确匹配并不是一个好的解决方案——即使是人类在这样的指标下也会表现不佳，因为我们都有自己的写作风格。

总而言之，最常用的指标之一是[ROUGE 分数](https://en.wikipedia.org/wiki/ROUGE_(metric))（Recall-Oriented Understudy for Gisting Evaluation 的缩写）。该指标背后的基本思想是将生成的摘要与一组通常由人类创建的参考摘要进行比较。为了更精确，假设我们要比较以下两个摘要：

```py
generated_summary = "I absolutely loved reading the Hunger Games"
reference_summary = "I loved reading the Hunger Games"
```

比较它们的一种方法是计算重叠单词的数量，在这种情况下为 6。但是，这有点粗糙，因此 ROUGE 是基于计算计算重叠的 *precision* 和 *recall* 分数。。

🙋 如果这是您第一次听说精确率和召回率，请不要担心——我们将一起通过一些明确的示例来说明一切。 这些指标通常在分类任务中遇到，因此如果您想了解在该上下文中如何定义精确度和召回率，我们建议查看 scikit-learn 指南 /auto_examples/model_selection/plot_precision_recall.html）。

对于 ROUGE，recall 衡量生成的参考摘要包含了多少参考摘要。如果我们只是比较单词，recall 可以根据以下公式计算： <math display="block"><semantics><mrow><mrow><mi mathvariant="normal">R</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">c</mi><mi mathvariant="normal">a</mi><mi mathvariant="normal">l</mi><mi mathvariant="normal">l</mi></mrow><mo>=</mo><mfrac><mrow><mi mathvariant="normal">N</mi><mi mathvariant="normal">u</mi><mi mathvariant="normal">m</mi><mi mathvariant="normal">b</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">f</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">v</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">l</mi><mi mathvariant="normal">a</mi><mi mathvariant="normal">p</mi><mi mathvariant="normal">p</mi><mi mathvariant="normal">i</mi><mi mathvariant="normal">n</mi><mi mathvariant="normal">g</mi><mi mathvariant="normal">w</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">d</mi><mi mathvariant="normal">s</mi></mrow><mrow><mi mathvariant="normal">T</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">t</mi><mi mathvariant="normal">a</mi><mi mathvariant="normal">l</mi><mi mathvariant="normal">n</mi><mi mathvariant="normal">u</mi><mi mathvariant="normal">m</mi><mi mathvariant="normal">b</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">f</mi><mi mathvariant="normal">w</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">d</mi><mi mathvariant="normal">s</mi><mi mathvariant="normal">i</mi><mi mathvariant="normal">n</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">f</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">n</mi><mi mathvariant="normal">c</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">s</mi><mi mathvariant="normal">u</mi><mi mathvariant="normal">m</mi><mi mathvariant="normal">m</mi><mi mathvariant="normal">a</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">y</mi></mrow></mfrac></mrow> <annotation encoding="application/x-tex">\mathrm{Recall} = \frac{\mathrm{Number\,of\,overlapping\, words}}{\mathrm{Total\, number\, of\, words\, in\, reference\, summary}}</annotation></semantics></math> Recall=TotalnumberofwordsinreferencesummaryNumberofoverlappingwords​

对于我们上面的简单例子，这个公式给出了 6/6 = 1 的完美召回率；即，参考摘要中的所有单词都已由模型生成。这听起来可能很棒，但想象一下，如果我们生成的摘要是“我真的很喜欢整晚阅读饥饿游戏”。这也将有完美的 recall，但可以说是一个更糟糕的总结，因为它很冗长。为了处理这些场景，我们还计算了 pecision，它在 ROUGE 上下文中衡量生成的摘要中有多少是相关的： <math display="block"><semantics><mrow><mrow><mi mathvariant="normal">P</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">c</mi><mi mathvariant="normal">i</mi><mi mathvariant="normal">s</mi><mi mathvariant="normal">i</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">n</mi></mrow><mo>=</mo><mfrac><mrow><mi mathvariant="normal">N</mi><mi mathvariant="normal">u</mi><mi mathvariant="normal">m</mi><mi mathvariant="normal">b</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">f</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">v</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">l</mi><mi mathvariant="normal">a</mi><mi mathvariant="normal">p</mi><mi mathvariant="normal">p</mi><mi mathvariant="normal">i</mi><mi mathvariant="normal">n</mi><mi mathvariant="normal">g</mi><mi mathvariant="normal">w</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">d</mi><mi mathvariant="normal">s</mi></mrow><mrow><mi mathvariant="normal">T</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">t</mi><mi mathvariant="normal">a</mi><mi mathvariant="normal">l</mi><mi mathvariant="normal">n</mi><mi mathvariant="normal">u</mi><mi mathvariant="normal">m</mi><mi mathvariant="normal">b</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">f</mi><mi mathvariant="normal">w</mi><mi mathvariant="normal">o</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">d</mi><mi mathvariant="normal">s</mi><mi mathvariant="normal">i</mi><mi mathvariant="normal">n</mi><mi mathvariant="normal">g</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">n</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">a</mi><mi mathvariant="normal">t</mi><mi mathvariant="normal">e</mi><mi mathvariant="normal">d</mi><mi mathvariant="normal">s</mi><mi mathvariant="normal">u</mi><mi mathvariant="normal">m</mi><mi mathvariant="normal">m</mi><mi mathvariant="normal">a</mi><mi mathvariant="normal">r</mi><mi mathvariant="normal">y</mi></mrow></mfrac></mrow> <annotation encoding="application/x-tex">\mathrm{Precision} = \frac{\mathrm{Number\,of\,overlapping\, words}}{\mathrm{Total\, number\, of\, words\, in\, generated\, summary}}</annotation></semantics></math> Precision=TotalnumberofwordsingeneratedsummaryNumberofoverlappingwords​

将此应用到我们的详细摘要中会得到 6/10 = 0.6 的精度，这比我们较短的摘要获得的 6/7 = 0.86 的精度要差得多。在实践中，通常计算精度和召回率，然后报告 F1-score（精度和召回率的调和平均值）。我们可以在 🤗 Datasets 中通过安装 **rouge_score** 包来计算他们：

```py
!pip install rouge_score
```

然后按如下方式加载 ROUGE 指标：

```py
import evaluate

rouge_score = evaluate.load("rouge")
```

然后我们可以使用 **rouge_score.compute()** 一次性计算所有指标的函数：

```py
scores = rouge_score.compute(
    predictions=[generated_summary], references=[reference_summary]
)
scores
```

```py
{'rouge1': AggregateScore(low=Score(precision=0.86, recall=1.0, fmeasure=0.92), mid=Score(precision=0.86, recall=1.0, fmeasure=0.92), high=Score(precision=0.86, recall=1.0, fmeasure=0.92)),
 'rouge2': AggregateScore(low=Score(precision=0.67, recall=0.8, fmeasure=0.73), mid=Score(precision=0.67, recall=0.8, fmeasure=0.73), high=Score(precision=0.67, recall=0.8, fmeasure=0.73)),
 'rougeL': AggregateScore(low=Score(precision=0.86, recall=1.0, fmeasure=0.92), mid=Score(precision=0.86, recall=1.0, fmeasure=0.92), high=Score(precision=0.86, recall=1.0, fmeasure=0.92)),
 'rougeLsum': AggregateScore(low=Score(precision=0.86, recall=1.0, fmeasure=0.92), mid=Score(precision=0.86, recall=1.0, fmeasure=0.92), high=Score(precision=0.86, recall=1.0, fmeasure=0.92))}
```

哇，那个输出中有很多信息——这都是什么意思？首先，🤗 Datasets 实际上计算了精度、召回率和 F1 分数的置信区间；这些是你可以在这里看到的 **low** , **mid** ， 和 **high** 属性。此外，🤗 Datasets 在比较生成摘要和参考摘要时，会根据不同类型的文本粒度计算各种 ROUGE 分数。这 **rouge1** 变体是一元组的重叠——这只是表达单词重叠的一种奇特方式，这正是我们上面讨论的度量标准。为了验证这一点，让我们输出 **mid** 的数值：

```py
scores["rouge1"].mid
```

```py
Score(precision=0.86, recall=1.0, fmeasure=0.92)
```

太好了，准确率和召回率匹配了！那么其他的 ROUGE 分数呢？ **rouge2** 测量二元组之间的重叠（想想单词对的重叠），而 **rougeL** 和 **rougeLsum** 通过在生成的和参考摘要中查找最长的公共子串来测量最长的单词匹配序列。中的“总和” **rougeLsum** 指的是这个指标是在整个摘要上计算的，而 **rougeL** 计算为单个句子的平均值。

✏️ **试试看！** 创建您自己的生成和参考摘要示例，并查看生成的 ROUGE 分数是否与基于精确度和召回率公式的手动计算一致。 对于附加分，将文本拆分为二元组并比较“rouge2”指标的精度和召回率。

我们将使用这些 ROUGE 分数来跟踪我们模型的性能，但在此之前，让我们做每个优秀的 NLP 从业者都应该做的事情：创建一个强大而简单的 baseline！

### 创建强大的 baseline

文本摘要的一个常见基线是简单地取一篇文章的前三个句子，通常称为 *lead-3* 基线。 我们可以使用句号(英文使用.)来跟踪句子边界，但这在”U.S.” or “U.N.”之类的首字母缩略词上会失败。所以我们将使用 `nltk` 库，它包含一个更好的算法来处理这些情况。 您可以使用 `pip` 安装软件包，如下所示：

```py
!pip install nltk
```

然后下载标点规则：

```py
import nltk

nltk.download("punkt")
```

接下来，我们从 `nltk` 导入句子标记器并创建一个简单的函数来提取评论中的前三个句子。 文本摘要的约定是用换行符分隔每个摘要，因此我们也将其包含在内并在训练示例上对其进行测试：

```py
from nltk.tokenize import sent_tokenize

def three_sentence_summary(text):
    return "\n".join(sent_tokenize(text)[:3])

print(three_sentence_summary(books_dataset["train"][1]["review_body"]))
```

```py
'I grew up reading Koontz, and years ago, I stopped,convinced i had "outgrown" him.'
'Still,when a friend was looking for something suspenseful too read, I suggested Koontz.'
'She found Strangers.'
```

这似乎有效，所以让我们现在实现一个函数，从数据集中提取这些“摘要”并计算 baseline 的 ROUGE 分数：

```py
def evaluate_baseline(dataset, metric):
    summaries = [three_sentence_summary(text) for text in dataset["review_body"]]
    return metric.compute(predictions=summaries, references=dataset["review_title"])
```

然后我们可以使用这个函数来计算验证集上的 ROUGE 分数，并使用 Pandas 对它们进行一些美化：

```py
import pandas as pd

score = evaluate_baseline(books_dataset["validation"], rouge_score)
rouge_names = ["rouge1", "rouge2", "rougeL", "rougeLsum"]
rouge_dict = dict((rn, round(score[rn].mid.fmeasure * 100, 2)) for rn in rouge_names)
rouge_dict
```

```py
{'rouge1': 16.74, 'rouge2': 8.83, 'rougeL': 15.6, 'rougeLsum': 15.96}
```

我们可以看到`rouge2`的分数明显低于其他； 这可能反映了这样一个事实，即评论标题通常很简洁，因此 lead-3 baseline 过于冗长。 现在我们有了一个很好的基准，让我们将注意力转向微调 mT5！

## 使用 Trainer API 微调 mT5

微调模型以进行提取摘要与我们在本章中介绍的其他任务非常相似。 我们需要做的第一件事是从`mt5-small`检查点加载预训练模型。 由于摘要提取是一个序列到序列的任务，我们可以使用 AutoModelForSeq2SeqLM 类加载模型，该类会自动下载并缓存权重：

```py
from transformers import AutoModelForSeq2SeqLM

model = AutoModelForSeq2SeqLM.from_pretrained(model_checkpoint)
```

💡 If you’re wondering why you don’t see any warnings about fine-tuning the model on a downstream task, that’s because for sequence-to-sequence tasks we keep all the weights of the network. Compare this to our text classification model in Chapter 3, where the head of the pretrained model was replaced with a randomly initialized network. 💡 如果您想知道为什么在下游任务中没有看到任何关于微调模型的警告，那是因为对于序列到序列的任务，我们保留了网络的所有权重。与我们在[第三章] (/course/chapter3)中的文本分类模型进行比较，文本分类模型预训练模型的头部被随机初始化的网络替换。

我们需要做的下一件事是登录 Hugging Face Hub。如果您在 notebook 中运行此代码，则可以使用以下实用程序函数执行此操作：

```py
from huggingface_hub import notebook_login

notebook_login()
```

这将显示一个小部件，您可以在其中输入您的凭据。或者，您可以在终端中运行此命令并在那里登录：

```py
huggingface-cli login
```

我们需要生成摘要以便在训练期间计算 ROUGE 分数。幸运的是，🤗 Transformers 提供了专用的 `Seq2SeqTrainingArguments` 和 `Seq2SeqTrainer` 类，可以自动为我们完成这项工作！ 为了了解它是如何工作的，让我们首先为我们的实验定义超参数和其他参数：

```py
from transformers import Seq2SeqTrainingArguments

batch_size = 8
num_train_epochs = 8
# Show the training loss with every epoch
logging_steps = len(tokenized_datasets["train"]) // batch_size
model_name = model_checkpoint.split("/")[-1]

args = Seq2SeqTrainingArguments(
    output_dir=f"{model_name}-finetuned-amazon-en-es",
    evaluation_strategy="epoch",
    learning_rate=5.6e-5,
    per_device_train_batch_size=batch_size,
    per_device_eval_batch_size=batch_size,
    weight_decay=0.01,
    save_total_limit=3,
    num_train_epochs=num_train_epochs,
    predict_with_generate=True,
    logging_steps=logging_steps,
    push_to_hub=True,
)
```

在这里， **predict_with_generate** 参数已设置为 True 表明我们应该在评估期间生成摘要，以便我们可以计算每个时期的 ROUGE 分数。正如在第一章所讨论的，解码器通过逐个预测令牌来执行推理，这是由模型的 **generate()** 方法实现的。设置 **predict_with_generate=True** 告诉 **Seq2SeqTrainer** 使用该方法进行评估。我们还调整了一些默认的超参数，例如学习率、epoch 数和权重衰减，并且我们设置了 **save_total_limit** 训练期间最多只保存 3 个检查点的选项——这是因为即使是 mT5 的“small”版本也使用大约 1 GB 的硬盘空间，我们可以通过限制我们保存的副本数量来节省一点空间。

`push_to_hub=True` 参数将允许我们在训练后将模型推送到 Hub； 您将在`output_dir`定义的位置中的用户配置文件下找到存储库。 请注意，您可以使用 `hub_model_id` 参数指定要推送到的存储库的名称（特别是当您想要推送到组织时，您必须使用此参数）。 例如，当我们将模型推送到 [`huggingface-course` 组织](https://huggingface.co/huggingface-course) 时，我们添加了`hub_model_id="huggingface-course/mt5-finetuned-amazon-en-es"` 到 `Seq2SeqTrainingArguments`。

我们需要做的下一件事是为训练器提供一个“compute_metrics()”函数，以便我们可以在训练期间评估我们的模型。 总结起来，这比简单地在模型的预测上调用 `rouge_score.compute()` 更复杂一些，因为我们需要在计算 ROUGE 分数之前将输出和标签解码为文本。 下面的函数正是这样做的，并且还利用 `nltk` 中的 `sent_tokenize()` 函数来用换行符分隔摘要语句：

```py
import numpy as np

def compute_metrics(eval_pred):
    predictions, labels = eval_pred
    # Decode generated summaries into text
    decoded_preds = tokenizer.batch_decode(predictions, skip_special_tokens=True)
    # Replace -100 in the labels as we can't decode them
    labels = np.where(labels != -100, labels, tokenizer.pad_token_id)
    # Decode reference summaries into text
    decoded_labels = tokenizer.batch_decode(labels, skip_special_tokens=True)
    # ROUGE expects a newline after each sentence
    decoded_preds = ["\n".join(sent_tokenize(pred.strip())) for pred in decoded_preds]
    decoded_labels = ["\n".join(sent_tokenize(label.strip())) for label in decoded_labels]
    # Compute ROUGE scores
    result = rouge_score.compute(
        predictions=decoded_preds, references=decoded_labels, use_stemmer=True
    )
    # Extract the median scores
    result = {key: value.mid.fmeasure * 100 for key, value in result.items()}
    return {k: round(v, 4) for k, v in result.items()}
```

接下来，我们需要为我们的序列到序列任务定义一个数据整理器。由于 mT5 是一个编码器-解码器 Transformer 模型，准备我们的批次的一个微妙之处是，在解码过程中，我们需要将标签向右移动一个。 这是为了确保解码器只看到之前的真实的标签，而不是当前或未来的标签，这对于模型来说很容易记忆。 这类似于在 因果语言建模 等任务中如何将掩蔽的自我注意应用于输入。

幸运的是，🤗 Transformers 提供了一个 `DataCollatorForSeq2Seq` 整理器，它将为我们动态填充输入和标签。 要实例化这个收集器，我们只需要提供 `tokenizer` 和 `model`：

```py
from transformers import DataCollatorForSeq2Seq

data_collator = DataCollatorForSeq2Seq(tokenizer, model=model)
```

让我们看看这个整理器在输入一小批示例时会产生什么。 首先，我们需要删除带有字符串的列，因为整理器不知道如何填充这些元素：

```py
tokenized_datasets = tokenized_datasets.remove_columns(
    books_dataset["train"].column_names
)
```

由于 collator 需要一个 `dict` 的列表，其中每个 `dict` 代表数据集中的一个示例，我们还需要在将数据传递给 data collator 之前将数据整理成预期的格式：

```py
features = [tokenized_datasets["train"][i] for i in range(2)]
data_collator(features)
```

```py
{'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
         1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0],
        [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
         1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]]), 'input_ids': tensor([[  1494,    259,   8622,    390,    259,    262,   2316,   3435,    955,
            772,    281,    772,   1617,    263,    305,  14701,    260,   1385,
           3031,    259,  24146,    332,   1037,    259,  43906,    305,    336,
            260,      1,      0,      0,      0,      0,      0,      0],
        [   259,  27531,  13483,    259,   7505,    260, 112240,  15192,    305,
          53198,    276,    259,  74060,    263,    260,    459,  25640,    776,
           2119,    336,    259,   2220,    259,  18896,    288,   4906,    288,
           1037,   3931,    260,   7083, 101476,   1143,    260,      1]]), 'labels': tensor([[ 7483,   259,  2364, 15695,     1,  -100],
        [  259, 27531, 13483,   259,  7505,     1]]), 'decoder_input_ids': tensor([[    0,  7483,   259,  2364, 15695,     1],
        [    0,   259, 27531, 13483,   259,  7505]])}
```

这里要注意的主要是第一个例子比第二个例子要长，所以第二个例子的 `input_ids` 和 `attention_mask` 已经在右侧填充了一个 `[PAD]` 标记（其 ID 是 `0`）。 类似地，我们可以看到 `labels` 已用 `-100` 填充，以确保填充标记被损失函数忽略。 最后，我们可以看到一个新的 `decoder_input_ids`，它通过在第一个条目中插入 `[PAD]` 标记将标签向右移动。

我们终于拥有了训练所需的所有的前期准备！我们现在只需要使用标准参数实例化训练器：

```py
from transformers import Seq2SeqTrainer

trainer = Seq2SeqTrainer(
    model,
    args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation"],
    data_collator=data_collator,
    tokenizer=tokenizer,
    compute_metrics=compute_metrics,
)
```

并启动我们的训练：

```py
trainer.train()
```

在训练期间，您应该会看到训练损失减少并且 ROUGE 分数随着每个 epoch 增加。训练完成后，您可以通过运行**Trainer.evaluate()** 查看最终的 ROUGE 分数 ：

```py
trainer.evaluate()
```

```py
{'eval_loss': 3.028524398803711,
 'eval_rouge1': 16.9728,
 'eval_rouge2': 8.2969,
 'eval_rougeL': 16.8366,
 'eval_rougeLsum': 16.851,
 'eval_gen_len': 10.1597,
 'eval_runtime': 6.1054,
 'eval_samples_per_second': 38.982,
 'eval_steps_per_second': 4.914}
```

从分数中我们可以看到，我们的模型轻松超过了我们的 lead-3 baseline——很好！最后要做的是将模型权重推送到 Hub，如下所示：

```py
trainer.push_to_hub(commit_message="Training complete", tags="summarization")
```

```py
'https://huggingface.co/huggingface-course/mt5-finetuned-amazon-en-es/commit/aa0536b829b28e73e1e4b94b8a5aacec420d40e0'
```

这会将检查点和配置文件保存到 **output_dir** , 在将所有文件上传到集线器之前。通过指定 **tags** 参数，我们还确保集线器上的小部件将是一个用于汇总管道的小部件，而不是与 mT5 架构关联的默认文本生成小部件（有关模型标签的更多信息，请参阅[🤗 Hub 文档](https://huggingface.co/docs/hub/main#how-is-a-models-type-of-inference-api-and-widget-determined)）。输出来自 **trainer.push_to_hub()** 是 Git 提交哈希的 URL，因此您可以轻松查看对模型存储库所做的更改！

在结束本节之前，让我们看一下如何使用 🤗 Accelerate 提供的底层 API 对 mT5 进行微调。

## 使用 🤗 Accelerate 微调 mT5

使用 🤗 Accelerate 微调我们的模型与我们在 Chapter 3 中遇到的文本分类示例非常相似。 主要区别在于需要在训练期间显式生成摘要并定义我们如何计算 ROUGE 分数（回想一下，`Seq2SeqTrainer` 为我们生成了摘要）。 让我们看看我们如何在 🤗 Accelerate 中实现这两个要求！

### 为训练做好一切准备

The first thing we need to do is create a `DataLoader` for each of our splits. Since the PyTorch dataloaders expect batches of tensors, we need to set the format to `"torch"` in our datasets: 我们需要做的第一件事是为每个数据集的每一个拆分创建一个`DataLoader`。 由于 PyTorch 数据加载器需要成批的张量，我们需要在数据集中将格式设置为`torch`：

```py
tokenized_datasets.set_format("torch")
```

现在我们已经有了仅由张量组成的数据集，接下来要做的是再次实例化`DataCollatorForSeq2Seq`。 为此，我们需要提供模型微调前的版本，所以让我们从缓存中再次加载它：

```py
model = AutoModelForSeq2SeqLM.from_pretrained(model_checkpoint)
```

然后我们可以实例化数据整理器并使用它来定义我们的数据加载器：

```py
from torch.utils.data import DataLoader

batch_size = 8
train_dataloader = DataLoader(
    tokenized_datasets["train"],
    shuffle=True,
    collate_fn=data_collator,
    batch_size=batch_size,
)
eval_dataloader = DataLoader(
    tokenized_datasets["validation"], collate_fn=data_collator, batch_size=batch_size
)
```

接下来要做的是定义我们想要使用的优化器。与我们的其他示例一样，我们将使用 **AdamW** ，这适用于大多数问题：

```py
from torch.optim import AdamW

optimizer = AdamW(model.parameters(), lr=2e-5)
```

最后，我们将模型、优化器和数据加载器提供给 **accelerator.prepare()** 方法：

```py
from accelerate import Accelerator

accelerator = Accelerator()
model, optimizer, train_dataloader, eval_dataloader = accelerator.prepare(
    model, optimizer, train_dataloader, eval_dataloader
)
```

🚨如果您在 TPU 上进行训练，则需要将上述所有代码移动到专门的训练函数中。有关详细信息，请参阅第三章。

现在我们已经准备好了我们索要用的对象，还有三件事要做：

*   定义学习率调度计划。
*   实现一个功能来对摘要进行后续处理以进行评估。
*   在 Hub 上创建一个存储库，我们可以将模型推送到该存储库。

对于学习率调度，我们将使用前几节中的标准线性衰减：

```py
from transformers import get_scheduler

num_train_epochs = 10
num_update_steps_per_epoch = len(train_dataloader)
num_training_steps = num_train_epochs * num_update_steps_per_epoch

lr_scheduler = get_scheduler(
    "linear",
    optimizer=optimizer,
    num_warmup_steps=0,
    num_training_steps=num_training_steps,
)
```

对于后续处理，我们需要一个函数，将生成的摘要拆分为由换行符分隔的句子。 这是 ROUGE 指标所期望的格式，我们可以使用以下代码片段来实现：

```py
def postprocess_text(preds, labels):
    preds = [pred.strip() for pred in preds]
    labels = [label.strip() for label in labels]

    # ROUGE expects a newline after each sentence
    preds = ["\n".join(nltk.sent_tokenize(pred)) for pred in preds]
    labels = ["\n".join(nltk.sent_tokenize(label)) for label in labels]

    return preds, labels
```

如果你还记得我们是如何定义 `Seq2SeqTrainer` 的 `compute_metrics()` 函数的，这对你来说应该很熟悉。

最后，我们需要在 Hugging Face Hub 上创建一个模型存储库。 为此，我们可以使用🤗 Hub 库的 get_full_repo_name。 我们只需要为我们的存储库定义一个名称，该库有一个非常好用的函数可以将存储库 ID 与用户配置文件结合起来：

```py
from huggingface_hub import get_full_repo_name

model_name = "test-bert-finetuned-squad-accelerate"
repo_name = get_full_repo_name(model_name)
repo_name
```

```py
'lewtun/mt5-finetuned-amazon-en-es-accelerate'
```

现在我们可以使用这个存储库名称将本地版本克隆到我们的结果目录中，该目录将存储训练的模型：

```py
from huggingface_hub import Repository

output_dir = "results-mt5-finetuned-squad-accelerate"
repo = Repository(output_dir, clone_from=repo_name)
```

这将允许我们在训练期间通过调用 `repo.push_to_hub()` 方法将模型推送到 Hub！ 现在让我们通过写出完整的训练循环来结束我们的分析。

### 训练循环

文本摘要的训练循环与我们遇到的其他 🤗 Accelerate 示例非常相似，大致分为四个主要步骤：这

1.  通过在每个 epoch 迭代 `train_dataloader` 中的所有示例来训练模型。
2.  在每个 epoch 结束时生成模型摘要，首先生成标记，然后将它们（和参考摘要）解码为文本。
3.  使用我们之前看到的相同技术计算 ROUGE 分数。
4.  保存检查点并将所有内容推送到 Hub。 在这里，我们依赖 `Repository` 对象的巧妙的 `blocking=False` 参数，以便我们可以在每个 epoch 异步地上传检查点。 这使我们能够继续训练，而不必等待与 GB 大小的模型慢呼呼的上传！

这些步骤可以在以下代码块中看到：

```py
from tqdm.auto import tqdm
import torch
import numpy as np

progress_bar = tqdm(range(num_training_steps))

for epoch in range(num_train_epochs):
    # Training
    model.train()
    for step, batch in enumerate(train_dataloader):
        outputs = model(**batch)
        loss = outputs.loss
        accelerator.backward(loss)

        optimizer.step()
        lr_scheduler.step()
        optimizer.zero_grad()
        progress_bar.update(1)

    # Evaluation
    model.eval()
    for step, batch in enumerate(eval_dataloader):
        with torch.no_grad():
            generated_tokens = accelerator.unwrap_model(model).generate(
                batch["input_ids"],
                attention_mask=batch["attention_mask"],
            )

            generated_tokens = accelerator.pad_across_processes(
                generated_tokens, dim=1, pad_index=tokenizer.pad_token_id
            )
            labels = batch["labels"]

            # If we did not pad to max length, we need to pad the labels too
            labels = accelerator.pad_across_processes(
                batch["labels"], dim=1, pad_index=tokenizer.pad_token_id
            )

            generated_tokens = accelerator.gather(generated_tokens).cpu().numpy()
            labels = accelerator.gather(labels).cpu().numpy()

            # Replace -100 in the labels as we can't decode them
            labels = np.where(labels != -100, labels, tokenizer.pad_token_id)
            if isinstance(generated_tokens, tuple):
                generated_tokens = generated_tokens[0]
            decoded_preds = tokenizer.batch_decode(
                generated_tokens, skip_special_tokens=True
            )
            decoded_labels = tokenizer.batch_decode(labels, skip_special_tokens=True)

            decoded_preds, decoded_labels = postprocess_text(
                decoded_preds, decoded_labels
            )

            rouge_score.add_batch(predictions=decoded_preds, references=decoded_labels)

    # Compute metrics
    result = rouge_score.compute()
    # Extract the median ROUGE scores
    result = {key: value.mid.fmeasure * 100 for key, value in result.items()}
    result = {k: round(v, 4) for k, v in result.items()}
    print(f"Epoch {epoch}:", result)

    # Save and upload
    accelerator.wait_for_everyone()
    unwrapped_model = accelerator.unwrap_model(model)
    unwrapped_model.save_pretrained(output_dir, save_function=accelerator.save)
    if accelerator.is_main_process:
        tokenizer.save_pretrained(output_dir)
        repo.push_to_hub(
            commit_message=f"Training in progress epoch {epoch}", blocking=False
        )
```

```py
Epoch 0: {'rouge1': 5.6351, 'rouge2': 1.1625, 'rougeL': 5.4866, 'rougeLsum': 5.5005}
Epoch 1: {'rouge1': 9.8646, 'rouge2': 3.4106, 'rougeL': 9.9439, 'rougeLsum': 9.9306}
Epoch 2: {'rouge1': 11.0872, 'rouge2': 3.3273, 'rougeL': 11.0508, 'rougeLsum': 10.9468}
Epoch 3: {'rouge1': 11.8587, 'rouge2': 4.8167, 'rougeL': 11.7986, 'rougeLsum': 11.7518}
Epoch 4: {'rouge1': 12.9842, 'rouge2': 5.5887, 'rougeL': 12.7546, 'rougeLsum': 12.7029}
Epoch 5: {'rouge1': 13.4628, 'rouge2': 6.4598, 'rougeL': 13.312, 'rougeLsum': 13.2913}
Epoch 6: {'rouge1': 12.9131, 'rouge2': 5.8914, 'rougeL': 12.6896, 'rougeLsum': 12.5701}
Epoch 7: {'rouge1': 13.3079, 'rouge2': 6.2994, 'rougeL': 13.1536, 'rougeLsum': 13.1194}
Epoch 8: {'rouge1': 13.96, 'rouge2': 6.5998, 'rougeL': 13.9123, 'rougeLsum': 13.7744}
Epoch 9: {'rouge1': 14.1192, 'rouge2': 7.0059, 'rougeL': 14.1172, 'rougeLsum': 13.9509}
```

就是这样！ 运行此程序后，您将获得与我们使用“Trainer”获得的模型和结果非常相似的模型和结果。

## 使用您微调的模型

将模型推送到 Hub 后，您可以通过推理小部件或“管道”对象来使用它，如下所示：

```py
from transformers import pipeline

hub_model_id = "huggingface-course/mt5-small-finetuned-amazon-en-es"
summarizer = pipeline("summarization", model=hub_model_id)
```

我们可以将测试集中的一些示例（模型还没有看到）提供给我们的管道，以了解生成摘要的质量。 首先让我们实现一个简单的函数来一起显示评论、标题和生成的摘要：

```py
def print_summary(idx):
    review = books_dataset["test"][idx]["review_body"]
    title = books_dataset["test"][idx]["review_title"]
    summary = summarizer(books_dataset["test"][idx]["review_body"])[0]["summary_text"]
    print(f"'>>> Review: {review}'")
    print(f"\n'>>> Title: {title}'")
    print(f"\n'>>> Summary: {summary}'")
```

让我们看一下我们得到的一个英文例子：

```py
print_summary(100)
```

```py
'>>> Review: Nothing special at all about this product... the book is too small and stiff and hard to write in. The huge sticker on the back doesn’t come off and looks super tacky. I would not purchase this again. I could have just bought a journal from the dollar store and it would be basically the same thing. It’s also really expensive for what it is.'

'>>> Title: Not impressed at all... buy something else'

'>>> Summary: Nothing special at all about this product'
```

这还不错！ 我们可以看到，我们的模型实际上已经能够通过增加部分新词来执行抽象摘要。 也许我们模型最酷的方面是它是双语的，所以我们还可以生成西班牙语评论的摘要：

```py
print_summary(0)
```

```py
'>>> Review: Es una trilogia que se hace muy facil de leer. Me ha gustado, no me esperaba el final para nada'

'>>> Title: Buena literatura para adolescentes'

'>>> Summary: Muy facil de leer'
```

摘要翻译成了英文的“非常容易阅读”，在这种情况下，我们可以看到它是直接从评论中提取的。 这显示了 mT5 模型的多功能性，并让您体验了处理多语言语料库的感觉！

接下来，我们将把注意力转向稍微复杂的任务：从头开始训练语言模型。