# 自定义指标

> 原文链接：[https://huggingface.co/docs/competitions/custom_metric](https://huggingface.co/docs/competitions/custom_metric)

如果你不满足于默认的scikit-learn指标，你可以定义自己的指标。

在这里，我们期望组织者了解python。

### 如何定义自定义指标

要定义一个自定义指标，需要将`conf.json`中的`EVAL_METRIC`更改为`custom`。你还必须确保`EVAL_HIGHER_IS_BETTER`设置为`1`或`0`，取决于指标的较高值是否更好。

第二步是在私有比赛存储库中创建一个名为`metric.py`的文件。该文件应包含一个以竞赛参数为输入的`compute`函数。

这是我们检查指标是否为自定义并计算指标值的部分：

```py
def compute_metrics(params):
    if params.metric == "custom":
        metric_file = hf_hub_download(
            repo_id=params.competition_id,
            filename="metric.py",
            token=params.token,
            repo_type="dataset",
        )
        sys.path.append(os.path.dirname(metric_file))
        metric = importlib.import_module("metric")
        evaluation = metric.compute(params)
    .
    .
    .
```

你可以在比赛的github存储库`compute_metrics.py`中找到上述部分。

`params`被定义为：

```py
class EvalParams(BaseModel):
    competition_id: str
    competition_type: str
    metric: str
    token: str
    team_id: str
    submission_id: str
    submission_id_col: str
    submission_cols: List[str]
    submission_rows: int
    output_path: str
    submission_repo: str
    time_limit: int
    dataset: str  # private test dataset, used only for script competitions
```

在`compute`函数中，你可以自由地做任何你想做的事情。最终它必须返回一个带有以下键的字典：

```py
{
    "public_score": {
        "metric1": metric_value,
    },,
    "private_score": {
        "metric1": metric_value,
    },,
}
```

公共和私有分数必须是字典！你也可以使用多个指标。多个指标的示例：

```py
{
    "public_score": {
        "metric1": metric_value,
        "metric2": metric_value,
    },
    "private_score": {
        "metric1": metric_value,
        "metric2": metric_value,
    },
}
```

注意：当使用多个指标时，`conf.json`必须指定`SCORING_METRIC`以对比赛中的参与者进行排名。

例如，如果我想使用`metric2`来对参与者进行排名，我会在`conf.json`中将`SCORING_METRIC`设置为`metric2`。

### 自定义指标的示例

```py
import pandas as pd
from huggingface_hub import hf_hub_download

def compute(params):
    solution_file = hf_hub_download(
        repo_id=params.competition_id,
        filename="solution.csv",
        token=params.token,
        repo_type="dataset",
    )

    solution_df = pd.read_csv(solution_file)

    submission_filename = f"submissions/{params.team_id}-{params.submission_id}.csv"
    submission_file = hf_hub_download(
        repo_id=params.competition_id,
        filename=submission_filename,
        token=params.token,
        repo_type="dataset",
    )
    submission_df = pd.read_csv(submission_file)

    public_ids = solution_df[solution_df.split == "public"][params.submission_id_col].values
    private_ids = solution_df[solution_df.split == "private"][params.submission_id_col].values

    public_solution_df = solution_df[solution_df[params.submission_id_col].isin(public_ids)]
    public_submission_df = submission_df[submission_df[params.submission_id_col].isin(public_ids)]

    private_solution_df = solution_df[solution_df[params.submission_id_col].isin(private_ids)]
    private_submission_df = submission_df[submission_df[params.submission_id_col].isin(private_ids)]

    public_solution_df = public_solution_df.sort_values(params.submission_id_col).reset_index(drop=True)
    public_submission_df = public_submission_df.sort_values(params.submission_id_col).reset_index(drop=True)

    private_solution_df = private_solution_df.sort_values(params.submission_id_col).reset_index(drop=True)
    private_submission_df = private_submission_df.sort_values(params.submission_id_col).reset_index(drop=True)

    # CALCULATE METRICS HERE.......
    # _metric = SOME METRIC FUNCTION
    target_cols = [col for col in solution_df.columns if col not in [params.submission_id_col, "split"]]
    public_score = _metric(public_solution_df[target_cols], public_submission_df[target_cols])
    private_score = _metric(private_solution_df[target_cols], private_submission_df[target_cols])

    evaluation = {
        "public_score": {
            "metric1": public_score,
        },
        "private_score": {
            "metric1": public_score,
        }
    }
    return evaluation
```

仔细看上面的代码。你会看到我们正在从数据集存储库下载解决方案文件和提交文件。然后我们在解决方案和提交文件的公共和私有部分上计算指标。最后，我们将指标值以字典形式返回。
