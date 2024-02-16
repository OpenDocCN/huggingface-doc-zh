# LLM 微调

> 原始文本：[`huggingface.co/docs/autotrain/llm_finetuning`](https://huggingface.co/docs/autotrain/llm_finetuning)

通过 AutoTrain，您可以轻松地在自己的数据上微调大型语言模型（LLMs）！

AutoTrain 支持以下类型的 LLM 微调：

+   因果语言建模（CLM）

+   掩蔽语言建模（MLM）[即将推出]

## 数据准备

LLM 微调接受 CSV 格式的数据。

### SFT / 通用训练器的数据格式

对于 SFT / 通用训练器，数据应该采用以下格式：

| 文本 |
| --- |
| 人类：你好 \n 机器人：嗨很高兴见到你 |
| 人类：你好 \n 机器人：我很好 |
| 人类：你叫什么名字？ \n 机器人：我叫玛丽 |
| 人类：哪种是最好的编程语言？ \n 机器人：Python |

可以在这里找到符合此格式的示例数据集：[`huggingface.co/datasets/timdettmers/openassistant-guanaco`](https://huggingface.co/datasets/timdettmers/openassistant-guanaco)

对于 SFT / 通用训练，您的数据集必须有一个`text`列

### 奖励训练器的数据格式

对于奖励训练器，数据应该采用以下格式：

| 文本 | 被拒绝的文本 |
| --- | --- |
| 人类：你好 \n 机器人：嗨很高兴见到你 | 人类：你好 \n 机器人：别烦我 |
| 人类：你好 \n 机器人：我很好 | 人类：你好 \n 机器人：我不好 |
| 人类：你叫什么名字？ \n 机器人：我叫玛丽 | 人类：你叫什么名字？ \n 机器人：你关我什么事？ |
| 人类：哪种是最好的编程语言？ \n 机器人：Python | 人类：哪种是最好的编程语言？ \n 机器人：Javascript |

对于奖励训练器，您的数据集必须有一个`text`列（也称为选择的文本）和一个`rejected_text`列。

### DPO 训练器的数据格式

对于 DPO 训练器，数据应该采用以下格式：

| 提示 | 文本 | 被拒绝的文本 |
| --- | --- | --- |
| 你好 | 嗨很高兴见到你 | 别烦我 |
| 你好吗 | 我很好 | 我不好 |
| 你叫什么名字？ | 我叫玛丽 | 你关我什么事？ |
| 你叫什么名字？ | 我叫玛丽 | 我没有名字 |
| 哪种是最好的编程语言？ | Python | Javascript |
| 哪种是最好的编程语言？ | Python | C++ |
| 哪种是最好的编程语言？ | Java | C++ |

对于 DPO 训练器，您的数据集必须有一个`prompt`列，一个`text`列（也称为选择的文本）和一个`rejected_text`列。
