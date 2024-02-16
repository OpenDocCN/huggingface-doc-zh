# 在AWS Inferentia上使用llama-2-13B创建您自己的聊天机器人

> 原始文本：[https://huggingface.co/docs/optimum-neuron/tutorials/llama2-13b-chatbot](https://huggingface.co/docs/optimum-neuron/tutorials/llama2-13b-chatbot)

*这里有一个笔记本版本的教程[链接](https://github.com/huggingface/optimum-neuron/blob/main/notebooks/text-generation/llama2-13b-chatbot.ipynb)*。

本指南将详细介绍如何在AWS inferentia上导出、部署和运行**LLama-2 13B**聊天模型。

您将学习如何：

+   将Llama-2模型导出为Neuron格式，

+   将导出的模型推送到Hugging Face Hub，

+   部署模型并在聊天应用程序中使用它。

注意：本教程是在一个inf2.48xlarge的AWS EC2实例上创建的。

## 1\. 将Llama 2模型导出到Neuron

在本指南中，我们将使用非门控[NousResearch/Llama-2-13b-chat-hf](https://huggingface.co/NousResearch/Llama-2-13b-chat-hf)模型，它在功能上等同于原始的[meta-llama/Llama-2-13b-chat-hf](https://huggingface.co/meta-llama/Llama-2-13b-chat-hf)模型。

该模型是**Llama 2**系列模型的一部分，已经调整为识别*用户*和*助手*之间的聊天互动（稍后会详细介绍）。

如[最佳神经元文档](https://huggingface.co/docs/optimum-neuron/guides/export_model#why-compile-to-neuron-model)中所解释的，模型需要在Neuron设备上运行之前被编译和导出为序列化格式。

幸运的是，🤗 **optimum-neuron** 提供了一个非常简单的 [API](https://huggingface.co/docs/optimum-neuron/guides/models#configuring-the-export-of-a-generative-model) 来将标准 🤗 [transformers 模型](https://huggingface.co/docs/transformers/index) 导出为Neuron格式。

在导出模型时，我们将指定两组参数：

+   使用*compiler_args*，我们指定要部署模型的核心数（每个神经元设备有两个核心），以及精度（这里是*float16*），

+   使用*input_shapes*，我们设置模型的静态输入和输出维度。所有模型编译器都需要静态形状，神经元也不例外。请注意，*sequence_length*不仅限制了输入上下文的长度，还限制了Key/Value缓存的长度，因此也限制了输出长度。

根据您选择的参数和inferentia主机，这可能需要几分钟到一个多小时的时间。

为了方便起见，我们在Hugging Face hub上托管了该模型的预编译版本，因此您可以跳过导出并立即在第2节中开始使用该模型。

```py
from optimum.neuron import NeuronModelForCausalLM

compiler_args = {"num_cores": 24, "auto_cast_type": 'fp16'}
input_shapes = {"batch_size": 1, "sequence_length": 2048}
model = NeuronModelForCausalLM.from_pretrained(
        "NousResearch/Llama-2-13b-chat-hf",
        export=True,
        **compiler_args,
        **input_shapes)
```

这可能需要一段时间。

幸运的是，您只需要做一次这个操作，因为您可以保存您的模型并稍后重新加载它。

```py
model.save_pretrained("llama-2-13b-chat-neuron")
```

更好的是，您可以将其推送到[Hugging Face hub](https://huggingface.co/models)。

为此，您需要登录到[HuggingFace帐户](https://huggingface.co/join)。

在终端中，只需输入以下命令，并在请求时粘贴您的Hugging Face令牌：

```py
huggingface-cli login
```

默认情况下，模型将上传到您的帐户（组织等于您的用户名）。

如果您想将模型上传到特定的[Hugging Face组织](https://huggingface.co/docs/hub/organizations)，请随意编辑下面的代码。

```py
from huggingface_hub import whoami

org = whoami()['name']

repo_id = f"{org}/llama-2-13b-chat-neuron"

model.push_to_hub("llama-2-13b-chat-neuron", repository_id=repo_id)
```

### 关于导出参数的更多说明。

加载模型所需的最小内存可以通过以下方式计算：

```py
   memory = bytes per parameter * number of parameters
```

**Llama 2 13B**模型使用*float16*权重（存储在2字节中），有130亿参数，这意味着至少需要2 * 13B或~26GB的内存来存储其权重。

每个NeuronCore有16GB的内存，这意味着26GB的模型无法放入单个NeuronCore中。

实际上，由于缓存注意力层投影（KV缓存），所需的总空间远远大于参数数量。这种缓存机制会随着序列长度和批量大小线性增长内存分配。

在这里，我们将*batch_size*设置为1，这意味着我们只能并行处理一个输入提示。我们将*sequence_length*设置为2048，这对应于模型最大容量的一半（4096）。

评估KV缓存大小的公式更复杂，因为它还取决于与模型架构相关的参数，比如嵌入的宽度和解码器块的数量。

总的来说，为了使非常大的语言模型适应，张量并行性被用来在多个NeuronCores之间分割权重、数据和计算，需要注意的是每个核心上的内存不能超过16GB。

请注意，将核心数量增加到最低要求之上几乎总是会导致模型运行更快。增加张量并行度会提高内存带宽，从而提高模型性能。

为了优化性能，建议使用实例上所有可用的核心。

在本指南中，我们使用*inf2.48xlarge*的所有24个核心，但如果您使用*inf2.24xlarge*实例，则应将其更改为12个核心。

## 2. 在AWS Inferentia2上使用Llama 2生成文本

一旦您的模型被导出，您可以使用transformers库生成文本，如在[这篇文章](https://huggingface.co/blog/how-to-generate)中详细描述的。

如果您按照建议跳过了第一部分，不用担心：我们将使用hub上已经存在的预编译模型。

```py
from optimum.neuron import NeuronModelForCausalLM

try:
    model
except NameError:
    # Edit this to use another base model
    model = NeuronModelForCausalLM.from_pretrained('aws-neuron/Llama-2-13b-chat-hf-neuron-latency')
```

我们将需要一个*Llama 2*分词器将提示字符串转换为文本标记。

```py
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("NousResearch/Llama-2-13b-chat-hf")
```

支持以下生成策略：

+   贪婪搜索，

+   使用top-k和top-p（带有温度）的多项式采样。

大多数对数预处理/过滤（如重复惩罚）都受支持。

```py
inputs = tokenizer("What is deep-learning ?", return_tensors="pt")
outputs = model.generate(**inputs,
                         max_new_tokens=128,
                         do_sample=True,
                         temperature=0.9,
                         top_k=50,
                         top_p=0.9)
tokenizer.batch_decode(outputs, skip_special_tokens=True)
```

## 3. 在AWS Inferentia2上使用llama创建聊天应用

我们特意选择了**Llama 2**聊天变体，以展示导出模型在编码上下文长度增加时的出色行为。

模型期望提示按照特定模板格式化，该模板对应于*用户*角色和*助手*角色之间的互动。

每个聊天模型都有自己的约定来编码这些内容，我们在本指南中不会详细介绍，因为我们将直接使用与我们的模型对应的[Hugging Face聊天模板](https://huggingface.co/blog/chat-templates)。

下面的实用函数将把用户和模型之间的交流列表转换为格式良好的聊天提示。

```py
def format_chat_prompt(message, history, max_tokens):
    """ Convert a history of messages to a chat prompt

    Args:
        message(str): the new user message.
        history (List[str]): the list of user messages and assistant responses.
        max_tokens (int): the maximum number of input tokens accepted by the model.

    Returns:
        a `str` prompt.
    """
    chat = []
    # Convert all messages in history to chat interactions
    for interaction in history:
        chat.append({"role": "user", "content" : interaction[0]})
        chat.append({"role": "assistant", "content" : interaction[1]})
    # Add the new message
    chat.append({"role": "user", "content" : message})
    # Generate the prompt, verifying that we don't go beyond the maximum number of tokens
    for i in range(0, len(chat), 2):
        # Generate candidate prompt with the last n-i entries
        prompt = tokenizer.apply_chat_template(chat[i:], tokenize=False)
        # Tokenize to check if we're over the limit
        tokens = tokenizer(prompt)
        if len(tokens.input_ids) <= max_tokens:
            # We're good, stop here
            return prompt
    # We shall never reach this line
    raise SystemError
```

现在我们已经准备好构建一个简单的聊天应用程序。

我们简单地将用户和助手之间的互动存储在一个列表中，我们用这个列表来生成输入提示。

```py
history = []
max_tokens = 1024

def chat(message, history, max_tokens):
    prompt = format_chat_prompt(message, history, max_tokens)
    # Uncomment the line below to see what the formatted prompt looks like
    #print(prompt)
    inputs = tokenizer(prompt, return_tensors="pt")
    outputs = model.generate(**inputs,
                             max_length=2048,
                             do_sample=True,
                             temperature=0.9,
                             top_k=50,
                             repetition_penalty=1.2)
    # Do not include the input tokens
    outputs = outputs[0, inputs.input_ids.size(-1):]
    response = tokenizer.decode(outputs, skip_special_tokens=True)
    history.append([message, response])
    return response
```

要测试聊天应用程序，您可以使用以下提示序列：

```py
print(chat("My favorite color is blue. My favorite fruit is strawberry.", history, max_tokens))
print(chat("Name a fruit that is on my favorite colour.", history, max_tokens))
print(chat("What is the colour of my favorite fruit ?", history, max_tokens))
```

<警告>

尽管非常强大，大型语言模型有时可能会“幻觉”。我们称生成的内容为“幻觉”，如果内容与主题无关或是虚构的，但模型呈现出来的好像是准确的。这是LLM的一个缺陷，不是在Trainium / Inferentia上使用它们的副作用。

</警告>
