# 数字对象标识符（DOI）

> 原文：[`huggingface.co/docs/hub/doi`](https://huggingface.co/docs/hub/doi)

Hugging Face Hub 提供了为您的模型或数据集生成 DOI 的可能性。 DOI（数字对象标识符）是唯一标识数字对象的字符串，从文章到图表，包括数据集和模型。 DOI 与对象的元数据相关联，包括对象的 URL、版本、创建日期、描述等。它们是研究和学术界广泛接受的数字资源引用；它们类似于书籍的 ISBN。

## 如何生成 DOI？

要做到这一点，您必须进入您的模型或数据集的设置。在 DOI 部分，应该出现一个名为“生成 DOI”的按钮：

![](img/941cecea757f7df0f64d009db0bab5ef.png) ![](img/2deeb2fc2e023b60adfcd852b4c10360.png)

要为此模型或数据集生成 DOI，您需要点击此按钮并确认一些功能将受到限制，您的一些信息（您的全名）将被传输给我们的合作伙伴 DataCite：

![](img/8b6e8bb400cb895c5dcce609ace0872f.png) ![](img/8c7bb52d15524905f4828f1848d9a84e.png)

一旦您同意这些条款，您的模型或数据集将被分配 DOI，并且您的模型或数据集标题中应该出现一个新的标签，允许您引用它。

![](img/d00e564e6d7c2bd22568e31a0ab94ceb.png) ![](img/e3c759233b11c82e15db66b5a1789f38.png)

## 如果我的模型或数据集发生变化，我可以重新生成新的 DOI 吗？

如果模型或数据集有新版本，可以轻松分配新的 DOI，并且以前版本的 DOI 将过时。这使得即使对象发生变化，也可以轻松引用特定版本的对象。

![](img/e7e5b5257ea121a255f03c334ffe108e.png) ![](img/ace51d18e404362cff61495d44beac11.png)

您只需点击“生成新 DOI”，然后🎉新的 DOI 将分配给您模型或数据集的当前修订版本。

## 为什么在删除、重命名和更改模型或数据集的可见性操作时会出现“由 DOI 锁定”的消息？

DOI 使查找有关模型或数据集的信息更容易，并通过永久链接与世界分享，该链接永远不会过期或更改。因此，具有 DOI 的数据集/模型旨在永久存在，并且只能在向我们的支持部门提出请求（网站在 huggingface.co）后才能删除、重命名和更改其可见性。

## 进一步阅读

+   [介绍 DOI：数字对象标识符到数据集和模型](https://huggingface.co/blog/introducing-doi)
