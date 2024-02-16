# 文本分类

> 原始文本：[https://huggingface.co/docs/autotrain/text_classification](https://huggingface.co/docs/autotrain/text_classification)

使用AutoTrain训练文本分类模型非常简单！将您的数据准备好并以正确的格式，然后只需点击几下，您的最先进的模型就可以准备好用于生产。

## 数据格式

让我们训练一个模型来对电影评论的情感进行分类。数据应该以以下CSV格式呈现：

```py
text,target
"this movie is great",positive
"this movie is bad",negative
.
.
.
```

正如您所看到的，CSV文件中有两列。一列是文本，另一列是标签。标签可以是任何字符串。在这个例子中，我们有两个标签：`positive` 和 `negative`。您可以有任意多个标签。

如果您的CSV文件很大，您可以将其分成多个CSV文件并分别上传。请确保所有CSV文件中的列名相同。

使用pandas 分割CSV文件的一种方法如下：

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

您的CSV数据集必须有两列：`text` 和 `target`。
