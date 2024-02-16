# 图像分类

> 原文链接：[https://huggingface.co/docs/autotrain/image_classification](https://huggingface.co/docs/autotrain/image_classification)

图像分类是一个监督学习问题：定义一组目标类别（要在图像中识别的对象），并训练一个模型使用带标签的示例照片来识别它们。使用AutoTrain，训练一个最先进的图像分类模型非常容易。只需上传一组图像，AutoTrain将自动训练一个模型来对它们进行分类。

## 数据准备

图像分类的数据必须以zip格式存储，每个类别应该在一个单独的子文件夹中。例如，如果你想分类猫和狗，你的zip文件应该是这样的：

```py
cats_and_dogs.zip
├── cats
│   ├── cat.1.jpg
│   ├── cat.2.jpg
│   ├── cat.3.jpg
│   └── ...
└── dogs
    ├── dog.1.jpg
    ├── dog.2.jpg
    ├── dog.3.jpg
    └── ...
```

一些需要记住的要点：

+   zip文件应该包含多个文件夹（类别），每个文件夹应该包含一个类别的图像。

+   文件夹的名称应该是类别的名称。

+   图像必须是jpeg、jpg或png格式。

+   每个类别至少应该有5张图像。

+   在zip文件中不应该有其他文件。

+   在zip文件夹内不应该有其他文件夹。

当train.zip被解压缩时，会创建两个文件夹：cats和dogs。这是用于分类的两个类别。每个类别的图像都在各自的文件夹中。你可以有任意多个类别。
