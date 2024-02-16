# Seq2Seq

> 原始文本：[`huggingface.co/docs/autotrain/seq2seq`](https://huggingface.co/docs/autotrain/seq2seq)

Seq2Seq 是一个任务，涉及将一系列单词转换为另一系列单词。它用于机器翻译、文本摘要和问答。

## 数据格式

```py
text,target
"this movie is great","dieser Film ist großartig"
"this movie is bad","dieser Film ist schlecht"
.
.
.
```

## 列

您的 CSV 数据集必须有两列：`text`和`target`。
