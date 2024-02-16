# 竞赛仓库

> 原始文本：[`huggingface.co/docs/competitions/competition_repo`](https://huggingface.co/docs/competitions/competition_repo)

NOTE: 竞赛仓库必须始终保持私密。不要公开！

竞赛仓库包括以下文件：

```py
├── COMPETITION_DESC.md
├── conf.json
├── DATASET_DESC.md
├── solution.csv
├── SUBMISSION_DESC.md
├── submission_info
│   └── *.json
├── submissions
│   └── *.csv
├── teams.json
└── user_team.json
```

### COMPETITION_DESC.md

这个文件包含了竞赛的描述。这是一个 markdown 文件。您可以使用 markdown 语法格式化文本，并根据您的需求修改文件。竞赛描述显示在竞赛的首页上。

### DATASET_DESC.md

这个文件包含了数据集的描述。这又是一个 markdown 文件。这个文件用于描述数据集，并显示在数据集页面上。在这个文件中，您可以提及数据集中存在哪些列，每列的含义是什么，数据集的格式是什么等等。

### conf.json

conf.json 是竞赛的配置文件。下面显示了一个示例 conf.json：

```py
{
   "COMPETITION_TYPE":"generic",
   "SUBMISSION_LIMIT":5,
   "TIME_LIMIT": 10,
   "HARDWARE":"cpu-basic",
   "SELECTION_LIMIT":10,
   "END_DATE":"2024-05-25",
   "EVAL_HIGHER_IS_BETTER":1,
   "SUBMISSION_ID_COLUMN":"id",
   "SUBMISSION_COLUMNS":"id,pred",
   "SUBMISSION_ROWS":10000,
   "EVAL_METRIC":"roc_auc_score",
   "LOGO":"https://github.com/abhishekkrthakur/public_images/blob/main/song.png?raw=true",
   "DATASET": "",
   "SUBMISSION_FILENAMES": ["submission.csv"]
   "SCORING_METRIC": "roc_auc_score"
}
```

这个文件是在创建新竞赛时创建的。您可以根据需要修改此文件。但是，我们不建议在竞赛开始后更改评估指标字段，因为这将需要重新评估所有提交。

+   COMPETITION_TYPE: 该字段用于指定竞赛的类型。目前，我们支持两种类型的竞赛：“通用”和`script`。

    +   “通用”竞赛是一种用户可以提交 csv 文件（或其他格式）并使用指标进行评估的竞赛。

    +   `script`竞赛是一种用户可以提交包含 script.py 的 huggingface 模型仓库的竞赛。script.py 被运行以生成 submission.csv，然后使用指标进行评估。

+   SUBMISSION_LIMIT: 该字段用于指定用户每天可以提交的次数。

+   TIME_LIMIT: 该字段用于指定每个提交的时间限制（以秒为单位）。（仅用于`script`竞赛）

+   HARDWARE: 该字段用于指定用于评估提交的硬件。

+   SELECTION_LIMIT: 该字段用于指定将被选入排行榜的提交数量。（仅用于`script`竞赛）

+   END_DATE: 该字段用于指定竞赛的结束日期。竞赛将在结束日期自动关闭。私密排行榜将在结束日期上提供。

+   EVAL_HIGHER_IS_BETTER: 该字段用于指定评估指标是越高越好还是越低越好。如果值为 1，则越高越好。如果值为 0，则越低越好。

+   SUBMISSION_ID_COLUMN: 该字段用于指定提交文件中 id 列的名称。

+   SUBMISSION_COLUMNS: 该字段用于指定提交文件中列的名称。名称必须用逗号分隔，不能有空格。

+   SUBMISSION_ROWS: 该字段用于指定提交文件中不包括标题的行数。

+   EVAL_METRIC: 该字段用于指定评估指标。我们支持所有 scikit-learn 指标甚至自定义指标。

+   LOGO: 该字段用于指定竞赛的 logo。logo 必须是一个 png 文件。logo 显示在竞赛的所有页面上。

+   DATASET: 该字段用于指定竞赛中使用的私密数据集。数据集仅在脚本运行期间对用户可用。仅用于脚本竞赛。

+   SUBMISSION_FILENAMES: 该字段用于指定提交文件的名称。仅用于具有自定义指标的脚本竞赛，对于通用竞赛不得更改。

+   SCORING_METRIC: 当使用自定义指标/多个指标时，该字段用于指定用于评分提交的指标名称。

### solution.csv

这个文件包含了竞赛的解决方案。这是一个 csv 文件。下面显示了一个示例：

```py
id,pred,split
0,1,public
1,0,private
2,0,private
3,1,private
4,0,public
5,1,private
6,1,public
7,1,private
8,0,public
9,0,private
10,0,private
11,0,private
12,1,private
13,0,private
14,1,public
```

解决方案文件用于评估提交。解决方案文件必须始终具有 id 列和 split 列。split 列用于将解决方案分为公共部分和私有部分。split 列可以有两个值：`public`和`private`。您可以在解决方案文件中有多个列。但是，评估指标必须支持多个列。

例如，如果评估指标是`roc_auc_score`，则解决方案文件必须有两列：`id`和`pred`。id 和 pred 列的名称可以是任意的。名称将从`conf.json`文件中获取。请确保在`conf.json`文件中有适当的列名，并且在解决方案文件中同时具有公共和私有拆分。

### SUBMISSION_DESC.md

这个文件包含提交的描述。这是一个 markdown 文件。您可以使用 markdown 语法来格式化文本并根据需要修改文件。提交描述显示在提交页面上。

在这里，您可以提及提交文件的格式，提交文件中需要哪些列等。

对于上面显示的示例解决方案文件，提交文件必须有两列：`id`和`pred`。示例 sample_submission.csv 如下所示：

```py
id,pred
0,0.6
1,0.1
2,0.5
3,1.6
4,0.8
5,1
6,1
7,1
8,0
9,0
10,0.1
11,0.4
12,1.9
13,0.01
14,1.1
```

当用户提交提交文件时，系统将检查提交文件是否具有所需的列。如果提交文件没有所需的列，则提交将被拒绝。

组织者有责任确保他们提供正确格式的样本提交文件和提交描述文件。

### 提交信息

这个文件夹包含提交信息文件。每个提交信息文件包含有关提交的信息。当第一次提交时，将创建此文件夹。提交信息文件是 json 文件。

### 提交

这个文件夹包含用户提交的提交。每个提交是一个 csv 文件（或其他格式）。当第一次提交时，将创建此文件夹。

### 其他文件

其他文件 teams.json 和 user_team.json 用于存储有关团队的信息。
