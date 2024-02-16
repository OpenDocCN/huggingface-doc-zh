# Pandas

> 原始文本：[https://huggingface.co/docs/datasets-server/pandas](https://huggingface.co/docs/datasets-server/pandas)

[Pandas](https://pandas.pydata.org/docs/index.html)是一个用于数据分析的流行DataFrame库。

要从单个Parquet文件中读取数据，请使用[`read_parquet`](https://pandas.pydata.org/docs/reference/api/pandas.read_parquet.html)函数将其读取到DataFrame中：

```py
import pandas as pd

df = (
    pd.read_parquet("https://huggingface.co/datasets/blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/train/0000.parquet")
    .groupby('horoscope')['text']
    .apply(lambda x: x.str.len().mean())
    .sort_values(ascending=False)
    .head(5)
)
```

要读取多个Parquet文件 - 例如，如果数据集被分片 - 您需要使用[`concat`](https://pandas.pydata.org/docs/reference/api/pandas.concat.html)函数将文件连接成一个单独的DataFrame：

```py
df = (
      pd.concat([pd.read_parquet(url) for url in urls])
      .groupby('horoscope')['text']
      .apply(lambda x: x.str.len().mean())
      .sort_values(ascending=False)
      .head(5)
)
```
