# 构建你的第一个演示

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter9/2?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter9/2?fw=pt)

             ![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/chapter9/section2.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/chapter9/section2.ipynb)

让我们从安装 Gradio 开始吧！ 由于它是一个 Python 包，只需运行：

`$ pip install gradio`

您可以在任何地方运行 Gradio，无论是从您最喜欢的 Python IDE、Jupyter notebook 还是 Google Colab 🤯！ 所以无论你在哪里运行 Python，都可以安装 Gradio！

让我们从一个简单的“Hello World”示例开始，熟悉 Gradio 语法：

```py
import gradio as gr

def greet(name):
    return "Hello " + name

demo = gr.Interface(fn=greet, inputs="text", outputs="text")

demo.launch()
```

让我们看一下上面的代码：

-首先，我们定义一个名为 `greet()` 的函数。 在这种情况下，它是一个在您的名字前添加“Hello”的简单函数，但它通常可以是 *any* Python 函数。 例如，在机器学习应用程序中，此函数将*调用模型以对输入进行预测*并返回输出。

*   然后，我们创建一个带有三个参数的 Gradio `Interface`，`fn`、`inputs` 和 `outputs`。 这些参数定义了预测函数，以及我们想要的输入和输出组件的*type*。 在我们的例子中，两个组件都是简单的文本框。
*   然后我们在我们创建的 `Interface` 上调用 `launch()` 方法。

如果你运行这段代码，下面的界面会自动出现在 Jupyter/Colab notebook 中，或者在浏览器中弹出 **[`localhost:7860`](http://localhost:7860/)** 如果运行 从一个脚本。

[`course-demos-hello-world.hf.space`](https://course-demos-hello-world.hf.space)

立即尝试使用您自己的姓名或其他输入来使用此 GUI！

您会注意到，在这个 GUI 中，Gradio 自动推断输入参数的名称 (`name`)并将其应用为文本框顶部的标签。 如果你想改变它怎么办？或者，如果您想以其他方式自定义文本框？ 在这种情况下，您可以实例化一个表示输入组件的类对象。

看看下面的例子：

```py
import gradio as gr

def greet(name):
    return "Hello " + name

# We instantiate the Textbox class
textbox = gr.Textbox(label="Type your name here:", placeholder="John Doe", lines=2)

gr.Interface(fn=greet, inputs=textbox, outputs="text").launch()
```

[`course-demos-hello-world-custom.hf.space`](https://course-demos-hello-world-custom.hf.space)

在这里，我们创建了一个带有标签、占位符和一组行数的输入文本框。您可以对输出文本框执行相同的操作，但我们现在将其保留。

我们已经看到，只需几行代码，Gradio 就可以让您围绕任何具有任何类型输入或输出的函数创建一个简单的界面。 在本节中，我们从一个简单的文本框开始，但在接下来的部分中，我们将介绍其他类型的输入和输出。 现在让我们看看在 Gradio 应用程序中包含一些 NLP。

## 🤖 包括模型预测

现在让我们构建一个简单的界面，让您可以演示像 GPT-2 这样的**文本生成**模型。

我们将使用 🤗 Transformers 中的 `pipeline()` 函数加载我们的模型。 如果您需要快速复习，您可以返回到 第一章中的那个部分。

首先，我们定义一个接受文本提示并返回文本完成的预测函数：

```py
from transformers import pipeline

model = pipeline("text-generation")

def predict(prompt):
    completion = model(prompt)[0]["generated_text"]
    return completion
```

此函数完成您提供的提示，您可以使用自己的输入提示运行它以查看它是如何工作的。 这是一个示例（您可能会得到不同的完成）：

```py
predict("My favorite programming language is")
```

```py
>> My favorite programming language is Haskell. I really enjoyed the Haskell language, but it doesn't have all the features that can be applied to any other language. For example, all it does is compile to a byte array.
```

现在我们有了一个生成预测的函数，我们可以像之前一样创建和启动一个“接口”：

```py
import gradio as gr

gr.Interface(fn=predict, inputs="text", outputs="text").launch()
```

就是这样！ 您现在可以使用此接口使用 GPT-2 模型生成文本，如下所示 🤯.

[`course-demos-gpt-2.hf.space`](https://course-demos-gpt-2.hf.space)

继续阅读以了解如何使用 Gradio 构建其他类型的演示！