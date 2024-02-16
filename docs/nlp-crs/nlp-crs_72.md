# 调试训练管道

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter8/4?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter8/4?fw=pt)

                Pytorch TensorFlow![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/chapter8/section4.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/chapter8/section4.ipynb)

你已经编写了一个漂亮的脚本来训练或微调给定任务的模型，尽职尽责地遵循 Chapter 7 中的建议。 但是当你启动命令 `trainer.train()` 时，可怕的事情发生了：你得到一个错误😱！ 或者更糟糕的是，一切似乎都很好，训练运行没有错误，但生成的模型很糟糕。 在本节中，我们将向您展示如何调试此类问题。

## 调试训练管道

[`www.youtube-nocookie.com/embed/L-WSwUWde1U`](https://www.youtube-nocookie.com/embed/L-WSwUWde1U)

当您在 `trainer.train()` 中遇到错误时，它可能来自多个来源，因为 `Trainer` 通常会将很多东西放在一起组合运行。 它将 datasets 转换为 dataloaders，因此问题可能出在 datasets 中，或者在尝试将 datasets 的元素一起批处理时出现问题。 然后它需要准备一批数据并将其提供给模型，因此问题可能出在模型代码中。 之后，它会计算梯度并执行优化器，因此问题也可能出在您的优化器中。 即使训练一切顺利，如果您的评估指标有问题，评估期间仍然可能出现问题。

调试 `trainer.train()` 中出现的错误的最佳方法是手动检查整个管道，看看哪里出了问题。 错误通常很容易解决。

为了证明这一点，我们将使用以下脚本（尝试）在 [MNLI 数据集](https://huggingface.co/datasets/glue)上微调 DistilBERT 模型：

```py
from datasets import load_dataset
from transformers import (
    AutoTokenizer,
    AutoModelForSequenceClassification,
    TrainingArguments,
    Trainer,
)

raw_datasets = evaluate.load("glue", "mnli")

model_checkpoint = "distilbert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(model_checkpoint)

def preprocess_function(examples):
    return tokenizer(examples["premise"], examples["hypothesis"], truncation=True)

tokenized_datasets = raw_datasets.map(preprocess_function, batched=True)
model = AutoModelForSequenceClassification.from_pretrained(model_checkpoint)

args = TrainingArguments(
    f"distilbert-finetuned-mnli",
    evaluation_strategy="epoch",
    save_strategy="epoch",
    learning_rate=2e-5,
    num_train_epochs=3,
    weight_decay=0.01,
)

metric = evaluate.load("glue", "mnli")

def compute_metrics(eval_pred):
    predictions, labels = eval_pred
    return metric.compute(predictions=predictions, references=labels)

trainer = Trainer(
    model,
    args,
    train_dataset=raw_datasets["train"],
    eval_dataset=raw_datasets["validation_matched"],
    compute_metrics=compute_metrics,
)
trainer.train()
```

如果你尝试执行它，你会遇到一个相当神秘的错误：

```py
'ValueError: You have to specify either input_ids or inputs_embeds'
```

### 检查数据

这是不言而喻的，如果你的数据被破坏，“Trainer”将无法形成批次，更不用说训练你的模型了。 所以首先，你需要看看你的训练集中有什么。

为了避免花费无数小时试图检查和修复不是错误来源的东西，我们建议您使用 `trainer.train_dataset` 进行检查。 所以让我们在这里这样做：

```py
trainer.train_dataset[0]
```

```py
{'hypothesis': 'Product and geography are what make cream skimming work. ',
 'idx': 0,
 'label': 1,
 'premise': 'Conceptually cream skimming has two basic dimensions - product and geography.'}
```

你注意到有什么不对吗？ 与缺少有关 `input_ids` 的错误消息相结合，应该让您意识到数据集里是文本，而不是模型可以理解的数字。 在这个例子，输出的原始错误信息非常具有误导性，因为 `Trainer` 会自动删除与模型签名不匹配的列（即模型预期的参数）。 这意味着在这里，除了标签之外的所有东西都被丢弃了。 因此，创建批次然后将它们发送到模型没有问题，而模型又抱怨它没有收到正确的输入。

为什么没有处理数据生成标记呢？ 我们确实在数据集上使用了“Dataset.map()”方法来对每个样本应用标记器。 但是如果你仔细看代码，你会发现我们在将训练和评估集传递给`Trainer`时犯了一个错误。 我们在这里没有使用 `tokenized_datasets`，而是使用了 `raw_datasets` 🤦。 所以让我们解决这个问题！

```py
from datasets import load_dataset
from transformers import (
    AutoTokenizer,
    AutoModelForSequenceClassification,
    TrainingArguments,
    Trainer,
)

raw_datasets = evaluate.load("glue", "mnli")

model_checkpoint = "distilbert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(model_checkpoint)

def preprocess_function(examples):
    return tokenizer(examples["premise"], examples["hypothesis"], truncation=True)

tokenized_datasets = raw_datasets.map(preprocess_function, batched=True)
model = AutoModelForSequenceClassification.from_pretrained(model_checkpoint)

args = TrainingArguments(
    f"distilbert-finetuned-mnli",
    evaluation_strategy="epoch",
    save_strategy="epoch",
    learning_rate=2e-5,
    num_train_epochs=3,
    weight_decay=0.01,
)

metric = evaluate.load("glue", "mnli")

def compute_metrics(eval_pred):
    predictions, labels = eval_pred
    return metric.compute(predictions=predictions, references=labels)

trainer = Trainer(
    model,
    args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation_matched"],
    compute_metrics=compute_metrics,
)
trainer.train()
```

这个新代码现在会给出一个不同的错误：

```py
'ValueError: expected sequence of length 43 at dim 1 (got 37)'
```

查看 traceback，我们可以看到错误发生在数据整理步骤中：

```py
~/git/transformers/src/transformers/data/data_collator.py in torch_default_data_collator(features)
    105                 batch[k] = torch.stack([f[k] for f in features])
    106             else:
--> 107                 batch[k] = torch.tensor([f[k] for f in features])
    108 
    109     return batch
```

所以，我们应该去研究一下那个。 然而，在我们这样做之前，让我们完成检查我们的数据， 先确定它 100%是正确的。

在调试课程的内容时，您应该始终做的一件事是查看模型的解码输入。 我们无法理解直接提供给它的数字，所以我们应该看看这些数字代表什么。 例如，在计算机视觉中，这意味着查看您传递的图片像素的解码，在语音中意味着解码后的音频样本，对于我们的 NLP 示例，这意味着使用我们的标记器解码的输入：

```py
tokenizer.decode(trainer.train_dataset[0]["input_ids"])
```

```py
'[CLS] conceptually cream skimming has two basic dimensions - product and geography. [SEP] product and geography are what make cream skimming work. [SEP]'
```

所以这似乎是正确的。 您应该对输入中的所有键执行此操作：

```py
trainer.train_dataset[0].keys()
```

```py
dict_keys(['attention_mask', 'hypothesis', 'idx', 'input_ids', 'label', 'premise'])
```

请注意，与模型接受的输入不对应的键将被自动丢弃，因此这里我们将仅保留 `input_ids`、`attention_mask` 和 `label`（将重命名为 `labels`）。 要仔细检查模型输入的列，您可以打印模型的类，然后查看其文档：

```py
type(trainer.model)
```

```py
transformers.models.distilbert.modeling_distilbert.DistilBertForSequenceClassification
```

所以在我们的例子中，我们在[在这个页面](https://huggingface.co/transformers/model_doc/distilbert.html#distilbertforsequenceclassification)可以检查上接受的参数。 `Trainer` 也会记录它丢弃的列。

我们通过解码检查了输入 ID 是否正确。 接下来是检查 `attention_mask`：

```py
trainer.train_dataset[0]["attention_mask"]
```

```py
[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
```

由于我们没有在预处理中应用填充，这看起来非常自然。 为确保该注意掩码没有问题，让我们检查它与输入 ID 的长度是否相同：

```py
len(trainer.train_dataset[0]["attention_mask"]) == len(
    trainer.train_dataset[0]["input_ids"]
)
```

```py
True
```

那挺好的！ 最后，让我们检查一下我们的标签：

```py
trainer.train_dataset[0]["label"]
```

```py
1
```

与输入 ID 一样，这是一个本身并没有真正意义的数字。 正如我们之前看到的，整数和标签名称之间的映射存储在数据集相应 *feature* 的 `names` 属性中：

```py
trainer.train_dataset.features["label"].names
```

```py
['entailment', 'neutral', 'contradiction']
```

所以`1`表示`neutral`，表示我们上面看到的两句话并不矛盾，也没有包含关系。 这似乎是正确的！

我们这里没有令牌类型 ID，因为 DistilBERT 不需要它们； 如果您的模型中有一些，您还应该确保它们正确匹配输入中第一句和第二句的位置。

✏️ **轮到你了！** 检查训练数据集的第二个元素是否正确。

我们在这里只对训练集进行检查，但您当然应该以同样的方式仔细检查验证集和测试集。

现在我们知道我们的数据集看起来不错，是时候检查训练管道的下一步了。

### 从 datasets 到 dataloaders

训练管道中可能出错的下一件事是当“Trainer”尝试从训练或验证集形成批次时。 一旦你确定 `Trainer` 的数据集是正确的，你可以尝试通过执行以下操作手动形成一个批次（可以将 `train` 替换为 `eval` 用于验证数据加载器）：

```py
for batch in trainer.get_train_dataloader():
    break
```

此代码创建训练数据加载器，然后对其进行迭代，在第一次迭代时停止。 如果代码执行没有错误，那么您就有了可以检查的第一个训练批次，如果代码出错，您可以确定问题出在数据加载器中，如下所示：

```py
~/git/transformers/src/transformers/data/data_collator.py in torch_default_data_collator(features)
    105                 batch[k] = torch.stack([f[k] for f in features])
    106             else:
--> 107                 batch[k] = torch.tensor([f[k] for f in features])
    108 
    109     return batch

ValueError: expected sequence of length 45 at dim 1 (got 76)
```

检查 trackback 的最后一个堆栈的输出应该足以给你一个线索，但让我们做更多的挖掘。 批处理创建过程中的大多数问题是由于将示例整理到单个批处理中而出现的，因此在有疑问时首先要检查的是您的 DataLoader 正在使用什么 collate_fn：

```py
data_collator = trainer.get_train_dataloader().collate_fn
data_collator
```

```py
<function transformers.data.data_collator.default_data_collator(features: List[InputDataClass], return_tensors='pt') -> Dict[str, Any]>
```

所以，目前使用的是 `default_data_collator`，但这不是我们在这种情况下想要的。 我们希望将示例填充到批处理中最长的句子，这是由 `DataCollatorWithPadding` 整理器完成的。 而这个数据收集器应该是默认被 `Trainer` 使用的，为什么这里没有使用呢？

答案是因为我们没有将 `tokenizer` 传递给 `Trainer`，所以它无法创建我们想要的 `DataCollatorWithPadding`。 在实践中，您应该明确地传递您想要使用的数据整理器，以确保避免这些类型的错误。 让我们调整我们的代码来做到这一点：

```py
from datasets import load_dataset
import evaluate
from transformers import (
    AutoTokenizer,
    AutoModelForSequenceClassification,
    DataCollatorWithPadding,
    TrainingArguments,
    Trainer,
)

raw_datasets = evaluate.load("glue", "mnli")

model_checkpoint = "distilbert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(model_checkpoint)

def preprocess_function(examples):
    return tokenizer(examples["premise"], examples["hypothesis"], truncation=True)

tokenized_datasets = raw_datasets.map(preprocess_function, batched=True)
model = AutoModelForSequenceClassification.from_pretrained(model_checkpoint)

args = TrainingArguments(
    f"distilbert-finetuned-mnli",
    evaluation_strategy="epoch",
    save_strategy="epoch",
    learning_rate=2e-5,
    num_train_epochs=3,
    weight_decay=0.01,
)

metric = evaluate.load("glue", "mnli")

def compute_metrics(eval_pred):
    predictions, labels = eval_pred
    return metric.compute(predictions=predictions, references=labels)

data_collator = DataCollatorWithPadding(tokenizer=tokenizer)

trainer = Trainer(
    model,
    args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation_matched"],
    compute_metrics=compute_metrics,
    data_collator=data_collator,
    tokenizer=tokenizer,
)
trainer.train()
```

好消息？ 我们没有得到与以前相同的错误，这绝对是进步。 坏消息？ 我们得到了一个臭名昭著的 CUDA 错误：

```py
RuntimeError: CUDA error: CUBLAS_STATUS_ALLOC_FAILED when calling `cublasCreate(handle)`
```

这很糟糕，因为 CUDA 错误通常很难调试。 我们稍后会看到如何解决这个问题，但首先让我们完成对批处理创建的分析。

如果您确定您的数据整理器是正确的，则应尝试将其应用于数据集的几个样本：

```py
data_collator = trainer.get_train_dataloader().collate_fn
batch = data_collator([trainer.train_dataset[i] for i in range(4)])
```

此代码将失败，因为 `train_dataset` 包含字符串列，`Trainer` 通常会删除这些列。 您可以手动删除它们，或者如果您想准确地修改 `Trainer` 在幕后所做的事情，您可以调用私有的 `Trainer._remove_unused_columns()` 方法来执行此操作：

```py
data_collator = trainer.get_train_dataloader().collate_fn
actual_train_set = trainer._remove_unused_columns(trainer.train_dataset)
batch = data_collator([actual_train_set[i] for i in range(4)])
```

如果错误仍然存在，您应该能够手动调试数据整理器内部以确定具体的问题。

现在我们已经调试了批处理创建过程，是时候将数据传递给模型了！

### 检查模型

您应该能够通过执行以下命令来获得一个批次的数据：

```py
for batch in trainer.get_train_dataloader():
    break
```

如果您在 notebook 中运行此代码，您可能会收到与我们之前看到的类似的 CUDA 错误，在这种情况下，您需要重新启动 notebook 并重新执行最后一个片段，而不运行 `trainer.train()` 行.这是关于 CUDA 错误的第二个最烦人的事情：它们会破坏您的 Cuda 内核，而且无法恢复。它们最烦人的事情是它们很难调试。

这是为什么？它与 GPU 的工作方式有关。它们在并行执行大量操作方面非常有效，但缺点是当其中一条指令导致错误时，您不会立即知道。只有当程序在 GPU 上调用多个进程的同步处理时，它才会意识到出现问题，因此错误实际上是在与创建它的原因无关的地方引发的。例如，如果我们查看之前的 trackback，错误是在向后传递期间引发的，但我们会在一分钟内看到它实际上源于向前传递中的某些东西。

那么我们如何调试这些错误呢？答案很简单：我们没有。除非您的 CUDA 错误是内存不足错误（这意味着您的 GPU 中没有足够的内存），否则您应该始终返回 CPU 进行调试。

为此，我们只需将模型放回 CPU 上并在我们的一批数据中调用它——“DataLoader”返回的那批数据尚未移动到 GPU：

```py
outputs = trainer.model.cpu()(**batch)
```

```py
~/.pyenv/versions/3.7.9/envs/base/lib/python3.7/site-packages/torch/nn/functional.py in nll_loss(input, target, weight, size_average, ignore_index, reduce, reduction)
   2386         )
   2387     if dim == 2:
-> 2388         ret = torch._C._nn.nll_loss(input, target, weight, _Reduction.get_enum(reduction), ignore_index)
   2389     elif dim == 4:
   2390         ret = torch._C._nn.nll_loss2d(input, target, weight, _Reduction.get_enum(reduction), ignore_index)

IndexError: Target 2 is out of bounds.
```

所以，思路越来越清晰了。 我们现在在损失计算中没有出现 CUDA 错误，而是有一个“IndexError”（因此与我们之前所说的反向传播无关）。 更准确地说，我们可以看到是 Target 2 造成了错误，所以这是检查模型标签数量的好时机：

```py
trainer.model.config.num_labels
```

```py
2
```

有两个标签，只允许 0 和 1 作为目标，但是根据错误信息我们得到一个 2。得到一个 2 实际上是正常的：如果我们记得我们之前提取的标签名称，有三个，所以我们有索引 0 , 1 和 2 在我们的数据集中。 问题是我们没有告诉我们的模型，它应该创建三个标签。 所以让我们解决这个问题！

```py
from datasets import load_dataset
import evaluate
from transformers import (
    AutoTokenizer,
    AutoModelForSequenceClassification,
    DataCollatorWithPadding,
    TrainingArguments,
    Trainer,
)

raw_datasets = evaluate.load("glue", "mnli")

model_checkpoint = "distilbert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(model_checkpoint)

def preprocess_function(examples):
    return tokenizer(examples["premise"], examples["hypothesis"], truncation=True)

tokenized_datasets = raw_datasets.map(preprocess_function, batched=True)
model = AutoModelForSequenceClassification.from_pretrained(model_checkpoint, num_labels=3)

args = TrainingArguments(
    f"distilbert-finetuned-mnli",
    evaluation_strategy="epoch",
    save_strategy="epoch",
    learning_rate=2e-5,
    num_train_epochs=3,
    weight_decay=0.01,
)

metric = evaluate.load("glue", "mnli")

def compute_metrics(eval_pred):
    predictions, labels = eval_pred
    return metric.compute(predictions=predictions, references=labels)

data_collator = DataCollatorWithPadding(tokenizer=tokenizer)

trainer = Trainer(
    model,
    args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation_matched"],
    compute_metrics=compute_metrics,
    data_collator=data_collator,
    tokenizer=tokenizer,
)
```

我们还没有包含 `trainer.train()` 行，以便花时间检查一切是否正常。 如果我们请求一个批次的数据并将其传递给我们的模型，它现在可以正常工作了！

```py
for batch in trainer.get_train_dataloader():
    break

outputs = trainer.model.cpu()(**batch)
```

下一步是回到 GPU 并检查一切是否仍然有效：

```py
import torch

device = torch.device("cuda") if torch.cuda.is_available() else torch.device("cpu")
batch = {k: v.to(device) for k, v in batch.items()}

outputs = trainer.model.to(device)(**batch)
```

如果仍然出现错误，请确保重新启动 notebook 并仅执行脚本的最后一个版本。

### 执行一个优化器步骤

现在我们知道我们可以构建实际通过模型检查的成批次的数据，我们已经为训练管道的下一步做好准备：计算梯度并执行优化步骤。

第一部分只是在 loss 上调用 `backward()` 方法：

```py
loss = outputs.loss
loss.backward()
```

在这个阶段很少出现错误，但如果确实出现错误，请返回 CPU 以获取有用的错误消息。

要执行优化步骤，我们只需要创建 `optimizer` 并调用它的 `step()` 方法：

```py
trainer.create_optimizer()
trainer.optimizer.step()
```

同样，如果您在 `Trainer` 中使用默认优化器，则在此阶段您不应该收到错误，但如果您有自定义优化器，则可能会出现一些问题需要在这里调试。 如果您在此阶段遇到奇怪的 CUDA 错误，请不要忘记返回 CPU。 说到 CUDA 错误，前面我们提到了一个特殊情况。 现在让我们来看看。

### 处理 CUDA out-of-memory 错误

每当您收到以`RuntimeError: CUDA out of memory`开头的错误消息时，这表明您的 GPU 内存不足。 这与您的代码没有直接关联，并且它可能发生在运行良好的代码中。 此错误意味着您试图在 GPU 的内部存储器中放入太多东西，这导致了错误。 与其他 CUDA 错误一样，您需要重新启动内核才能再次运行训练。

要解决这个问题，您只需要使用更少的 GPU 空间——这往往说起来容易做起来难。 首先，确保您没有同时在 GPU 上运行两个模型（当然，除非您的问题需要这样做）。 然后，您可能应该减少 batch 的大小，因为它直接影响模型的所有中间输出的大小及其梯度。 如果问题仍然存在，请考虑使用较小版本的模型。

在课程的下一部分中，我们将介绍更先进的技术，这些技术可以帮助您减少内存占用并让您微调最大的模型。

### 评估模型

现在我们已经解决了代码的所有问题，一切都很完美，训练应该可以顺利进行，对吧？ 没那么快！ 如果你运行 `trainer.train()` 命令，一开始一切看起来都不错，但过一会儿你会得到以下信息：

```py
# This will take a long time and error out, so you shouldn't run this cell
trainer.train()
```

```py
TypeError: only size-1 arrays can be converted to Python scalars
```

您将意识到此错误出现在评估阶段，因此这是我们需要调试的最后一件事。

您可以像这样在训练中独立运行`Trainer`的评估循环：

```py
trainer.evaluate()
```

```py
TypeError: only size-1 arrays can be converted to Python scalars
```

💡 您应该始终确保在启动 `trainer.train()` 之前 `trainer.evaluate()`是可以运行的，以避免在遇到错误之前浪费大量计算资源。

在尝试调试评估循环中的问题之前，您应该首先确保您已经查看了数据，能够正确地形成批处理，并且可以在其上运行您的模型。 我们已经完成了所有这些步骤，因此可以执行以下代码而不会出错：

```py
for batch in trainer.get_eval_dataloader():
    break

batch = {k: v.to(device) for k, v in batch.items()}

with torch.no_grad():
    outputs = trainer.model(**batch)
```

稍等一会儿，错误出现，在评估阶段结束时，如果我们查看 trackback，我们会看到：

```py
~/git/datasets/src/datasets/metric.py in add_batch(self, predictions, references)
    431         """
    432         batch = {"predictions": predictions, "references": references}
--> 433         batch = self.info.features.encode_batch(batch)
    434         if self.writer is None:
    435             self._init_writer()
```

这告诉我们错误源自 `datasets/metric.py` 模块——所以这是我们的 `compute_metrics()` 函数的问题。 它需要一个带有 logits 和标签的元组作为 NumPy 数组，所以让我们尝试输入它：

```py
predictions = outputs.logits.cpu().numpy()
labels = batch["labels"].cpu().numpy()

compute_metrics((predictions, labels))
```

```py
TypeError: only size-1 arrays can be converted to Python scalars
```

我们得到同样的错误，所以问题肯定出在那个函数上。 如果我们回顾它的代码，我们会发现它只是将“预测”和“真实的标签”转发到“metric.compute()”。 那么这种方法有问题吗？ 并不真地。 让我们快速浏览一下形状：

```py
predictions.shape, labels.shape
```

```py
((8, 3), (8,))
```

我们的预测仍然是 logits，而不是实际的预测，这就是 metrics 返回这个（有点模糊）错误的原因。 修复很简单； 我们只需要在 `compute_metrics()` 函数中添加一个 argmax：

```py
import numpy as np

def compute_metrics(eval_pred):
    predictions, labels = eval_pred
    predictions = np.argmax(predictions, axis=1)
    return metric.compute(predictions=predictions, references=labels)

compute_metrics((predictions, labels))
```

```py
{'accuracy': 0.625}
```

现在我们的错误已修复！ 这是最后一个，所以我们的脚本现在将正确训练模型。

作为参考，这里是完全修正好的脚本：

```py
import numpy as np
from datasets import load_dataset
import evaluate
from transformers import (
    AutoTokenizer,
    AutoModelForSequenceClassification,
    DataCollatorWithPadding,
    TrainingArguments,
    Trainer,
)

raw_datasets = evaluate.load("glue", "mnli")

model_checkpoint = "distilbert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(model_checkpoint)

def preprocess_function(examples):
    return tokenizer(examples["premise"], examples["hypothesis"], truncation=True)

tokenized_datasets = raw_datasets.map(preprocess_function, batched=True)
model = AutoModelForSequenceClassification.from_pretrained(model_checkpoint, num_labels=3)

args = TrainingArguments(
    f"distilbert-finetuned-mnli",
    evaluation_strategy="epoch",
    save_strategy="epoch",
    learning_rate=2e-5,
    num_train_epochs=3,
    weight_decay=0.01,
)

metric = evaluate.load("glue", "mnli")

def compute_metrics(eval_pred):
    predictions, labels = eval_pred
    predictions = np.argmax(predictions, axis=1)
    return metric.compute(predictions=predictions, references=labels)

data_collator = DataCollatorWithPadding(tokenizer=tokenizer)

trainer = Trainer(
    model,
    args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation_matched"],
    compute_metrics=compute_metrics,
    data_collator=data_collator,
    tokenizer=tokenizer,
)
trainer.train()
```

在这种情况下，如果没有更多错误，我们的脚本将微调一个应该给出合理结果的模型。 但是，如果训练没有任何错误，而训练出来的模型根本表现不佳，我们该怎么办？ 这是机器学习中最难的部分，我们将向您展示一些可以提供帮助的技术。

💡 如果您使用手动训练循环，则相同的步骤也适用于调试训练管道，而且更容易将它们分开。 但是，请确保您没有忘记正确位置的 `model.eval()` 或 `model.train()`，或者每个步骤中的 `zero_grad()`！

## 在训练期间调试静默（没有任何错误提示）错误

我们可以做些什么来调试一个没有错误地完成但没有得到好的结果的训练？ 我们会在这里给你一些提示，但请注意，这种调试是机器学习中最难的部分，并且没有神奇的答案。

### 检查您的数据（再次！）

只有在理论上可以从您的数据中学到任何东西时，您的模型才会学到一些东西。 如果存在损坏数据的错误或标签是随机属性的，那么您很可能不会在数据集上获得任何知识。 因此，始终首先仔细检查您的解码输入和标签，然后问自己以下问题：

*   解码后的数据是否可以理解？
*   你认同这些标签吗？
*   有没有一个标签比其他标签更常见？
*   如果模型预测随机的答案/总是相同的答案，那么 loss/评估指标应该是多少？

⚠️ 如果您正在进行分布式训练，请在每个过程中打印数据集的样本，并三次检查您是否得到相同的结果。 一个常见的错误是在数据创建中有一些随机性来源，这使得每个进程都有不同版本的数据集。

查看您的数据后，查看模型的一些预测并对其进行解码。 如果模型总是预测同样的事情，那可能是因为你的数据集偏向一个类别（针对分类问题）； 过采样稀有类等技术可能会有所帮助。

如果您在初始模型上获得的 loss/评估指标与您期望的随机预测的 loss/评估指标非常不同，请仔细检查您的 loss 或评估指标的计算方式，因为那里可能存在错误。 如果您使用最后添加的多个 loss，请确保它们具有相同的规模。

当您确定您的数据是完美的时，您可以通过一个简单的测试来查看模型是否能够对其进行训练。

### 在一批上过度拟合你的模型

过度拟合通常是我们在训练时尽量避免的事情，因为这意味着模型没有学习识别我们想要的一般特征，而只是记住了训练样本。 在这种情况下，一遍又一遍地尝试在一个批次上训练您的模型是一个很好的测试，可以检查您的问题是否可以通过您尝试训练的模型来解决。 它还将帮助您查看您的初始学习率是否太高。

一旦你定义了你的 `Trainer` 之后，这样做真的很容易； 只需获取一批训练数据，然后仅使用该批次运行一个小型手动训练循环，大约 20 步：

```py
for batch in trainer.get_train_dataloader():
    break

batch = {k: v.to(device) for k, v in batch.items()}
trainer.create_optimizer()

for _ in range(20):
    outputs = trainer.model(**batch)
    loss = outputs.loss
    loss.backward()
    trainer.optimizer.step()
    trainer.optimizer.zero_grad()
```

💡 如果您的训练数据不平衡，请确保构建一批包含所有标签的训练数据。

生成的模型在一个“批次”上应该有接近完美的结果。 让我们计算结果预测的指标：

```py
with torch.no_grad():
    outputs = trainer.model(**batch)
preds = outputs.logits
labels = batch["labels"]

compute_metrics((preds.cpu().numpy(), labels.cpu().numpy()))
```

```py
{'accuracy': 1.0}
```

100% 准确率，现在这是一个很好的过拟合示例（这意味着如果你在任何其他句子上尝试你的模型，它很可能会给你一个错误的答案）！

如果你没有设法让你的模型获得这样的完美结果，这意味着你构建问题或数据的方式有问题，所以你应该修复它。 只有当你可以通过过拟合测试时，你才能确定你的模型实际上可以学到一些东西。

⚠️ 在此测试之后，您将不得不重新创建您的模型和“Trainer”，因为获得的模型可能无法在您的完整数据集上恢复和学习有用的东西。

### 在你有第一个基线之前不要调整任何东西

超参数调优总是被强调为机器学习中最难的部分，但这只是帮助您在指标上有所收获的最后一步。 大多数情况下，`Trainer` 的默认超参数可以很好地为您提供良好的结果，因此在您获得超出数据集基线的东西之前，不要开始进行耗时且昂贵的超参数搜索 .

一旦你有一个足够好的模型，你就可以开始稍微调整一下。 不要尝试使用不同的超参数启动一千次运行，而是比较一个超参数的不同值的几次运行，以了解哪个影响最大。

如果您正在调整模型本身，不要尝试任何您无法合理证明的事情。 始终确保您返回过拟合测试以验证您的更改没有产生任何意外后果。

### 请求帮忙

希望您会在本节中找到一些可以帮助您解决问题的建议，但如果不是这样，请记住您可以随时在 [论坛](https://discuss.huggingface.co/) 上向社区提问。

以下是一些可能有用的额外资源：

*   [“作为工程最佳实践工具的再现性”](https://docs.google.com/presentation/d/1yHLPvPhUs2KGI5ZWo0sU-PKU3GimAk3iTsI38Z-B5Gw/edit#slide=id.p)，作者：Joel Grus
*   [“神经网络调试清单”](https://towardsdatascience.com/checklist-for-debugging-neural-networks-d8b2a9434f21) 作者：Cecelia Shao
*   [“如何对机器学习代码进行单元测试”](https://medium.com/@keeper6928/how-to-unit-test-machine-learning-code-57cf6fd81765) by Chase Roberts
*   [“训练神经网络的秘诀”](http://karpathy.github.io/2019/04/25/recipe/)作者：Andrej Karpathy

当然，并不是你在训练神经网络时遇到的每一个问题都是你自己的错！ 如果您在 🤗 Transformers 或 🤗 Datasets 库中遇到看起来不正确的内容，您可能遇到了错误。 你应该告诉我们这一切，在下一节中，我们将准确解释如何做到这一点。