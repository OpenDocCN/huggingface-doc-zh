# 在 Hub 上分析数据集

> 原文链接：[`huggingface.co/docs/datasets-server/analyze_data`](https://huggingface.co/docs/datasets-server/analyze_data)

在快速入门中，您已经了解了与 Hub 上的数据集交互的各种端点。其中最有用的之一是`/parquet`端点，它允许您获取存储在 Hub 上的数据集并对其进行分析。这是探索数据集的好方法，可以更好地了解其内容。

为了演示，本指南将向您展示如何从 Hub 检索数据集并使用 Pandas 库进行基本数据分析的端到端示例。

## 获取数据集

[Hub](https://huggingface.co/datasets)拥有超过 40,000 个数据集，涵盖各种任务、大小和语言。在此示例中，您将使用[`codeparrot/codecomplex`](https://huggingface.co/datasets/codeparrot/codecomplex)数据集，但请随意探索并找到另一个您感兴趣的数据集！该数据集包含来自编程竞赛的 Java 代码，并由一组算法专家标记了代码的时间复杂度。

假设您对提交的代码的平均长度与时间复杂度有兴趣。以下是如何开始的。

使用`/parquet`端点将数据集转换为 Parquet 文件并返回其 URL：

```py
import requests
API_URL = "https://datasets-server.huggingface.co/parquet?dataset=codeparrot/codecomplex"
def query():
    response = requests.get(API_URL)
    return response.json()
data = query()
print(data)
{'parquet_files': 
    [
        {'dataset': 'codeparrot/codecomplex', 'config': 'default', 'split': 'train', 'url': 'https://huggingface.co/datasets/codeparrot/codecomplex/resolve/refs%2Fconvert%2Fparquet/default/train/0000.parquet', 'filename': '0000.parquet', 'size': 4115908}
    ], 
 'pending': [], 'failed': [], 'partial: false
}
```

## 使用 Pandas 读取数据集

有了 URL，您可以将 Parquet 文件读入 Pandas DataFrame 中：

```py
import pandas as pd

url = "https://huggingface.co/datasets/codeparrot/codecomplex/resolve/refs%2Fconvert%2Fparquet/default/train/0000.parquet"
df = pd.read_parquet(url)
df.head(5)
```

| src | 复杂度 | 问题 | 来自 |
| --: | --: | --: | --: |
| import java.io.*;\nimport java.math.BigInteger… | 二次 | 1179_B. Tolik 和他的叔叔 | CODEFORCES |
| import java.util.Scanner;\n \npublic class pil… | 线性 | 1197_B. 柱子 | CODEFORCES |
| import java.io.BufferedReader;\nimport java.io… | 线性 | 1059_C. 序列转换 | CODEFORCES |
| import java.util.*;\n\nimport java.io.*;\npubl… | 线性 | 1011_A. 阶段 | CODEFORCES |
| import java.io.OutputStream;\nimport java.io.I… | 线性 | 1190_C. Tokitsukaze 和决斗 | CODEFORCES |

## 通过时间复杂度计算平均代码长度

Pandas 是用于数据分析的强大库；按时间复杂度对数据集进行分组，应用函数计算代码片段的平均长度，并绘制结果：

```py
df.groupby('complexity')['src'].apply(lambda x: x.str.len().mean()).sort_values(ascending=False).plot.barh(color="orange")
```

![](img/7de93a91b1115e077942c544945b223d.png)
