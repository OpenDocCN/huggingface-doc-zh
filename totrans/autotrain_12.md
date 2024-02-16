# 标记分类

> 原文链接：[`huggingface.co/docs/autotrain/token_classification`](https://huggingface.co/docs/autotrain/token_classification)

标记分类是对序列中的每个标记进行分类的任务。这可以用于命名实体识别（NER）、词性标注（POS）等。将您的数据准备好，然后只需点击几下，您的最先进模型就可以准备好用于生产。

## 数据格式

数据应该采用以下 CSV 格式：

```py
tokens,tags
"['I', 'love', 'Paris']", "['O', 'O', 'B-LOC']"
"['I', 'live', 'in', 'New', 'York']", "['O', 'O', 'O', 'B-LOC', 'I-LOC']"
.
.
.
```

正如您所看到的，我们的 CSV 文件中有两列。一列是标记，另一列是标签。这两列都是字符串化的列表！标记列包含句子的标记，标签列包含每个标记的标签。

如果您的 CSV 文件很大，可以将其分成多个 CSV 文件并分别上传。请确保所有 CSV 文件中的列名相同。

使用 pandas 划分 CSV 文件的一种方法如下：

```py
import pandas as pd

# Set the chunk size
chunk_size = 1000
i = 1

# Open the CSV file and read it in chunks
for chunk in pd.read_csv('example.csv', chunksize=chunk_size):
    # Save each chunk to a new file
    chunk.to_csv(f'chunk_{i}.csv', index=False)
    i += 1
```

## 列

您的 CSV 数据集必须有两列：`tokens`和`tags`。
