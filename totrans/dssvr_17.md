# 概述

> 原始文本：[https://huggingface.co/docs/datasets-server/parquet_process](https://huggingface.co/docs/datasets-server/parquet_process)

Datasets Server会自动将大小小于5GB的公共数据集转换并发布到Hub上作为Parquet文件。如果数据集已经是Parquet格式，它将按原样发布。[Parquet](https://parquet.apache.org/docs/)文件是基于列的，当处理大数据时非常出色。

对于私有数据集，如果存储库由[PRO用户](https://huggingface.co/pricing)或[企业Hub组织](https://huggingface.co/enterprise)拥有，则提供该功能。

有几种不同的库可以用来处理已发布的Parquet文件：

+   [ClickHouse](https://clickhouse.com/docs/en/intro), 一个面向列的数据库管理系统，用于在线分析处理

+   [DuckDB](https://duckdb.org/docs/), 一个用于分析查询的高性能SQL数据库

+   [Pandas](https://pandas.pydata.org/docs/index.html), 一个用于处理数据结构的数据分析工具

+   [Polars](https://pola-rs.github.io/polars-book/user-guide/), 一个基于Rust的DataFrame库
