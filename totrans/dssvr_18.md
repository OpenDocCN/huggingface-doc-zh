# ClickHouse

> 原始文本：[`huggingface.co/docs/datasets-server/clickhouse`](https://huggingface.co/docs/datasets-server/clickhouse)

[ClickHouse](https://clickhouse.com/docs/en/intro)是一个用于分析工作负载的快速高效的列式数据库，使用 SQL 轻松分析 Hub 托管的数据集。要快速开始，请使用[`clickhouse-local`](https://clickhouse.com/docs/en/operations/utilities/clickhouse-local)在命令行中运行 SQL 查询，避免完全安装 ClickHouse 的需要。

查看这篇[博客](https://clickhouse.com/blog/query-analyze-hugging-face-datasets-with-clickhouse)了解如何使用 ClickHouse 分析 Hub 上的数据集的更多细节。

首先，下载并安装`clickhouse-local`：

```py
curl https://clickhouse.com/ | sh
```

在这个例子中，您将分析包含有关 Spotify 歌曲信息的[maharshipandya/spotify-tracks-dataset](https://huggingface.co/datasets/maharshipandya/spotify-tracks-dataset)。Hub 上的数据集存储为 Parquet 文件，您可以使用`/parquet`端点访问它：

```py
import requests

r = requests.get("https://datasets-server.huggingface.co/parquet?dataset=maharshipandya/spotify-tracks-dataset")
j = r.json()
url = [f['url'] for f in j['parquet_files']]
url
['https://huggingface.co/datasets/maharshipandya/spotify-tracks-dataset/resolve/refs%2Fconvert%2Fparquet/default/train/0000.parquet']
```

## 聚合函数

现在您可以开始分析数据集了。使用`-q`参数指定要执行的查询，并使用[`url`](https://clickhouse.com/docs/en/sql-reference/table-functions/url)函数从 Parquet 文件中的数据创建表。

您应该将`enable_url_encoding`设置为 0，以确保 URL 中的转义字符按预期保留，并将`max_https_get_redirects`设置为 1，以便重定向到 Parquet 文件的路径。

让我们从识别最受欢迎的艺术家开始：

```py
./clickhouse local -q "
    SELECT count() AS c, artists 
    FROM url('https://huggingface.co/datasets/maharshipandya/spotify-tracks-dataset/resolve/refs%2Fconvert%2Fparquet/default/train/0000.parquet') 
    GROUP BY artists 
    ORDER BY c 
    DESC LIMIT 5
    SETTINGS enable_url_encoding=0, max_http_get_redirects=1"

┌───c─┬─artists─────────┐
│ 279 │ The Beatles 	│
│ 271 │ George Jones	│
│ 236 │ Stevie Wonder   │
│ 224 │ Linkin Park 	│
│ 222 │ Ella Fitzgerald │
└─────┴─────────────────┘
```

ClickHouse 还提供了用于可视化查询的函数。例如，您可以使用[`bar`](https://clickhouse.com/docs/en/sql-reference/functions/other-functions#bar)函数创建歌曲舞蹈性的柱状图：

```py
./clickhouse local -q "
    SELECT
        round(danceability, 1) AS danceability,
        bar(count(), 0, max(count()) OVER ()) AS dist
    FROM url('https://huggingface.co/datasets/maharshipandya/spotify-tracks-dataset/resolve/refs%2Fconvert%2Fparquet/default/train/0000.parquet')
    GROUP BY danceability
    ORDER BY danceability ASC
    SETTINGS enable_url_encoding=0, max_http_get_redirects=1"

┌─danceability─┬─dist─────────────────────────────────────────────────────────────────────────────────┐
│            0 │ ▍                                                                            	      │
│      	   0.1 │ ████▎                                                                        	      │
│      	   0.2 │ █████████████▍                                                               	      │
│      	   0.3 │ ████████████████████████                                                     	      │
│      	   0.4 │ ████████████████████████████████████████████▋                                	      │
│      	   0.5 │ ████████████████████████████████████████████████████████████████████▊        	      │
│      	   0.6 │ ████████████████████████████████████████████████████████████████████████████████     │
│      	   0.7 │ ██████████████████████████████████████████████████████████████████████       	      │
│      	   0.8 │ ██████████████████████████████████████████                                   	      │
│      	   0.9 │ ██████████▋                                                                  	      │
│            1 │ ▌                                                                            	      │
└──────────────┴──────────────────────────────────────────────────────────────────────────────────────┘
```

为了更深入地了解数据集，ClickHouse 提供了用于确定数据相关性、计算统计假设检验等的统计分析函数。查看 ClickHouse 的[聚合函数列表](https://clickhouse.com/docs/en/sql-reference/aggregate-functions/reference)获取可用聚合函数的完整列表。

## 用户定义函数（UDFs）

用户定义函数（UDF）允许您重用自定义逻辑。许多 Hub 数据集通常被分片成多个 Parquet 文件，因此创建一个 UDF 来列出并查询给定数据集的所有 Parquet 文件可能更容易和更高效，只需使用数据集名称即可。

在这个例子中，您需要在控制台模式下运行`clickhouse-local`，这样 UDF 在查询之间可以持久存在：

```py
./clickhouse local
```

记得将`enable_url_encoding`设置为 0，将`max_https_get_redirects`设置为 1，以便重定向到 Parquet 文件的路径：

```py
SET max_http_get_redirects = 1, enable_url_encoding = 0
```

让我们创建一个函数，返回[`blog_authorship_corpus`](https://huggingface.co/datasets/blog_authorship_corpus)中的 Parquet 文件列表：

```py
CREATE OR REPLACE FUNCTION hugging_paths AS dataset -> (
    SELECT arrayMap(x -> (x.1), JSONExtract(json, 'parquet_files', 'Array(Tuple(url String))'))
    FROM url('https://datasets-server.huggingface.co/parquet?dataset=' || dataset, 'JSONAsString')
);

SELECT hugging_paths('blog_authorship_corpus') AS paths

['https://huggingface.co/datasets/blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/train/0000.parquet','https://huggingface.co/datasets/blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/train/0001.parquet','https://huggingface.co/datasets/blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/validation/0000.parquet']
```

您可以通过创建另一个调用`hugging_paths`并基于数据集名称输出所有文件的函数来使这更加简单：

```py
CREATE OR REPLACE FUNCTION hf AS dataset -> (
    WITH hugging_paths(dataset) as urls
    SELECT multiIf(length(urls) = 0, '', length(urls) = 1, urls[1], 'https://huggingface.co/datasets/{' || arrayStringConcat(arrayMap(x -> replaceRegexpOne(replaceOne(x, 'https://huggingface.co/datasets/', ''), '\\.parquet$', ''), urls), ',') || '}.parquet')
);

SELECT hf('blog_authorship_corpus') AS pattern

['https://huggingface.co/datasets/{blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/blog_authorship_corpus-train-00000-of-00002,blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/blog_authorship_corpus-train-00001-of-00002,blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/blog_authorship_corpus-validation}.parquet']
```

现在使用`hf`函数通过传递数据集名称来查询任何数据集：

```py
SELECT horoscope, count(*), AVG(LENGTH(text)) AS avg_blog_length 
FROM url(hf('blog_authorship_corpus')) 
GROUP BY horoscope 
ORDER BY avg_blog_length 
DESC LIMIT(5) 

┌─────────────┬───────┬────────────────────┐
│  Aquarius   │ 51747 │ 1132.487873693161  │
├─────────────┼───────┼────────────────────┤
│ Cancer      │ 66944 │  1111.613109464627 │
│ Libra       │ 63994 │ 1060.3968184517298 │
│ Sagittarius │ 52753 │ 1055.7120732470191 │
│ Capricorn   │ 52207 │ 1055.4147719654452 │
└─────────────┴───────┴────────────────────┘
```
