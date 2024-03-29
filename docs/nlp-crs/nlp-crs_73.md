# 如何写一个好问题

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter8/5?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter8/5?fw=pt)

               ![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/chapter8/section5.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/chapter8/section5.ipynb)

当您遇到 Hugging Face 库中的一个看起来不正确的东西时，您一定要告诉我们，以便我们可以修复它（就此而言，任何开源库也是如此）。如果您不能完全确定错误是在您自己的代码中还是在我们的某个库中，那么首先要检查的是[forums](https://discuss.huggingface.co/).社区会帮助你解决这个问题，Hugging Face 团队也会密切关注那里的讨论。

[`www.youtube-nocookie.com/embed/_PAli-V4wj0`](https://www.youtube-nocookie.com/embed/_PAli-V4wj0)

当您确定手头有错误时，第一步是构建一个最小的可重现示例。

## 创建一个最小的可重现示例

隔离产生错误的代码段非常重要，因为 Hugging Face 团队中没有人是魔术师（目前），他们无法修复他们看不到的东西。顾名思义，最小的可重现示例应该是可重现的。这意味着它不应依赖于您可能拥有的任何外部文件或数据。尝试用一些看起来像真实值的虚拟值替换您正在使用的数据，但仍然会产生相同的错误。

🚨🤗 Transformers 存储库中的许多问题都没有解决，因为用于复制它们的数据不可访问。

一旦你有一些自包含的东西，你可以尝试将它减少到更少的代码行，构建我们所谓的最小的可重复示例.虽然这需要你做更多的工作，但如果你提供一个漂亮的、简短的错误重现器，你几乎可以保证得到帮助和修复。

如果您觉得足够舒服，请检查发生错误的源代码。您可能会找到问题的解决方案（在这种情况下，您甚至可以提出拉取请求来修复它），但更一般地说，这可以帮助维护人员在阅读您的报告时更好地理解来源。

## 填写问题模板

当您提交问题时，您会注意到有一个模板需要填写。我们将按照[🤗 Transformers issues](https://github.com/huggingface/transformers/issues/new/choose)在这里，但是如果您在另一个存储库中报告问题，则需要相同类型的信息。不要将模板留空：花时间填写它可以最大限度地提高您获得答案和解决问题的机会。

通常，在提交问题时，请始终保持礼貌。这是一个开源项目，因此您使用的是免费软件，没有人有任何义务帮助您。您可能会在您的问题中包含您认为合理的批评，但是维护人员很可能会认为它很糟糕并且不会急于帮助您。确保你阅读了[code of conduct](https://github.com/huggingface/transformers/blob/master/CODE_OF_CONDUCT.md)项目的。

### 包括您的环境信息

🤗 Transformers 提供了一个实用程序来获取我们需要的关于您的环境的所有信息。只需在终端中输入以下内容：

```py
transformers-cli env
```

你应该得到这样的东西：

```py
Copy-and-paste the text below in your GitHub issue and FILL OUT the two last points.

- `transformers` version: 4.12.0.dev0
- Platform: Linux-5.10.61-1-MANJARO-x86_64-with-arch-Manjaro-Linux
- Python version: 3.7.9
- PyTorch version (GPU?): 1.8.1+cu111 (True)
- Tensorflow version (GPU?): 2.5.0 (True)
- Flax version (CPU?/GPU?/TPU?): 0.3.4 (cpu)
- Jax version: 0.2.13
- JaxLib version: 0.1.65
- Using GPU in script?: <fill in>
- Using distributed or parallel set-up in script?: <fill in>
```

您还可以添加一个 **!** 在开始的时候 **transformers-cli env** 命令从笔记本单元执行它，然后在问题的开头复制并粘贴结果。

### 标记人员

通过输入标记人员 **@** 其次是他们的 GitHub 句柄将向他们发送通知，以便他们会看到您的问题并可能会更快地回复。适度使用它，因为如果您标记的人没有直接链接，他们可能不喜欢收到通知。如果您查看了与您的错误相关的源文件，您应该在您认为对您的问题负责的行中标记最后一个进行更改的人（您可以通过查看 GitHub 上的所述行找到此信息，选择它，然后单击“查看 git blame”）。

否则，模板会提供要标记的人的建议。一般来说，不要标记超过三个人！

### 包含一格可重复的示例

如果您已经设法创建了一个产生错误的独立示例，那么现在是包含它的时候了！键入一行包含三个反引号，后跟 **python** ， 像这样：

```py
```python
```

然后粘贴您的最小可重现示例并键入一个带有三个反引号的新行。这将确保您的代码格式正确。如果您没有设法创建可重现的示例，请以清晰的步骤解释您是如何解决问题的。如果可以，请包含指向错误所在的 Google Colab 笔记本的链接。你分享的信息越多，维护者就越有能力回复你。在所有情况下，您都应该复制并粘贴您收到的整个错误消息。如果您在 Colab 中工作，请记住，堆栈跟踪中的某些帧可能会自动折叠，因此请确保在复制之前展开它们。与代码示例一样，将该错误消息放在两行之间，并带有三个反引号，因此格式正确。

### 描述预期行为

用几行解释你期望得到什么，以便维护人员完全掌握问题。这部分通常很明显，所以应该用一句话来形容，但在某些情况下，您可能有很多话要说。

## 然后什么？

提交您的问题后，请确保快速检查一切是否正常。如果您犯了错误，您可以编辑问题，或者如果您发现问题与您最初的想法不同，甚至可以更改其标题。如果你没有得到答案，就没有必要对人进行 ping 操作。如果几天内没有人帮助您，很可能没有人能理解您的问题。不要犹豫，回到可重现的例子。你能不能让它更短更切题？如果您在一周内没有得到答复，您可以留言温和地寻求帮助，特别是如果您已编辑问题以包含有关该问题的更多信息。