# 数据文件配置

> 原始文本：[`huggingface.co/docs/hub/datasets-data-files-configuration`](https://huggingface.co/docs/hub/datasets-data-files-configuration)

关于如何构建数据集存储库没有任何限制。

但是，如果您希望数据集查看器显示特定的数据文件，或者将数据集分为训练/验证/测试集，您需要相应地构建数据集。通常只需根据它们的拆分名称命名数据文件，例如`train.csv`和`test.csv`。

## 文件名和拆分

要通过根据其拆分名称命名数据文件或目录来构建数据集，请参阅文件名和拆分文档。

## 手动配置

您可以使用 YAML 选择要在数据集查看器中显示的数据文件。如果您想要手动指定哪个文件放入哪个拆分，这将非常有用。

您还可以为数据集定义多个配置（或子集），并传递数据集构建参数（例如用于 CSV 文件的分隔符）。

有关更多信息，请参阅手动配置上的文档。

## 图像和音频数据集

对于图像和音频分类数据集，您还可以使用目录来命名图像和音频类别。如果您的图像/音频文件具有元数据（例如标题、边界框、转录等），您可以在它们旁边放置元数据文件。

我们提供了两个您可以查看的指南：

+   如何创建图像数据集

+   [如何创建音频数据集](https://huggingface.co/docs/datasets/audio_dataset)