# 表格分类/回归

> 原文：[https://huggingface.co/docs/autotrain/tabular](https://huggingface.co/docs/autotrain/tabular)

使用AutoTrain，您可以轻松训练一个模型来对表格数据进行分类或回归。您只需要从模型列表中选择并上传您的数据集。参数调整会自动完成。

## 模型

以下模型适用于表格分类/回归。

+   xgboost

+   random_forest

+   岭回归

+   逻辑回归

+   svm

+   额外树

+   梯度提升

+   adaboost

+   decision_tree

+   knn

## 数据格式

```py
id,category1,category2,feature1,target
1,A,X,0.3373961604172684,1
2,B,Z,0.6481718720511972,0
3,A,Y,0.36824153984054797,1
4,B,Z,0.9571551589530464,1
5,B,Z,0.14035078041264515,1
6,C,X,0.8700872583584364,1
7,A,Y,0.4736080452737105,0
8,C,Y,0.8009107519796442,1
9,A,Y,0.5204774795512048,0
10,A,Y,0.6788795301189603,0
.
.
.
```

## 列

您的CSV数据集必须有两列：`id`和`target`。
