# 创建自定义推理处理程序

> 原文：[`huggingface.co/docs/inference-endpoints/guides/custom_handler`](https://huggingface.co/docs/inference-endpoints/guides/custom_handler)

Hugging Face Endpoints 支持所有 Transformers 和 Sentence-Transformers 任务，并且可以支持自定义任务，包括自定义预处理和后处理。定制可以通过 Hugging Face Hub 上模型存储库中的[handler.py](https://huggingface.co/philschmid/distilbert-onnx-banking77/blob/main/handler.py)文件完成。

[handler.py](https://huggingface.co/philschmid/distilbert-onnx-banking77/blob/main/handler.py)需要实现[EndpointHandler](https://huggingface.co/philschmid/distilbert-onnx-banking77/blob/main/handler.py)类，其中包含`__init__`和`__call__`方法。

如果您想使用自定义依赖项，例如[optimum](https://raw.githubusercontent.com/huggingface/optimum)，则这些依赖项必须在`requirements.txt`中列出，如上述“添加自定义依赖项”中所述。

## 自定义处理程序示例

在[Hugging Face Hub](https://huggingface.co/models?other=endpoints-template)上已经有几个公共示例，您可以从中获取灵感或直接使用。这些存储库带有`endpoints-template`标签，可以在此[链接](https://huggingface.co/models?other=endpoints-template)下找到。

包含的示例有：

+   [Optimum 和 ONNX Runtime](https://huggingface.co/philschmid/distilbert-onnx-banking77)

+   [BLIP 图像嵌入](https://huggingface.co/florentgbelidji/blip_image_embeddings)

+   [TrOCR 用于 OCR 检测](https://huggingface.co/philschmid/trocr-base-printed)

+   [优化的句子转换器与 Optimum](https://huggingface.co/philschmid/all-MiniLM-L6-v2-optimum-embeddings)

+   [Pyannote 说话人分割](https://huggingface.co/philschmid/pyannote-speaker-diarization-endpoint)

+   [LayoutLM](https://huggingface.co/philschmid/layoutlm-funsd)

+   [Flair NER](https://huggingface.co/philschmid/flair-ner-english-ontonotes-large)

+   [GPT-J 6B 单 GPU](https://huggingface.co/philschmid/gpt-j-6B-fp16-sharded)

+   [Donut 文档理解](https://huggingface.co/philschmid/donut-base-finetuned-cord-v2)

+   [SetFit 分类器](https://huggingface.co/philschmid/setfit-ag-news-endpoint)

## 教程

在创建自定义处理程序之前，您需要一个带有模型权重的 Hugging Face 模型存储库，并且需要一个具有对存储库的*写*访问权限的访问令牌。要查找、创建和管理访问令牌，请单击[此处](https://huggingface.co/settings/tokens)。

如果您想为社区中现有模型编写自定义处理程序，可以使用[repo_duplicator](https://huggingface.co/spaces/osanseviero/repo_duplicator)创建存储库分支。

代码也可以在此[笔记本](https://colab.research.google.com/drive/1hANJeRa1PK1gZaUorobnQGu4bFj4_4Rf?usp=sharing)中找到。

您还可以在此处搜索已存在的自定义处理程序：[`huggingface.co/models?other=endpoints-template`](https://huggingface.co/models?other=endpoints-template)

### 1. 设置开发环境

开发自定义处理程序的最简单方法是设置本地开发环境，进行实现、测试和迭代，然后将其部署为推理端点。第一步是安装所有必需的开发依赖项。*用于创建自定义处理程序，不用于推理*

```py
# install git-lfs to interact with the repository
sudo apt-get update
sudo apt-get install git-lfs
# install transformers (not needed since it is installed by default in the container)
pip install transformers[sklearn,sentencepiece,audio,vision]
```

安装完库后，我们将克隆我们的存储库到开发环境中。

我们将在教程中使用[philschmid/distilbert-base-uncased-emotion](https://huggingface.co/philschmid/distilbert-base-uncased-emotion)。

```py
git lfs install
git clone https://huggingface.co/philschmid/distilbert-base-uncased-emotion
```

要能够稍后推送我们的 CP，您需要登录到我们的 HF 帐户。这可以通过使用`huggingface-cli`来完成。

*注意：确保配置 git config。*

```py
# setup cli with token
huggingface-cli login
git config --global credential.helper store
```

### 2. 创建 EndpointHandler（CP）

在设置环境后，我们可以开始创建您的自定义处理程序。自定义处理程序是存储库中的`handler.py`文件中的 Python 类（`EndpointHandler`）。`EndpointHandler`需要实现一个`__init__`和一个`__call__`方法。

+   `__init__`方法将在启动端点时调用，并接收 1 个参数，一个带有模型权重路径的字符串。这使您可以正确加载您的模型。

+   `__call__`方法将在每个请求上调用，并接收一个包含您的请求正文的字典作为 Python 字典。它将始终包含`inputs`键。

第一步是在存储库的本地克隆中创建我们的`handler.py`。

```py
!cd distilbert-base-uncased-emotion && touch handler.py
```

在其中，您定义您的`EndpointHandler`类与`__init__`和`__call__`方法。

```py
from typing import Dict, List, Any

class EndpointHandler():
    def __init__(self, path=""):
        # Preload all the elements you are going to need at inference.
        # pseudo:
        # self.model= load_model(path)

    def __call__(self, data: Dict[str, Any]) -> List[Dict[str, Any]]:
        """
       data args:
            inputs (:obj: `str` | `PIL.Image` | `np.array`)
            kwargs
      Return:
            A :obj:`list` | `dict`: will be serialized and returned
        """

        # pseudo
        # self.model(input)
```

### 3\. 自定义 EndpointHandler

现在，您可以将您想要在初始化或推理期间使用的所有自定义逻辑添加到您的 CP 中。如果需要一些灵感，您可以在 Hub 上找到多个[自定义处理程序](https://huggingface.co/models?other=endpoints-template)。在我们的示例中，我们将根据额外的负载信息添加自定义条件。

*我们在教程中使用的模型经过微调以检测情绪。我们将为日期添加一个额外的负载字段，并使用外部包来检查是否是假期，以添加一个条件，即当输入日期是假期时，模型返回“快乐” - 因为每个人在假期时都很快乐* 🌴🎉😆

首先，我们需要创建一个新的`requirements.txt`文件，并添加我们的[假期检测包](https://pypi.org/project/holidays/)，确保我们在开发环境中也已安装。

```py
!echo "holidays" >> requirements.txt
!pip install -r requirements.txt
```

接下来，我们必须调整我们的`handler.py`和`EndpointHandler`以匹配我们的条件。

```py
from typing import Dict, List, Any
from transformers import pipeline
import holidays

class EndpointHandler():
    def __init__(self, path=""):
        self.pipeline = pipeline("text-classification",model=path)
        self.holidays = holidays.US()

    def __call__(self, data: Dict[str, Any]) -> List[Dict[str, Any]]:
        """
       data args:
            inputs (:obj: `str`)
            date (:obj: `str`)
      Return:
            A :obj:`list` | `dict`: will be serialized and returned
        """
        # get inputs
        inputs = data.pop("inputs",data)
        date = data.pop("date", None)

        # check if date exists and if it is a holiday
        if date is not None and date in self.holidays:
          return [{"label": "happy", "score": 1}]

        # run normal prediction
        prediction = self.pipeline(inputs)
        return prediction
```

### 4\. 测试 EndpointHandler

为了测试我们的 EndpointHandler，我们可以简化导入，初始化和测试。因此，我们只需要准备一个示例负载。

```py
from handler import EndpointHandler

# init handler
my_handler = EndpointHandler(path=".")

# prepare sample payload
non_holiday_payload = {"inputs": "I am quite excited how this will turn out", "date": "2022-08-08"}
holiday_payload = {"inputs": "Today is a though day", "date": "2022-07-04"}

# test the handler
non_holiday_pred=my_handler(non_holiday_payload)
holiday_payload=my_handler(holiday_payload)

# show results
print("non_holiday_pred", non_holiday_pred)
print("holiday_payload", holiday_payload)

# non_holiday_pred [{'label': 'joy', 'score': 0.9985942244529724}]
# holiday_payload [{'label': 'happy', 'score': 1}]
```

成功了！！！🎉

*注意：如果您使用的是笔记本，在对`handler.py`进行更改时，可能需要重新启动内核，因为它不会自动重新导入。*

### 5\. 将自定义处理程序推送到您的存储库

在本地成功测试处理程序后，您可以通过简单使用基本的 git 命令将其推送到存储库。

```py
# add all our new files
!git add *
# commit our files
!git commit -m "add custom handler"
# push the files to the hub
!git push
```

现在，您应该在您的存储库的[“文件和版本”](https://huggingface.co/philschmid/distilbert-base-uncased-emotion/tree/main)选项卡中看到您的`handler.py`和`requirements.txt`。

### 6\. 将您的自定义处理程序部署为推理端点

最后一步是将您的自定义处理程序部署为推理端点。您可以像部署常规推理端点一样部署您的自定义处理程序。添加您的存储库，选择您的云和区域，您的实例和安全设置，然后部署。

在创建端点时，推理端点服务将检查是否有可用且有效的`handler.py`，并将用于为请求提供服务，无论您选择哪个“任务”。

*注意：在您的[推理端点仪表板](https://ui.endpoints.huggingface.co/)中，此端点的任务现在应设置为自定义*
