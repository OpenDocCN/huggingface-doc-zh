# DuckDB

> 原文链接：[https://huggingface.co/docs/datasets-server/duckdb](https://huggingface.co/docs/datasets-server/duckdb)

[DuckDB](https://duckdb.org/docs/) 是一个支持快速读取和查询 Parquet 文件的数据库。首先创建一个连接到 DuckDB，然后安装并加载 [`httpfs`](https://duckdb.org/docs/extensions/httpfs.html) 扩展来读取和写入远程文件：

PythonJavaScript

```py
import duckdb

url = "https://huggingface.co/datasets/blog_authorship_corpus/resolve/refs%2Fconvert%2Fparquet/blog_authorship_corpus/train/0000.parquet"

con = duckdb.connect()
con.execute("INSTALL httpfs;")
con.execute("LOAD httpfs;")
```

现在你可以在 Parquet 文件上编写和执行你的 SQL 查询：

PythonJavaScript

```py
con.sql(f"SELECT horoscope, count(*), AVG(LENGTH(text)) AS avg_blog_length FROM '{url}' GROUP BY horoscope ORDER BY avg_blog_length DESC LIMIT(5)")
┌───────────┬──────────────┬────────────────────┐
│ horoscope │ count_star() │  avg_blog_length   │
│  varchar  │    int64     │       double       │
├───────────┼──────────────┼────────────────────┤
│ Aquarius  │        34062 │  1129.218836239798 │
│ Cancer    │        41509 │  1098.366812016671 │
│ Capricorn │        33961 │ 1073.2002002296751 │
│ Libra     │        40302 │ 1072.0718326633914 │
│ Leo       │        40587 │ 1064.0536871412028 │
└───────────┴──────────────┴────────────────────┘
```

要查询多个文件 - 例如，如果数据集是分片的：

PythonJavaScript

```py
con.sql(f"SELECT horoscope, count(*), AVG(LENGTH(text)) AS avg_blog_length FROM read_parquet({urls[:2]}) GROUP BY horoscope ORDER BY avg_blog_length DESC LIMIT(5)")
┌─────────────┬──────────────┬────────────────────┐
│  horoscope  │ count_star() │  avg_blog_length   │
│   varchar   │    int64     │       double       │
├─────────────┼──────────────┼────────────────────┤
│ Aquarius    │        49568 │ 1125.8306770497095 │
│ Cancer      │        63512 │   1097.95608703867 │
│ Libra       │        60304 │ 1060.6110539931017 │
│ Capricorn   │        49402 │ 1059.5552609206104 │
│ Sagittarius │        50431 │ 1057.4589835616982 │
└─────────────┴──────────────┴────────────────────┘
```

[DuckDB-Wasm](https://duckdb.org/docs/api/wasm)，一个由 [WebAssembly](https://webassembly.org/) 提供支持的包，也可用于在任何浏览器中运行 DuckDB。这可能会很有用，例如，如果你想要创建一个 Web 应用程序来从浏览器查询 Parquet 文件！
