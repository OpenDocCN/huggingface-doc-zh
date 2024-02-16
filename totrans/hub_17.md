# 下载模型

> 原始文本：[https://huggingface.co/docs/hub/models-downloading](https://huggingface.co/docs/hub/models-downloading)

## 集成库

如果Hub上的模型与[支持的库](./models-libraries)相关联，只需几行代码即可加载模型。有关访问模型的信息，您可以单击模型页面上的“在*Library*中使用”按钮，查看如何操作。例如，`distilgpt2`显示了如何在🤗 Transformers中执行此操作。

![](../Images/d46b2aaf10a61547f1ebc123ee09e589.png) ![](../Images/6974e1562f32adf29af261d699a786d1.png)![](../Images/bb722a594f057ca88284214c3dcb4d4c.png) ![](../Images/0fc3bb568df5ae4b856f645d9cc40a8e.png)

## 使用Hugging Face客户端库

您可以使用[`huggingface_hub`](https://github.com/huggingface/huggingface_hub)库来创建、删除、更新和检索存储库中的信息。您还可以从存储库中下载文件或将其集成到您的库中！例如，您可以使用几行代码快速加载一个Scikit-learn模型。

```py
from huggingface_hub import hf_hub_download
import joblib

REPO_ID = "YOUR_REPO_ID"
FILENAME = "sklearn_model.joblib"

model = joblib.load(
    hf_hub_download(repo_id=REPO_ID, filename=FILENAME)
)
```

## 使用Git

由于模型Hub上的所有模型都是Git存储库，您可以通过运行以下命令在本地克隆模型：

```py
git lfs install
git clone git@hf.co:<MODEL ID> # example: git clone git@hf.co:bigscience/bloom
```

如果您对特定模型存储库具有写入权限，您还可以提交和推送对模型的修订。

将您的SSH公钥添加到[您的用户设置](https://huggingface.co/settings/keys)中，以推送更改和/或访问私有存储库。
