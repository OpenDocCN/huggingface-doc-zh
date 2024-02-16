# AutoTrain

> 原始文本：[`huggingface.co/docs/autotrain/index`](https://huggingface.co/docs/autotrain/index)

🤗 AutoTrain 是一个用于训练最先进模型的无代码工具，用于自然语言处理（NLP）任务，计算机视觉（CV）任务，语音任务甚至表格任务。它是建立在 Hugging Face 团队开发的出色工具之上，并且设计为易于使用。

## 谁应该使用 AutoTrain？

AutoTrain 适用于任何想要为 NLP、CV、语音或表格任务训练最先进模型的人，但不想花时间在训练模型的技术细节上。AutoTrain 也适用于任何想要为自定义数据集训练模型的人，但不想花时间在训练模型的技术细节上。我们的目标是让任何人都能轻松训练任何任务的最先进模型，我们的重点不仅是数据科学家或机器学习工程师，还包括非技术用户。

## 如何使用 AutoTrain？

我们提供几种使用 AutoTrain 的方式：

+   非编码用户可以通过使用 AutoTrain Docker 镜像创建一个新空间来使用`AutoTrain Advanced`：[`huggingface.co/new-space?template=autotrain-projects/autotrain-advanced`](https://huggingface.co/new-space?template=autotrain-projects/autotrain-advanced)。请确保将空间保持私密，并将适当的硬件附加到空间上。

+   开发人员可以通过使用 Python API 访问和构建 AutoTrain，或在本地运行 AutoTrain Advanced UI。Python API 可在`autotrain-advanced`软件包中使用。您可以使用 pip 安装它：

```py
pip install autotrain-advanced
```

## 什么是 AutoTrain Advanced？

AutoTrain Advanced 可以在 Hugging Face 空间中或本地（如果使用 pip 本地安装）处理您的数据。这样可以节省时间，因为数据处理不是由 AutoTrain 后端完成，导致您的作业不会排队。AutoTrain Advanced 还允许您使用自己的硬件（更好的 CPU 和 RAM）来处理数据，从而使数据处理更快。

使用 AutoTrain Advanced，高级用户还可以控制每个作业训练所使用的超参数。这使您可以使用不同超参数训练多个模型并比较结果。

其他一切与 AutoTrain 相同。您可以使用 AutoTrain Advanced 来训练 NLP、CV、语音和表格任务的模型。

我们建议使用[AutoTrain Advanced](https://huggingface.co/new-space?template=autotrain-projects/autotrain-advanced)，因为它更快、更灵活，并且将来会有更多支持的任务和功能。
