# Polars

> 原文链接：[`huggingface.co/docs/datasets-server/polars`](https://huggingface.co/docs/datasets-server/polars)

[Polars](https://pola-rs.github.io/polars-book/user-guide/)是一个快速的 DataFrame 库，用 Rust 编写，以 Arrow 为基础。

💡 了解如何在 List Parquet files 指南中获取数据集 URL 的更多信息。

让我们从数据集服务器中获取[`blog_authorship_corpus`](https://huggingface.co/datasets/blog_authorship_corpus)数据集的`train`拆分的 URL：

```py
r = requests.get("https://datasets-server.huggingface.co/parquet?dataset=blog_authorship_corpus")
j = r.json()
urls = [f['url'] for f in j['parquet_files'] if f['split'] == 'train']
urls
['https://huggingface.co/datasets/blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/train/0000.parquet',
 'https://huggingface.co/datasets/blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/train/0001.parquet']
```

要从单个 Parquet 文件中读取，请使用[`read_parquet`](https://pola-rs.github.io/polars/py-polars/html/reference/api/polars.read_parquet.html)函数将其读入 DataFrame，然后执行您的查询：

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

要读取多个 Parquet 文件 - 例如，如果数据集被分片 - 您需要使用[`concat`](https://pola-rs.github.io/polars/py-polars/html/reference/api/polars.concat.html)函数将文件连接成单个 DataFrame：

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

## 懒惰的 API

Polars 提供了一个[懒惰的 API](https://pola-rs.github.io/polars-book/user-guide/lazy/using/)，对于大型 Parquet 文件来说更高效和节省内存。LazyFrame API 会跟踪您想要做的事情，只有在您准备好时才会执行整个查询。这样，懒惰的 API 不会预先将所有内容加载到 RAM 中，它允许您处理比可用 RAM 更大的数据集。

要惰性地读取 Parquet 文件，请改用[`scan_parquet`](https://pola-rs.github.io/polars/py-polars/html/reference/api/polars.scan_parquet.html)函数。然后，使用[`collect`](https://pola-rs.github.io/polars/py-polars/html/reference/lazyframe/api/polars.LazyFrame.collect.html)函数执行整个查询：

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
