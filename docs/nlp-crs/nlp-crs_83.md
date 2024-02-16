# Gradio 块简介

> 原文：[`huggingface.co/learn/nlp-course/zh-CN/chapter9/7?fw=pt`](https://huggingface.co/learn/nlp-course/zh-CN/chapter9/7?fw=pt)

              ![Open In Colab](https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/chapter9/section7.ipynb) ![Open In Studio Lab](https://studiolab.sagemaker.aws/import/github/huggingface/notebooks/blob/master/course/chapter9/section7.ipynb)

在前面的部分中, 我们已经使用 `Interface` 类探索并创建了演示。在本节中, 我们将介绍我们 **新开发**的称为`gradio.Blocks`低级 API。

现在, `接口`和`块`之间有什么区别?

*   ⚡ `接口`: 一个高级 API, 让你只需提供输入和输出列表即可创建完整的机器学习演示。

*   🧱 `块`: :一个低级的 API, 它允许你完全控制你的应用程序的数据流和布局。您可以使用`块`(如 “构建块”)构建非常复杂的多步骤应用程序。

### 为什么要块 🧱?

正如我们在前几节中看到的, `Interface` 类允许你使用几行代码轻松创建成熟的机器学习 demo。`Interface` API 非常易于使用, 但缺乏 `Blocks` API 提供的灵活性。例如, 你可能想要:

*   将相关演示组合为一个 web 应用程序中的多个选项卡
*   更改 demo 的布局, 例如指定输入和输出的位置
*   具有多步骤接口, 其中一个模型的输出成为下一个模型的输入, 或者通常具有更灵活的数据流
*   根据用户输入更改组件的属性 (例如, 下拉列表中的选项) 或其可见性

我们将在下面探讨所有这些概念。

### 使用块创建简单 demo

安装 Gradio 后, 将以下代码作为 Python 脚本、Jupyter 笔记本或 Colab 笔记本运行。

```py
import gradio as gr

def flip_text(x):
    return x[::-1]

demo = gr.Blocks()

with demo:
    gr.Markdown(
        """
    # Flip Text!
    Start typing below to see the output.
    """
    )
    input = gr.Textbox(placeholder="Flip this text")
    output = gr.Textbox()

    input.change(fn=flip_text, inputs=input, outputs=output)

demo.launch()
```

[`course-demos-flip-text.hf.space`](https://course-demos-flip-text.hf.space)

上述简单示例介绍了块的 4 个基本概念:

1.  块允许你允许你构建结合 markdown、HTML、按钮和交互组件的 web 应用程序, 只需在一个带有 gradio 的 Python 中实例化对象。

    🙋如果你不熟悉 Python 中的 `with` 语句, 我们建议你查看来自 Real Python 的极好的[教程](https://realpython.com/python-with-statement/)。看完后回到这里 🤗实例化组件的顺序很重要, 因为每个元素都按照创建的顺序呈现到 Web 应用程序中。(更复杂的布局在下面讨论)
2.  你可以在代码中的任何位置定义常规 Python 函数, 并使用`块`在用户输入的情况下运行它们。在我们的示例中, 们有一个”翻转”输入文本的简单函数, 但你可以编写任何 Python 函数, 从简单的计算到处理机器学习模型的预测。

3.  你可以将事件指定给任何`块`组件。这将在组件被单击、更改等情况下运行函数。当你分配一个事件时, 你传入三个参数: `fn`: 应该被调用的函数, `inputs`: 输入组件的(列表), 以及 `outputs`: 应该被调用的输出组件的(列表)。

    在上面的示例中, 当名为 `input` 的 `Textbox` 中的值发生变化时, 我们运行 `flip_text()` 函数。该事件读取`输入`中的值, 将其作为名称参数传递给 `flip_text()`, 然后它返回一个值, 该值被分配给我们的第二个名为 `output` 的 `Textbox`。

    要查看每个组件支持的事件列表, 请参阅 Gradio [文档](https://www.gradio.app/docs/)。

4.  块会根据你定义的事件触发器自动确定组件是否应该是交互式的 (接受用户输入)。在我们的示例中, 第一个文本框是交互式的, 因为它的值由 `flip_text()` 函数使用。第二个文本框不是交互式的, 因为它的值从不用作输入。在某些情况下, 你可能想要覆盖它, 你可以通过传递一个布尔值给组件的`交互`参数(例如 `gr.Textbox(placeholder="Flip this text", interactive=True)`)。

### 自定义演示的布局

我们如何使用`块`来定制我们的演示的布局? 默认情况下, `块`在一列中垂直呈现创建的组件。你可以通过使用 `with gradio.Column():` 创建其他列或使用 `with gradio.Row():` 创建其他行并在这些上下文中创建组件来改变这一点。

你应该记住: 在 `列` 下创建的任何组件(这也是默认设置) 都将垂直布局。在 `Row` 下创建的任何组件都将水平布局, 类似于 [Web 开发中的 flexbox 模型](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)。

最后, 你还可以使用 `with gradio.Tabs()` 上下文管理器为您的 demo 创建选项卡。在此上下文中, 您可以通过使用 `gradio.TabItem(name_of_tab):` 指定来创建多个选项卡。在 `gradio.TabItem(name_of_tab):` 中创建的任何组件都会出现在该选项卡中。

现在让我们在 demo 中添加一个 `flip_image()`函数并添加一个翻转图像的新选项卡。下面是一个带有 2 个选项卡的示例, 也使用了一个行:

```py
import numpy as np
import gradio as gr

demo = gr.Blocks()

def flip_text(x):
    return x[::-1]

def flip_image(x):
    return np.fliplr(x)

with demo:
    gr.Markdown("Flip text or image files using this demo.")
    with gr.Tabs():
        with gr.TabItem("Flip Text"):
            with gr.Row():
                text_input = gr.Textbox()
                text_output = gr.Textbox()
            text_button = gr.Button("Flip")
        with gr.TabItem("Flip Image"):
            with gr.Row():
                image_input = gr.Image()
                image_output = gr.Image()
            image_button = gr.Button("Flip")

    text_button.click(flip_text, inputs=text_input, outputs=text_output)
    image_button.click(flip_image, inputs=image_input, outputs=image_output)

demo.launch()
```

[`course-demos-flip-text-image.hf.space`](https://course-demos-flip-text-image.hf.space)

你会注意到, 在这个示例中, 我们还在每个选项卡中创建了一个 `Button` 组件, 并且我们为每个按钮分配了一个点击事件,这是实际运行该函数的事件。

### 探索事件和状态

正如你可以控制布局一样, `块` 可以让你对触发函数调用的事件进行细粒度控制。每个组件和许多布局都有它们支持的特定事件。

例如, `Textbox` 组件有两个事件: `change()` (当文本框内的值发生变化时), 和 `submit()` (当用户在关注文本框时按下 enter 键)。更复杂的组件可以有更多的事件: 例如,`Audio`组件也有单独的事件, 用于播放、清除、暂停音频文件等。请参阅文档了解每个组件支持的事件。

你可以将事件触发器附加到这些事件中的一个、一个或多个。你可以通过在组件实例中调用事件名称作为函数来创建一个事件触发器 — 例如 `textbox.change(...)` 或 `btn.click(...)`。如前所述, 该函数接受三个参数:

*   `fn`: 要运行的函数
*   `inputs`: 组件的(列表), 其值应作为函数的输入参数提供。每个组件的值按顺序映射到相应的函数参数。如果函数不带任何参数, 则此参数可以为 None。
*   `outputs`: 应根据函数返回的值更新其值的组件(列表)。每个返回值按顺序设置相应组件的值。如果函数不返回任何内容, 则此参数可以为 None。

你甚至可以使输入和输出组件成为同一个组件, 就像我们在这个使用 GPT 模型进行文本补全的示例中所做的那样:

```py
import gradio as gr

api = gr.Interface.load("huggingface/EleutherAI/gpt-j-6B")

def complete_with_gpt(text):
    # Use the last 50 characters of the text as context
    return text[:-50] + api(text[-50:])

with gr.Blocks() as demo:
    textbox = gr.Textbox(placeholder="Type here and press enter...", lines=4)
    btn = gr.Button("Generate")

    btn.click(complete_with_gpt, textbox, textbox)

demo.launch()
```

[`course-demos-blocks-gpt.hf.space`](https://course-demos-blocks-gpt.hf.space)

### 创建多步 demo

在某些情况下, 您可能需要一个 *多步骤的 demo*, 其中重用一个函数的输出作为下一个函数的输入。使用 `块` 很容易做到这一点, 因为你可以使用组件作为一个事件触发器的输入, 但作为另一个事件触发器的输出。看看下面示例中的文本组件, 它的值是语音到文本模型的结果, 但也被传递到情感分析模型:

```py
from transformers import pipeline

import gradio as gr

asr = pipeline("automatic-speech-recognition", "facebook/wav2vec2-base-960h")
classifier = pipeline("text-classification")

def speech_to_text(speech):
    text = asr(speech)["text"]
    return text

def text_to_sentiment(text):
    return classifier(text)[0]["label"]

demo = gr.Blocks()

with demo:
    audio_file = gr.Audio(type="filepath")
    text = gr.Textbox()
    label = gr.Label()

    b1 = gr.Button("Recognize Speech")
    b2 = gr.Button("Classify Sentiment")

    b1.click(speech_to_text, inputs=audio_file, outputs=text)
    b2.click(text_to_sentiment, inputs=text, outputs=label)

demo.launch()
```

[`course-demos-blocks-multi-step.hf.space`](https://course-demos-blocks-multi-step.hf.space)

### 更新组件属性

到目前为止, 我们已经了解了如何创建事件来更新另一个组件的值。但是, 如果您想更改组件的其他属性, 例如文本框的可见性或单选按钮组中的选项, 会发生什么? 您可以通过返回组件类的 `update()` 方法而不是函数的常规返回值来做到这一点。

这很容易用一个例子来说明:

```py
import gradio as gr

def change_textbox(choice):
    if choice == "short":
        return gr.Textbox.update(lines=2, visible=True)
    elif choice == "long":
        return gr.Textbox.update(lines=8, visible=True)
    else:
        return gr.Textbox.update(visible=False)

with gr.Blocks() as block:
    radio = gr.Radio(
        ["short", "long", "none"], label="What kind of essay would you like to write?"
    )
    text = gr.Textbox(lines=2, interactive=True)

    radio.change(fn=change_textbox, inputs=radio, outputs=text)
    block.launch()
```

[`course-demos-blocks-update-component-properti-833c723.hf.space`](https://course-demos-blocks-update-component-properti-833c723.hf.space)

我们刚刚探索了`块`的所有核心概念! 就像 `参数一样`, 你可以创建很酷的 demo, 可以通过在`launch()`方法中使用`share=True`来共享, 或者部署在[Hugging Face Spaces](https://huggingface.co/spaces)上。