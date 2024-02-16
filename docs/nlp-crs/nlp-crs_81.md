# 与 Hugging Face Hub 整合

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter9/5?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter9/5?fw=pt)

             ![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/chapter9/section5.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/chapter9/section5.ipynb)

为了让你的生活更轻松, Gradio 直接与 Hugging Face Hub 和 Hugging Face Spaces 集成。你可以仅使用 *一行代码* 从中心和空间加载演示。

### 从 Hugging Face Hub 加载模型

首先, 从 Hugging Face 通过 Hub 提供的数千个模型中选择一个, 如 第四章 中所述。

使用特殊的 `Interface.load()` 方法, 你可以传递 `"model/"` (或者, 等效的, `"huggingface/"`) 后面是模型名称。例如, 这里是为大型语言模型 [GPT-J](https://huggingface.co/EleutherAI/gpt-j-6B)构建演示的代码, 添加几个示例输入:

```py
import gradio as gr

title = "GPT-J-6B"
description = "Gradio Demo for GPT-J 6B, a transformer model trained using Ben Wang's Mesh Transformer JAX. 'GPT-J' refers to the class of model, while '6B' represents the number of trainable parameters. To use it, simply add your text, or click one of the examples to load them. Read more at the links below."
article = "<p style='text-align: center'><a href='https://github.com/kingoflolz/mesh-transformer-jax' target='_blank'>GPT-J-6B: A 6 Billion Parameter Autoregressive Language Model</a></p>"
examples = [
    ["The tower is 324 metres (1,063 ft) tall,"],
    ["The Moon's orbit around Earth has"],
    ["The smooth Borealis basin in the Northern Hemisphere covers 40%"],
]
gr.Interface.load(
    "huggingface/EleutherAI/gpt-j-6B",
    inputs=gr.Textbox(lines=5, label="Input Text"),
    title=title,
    description=description,
    article=article,
    examples=examples,
    enable_queue=True,
).launch()
```

上述代码将生成以下界面:

[`course-demos-gpt-j-6B.hf.space`](https://course-demos-gpt-j-6B.hf.space)

以这种方式加载模型使用 Hugging Face 的 [Inference API](https://huggingface.co/inference-api),而不是将模型加载到内存中。这对于像 GPT-J 或 T0pp 这样需要大量 RAM 的大型模型是理想的。

### 从 Hugging Face Spaces 空间加载

要从 hugs Face Hub 加载任何空间并在本地重新创建它, 你可以将 `spaces/` 传递给 `Interface`, 再加上空间的名称。

还记得第 1 节中删除图像背景的演示吗? 让我们从 Hugging Face Spaces 加载它:

```py
gr.Interface.load("spaces/abidlabs/remove-bg").launch()
```

[`course-demos-remove-bg-original.hf.space`](https://course-demos-remove-bg-original.hf.space)

从 Hub 或 Spaces 加载演示的一个很酷的地方是, 你可以通过覆盖任何参数来自定义它们。在这里, 我们添加一个标题并让它与网络摄像头一起使用:

```py
gr.Interface.load(
    "spaces/abidlabs/remove-bg", inputs="webcam", title="Remove your webcam background!"
).launch()
```

[`course-demos-Remove-bg.hf.space`](https://course-demos-Remove-bg.hf.space)

现在我们已经探索了几种将 Gradio 与 hugs Face Hub 集成的方法, 让我们来看看 `Interface` 类的一些高级功能。这就是下一节的主题！