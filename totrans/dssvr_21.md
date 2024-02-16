# Polars

> 原文链接：[https://huggingface.co/docs/datasets-server/polars](https://huggingface.co/docs/datasets-server/polars)

[Polars](https://pola-rs.github.io/polars-book/user-guide/)是一个快速的DataFrame库，用Rust编写，以Arrow为基础。

💡 了解如何在[List Parquet files](parquet)指南中获取数据集URL的更多信息。

让我们从数据集服务器中获取[`blog_authorship_corpus`](https://huggingface.co/datasets/blog_authorship_corpus)数据集的`train`拆分的URL：

```py
r = requests.get("https://datasets-server.huggingface.co/parquet?dataset=blog_authorship_corpus")
j = r.json()
urls = [f['url'] for f in j['parquet_files'] if f['split'] == 'train']
urls
['https://huggingface.co/datasets/blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/train/0000.parquet',
 'https://huggingface.co/datasets/blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/train/0001.parquet']
```

要从单个Parquet文件中读取，请使用[`read_parquet`](https://pola-rs.github.io/polars/py-polars/html/reference/api/polars.read_parquet.html)函数将其读入DataFrame，然后执行您的查询：

```py
import polars as pl

df = (
    pl.read_parquet("https://huggingface.co/datasets/blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/train/0000.parquet")
    .groupby("horoscope")
    .agg(
        [
            pl.count(),
            pl.col("text").str.n_chars().mean().alias("avg_blog_length")
        ]
    )
    .sort("avg_blog_length", descending=True)
    .limit(5)
)
print(df)
shape: (5, 3)
┌───────────┬───────┬─────────────────┐
│ horoscope ┆ count ┆ avg_blog_length │
│ ---       ┆ ---   ┆ ---             │
│ str       ┆ u32   ┆ f64             │
╞═══════════╪═══════╪═════════════════╡
│ Aquarius  ┆ 34062 ┆ 1129.218836     │
│ Cancer    ┆ 41509 ┆ 1098.366812     │
│ Capricorn ┆ 33961 ┆ 1073.2002       │
│ Libra     ┆ 40302 ┆ 1072.071833     │
│ Leo       ┆ 40587 ┆ 1064.053687     │
└───────────┴───────┴─────────────────┘
```

要读取多个Parquet文件 - 例如，如果数据集被分片 - 您需要使用[`concat`](https://pola-rs.github.io/polars/py-polars/html/reference/api/polars.concat.html)函数将文件连接成单个DataFrame：

```py
import polars as pl
df = (
    pl.concat([pl.read_parquet(url) for url in urls])
    .groupby("horoscope")
    .agg(
        [
            pl.count(),
            pl.col("text").str.n_chars().mean().alias("avg_blog_length")
        ]
    )
    .sort("avg_blog_length", descending=True)
    .limit(5)
)
print(df)
shape: (5, 3)
┌─────────────┬───────┬─────────────────┐
│ horoscope   ┆ count ┆ avg_blog_length │
│ ---         ┆ ---   ┆ ---             │
│ str         ┆ u32   ┆ f64             │
╞═════════════╪═══════╪═════════════════╡
│ Aquarius    ┆ 49568 ┆ 1125.830677     │
│ Cancer      ┆ 63512 ┆ 1097.956087     │
│ Libra       ┆ 60304 ┆ 1060.611054     │
│ Capricorn   ┆ 49402 ┆ 1059.555261     │
│ Sagittarius ┆ 50431 ┆ 1057.458984     │
└─────────────┴───────┴─────────────────┘
```

## 懒惰的API

Polars提供了一个[懒惰的API](https://pola-rs.github.io/polars-book/user-guide/lazy/using/)，对于大型Parquet文件来说更高效和节省内存。LazyFrame API会跟踪您想要做的事情，只有在您准备好时才会执行整个查询。这样，懒惰的API不会预先将所有内容加载到RAM中，它允许您处理比可用RAM更大的数据集。

要惰性地读取Parquet文件，请改用[`scan_parquet`](https://pola-rs.github.io/polars/py-polars/html/reference/api/polars.scan_parquet.html)函数。然后，使用[`collect`](https://pola-rs.github.io/polars/py-polars/html/reference/lazyframe/api/polars.LazyFrame.collect.html)函数执行整个查询：

```py
import polars as pl

q = (
    pl.scan_parquet("https://huggingface.co/datasets/blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/train/0000.parquet")
    .groupby("horoscope")
    .agg(
        [
            pl.count(),
            pl.col("text").str.n_chars().mean().alias("avg_blog_length")
        ]
    )
    .sort("avg_blog_length", descending=True)
    .limit(5)
)
df = q.collect()
```
