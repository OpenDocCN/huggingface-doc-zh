# 数据集下载统计信息

> 原文链接：[https://huggingface.co/docs/hub/datasets-download-stats](https://huggingface.co/docs/hub/datasets-download-stats)

## 数据集的下载统计信息是如何生成的？

Hub提供了所有可以通过`datasets`库加载的数据集的下载统计信息。为了确定下载次数，Hub会计算每次在Python中调用`load_dataset`的次数，但不包括在GitHub上使用Hugging Face的CI工具。用户不会发送任何信息，也不会为此进行额外的调用。计数是在服务器端完成的，因为我们为下载提供文件。这意味着：

+   无论数据是直接存储在Hub仓库上，还是仓库具有[脚本](https://huggingface.co/docs/datasets/dataset_script)从外部来源加载数据，下载次数都是相同的。

+   如果用户使用诸如`wget`或Hub的用户界面（UI）等工具手动下载数据，则这些下载不会计入下载次数。
