# 神经元模型推理

> 原文链接：[`huggingface.co/docs/optimum-neuron/guides/models`](https://huggingface.co/docs/optimum-neuron/guides/models)

*以下文档中介绍的 API 适用于[inf2](https://aws.amazon.com/ec2/instance-types/inf2/)、[trn1](https://aws.amazon.com/ec2/instance-types/trn1/)和[inf1](https://aws.amazon.com/ec2/instance-types/inf1/)上的推理。*

`NeuronModelForXXX`类有助于从 Hugging Face Hub 加载模型并将其编译为针对神经元设备优化的序列化格式。然后，您将能够加载模型并通过 AWS Neuron 设备提供的加速运行推理。

## 从 Transformers 切换到 Optimum

`optimum.neuron.NeuronModelForXXX`模型类是与 Hugging Face Transformers 模型兼容的 API。这意味着与 Hugging Face 的生态系统无缝集成。您只需在`optimum.neuron`中用相应的`NeuronModelForXXX`类替换您的`AutoModelForXXX`类。

如果您已经使用 Transformers，您将能够通过替换模型类来重用您的代码：

```py
from transformers import AutoTokenizer
-from transformers import AutoModelForSequenceClassification
+from optimum.neuron import NeuronModelForSequenceClassification

# PyTorch checkpoint
-model = AutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")

+model = NeuronModelForSequenceClassification.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english",
+                                                             export=True, **neuron_kwargs)
```

如上所示，当您第一次使用`NeuronModelForXXX`时，您需要设置`export=True`将您的模型从 PyTorch 编译为神经元兼容格式。

您还需要传递神经元特定的参数来配置导出。每个模型架构都有自己的一组参数，如下一段详细说明。

一旦您的模型被导出，您可以将其保存在本地或[Hugging Face Model Hub](https://hf.co/models)中：

```py
# Save the neuron model
>>> model.save_pretrained("a_local_path_for_compiled_neuron_model")

# Push the neuron model to HF Hub
>>> model.push_to_hub(
...     "a_local_path_for_compiled_neuron_model", repository_id="my-neuron-repo", use_auth_token=True
... )
```

下次当您想要运行推理时，只需加载您编译的模型，这将节省您的编译时间：

```py
>>> from optimum.neuron import NeuronModelForSequenceClassification
>>> model = NeuronModelForSequenceClassification.from_pretrained("my-neuron-repo")
```

正如您所见，无需传递导出期间使用的神经元参数，因为它们保存在`config.json`文件中，并将由`NeuronModelForXXX`类自动恢复。

第一次运行推理时，当您第一次运行管道时会有一个预热阶段。这次运行的延迟比常规运行高 3 倍至 4 倍。

## 歧视性 NLP 模型

如前所述，您只需要对 Transformers 代码进行少量修改，即可导出和运行 NLP 模型：

```py
from transformers import AutoTokenizer
-from transformers import AutoModelForSequenceClassification
+from optimum.neuron import NeuronModelForSequenceClassification

# PyTorch checkpoint
-model = AutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")

# Compile your model during the first time
+compiler_args = {"auto_cast": "matmul", "auto_cast_type": "bf16"}
+input_shapes = {"batch_size": 1, "sequence_length": 64}
+model = NeuronModelForSequenceClassification.from_pretrained(
+    "distilbert-base-uncased-finetuned-sst-2-english", export=True, **compiler_args, **input_shapes,
+)

tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")
inputs = tokenizer("Hamilton is considered to be the best musical of human history.", return_tensors="pt")

logits = model(**inputs).logits
print(model.config.id2label[logits.argmax().item()])
# 'POSITIVE'
```

`compiler_args`是编译器的可选参数，这些参数通常控制编译器在推理性能（延迟和吞吐量）和准确性之间做出权衡。在这里，我们使用神经元矩阵乘法引擎将 FP32 操作转换为 BF16。

`input_shapes`是您需要发送给神经元编译器的强制静态形状信息。想知道您的模型需要哪些强制形状？使用以下代码查看：

```py
>>> from transformers import AutoModelForSequenceClassification
>>> from optimum.exporters import TasksManager

>>> model = AutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")

# Infer the task name if you don't know
>>> task = TasksManager.infer_task_from_model(model)  # 'text-classification'

>>> neuron_config_constructor = TasksManager.get_exporter_config_constructor(
...     model=model, exporter="neuron", task='text-classification'
... )
>>> print(neuron_config_constructor.func.get_mandatory_axes_for_task(task))
# ('batch_size', 'sequence_length')
```

请注意，用于编译的输入形状应该小于您在推理过程中将馈送到模型中的输入大小。

+   如果输入大小小于编译输入形状怎么办？

别担心，`NeuronModelForXXX`类将填充您的输入到一个合适的形状。此外，您可以在`from_pretrained`方法中设置`dynamic_batch_size=True`来启用动态批处理，这意味着您的输入可以具有可变的批处理大小。

（只需记住：动态性和填充不仅带来了灵活性，还带来了性能下降。够公平！）

## 生成性 NLP 模型

如前所述，您只需要对 Transformers 代码进行少量修改，即可导出和运行 NLP 模型：

### 配置生成模型的导出

对于非生成模型，可以传递两组参数给`from_pretrained()`方法，以配置如何将 transformers 检查点导出为神经元优化模型：

+   `compiler_args = { num_cores, auto_cast_type }`是编译器的可选参数，这些参数通常控制编译器在推理延迟、吞吐量和准确性之间做出权衡。

+   `input_shapes = { batch_size, sequence_length }`对应于模型输入和 KV-cache（过去标记的注意力键和值）的静态形状。

+   `num_cores`是实例化模型时使用的神经元核心数量。每个神经元核心有 16Gb 的内存，这意味着更大的模型需要分割到多个核心上。默认为 1，

+   `auto_cast_type`指定编码权重的格式。可以是`fp32`（`float32`），`fp16`（`float16`）或`bf16`（`bfloat16`）。默认为`fp32`。

+   `batch_size`是模型将接受的输入序列的数量。默认为 1，

+   `sequence_length`是输入序列中标记的最大数量。默认为`max_position_embeddings`（旧模型的`n_positions`）。

```py
from transformers import AutoTokenizer
-from transformers import AutoModelForCausalLM
+from optimum.neuron import NeuronModelForCausalLM

# Instantiate and convert to Neuron a PyTorch checkpoint
+compiler_args = {"num_cores": 1, "auto_cast_type": 'fp32'}
+input_shapes = {"batch_size": 1, "sequence_length": 512}
-model = AutoModelForCausalLM.from_pretrained("gpt2")
+model = NeuronModelForCausalLM.from_pretrained("gpt2", export=True, **compiler_args, **input_shapes)
```

如前所述，这些参数只能在导出期间配置。这意味着在推理期间：

+   输入的`batch_size`应等于导出时使用的`batch_size`，

+   输入序列的`length`应低于导出时使用的`sequence_length`，

+   最大标记数（输入+生成）不能超过导出时使用的`sequence_length`。

### 文本生成推理

与原始 transformers 模型一样，使用`generate()`而不是`forward()`来生成文本序列。

```py
from transformers import AutoTokenizer
-from transformers import AutoModelForCausalLM
+from optimum.neuron import NeuronModelForCausalLM

# Instantiate and convert to Neuron a PyTorch checkpoint
-model = AutoModelForCausalLM.from_pretrained("gpt2")
+model = NeuronModelForCausalLM.from_pretrained("gpt2", export=True)

tokenizer = AutoTokenizer.from_pretrained("gpt2")
tokenizer.pad_token_id = tokenizer.eos_token_id

tokens = tokenizer("I really wish ", return_tensors="pt")
with torch.inference_mode():
    sample_output = model.generate(
        **tokens,
        do_sample=True,
        min_length=128,
        max_length=256,
        temperature=0.7,
    )
    outputs = [tokenizer.decode(tok) for tok in sample_output]
    print(outputs)
```

生成是高度可配置的。请参考[`huggingface.co/docs/transformers/generation_strategies`](https://huggingface.co/docs/transformers/generation_strategies)获取详细信息。

请注意：

+   对于每个模型架构，为所有参数提供了默认值，但传递给`generate`方法的值将优先考虑，

+   生成参数可以存储在`generation_config.json`文件中。当模型目录中存在这样一个文件时，将解析它以设置默认参数（传递给`generate`方法的值仍然优先）。

祝您在 Neuron 中进行愉快的推理！🚀
