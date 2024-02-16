# 模型

> 原始文本：[`huggingface.co/docs/optimum-neuron/package_reference/modeling`](https://huggingface.co/docs/optimum-neuron/package_reference/modeling)

## 通用模型类

### NeuronBaseModel

`NeuronBaseModel` 类可用于实例化一个没有特定头部的基础神经元模型。它用作所有任务的基类，但不包括文本生成。

### `class optimum.neuron.NeuronBaseModel`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_base.py#L55)

```py
( model: ScriptModule config: PretrainedConfig model_save_dir: Union = None model_file_name: Optional = None preprocessors: Optional = None neuron_config: Optional = None **kwargs )
```

在神经元设备上运行编译和优化模型的基类。

它实现了与 Hugging Face Hub 交互的通用方法，以及将原始变压器模型编译为经过神经元优化的 TorchScript 模块并使用 `optimum.exporters.neuron` 工具链导出它。

类属性：

+   model_type（`str`，*可选*，默认为`"neuron_model"`）- 在注册 `NeuronBaseModel` 类时要使用的模型类型的名称。

+   auto_model_class（`Type`，*可选*，默认为`AutoModel`）- 要由当前 `NeuronBaseModel` 类表示的 `AutoModel` 类。

常见属性：

+   model（`torch.jit._script.ScriptModule`）- 为神经元设备编译的加载的 `ScriptModule`。

+   配置（[PretrainedConfig](https://huggingface.co/docs/transformers/main/en/main_classes/configuration#transformers.PretrainedConfig)）- 模型的配置。

+   model_save_dir（`Path`）- 保存神经元编译模型的目录。默认情况下，如果加载的模型是本地的，则将使用原始模型的目录。否则，将使用缓存目录。

#### `get_input_static_shapes`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_base.py#L451)

```py
( neuron_config: NeuronDefaultConfig )
```

获取具有其有效静态形状的输入字典。

#### `load_model`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_base.py#L98)

```py
( path: Union )
```

参数

+   `path`（`Union[str, Path]`）- 编译模型的路径。

加载由神经元（x）-cc 编译器编译的 TorchScript 模块。它将首先加载到 CPU，然后移动到一个或多个 [NeuronCore](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/general/arch/neuron-hardware/neuroncores-arch.html)。

#### `remove_padding`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_base.py#L548)

```py
( outputs: List dims: List indices: List padding_side: Literal = 'right' )
```

参数

+   `outputs`（`List[torch.Tensor]`）- 推理输出的 torch 张量列表。

+   `dims`（`List[int]`）- 我们在其中切片张量的维度列表。

+   `indices`（`List[int]`）- 我们沿着一个轴切片张量的索引列表。

+   `padding_side`（`Literal["right", "left"]`，默认为“right”）- 应用填充的一侧。

从输出张量中删除填充。

### NeuronDecoderModel

`NeuronDecoderModel` 类是文本生成模型的基类。

### `class optimum.neuron.NeuronDecoderModel`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_decoder.py#L76)

```py
( config: PretrainedConfig checkpoint_dir: Union compiled_dir: Union = None generation_config: Optional = None )
```

将预训练的变压器解码器模型转换并在神经元设备上运行的基类。

它实现了将预训练的变压器解码器模型转换为神经元变压器模型的方法：

+   将原始模型的检查点权重转移到优化的神经元图中，

+   使用神经元编译器编译生成的图。

常见属性：

+   model（`torch.nn.Module`）- 具有为神经元设备优化的图形的解码器模型。

+   config（[PretrainedConfig](https://huggingface.co/docs/transformers/main/en/main_classes/configuration#transformers.PretrainedConfig)）- 原始模型的配置。

+   generation_config（[GenerationConfig](https://huggingface.co/docs/transformers/main/en/main_classes/text_generation#transformers.GenerationConfig)）- 调用 `generate()` 时默认使用的生成配置。

## 自然语言处理

以下 Neuron 模型类适用于自然语言处理任务。

### 用于特征提取的 NeuronModelForFeatureExtraction

### `class optimum.neuron.NeuronModelForFeatureExtraction`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L122)

```py
( model: ScriptModule config: PretrainedConfig model_save_dir: Union = None model_file_name: Optional = None preprocessors: Optional = None neuron_config: Optional = None **kwargs )
```

参数

+   `config`（`transformers.PretrainedConfig`）— [PretrainedConfig](https://huggingface.co/docs/transformers/main_classes/configuration#transformers.PretrainedConfig)是具有模型所有参数的模型配置类。使用配置文件初始化不会加载与模型关联的权重，只加载配置。查看`optimum.neuron.modeling.NeuronBaseModel.from_pretrained`方法以加载模型权重。

+   `model`（`torch.jit._script.ScriptModule`）— [torch.jit._script.ScriptModule](https://pytorch.org/docs/stable/generated/torch.jit.ScriptModule.html)是由 neuron(x)编译器编译的嵌入 NEFF（Neuron Executable File Format）的 TorchScript 模块。

用于特征提取任务的带有 BaseModelOutput 的 Neuron 模型。

此模型继承自`~neuron.modeling.NeuronBaseModel`。查看超类文档以了解库为所有模型实现的通用方法（如下载或保存）

Neuron 设备上的特征提取模型。

#### `forward`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L135)

```py
( input_ids: Tensor attention_mask: Tensor token_type_ids: Optional = None **kwargs )
```

参数

+   `input_ids`（形状为`(batch_size, sequence_length)`的`torch.Tensor`）— 词汇表中输入序列标记的索引。可以使用[`AutoTokenizer`](https://huggingface.co/docs/transformers/autoclass_tutorial#autotokenizer)获取索引。有关详细信息，请参阅[`PreTrainedTokenizer.encode`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.encode)和[`PreTrainedTokenizer.__call__`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.__call__)。[什么是输入 ID？](https://huggingface.co/docs/transformers/glossary#input-ids)

+   `attention_mask`（形状为`(batch_size, sequence_length)`的`Union[torch.Tensor, None]`，默认为`None`）— 避免在填充标记索引上执行注意力的掩码。掩码值在`[0, 1]`中选择：

    +   1 用于“未被掩盖”的标记，

    +   0 用于“被掩盖”的标记。[什么是注意力掩码？](https://huggingface.co/docs/transformers/glossary#attention-mask)

+   `token_type_ids`（形状为`(batch_size, sequence_length)`的`Union[torch.Tensor, None]`，默认为`None`）— 分段标记索引，指示输入的第一部分和第二部分。索引在`[0, 1]`中选择：

    +   1 用于“句子 A”的标记，

    +   0 用于“句子 B”的标记。[什么是标记类型 ID？](https://huggingface.co/docs/transformers/glossary#token-type-ids)

NeuronModelForFeatureExtraction 的前向方法覆盖了`__call__`特殊方法。

虽然前向传递的步骤需要在此函数内定义，但应该在此之后调用`Module`实例，而不是在此处调用，因为前者负责运行预处理和后处理步骤，而后者会默默地忽略它们。

特征提取示例：*(以下模型使用 neuronx 编译器编译，只能在 INF2 上运行。如果您使用 INF1，请将“neuronx”替换为“neuron”)*

```py
>>> from transformers import AutoTokenizer
>>> from optimum.neuron import NeuronModelForFeatureExtraction

>>> tokenizer = AutoTokenizer.from_pretrained("optimum/all-MiniLM-L6-v2-neuronx")
>>> model = NeuronModelForFeatureExtraction.from_pretrained("optimum/all-MiniLM-L6-v2-neuronx")

>>> inputs = tokenizer("Dear Evan Hansen is the winner of six Tony Awards.", return_tensors="pt")

>>> outputs = model(**inputs)
>>> last_hidden_state = outputs.last_hidden_state
>>> list(last_hidden_state.shape)
[1, 13, 384]
```

### NeuronModelForSentenceTransformers

### `class optimum.neuron.NeuronModelForSentenceTransformers`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L195)

```py
( model: ScriptModule config: PretrainedConfig model_save_dir: Union = None model_file_name: Optional = None preprocessors: Optional = None neuron_config: Optional = None **kwargs )
```

参数

+   `config` (`transformers.PretrainedConfig`) — [PretrainedConfig](https://huggingface.co/docs/transformers/main_classes/configuration#transformers.PretrainedConfig) 是具有模型所有参数的模型配置类。使用配置文件初始化不会加载与模型关联的权重，只会加载配置。查看 `optimum.neuron.modeling.NeuronBaseModel.from_pretrained` 方法以加载模型权重。

+   `model` (`torch.jit._script.ScriptModule`) — [torch.jit._script.ScriptModule](https://pytorch.org/docs/stable/generated/torch.jit.ScriptModule.html) 是由 neuron(x) 编译器编译的嵌入 NEFF(Neuron Executable File Format) 的 TorchScript 模块。

句子转换模型的神经元模型。

该模型继承自`~neuron.modeling.NeuronBaseModel`。查看超类文档以了解库为所有模型实现的通用方法（如下载或保存）

Neuron 设备上的句子转换模型。

#### `forward`

[< source >](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L208)

```py
( input_ids: Tensor attention_mask: Tensor token_type_ids: Optional = None **kwargs )
```

参数

+   `input_ids` (`torch.Tensor` of shape `(batch_size, sequence_length)`) — 词汇表中输入序列标记的索引。可以使用 [`AutoTokenizer`](https://huggingface.co/docs/transformers/autoclass_tutorial#autotokenizer) 获取索引。有关详细信息，请参阅 [`PreTrainedTokenizer.encode`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.encode) 和 [`PreTrainedTokenizer.__call__`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.__call__)。[什么是输入 ID？](https://huggingface.co/docs/transformers/glossary#input-ids)

+   `attention_mask` (`Union[torch.Tensor, None]` of shape `(batch_size, sequence_length)`, defaults to `None`) — 用于避免在填充标记索引上执行注意力的掩码。选择的掩码值在 `[0, 1]` 范围内：

    +   对于未被遮罩的标记为 1，

    +   对于被遮罩的标记为 0。[什么是注意力掩码？](https://huggingface.co/docs/transformers/glossary#attention-mask)

+   `token_type_ids` (`Union[torch.Tensor, None]` of shape `(batch_size, sequence_length)`, defaults to `None`) — 段标记索引，用于指示输入的第一部分和第二部分。索引在 `[0, 1]` 范围内选择：

    +   对于`句子 A`的标记为 1，

    +   对于`句子 B`的标记为 0。[什么是标记类型 ID？](https://huggingface.co/docs/transformers/glossary#token-type-ids)

NeuronModelForSentenceTransformers 的前向方法覆盖了 `__call__` 特殊方法。

虽然前向传递的步骤需要在此函数内定义，但应该在此之后调用 `Module` 实例，而不是在此处调用，因为前者会负责运行前后处理步骤，而后者会默默地忽略它们。

文本句子转换模型示例：

```py
>>> from transformers import AutoTokenizer
>>> from optimum.neuron import NeuronModelForSentenceTransformers

>>> tokenizer = AutoTokenizer.from_pretrained("optimum/bge-base-en-v1.5-neuronx")
>>> model = NeuronModelForSentenceTransformers.from_pretrained("optimum/bge-base-en-v1.5-neuronx")

>>> inputs = tokenizer("In the smouldering promise of the fall of Troy, a mythical world of gods and mortals rises from the ashes.", return_tensors="pt")

>>> outputs = model(**inputs)
>>> token_embeddings = outputs.token_embeddings
>>> sentence_embedding = = outputs.sentence_embedding
```

### NeuronModelForMaskedLM

### `class optimum.neuron.NeuronModelForMaskedLM`

[< source >](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L266)

```py
( model: ScriptModule config: PretrainedConfig model_save_dir: Union = None model_file_name: Optional = None preprocessors: Optional = None neuron_config: Optional = None **kwargs )
```

参数

+   `config` (`transformers.PretrainedConfig`) — [PretrainedConfig](https://huggingface.co/docs/transformers/main_classes/configuration#transformers.PretrainedConfig) 是具有模型所有参数的模型配置类。使用配置文件初始化不会加载与模型关联的权重，只会加载配置。查看 `optimum.neuron.modeling.NeuronBaseModel.from_pretrained` 方法以加载模型权重。

+   `model` (`torch.jit._script.ScriptModule`) — [torch.jit._script.ScriptModule](https://pytorch.org/docs/stable/generated/torch.jit.ScriptModule.html) 是由 neuron(x) 编译器编译的嵌入 NEFF(Neuron Executable File Format) 的 TorchScript 模块。

带有 MaskedLMOutput 用于遮罩语言建模任务的神经元模型。

此模型继承自`~neuron.modeling.NeuronBaseModel`。查看超类文档以了解库为所有模型实现的通用方法（如下载或保存）

神经元设备上的掩码语言模型。

#### `forward`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L279)

```py
( input_ids: Tensor attention_mask: Tensor token_type_ids: Optional = None **kwargs )
```

参数

+   `input_ids` (`torch.Tensor`，形状为`(batch_size, sequence_length)`) — 词汇表中输入序列标记的索引。可以使用[`AutoTokenizer`](https://huggingface.co/docs/transformers/autoclass_tutorial#autotokenizer)获取索引。查看[`PreTrainedTokenizer.encode`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.encode)和[`PreTrainedTokenizer.__call__`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.__call__)以获取详细信息。[什么是输入 ID？](https://huggingface.co/docs/transformers/glossary#input-ids)

+   `attention_mask` (`Union[torch.Tensor, None]`，形状为`(batch_size, sequence_length)`，默认为`None`) — 用于避免在填充标记索引上执行注意力的掩码。选择的掩码值在`[0, 1]`中：

    +   对于被“未掩码”的标记为 1，

    +   对于被“掩码”的标记为 0。[什么是注意力掩码？](https://huggingface.co/docs/transformers/glossary#attention-mask)

+   `token_type_ids` (`Union[torch.Tensor, None]`，形状为`(batch_size, sequence_length)`，默认为`None`) — 段标记索引，指示输入的第一部分和第二部分。索引在`[0, 1]`中选择：

    +   对于被“句子 A”的标记为 1，

    +   对于被“句子 B”的标记为 0。[什么是标记类型 ID？](https://huggingface.co/docs/transformers/glossary#token-type-ids)

NeuronModelForMaskedLM 的前向方法，覆盖了`__call__`特殊方法。

尽管前向传递的方法需要在这个函数内定义，但应该在此之后调用`Module`实例，而不是这个，因为前者负责运行前处理和后处理步骤，而后者则默默地忽略它们。

填充掩码的示例：*(以下模型使用 neuronx 编译器编译，并且只能在 INF2 上运行。如果您使用 INF1，请将“neuronx”替换为“neuron”)*

```py
>>> from transformers import AutoTokenizer
>>> from optimum.neuron import NeuronModelForMaskedLM
>>> import torch

>>> tokenizer = AutoTokenizer.from_pretrained("optimum/legal-bert-base-uncased-neuronx")
>>> model = NeuronModelForMaskedLM.from_pretrained("optimum/legal-bert-base-uncased-neuronx")

>>> inputs = tokenizer("This [MASK] Agreement is between General Motors and John Murray.", return_tensors="pt")

>>> outputs = model(**inputs)
>>> logits = outputs.logits
>>> list(logits.shape)
[1, 13, 30522]
```

### NeuronModelForSequenceClassification

### `class optimum.neuron.NeuronModelForSequenceClassification`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L404)

```py
( model: ScriptModule config: PretrainedConfig model_save_dir: Union = None model_file_name: Optional = None preprocessors: Optional = None neuron_config: Optional = None **kwargs )
```

参数

+   `config` (`transformers.PretrainedConfig`) — [PretrainedConfig](https://huggingface.co/docs/transformers/main_classes/configuration#transformers.PretrainedConfig) 是具有模型所有参数的模型配置类。使用配置文件初始化不会加载与模型相关的权重，只加载配置。查看`optimum.neuron.modeling.NeuronBaseModel.from_pretrained`方法以加载模型权重。

+   `model` (`torch.jit._script.ScriptModule`) — [torch.jit._script.ScriptModule](https://pytorch.org/docs/stable/generated/torch.jit.ScriptModule.html) 是带有由 neuron(x)编译器编译的嵌入式 NEFF（Neuron Executable File Format）的 TorchScript 模块。

在顶部带有序列分类/回归头部的神经元模型（在汇总输出的顶部有一个线性层），例如用于 GLUE 任务。

此模型继承自`~neuron.modeling.NeuronBaseModel`。查看超类文档以了解库为所有模型实现的通用方法（如下载或保存）

神经元设备上的序列分类模型。

#### `forward`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L418)

```py
( input_ids: Tensor attention_mask: Tensor token_type_ids: Optional = None **kwargs )
```

参数

+   `input_ids`（形状为`(batch_size, sequence_length)`的`torch.Tensor`）- 词汇表中输入序列标记的索引。可以使用[`AutoTokenizer`](https://huggingface.co/docs/transformers/autoclass_tutorial#autotokenizer)获取索引。有关详细信息，请参阅[`PreTrainedTokenizer.encode`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.encode)和[`PreTrainedTokenizer.__call__`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.__call__)。[什么是输入 ID？](https://huggingface.co/docs/transformers/glossary#input-ids)

+   `attention_mask`（形状为`(batch_size, sequence_length)`的`Union[torch.Tensor, None]`，默认为`None`）- 避免在填充标记索引上执行注意力的掩码。掩码值选在`[0, 1]`之间：

    +   对于未被`masked`的标记为 1，

    +   对于被`masked`的标记为 0。[什么是注意力掩码？](https://huggingface.co/docs/transformers/glossary#attention-mask)

+   `token_type_ids`（形状为`(batch_size, sequence_length)`的`Union[torch.Tensor, None]`，默认为`None`）- 段标记索引，指示输入的第一部分和第二部分。索引选在`[0, 1]`之间：

    +   对于被`sentence A`的标记为 1。

    +   对于被`sentence B`的标记为 0。[什么是标记类型 ID？](https://huggingface.co/docs/transformers/glossary#token-type-ids)

NeuronModelForSequenceClassification 的前向方法，覆盖了`__call__`特殊方法。

虽然前向传递的步骤需要在此函数内定义，但应该在此之后调用`Module`实例，而不是在此处调用，因为前者会负责运行前后处理步骤，而后者会默默地忽略它们。

单标签分类示例：*(以下模型使用 neuronx 编译器编译，并且只能在 INF2 上运行。)*

```py
>>> from transformers import AutoTokenizer
>>> from optimum.neuron import NeuronModelForSequenceClassification

>>> tokenizer = AutoTokenizer.from_pretrained("optimum/distilbert-base-uncased-finetuned-sst-2-english-neuronx")
>>> model = NeuronModelForSequenceClassification.from_pretrained("optimum/distilbert-base-uncased-finetuned-sst-2-english-neuronx")

>>> inputs = tokenizer("Hamilton is considered to be the best musical of human history.", return_tensors="pt")

>>> outputs = model(**inputs)
>>> logits = outputs.logits
>>> list(logits.shape)
[1, 2]
```

### NeuronModelForQuestionAnswering

### `class optimum.neuron.NeuronModelForQuestionAnswering`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L336)

```py
( model: ScriptModule config: PretrainedConfig model_save_dir: Union = None model_file_name: Optional = None preprocessors: Optional = None neuron_config: Optional = None **kwargs )
```

参数

+   `config`（`transformers.PretrainedConfig`）- [PretrainedConfig](https://huggingface.co/docs/transformers/main_classes/configuration#transformers.PretrainedConfig)是具有模型所有参数的模型配置类。使用配置文件初始化不会加载与模型关联的权重，只加载配置。查看`optimum.neuron.modeling.NeuronBaseModel.from_pretrained`方法以加载模型权重。

+   `model`（`torch.jit._script.ScriptModule`）- [torch.jit._script.ScriptModule](https://pytorch.org/docs/stable/generated/torch.jit.ScriptModule.html)是由 neuron(x)编译器编译的嵌入 NEFF（Neuron Executable File Format）的 TorchScript 模块。

具有用于提取式问答任务（如 SQuAD）的 QuestionAnsweringModelOutput 的 Neuron 模型。

此模型继承自`~neuron.modeling.NeuronBaseModel`。查看超类文档以了解库为所有模型实现的通用方法（如下载或保存）

Neuron 设备上的问答模型。

#### `forward`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L349)

```py
( input_ids: Tensor attention_mask: Tensor token_type_ids: Optional = None **kwargs )
```

参数

+   `input_ids`（形状为`(batch_size, sequence_length)`的`torch.Tensor`）- 词汇表中输入序列标记的索引。可以使用[`AutoTokenizer`](https://huggingface.co/docs/transformers/autoclass_tutorial#autotokenizer)获取索引。有关详细信息，请参阅[`PreTrainedTokenizer.encode`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.encode)和[`PreTrainedTokenizer.__call__`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.__call__)。[什么是输入 ID？](https://huggingface.co/docs/transformers/glossary#input-ids)

+   `attention_mask`（`Union[torch.Tensor, None]`，形状为`(batch_size, sequence_length)`，默认为`None`）— 用于避免在填充标记索引上执行注意力的掩码。选择的掩码值在`[0, 1]`范围内：

    +   对于未被`masked`的标记为 1，

    +   对于被`masked`的标记为 0。[什么是注意力掩码？](https://huggingface.co/docs/transformers/glossary#attention-mask)

+   `token_type_ids`（`Union[torch.Tensor, None]`，形状为`(batch_size, sequence_length)`，默认为`None`）— 段标记索引，指示输入的第一部分和第二部分。索引在`[0, 1]`范围内选择：

    +   对于被`sentence A`的标记为 1，

    +   对于被`sentence B`的标记为 0。[什么是标记类型 ID？](https://huggingface.co/docs/transformers/glossary#token-type-ids)

NeuronModelForQuestionAnswering 的前向方法覆盖了`__call__`特殊方法。

虽然前向传递的步骤需要在此函数内定义，但应该在此之后调用`Module`实例，而不是在此处调用，因为前者会负责运行前处理和后处理步骤，而后者会默默地忽略它们。

问答示例：*(以下模型使用 neuronx 编译器编译，只能在 INF2 上运行。)*

```py
>>> import torch
>>> from transformers import AutoTokenizer
>>> from optimum.neuron import NeuronModelForQuestionAnswering

>>> tokenizer = AutoTokenizer.from_pretrained("optimum/roberta-base-squad2-neuronx")
>>> model = NeuronModelForQuestionAnswering.from_pretrained("optimum/roberta-base-squad2-neuronx")

>>> question, text = "Are there wheelchair spaces in the theatres?", "Yes, we have reserved wheelchair spaces with a good view."
>>> inputs = tokenizer(question, text, return_tensors="pt")
>>> start_positions = torch.tensor([1])
>>> end_positions = torch.tensor([12])

>>> outputs = model(**inputs, start_positions=start_positions, end_positions=end_positions)
>>> start_scores = outputs.start_logits
>>> end_scores = outputs.end_logits
```

### NeuronModelForTokenClassification

### `class optimum.neuron.NeuronModelForTokenClassification`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L472)

```py
( model: ScriptModule config: PretrainedConfig model_save_dir: Union = None model_file_name: Optional = None preprocessors: Optional = None neuron_config: Optional = None **kwargs )
```

参数

+   `config`（`transformers.PretrainedConfig`）— [PretrainedConfig](https://huggingface.co/docs/transformers/main_classes/configuration#transformers.PretrainedConfig)是具有模型所有参数的模型配置类。使用配置文件初始化不会加载与模型关联的权重，只加载配置。查看`optimum.neuron.modeling.NeuronBaseModel.from_pretrained`方法以加载模型权重。

+   `model`（`torch.jit._script.ScriptModule`）— [torch.jit._script.ScriptModule](https://pytorch.org/docs/stable/generated/torch.jit.ScriptModule.html)是由 neuron(x)编译器编译的带有嵌入 NEFF（Neuron Executable File Format）的 TorchScript 模块。

神经元模型，顶部带有一个标记分类头（隐藏状态输出顶部的线性层），例如用于命名实体识别（NER）任务。

此模型继承自`~neuron.modeling.NeuronBaseModel`。查看超类文档以了解库为所有模型实现的通用方法（例如下载或保存）

神经元设备上的标记分类模型。

#### `forward`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L486)

```py
( input_ids: Tensor attention_mask: Tensor token_type_ids: Optional = None **kwargs )
```

参数

+   `input_ids`（形状为`(batch_size, sequence_length)`的`torch.Tensor`）— 词汇表中输入序列标记的索引。可以使用[`AutoTokenizer`](https://huggingface.co/docs/transformers/autoclass_tutorial#autotokenizer)获取索引。有关详细信息，请参阅[`PreTrainedTokenizer.encode`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.encode)和[`PreTrainedTokenizer.__call__`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.__call__)。[什么是输入 ID？](https://huggingface.co/docs/transformers/glossary#input-ids)

+   `attention_mask`（`Union[torch.Tensor, None]`，形状为`(batch_size, sequence_length)`，默认为`None`）— 用于避免在填充标记索引上执行注意力的掩码。选择的掩码值在`[0, 1]`范围内：

    +   对于未被`masked`的标记为 1，

    +   对于被`masked`的标记为 0。[什么是注意力掩码？](https://huggingface.co/docs/transformers/glossary#attention-mask)

+   `token_type_ids`（`Union[torch.Tensor, None]`，形状为`(batch_size, sequence_length)`，默认为`None`）— 段标记索引，指示输入的第一部分和第二部分。索引在`[0, 1]`范围内选择：

    +   1 表示“句子 A”,

    +   0 表示“句子 B”的标记。[什么是标记类型 ID？](https://huggingface.co/docs/transformers/glossary#token-type-ids)

NeuronModelForTokenClassification 的前向方法，覆盖了`__call__`特殊方法。

虽然前向传递的步骤需要在此函数内定义，但应该在此之后调用`Module`实例，而不是在此处调用，因为前者会负责运行预处理和后处理步骤，而后者会默默地忽略它们。

标记分类示例：*(以下模型使用 neuronx 编译器编译，只能在 INF2 上运行。)*

```py
>>> from transformers import AutoTokenizer
>>> from optimum.neuron import NeuronModelForTokenClassification

>>> tokenizer = AutoTokenizer.from_pretrained("optimum/bert-base-NER-neuronx")
>>> model = NeuronModelForTokenClassification.from_pretrained("optimum/bert-base-NER-neuronx")

>>> inputs = tokenizer("Lin-Manuel Miranda is an American songwriter, actor, singer, filmmaker, and playwright.", return_tensors="pt")

>>> outputs = model(**inputs)
>>> logits = outputs.logits
>>> list(logits.shape)
[1, 20, 9]
```

### NeuronModelForMultipleChoice

### `class optimum.neuron.NeuronModelForMultipleChoice`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L553)

```py
( model: ScriptModule config: PretrainedConfig model_save_dir: Union = None model_file_name: Optional = None preprocessors: Optional = None neuron_config: Optional = None **kwargs )
```

参数

+   `config`（`transformers.PretrainedConfig`）- [PretrainedConfig](https://huggingface.co/docs/transformers/main_classes/configuration#transformers.PretrainedConfig)是具有模型所有参数的模型配置类。使用配置文件初始化不会加载与模型关联的权重，只加载配置。查看`optimum.neuron.modeling.NeuronBaseModel.from_pretrained`方法以加载模型权重。

+   `model`（`torch.jit._script.ScriptModule`）- [torch.jit._script.ScriptModule](https://pytorch.org/docs/stable/generated/torch.jit.ScriptModule.html)是由 neuron(x)编译器编译的嵌入 NEFF（Neuron Executable File Format）的 TorchScript 模块。

带有多选分类头部的神经元模型（池化输出顶部的线性层和 softmax），例如用于 RocStories/SWAG 任务。

此模型继承自`~neuron.modeling.NeuronBaseModel`。查看超类文档以了解库为所有模型实现的通用方法（如下载或保存）

在神经元设备上的多选模型。

#### `forward`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L567)

```py
( input_ids: Tensor attention_mask: Tensor token_type_ids: Optional = None **kwargs )
```

参数

+   `input_ids`（形状为`(batch_size, num_choices, sequence_length)`的`torch.Tensor`）- 词汇表中输入序列标记的索引。可以使用[`AutoTokenizer`](https://huggingface.co/docs/transformers/autoclass_tutorial#autotokenizer)获取索引。有关详细信息，请参阅[`PreTrainedTokenizer.encode`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.encode)和[`PreTrainedTokenizer.__call__`](https://huggingface.co/docs/transformers/main_classes/tokenizer#transformers.PreTrainedTokenizerBase.__call__)。[什么是输入 ID？](https://huggingface.co/docs/transformers/glossary#input-ids)

+   `attention_mask`（形状为`(batch_size, num_choices, sequence_length)`的`Union[torch.Tensor, None]`，默认为`None`）- 用于避免在填充标记索引上执行注意力的掩码。掩码值在`[0, 1]`中选择：

    +   1 表示“未被掩盖”的标记，

    +   0 表示“被掩盖”的标记。[什么是注意力掩码？](https://huggingface.co/docs/transformers/glossary#attention-mask)

+   `token_type_ids`（形状为`(batch_size, num_choices, sequence_length)`的`Union[torch.Tensor, None]`，默认为`None`）- 段标记索引，指示输入的第一部分和第二部分。索引在`[0, 1]`中选择：

    +   1 表示“句子 A”,

    +   0 表示“句子 B”的标记。[什么是标记类型 ID？](https://huggingface.co/docs/transformers/glossary#token-type-ids)

NeuronModelForMultipleChoice 的前向方法，覆盖了`__call__`特殊方法。

虽然前向传递的配方需要在此函数内定义，但应该在此之后调用`Module`实例，而不是在此之后调用，因为前者负责运行前处理和后处理步骤，而后者会默默地忽略它们。

多项选择示例：*(以下模型使用神经元编译器编译，只能在 INF2 上运行。)*

```py
>>> from transformers import AutoTokenizer
>>> from optimum.neuron import NeuronModelForMultipleChoice

>>> tokenizer = AutoTokenizer.from_pretrained("optimum/bert-base-uncased_SWAG-neuronx")
>>> model = NeuronModelForMultipleChoice.from_pretrained("optimum/bert-base-uncased_SWAG-neuronx", export=True)

>>> num_choices = 4
>>> first_sentence = ["Members of the procession walk down the street holding small horn brass instruments."] * num_choices
>>> second_sentence = [
...     "A drum line passes by walking down the street playing their instruments.",
...     "A drum line has heard approaching them.",
...     "A drum line arrives and they're outside dancing and asleep.",
...     "A drum line turns the lead singer watches the performance."
... ]
>>> inputs = tokenizer(first_sentence, second_sentence, truncation=True, padding=True)

# Unflatten the inputs values expanding it to the shape [batch_size, num_choices, seq_length]
>>> for k, v in inputs.items():
...     inputs[k] = [v[i: i + num_choices] for i in range(0, len(v), num_choices)]
>>> inputs = dict(inputs.convert_to_tensors(tensor_type="pt"))
>>> outputs = model(**inputs)
>>> logits = outputs.logits
>>> logits.shape
[1, 4]
```

### NeuronModelForCausalLM

### `class optimum.neuron.NeuronModelForCausalLM`

[< source >](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L640)

```py
( config: PretrainedConfig checkpoint_dir: Union compiled_dir: Union = None generation_config: Optional = None )
```

参数

+   `model` (`torch.nn.Module`) — [torch.nn.Module](https://pytorch.org/docs/stable/generated/torch.nn.Module.html) 是神经元解码器图。

+   `config` (`transformers.PretrainedConfig`) — [PretrainedConfig](https://huggingface.co/docs/transformers/main_classes/configuration#transformers.PretrainedConfig) 是包含模型所有参数的模型配置类。

+   `model_path` (`Path`) — 存储模型编译成果的目录。如果模型以前从未在本地保存过，则可以是一个临时目录。

+   `generation_config` (`transformers.GenerationConfig`) — [GenerationConfig](https://huggingface.co/docs/transformers/main_classes/text_generation#transformers.GenerationConfig) 包含模型生成任务的配置。

神经元模型，带有用于神经元设备推理的因果语言建模头。

该模型继承自`~neuron.modeling.NeuronDecoderModel`。请查看超类文档，了解库为所有模型实现的通用方法（如下载或保存）。

#### `can_generate`

[< source >](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L717)

```py
( )
```

返回 True 以验证在`GenerationMixin.generate()`中进行的检查。

#### `forward`

[< source >](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L667)

```py
( input_ids: Tensor cache_ids: Tensor start_ids: Tensor = None return_dict: bool = True )
```

参数

+   `input_ids` (`torch.LongTensor`) — 解码器输入序列标记的索引，形状为`(batch_size, sequence_length)`。

+   `cache_ids` (`torch.LongTensor`) — 需要存储当前输入的缓存键和值的索引。

+   `start_ids` (`torch.LongTensor`) — 要处理的第一个标记的索引，从注意力掩码中推断出。

NeuronModelForCausalLM 的前向方法重写了`__call__`特殊方法。

虽然前向传递的配方需要在此函数内定义，但应该在此之后调用`Module`实例，而不是在此之后调用，因为前者负责运行前处理和后处理步骤，而后者会默默地忽略它们。

文本生成示例：

```py
>>> from transformers import AutoTokenizer
>>> from optimum.neuron import NeuronModelForCausalLM
>>> import torch

>>> tokenizer = AutoTokenizer.from_pretrained("gpt2")
>>> model = NeuronModelForCausalLM.from_pretrained("gpt2", export=True)

>>> inputs = tokenizer("My favorite moment of the day is", return_tensors="pt")

>>> gen_tokens = model.generate(**inputs, do_sample=True, temperature=0.9, min_length=20, max_length=20)
>>> tokenizer.batch_decode(gen_tokens)
```

#### `generate`

[< source >](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L721)

```py
( input_ids: Tensor attention_mask: Optional = None generation_config: Optional = None stopping_criteria: Optional = None **kwargs ) → export const metadata = 'undefined';torch.Tensor
```

参数

+   `input_ids` (`torch.Tensor`，形状为`(batch_size, sequence_length)`) — 用作生成提示的序列。

+   `attention_mask` (`torch.Tensor`，形状为`(batch_size, sequence_length)`，*可选*) — 用于避免在填充标记索引上执行注意力的掩码。

+   `generation_config` (`~transformers.generation.GenerationConfig`，*可选*) — 用作生成调用的基本参数化的生成配置。传递给与`generation_config`属性匹配的`**kwargs`将覆盖它们。如果未提供`generation_config`，将使用默认值，其加载优先级如下：1) 从`generation_config.json`模型文件中；2) 从模型配置中。请注意，未指定的参数将继承`GenerationConfig`的默认值，应检查其文档以参数化生成。

+   `stopping_criteria` (`Optional[transformers.generation.StoppingCriteriaList]，默认为`None`) — 自定义停止标准，补充了从参数和生成配置构建的默认停止标准。

返回

`torch.Tensor`

一个`torch.FloatTensor`。

一个简化的 generate()方法，覆盖了 transformers.GenerationMixin.generate()方法。

该方法使用与 transformers 库`generate()`方法相同的 logits 处理器/变形器和停止标准，但将生成限制为贪婪搜索和抽样。

它不支持 transformers `generate()` 的高级选项。

有关生成配置的详细信息，请参阅[`huggingface.co/docs/transformers/en/main_classes/text_generation#transformers.GenerationMixin.generate`](https://huggingface.co/docs/transformers/en/main_classes/text_generation#transformers.GenerationMixin.generate)。

#### `generate_tokens`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling.py#L802)

```py
( input_ids: LongTensor selector: TokenSelector batch_size: int attention_mask: Optional = None **model_kwargs ) → export const metadata = 'undefined';torch.LongTensor
```

参数

+   `input_ids` (`torch.LongTensor`，形状为`(batch_size, sequence_length)`) — 用作生成提示的序列。

+   `selector` (`TokenSelector`) — 基于 transformers 处理器和停止标准实现生成逻辑的对象。

+   `batch_size` (`int`) — 实际输入批处理大小。用于避免为填充输入生成标记。

+   `attention_mask` (`torch.Tensor`，形状为`(batch_size, sequence_length)`，*可选*) — 用于避免在填充标记索引上执行注意力的掩码。model_kwargs — 额外的模型特定 kwargs 将被转发到模型的`forward`函数。

返回

`torch.LongTensor`

包含生成标记的`torch.LongTensor`。

使用抽样或贪婪搜索生成标记。

## 稳定扩散

### NeuronStableDiffusionPipelineBase

### `class optimum.neuron.modeling_diffusion.NeuronStableDiffusionPipelineBase`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_diffusion.py#L81)

```py
( text_encoder: ScriptModule unet: ScriptModule vae_decoder: Union config: Dict tokenizer: CLIPTokenizer scheduler: Union data_parallel_mode: str vae_encoder: Union = None text_encoder_2: Union = None tokenizer_2: Optional = None feature_extractor: Optional = None configs: Optional = None neuron_configs: Optional = None model_save_dir: Union = None model_and_config_save_paths: Optional = None )
```

#### `load_model`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_diffusion.py#L243)

```py
( data_parallel_mode: Optional text_encoder_path: Union unet_path: Union vae_decoder_path: Union = None vae_encoder_path: Union = None text_encoder_2_path: Union = None dynamic_batch_size: bool = False )
```

参数

+   `data_parallel_mode` (`Optional[str]`) — 决定将哪些组件加载到 Neuron 设备的两个 NeuronCores 中的模式。可以是“none”（无数据并行），“unet”（仅将 unet 加载到每个设备的两个核心中），“all”（将整个管道加载到两个核心中）。

+   `text_encoder_path` (`Union[str, Path]`) — 编译后的文本编码器路径。

+   `unet_path` (`Union[str, Path]`) — 编译后的 U-NET 路径。

+   `vae_decoder_path` (`Optional[Union[str, Path]]`，默认为`None`) — 编译后的 VAE 解码器路径。

+   `vae_encoder_path` (`Optional[Union[str, Path]]`，默认为`None`) — 编译后的 VAE 编码器路径。这是可选的，仅用于接受图像作为输入的任务。

+   `text_encoder_2_path` (`Optional[Union[str, Path]]`，默认为`None`) — 编译后的第二个冻结文本编码器的路径。仅适用于 SDXL。

+   `dynamic_batch_size` (`bool`，默认为`False`) — 是否为神经元编译模型启用动态批处理大小。如果为`True`，输入批处理大小可以是编译期间批处理大小的倍数。

加载由 neuron(x)-cc 编译器编译的稳定扩散 TorchScript 模块。首先加载到 CPU，然后移动到一个或多个[NeuronCore](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/general/arch/neuron-hardware/neuroncores-arch.html)。

### NeuronStableDiffusionPipeline

### `class optimum.neuron.NeuronStableDiffusionPipeline`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_diffusion.py#L798)

```py
( text_encoder: ScriptModule unet: ScriptModule vae_decoder: Union config: Dict tokenizer: CLIPTokenizer scheduler: Union data_parallel_mode: str vae_encoder: Union = None text_encoder_2: Union = None tokenizer_2: Optional = None feature_extractor: Optional = None configs: Optional = None neuron_configs: Optional = None model_save_dir: Union = None model_and_config_save_paths: Optional = None )
```

#### `__call__`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/pipelines/diffusers/pipeline_stable_diffusion.py#L33)

```py
( prompt: Union = None num_inference_steps: int = 50 guidance_scale: float = 7.5 negative_prompt: Union = None num_images_per_prompt: int = 1 eta: float = 0.0 generator: Union = None latents: Optional = None prompt_embeds: Optional = None negative_prompt_embeds: Optional = None output_type: Optional = 'pil' return_dict: bool = True callback: Optional = None callback_steps: int = 1 cross_attention_kwargs: Optional = None guidance_rescale: float = 0.0 ) → export const metadata = 'undefined';diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput or tuple
```

参数

+   `prompt` (`Optional[Union[str, List[str]]]`，默认为`None`) — 用于指导图像生成的提示或提示。如果未定义，则需要传递`prompt_embeds`。

+   `num_inference_steps` (`int`, 默认为 50) — 降噪步骤的数量。更多的降噪步骤通常会导致图像质量更高，但推理速度较慢。

+   `guidance_scale` (`float`, 默认为 7.5) — 更高的引导比例值鼓励模型生成与文本 `prompt` 密切相关的图像，但会降低图像质量。当 `guidance_scale > 1` 时启用引导比例。

+   `negative_prompt` (`Optional[Union[str, List[str]]]`, 默认为 `None`) — 用于指导图像生成中不包含的提示或提示。如果未定义，则需要传递 `negative_prompt_embeds`。在不使用引导时（`guidance_scale < 1`）忽略。

+   `num_images_per_prompt` (`int`, 默认为 1) — 每个提示生成的图像数量。如果与编译使用的批处理大小不同，则会被神经元的静态批处理大小覆盖（除了动态批处理）。

+   `eta` (`float`, 默认为 0.0) — 对应于 [DDIM](https://arxiv.org/abs/2010.02502) 论文中的参数 eta (η)。仅适用于 `diffusers.schedulers.DDIMScheduler`，在其他调度程序中被忽略。

+   `generator` (`Optional[Union[torch.Generator, List[torch.Generator]]]`, 默认为 `None`) — 用于使生成过程确定性的 [`torch.Generator`](https://pytorch.org/docs/stable/generated/torch.Generator.html)。

+   `latents` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 从高斯分布中采样的预生成噪声潜变量，用作图像生成的输入。可用于使用不同提示微调相同生成。如果未提供，则通过使用提供的随机 `generator` 进行采样生成潜变量张量。

+   `prompt_embeds` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预生成的文本嵌入。可用于轻松微调文本输入（提示加权）。如果未提供，则从 `prompt` 输入参数生成文本嵌入。

+   `negative_prompt_embeds` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预生成的负面文本嵌入。可用于轻松微调文本输入（提示加权）。如果未提供，`negative_prompt_embeds` 将从 `negative_prompt` 输入参数生成。

+   `output_type` (`Optional[str]`, 默认为 `"pil"`) — 生成图像的输出格式。选择 `PIL.Image` 或 `np.array` 之间的一个。

+   `return_dict` (`bool`, 默认为 `True`) — 是否返回 `diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput` 而不是普通的 `tuple`。

+   `callback` (`Optional[Callable]`, 默认为 `None`) — 在推理过程中每隔 `callback_steps` 步调用的函数。该函数使用以下参数调用：`callback(step: int, timestep: int, latents: torch.FloatTensor)`。

+   `callback_steps` (`int`, 默认为 1) — 调用 `callback` 函数的频率。如果未指定，则在每一步调用回调。

+   `cross_attention_kwargs` (`dict`, 默认为 `None`) — 如果指定了，则作为参数传递给 [`self.processor`](https://github.com/huggingface/diffusers/blob/main/src/diffusers/models/attention_processor.py) 中定义的 `AttentionProcessor`。

+   `guidance_rescale` (`float`, 默认为 0.0) — 来自 [Common Diffusion Noise Schedules and Sample Steps are Flawed](https://arxiv.org/pdf/2305.08891.pdf) 的引导重新缩放因子。引导重新缩放因子应在使用零终端 SNR 时修复过曝光。

返回

`diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput` 或 `tuple`

如果 `return_dict` 为 `True`，则返回 `diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput`，否则返回一个 `tuple`，其中第一个元素是生成的图像列表，第二个元素是一个包含对应生成图像是否包含“不适宜工作场所”（nsfw）内容的 `bool` 列表。

用于生成的管道的调用函数。

示例：

```py
>>> from optimum.neuron import NeuronStableDiffusionPipeline

>>> compiler_args = {"auto_cast": "matmul", "auto_cast_type": "bf16"}
>>> input_shapes = {"batch_size": 1, "height": 512, "width": 512}

>>> stable_diffusion = NeuronStableDiffusionPipeline.from_pretrained(
...     "runwayml/stable-diffusion-v1-5", export=True, **compiler_args, **input_shapes
... )
>>> stable_diffusion.save_pretrained("sd_neuron/")

>>> prompt = "a photo of an astronaut riding a horse on mars"
>>> image = stable_diffusion(prompt).images[0]
```

### NeuronStableDiffusionImg2ImgPipeline

### `class optimum.neuron.NeuronStableDiffusionImg2ImgPipeline`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_diffusion.py#L802)

```py
( text_encoder: ScriptModule unet: ScriptModule vae_decoder: Union config: Dict tokenizer: CLIPTokenizer scheduler: Union data_parallel_mode: str vae_encoder: Union = None text_encoder_2: Union = None tokenizer_2: Optional = None feature_extractor: Optional = None configs: Optional = None neuron_configs: Optional = None model_save_dir: Union = None model_and_config_save_paths: Optional = None )
```

#### `__call__`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/pipelines/diffusers/pipeline_stable_diffusion_img2img.py#L83)

```py
( prompt: Union = None image: Optional = None strength: float = 0.8 num_inference_steps: int = 50 guidance_scale: float = 7.5 negative_prompt: Union = None num_images_per_prompt: int = 1 eta: float = 0.0 generator: Optional = None prompt_embeds: Optional = None negative_prompt_embeds: Optional = None output_type: str = 'pil' return_dict: bool = True callback: Optional = None callback_steps: int = 1 cross_attention_kwargs: Optional = None ) → export const metadata = 'undefined';diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput or tuple
```

参数

+   `prompt` (`Optional[Union[str, List[str]]]`, 默认为 `None`) — 用于指导图像生成的提示或提示。如果未定义，则需要传递`prompt_embeds`。

+   `image` (`Optional["PipelineImageInput"]`, 默认为 `None`) — 代表要用作起始点的图像批处理的`Image`、numpy 数组或张量。对于 numpy 数组和 pytorch 张量，期望值范围在`[0, 1]`之间。如果是张量或张量列表，期望形状应为`(B, C, H, W)`或`(C, H, W)`。如果是 numpy 数组或数组列表，期望形状应为`(B, H, W, C)`或`(H, W, C)`。它还可以接受图像潜变量作为`image`，但如果直接传递潜变量，则不会再次编码。

+   `strength` (`float`, 默认为 0.8) — 表示转换参考`image`的程度。必须在 0 和 1 之间。`image`用作起始点，随着`strength`的增加，添加的噪音越多。降噪步骤的数量取决于最初添加的噪音量。当`strength`为 1 时，添加的噪音最大，降噪过程将运行指定的`num_inference_steps`次迭代。值为 1 基本上忽略`image`。

+   `num_inference_steps` (`int`, 默认为 50) — 降噪步骤的数量。更多的降噪步骤通常会导致更高质量的图像，但会降低推理速度。此参数由`strength`调节。

+   `guidance_scale` (`float`, 默认为 7.5) — 更高的引导比例值鼓励模型生成与文本`prompt`密切相关的图像，但会降低图像质量。当`guidance_scale > 1`时启用引导比例。

+   `negative_prompt` (`Optional[Union[str, List[str]`, 默认为 `None`) — 用于指导图像生成中不包括的提示或提示。如果未定义，则需要传递`negative_prompt_embeds`。在不使用引导时（`guidance_scale < 1`）忽略。

+   `num_images_per_prompt` (`int`, 默认为 1) — 每个提示生成的图像数量。如果与编译使用的批处理大小不同，则将被神经元的静态批处理大小覆盖（除了动态批处理）。

+   `eta` (`float`, 默认为 0.0) — 对应于[DDIM](https://arxiv.org/abs/2010.02502)论文中的参数 eta (η)。仅适用于`diffusers.schedulers.DDIMScheduler`，在其他调度程序中被忽略。

+   `generator` (`Optional[Union[torch.Generator, List[torch.Generator]]]`, 默认为 `None`) — 用于使生成过程确定性的[`torch.Generator`](https://pytorch.org/docs/stable/generated/torch.Generator.html)。

+   `prompt_embeds` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预生成的文本嵌入。可用于轻松调整文本输入（提示加权）。如果未提供，则从`prompt`输入参数生成文本嵌入。

+   `negative_prompt_embeds` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预生成的负文本嵌入。可用于轻松调整文本输入（提示加权）。如果未提供，`negative_prompt_embeds`将从`negative_prompt`输入参数生成。

+   `output_type` (`Optional[str]`, 默认为 `"pil"`) — 生成图像的输出格式。选择`PIL.Image`或`np.array`之间。

+   `return_dict` (`bool`, 默认为 `True`) — 是否返回`diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput`而不是普通元组。

+   `callback` (`Optional[Callable]`, 默认为`None`) — 在推理过程中每隔`callback_steps`步调用的函数。该函数将使用以下参数调用：`callback(step: int, timestep: int, latents: torch.FloatTensor)`。

+   `callback_steps` (`int`, 默认为 1) — 调用`callback`函数的频率。如果未指定，将在每一步调用回调。

+   `cross_attention_kwargs` (`dict`, 默认为`None`) — 如果指定，将传递给`AttentionProcessor`的 kwargs 字典，如[`self.processor`](https://github.com/huggingface/diffusers/blob/main/src/diffusers/models/attention_processor.py)中定义的。

返回

`diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput`或`tuple`

如果`return_dict`为`True`，则返回`diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput`，否则返回一个`tuple`，其中第一个元素是生成的图像列表，第二个元素是一个包含相应生成图像是否包含“不适宜工作”(nsfw)内容的`bool`列表。

用于生成的管道的调用函数。

示例：

```py
>>> from optimum.neuron import NeuronStableDiffusionImg2ImgPipeline
>>> from diffusers.utils import load_image

>>> url = "https://raw.githubusercontent.com/CompVis/stable-diffusion/main/assets/stable-samples/img2img/sketch-mountains-input.jpg"
>>> init_image = load_image(url).convert("RGB")

>>> compiler_args = {"auto_cast": "matmul", "auto_cast_type": "bf16"}
>>> input_shapes = {"batch_size": 1, "height": 512, "width": 512}
>>> pipeline = NeuronStableDiffusionImg2ImgPipeline.from_pretrained(
...     "nitrosocke/Ghibli-Diffusion", export=True, **compiler_args, **input_shapes,
... )
>>> pipeline.save_pretrained("sd_img2img/")

>>> prompt = "ghibli style, a fantasy landscape with snowcapped mountains, trees, lake with detailed reflection."
>>> image = pipeline(prompt=prompt, image=init_image, strength=0.75, guidance_scale=7.5).images[0]
```

### NeuronStableDiffusionInpaintPipeline

### `class optimum.neuron.NeuronStableDiffusionInpaintPipeline`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_diffusion.py#L808)

```py
( text_encoder: ScriptModule unet: ScriptModule vae_decoder: Union config: Dict tokenizer: CLIPTokenizer scheduler: Union data_parallel_mode: str vae_encoder: Union = None text_encoder_2: Union = None tokenizer_2: Optional = None feature_extractor: Optional = None configs: Optional = None neuron_configs: Optional = None model_save_dir: Union = None model_and_config_save_paths: Optional = None )
```

#### `__call__`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/pipelines/diffusers/pipeline_stable_diffusion_inpaint.py#L48)

```py
( prompt: Union = None image: Optional = None mask_image: Optional = None masked_image_latents: Optional = None strength: float = 1.0 num_inference_steps: int = 50 guidance_scale: float = 7.5 negative_prompt: Union = None num_images_per_prompt: Optional = 1 eta: float = 0.0 generator: Union = None latents: Optional = None prompt_embeds: Optional = None negative_prompt_embeds: Optional = None output_type: Optional = 'pil' return_dict: bool = True callback: Optional = None callback_steps: int = 1 cross_attention_kwargs: Optional = None clip_skip: int = None ) → export const metadata = 'undefined';diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput or tuple
```

参数

+   `prompt` (`Optional[Union[str, List[str]]]`, 默认为`None`) — 用于指导图像生成的提示或提示。如果未定义，则需要传递`prompt_embeds`。

+   `image` (`Optional["PipelineImageInput"]`, 默认为`None`) — 代表要修复的图像批次的图像、numpy 数组或张量（使用`mask_image`遮罩和根据`prompt`重新绘制图像的部分）。对于 numpy 数组和 pytorch 张量，期望值范围在`[0, 1]`之间。如果是张量或张量列表，期望形状应为`(B, C, H, W)`或`(C, H, W)`。如果是 numpy 数组或数组列表，期望形状应为`(B, H, W, C)`或`(H, W, C)`。它还可以接受图像潜变量作为`image`，但如果直接传递潜变量，则不会再次编码。

+   `mask_image` (`Optional["PipelineImageInput"]`, 默认为`None`) — 代表要遮罩`image`的图像、numpy 数组或张量。遮罩中的白色像素被重新绘制，而黑色像素被保留。如果`mask_image`是 PIL 图像，则在使用之前将其转换为单通道（亮度）。如果是 numpy 数组或 pytorch 张量，则应包含一个颜色通道（L）而不是 3 个，因此 pytorch 张量的预期形状为`(B, 1, H, W)`、`(B, H, W)`、`(1, H, W)`、`(H, W)`。对于 numpy 数组，预期形状为`(B, H, W, 1)`、`(B, H, W)`、`(H, W, 1)`或`(H, W)`。

+   `strength` (`float`, 默认为 1.0) — 表示转换参考`image`的程度。必须在 0 和 1 之间。`image`被用作起点，`strength`越高，添加的噪音就越多。降噪步骤的数量取决于最初添加的噪音量。当`strength`为 1 时，添加的噪音最大，降噪过程将运行指定的`num_inference_steps`的全部迭代次数。值为 1 实际上忽略了`image`。

+   `num_inference_steps` (`int`, 默认为 50) — 降噪步骤的数量。更多的降噪步骤通常会导致图像质量更高，但推理速度较慢。此参数由`strength`调节。

+   `guidance_scale` (`float`, 默认为 7.5) — 更高的引导比例值鼓励模型生成与文本`prompt`密切相关的图像，但会降低图像质量。当`guidance_scale > 1`时启用引导比例。

+   `negative_prompt` (`Optional[Union[str, List[str]`, 默认为 `None`) — 用于指导图像生成中不包含的提示或提示。如果未定义，则需要传递 `negative_prompt_embeds`。在不使用指导时被忽略 (`guidance_scale < 1`)。

+   `num_images_per_prompt` (`int`, 默认为 1) — 每个提示生成的图像数量。如果与用于编译的批处理大小不同，则会被 neuron 的静态批处理大小覆盖（除了动态批处理）。

+   `eta` (`float`, 默认为 0.0) — 对应于 [DDIM](https://arxiv.org/abs/2010.02502) 论文中的参数 eta (η)。仅适用于 `diffusers.schedulers.DDIMScheduler`，在其他调度程序中被忽略。

+   `generator` (`Optional[Union[torch.Generator, List[torch.Generator]]]`, 默认为 `None`) — 用于使生成结果确定性的 [`torch.Generator`](https://pytorch.org/docs/stable/generated/torch.Generator.html)。

+   `latents` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 从高斯分布中采样的预生成噪声潜变量，用作图像生成的输入。可以用来使用不同提示调整相同生成。如果未提供，则通过使用提供的随机 `generator` 进行采样生成潜变量张量。

+   `prompt_embeds` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预生成的文本嵌入。可用于轻松调整文本输入（提示加权）。如果未提供，则从 `prompt` 输入参数生成文本嵌入。

+   `negative_prompt_embeds` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预生成的负面文本嵌入。可用于轻松调整文本输入（提示加权）。如果未提供，则从 `negative_prompt` 输入参数生成 `negative_prompt_embeds`。

+   `output_type` (`Optional[str]`, 默认为 `"pil"`) — 生成图像的输出格式。选择 `PIL.Image` 或 `np.array` 之间。

+   `return_dict` (`bool`, 默认为 `True`) — 是否返回 `diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput` 而不是普通的 tuple。

+   `callback` (`Optional[Callable]`, 默认为 `None`) — 在推断过程中每 `callback_steps` 步调用的函数。该函数使用以下参数调用：`callback(step: int, timestep: int, latents: torch.FloatTensor)`。

+   `callback_steps` (`int`, 默认为 1) — 调用 `callback` 函数的频率。如果未指定，则在每一步调用回调。

+   `cross_attention_kwargs` (`dict`, 默认为 `None`) — 一个 kwargs 字典，如果指定了，则会传递给 [`self.processor`](https://github.com/huggingface/diffusers/blob/main/src/diffusers/models/attention_processor.py) 中定义的 `AttentionProcessor`。

+   `clip_skip` (`int`, 默认为 `None`) — 在计算提示嵌入时要从 CLIP 跳过的层数。值为 1 表示将使用前一个层的输出来计算提示嵌入。

返回

`diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput` 或 `tuple`

如果 `return_dict` 为 `True`，则返回 `diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput`，否则返回一个 `tuple`，其中第一个元素是生成的图像列表，第二个元素是一个包含对应生成图像是否包含“不安全内容”（nsfw）的 `bool` 列表。

用于生成的管道的调用函数。

示例:

```py
>>> from optimum.neuron import NeuronStableDiffusionInpaintPipeline
>>> from diffusers.utils import load_image

>>> img_url = "https://raw.githubusercontent.com/CompVis/latent-diffusion/main/data/inpainting_examples/overture-creations-5sI6fQgYIuo.png"
>>> mask_url = "https://raw.githubusercontent.com/CompVis/latent-diffusion/main/data/inpainting_examples/overture-creations-5sI6fQgYIuo_mask.png"

>>> init_image = load_image(img_url).convert("RGB")
>>> mask_image = load_image(mask_url).convert("RGB")

>>> compiler_args = {"auto_cast": "matmul", "auto_cast_type": "bf16"}
>>> input_shapes = {"batch_size": 1, "height": 1024, "width": 1024}
>>> pipeline = NeuronStableDiffusionInpaintPipeline.from_pretrained(
...     "runwayml/stable-diffusion-inpainting", export=True, **compiler_args, **input_shapes,
... )
>>> pipeline.save_pretrained("sd_inpaint/")

>>> prompt = "Face of a yellow cat, high resolution, sitting on a park bench"
>>> image = pipeline(prompt=prompt, image=init_image, mask_image=mask_image).images[0]
```

### NeuronLatentConsistencyModelPipeline

### `class optimum.neuron.NeuronLatentConsistencyModelPipeline`

[< source >](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_diffusion.py#L814)

```py
( text_encoder: ScriptModule unet: ScriptModule vae_decoder: Union config: Dict tokenizer: CLIPTokenizer scheduler: Union data_parallel_mode: str vae_encoder: Union = None text_encoder_2: Union = None tokenizer_2: Optional = None feature_extractor: Optional = None configs: Optional = None neuron_configs: Optional = None model_save_dir: Union = None model_and_config_save_paths: Optional = None )
```

#### `__call__`

[< source >](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/pipelines/diffusers/pipeline_latent_consistency_text2img.py#L67)

```py
( prompt: Union = None num_inference_steps: int = 50 original_inference_steps: Optional = None guidance_scale: float = 8.5 num_images_per_prompt: int = 1 generator: Union = None latents: Optional = None prompt_embeds: Optional = None output_type: str = 'pil' return_dict: bool = True cross_attention_kwargs: Optional = None clip_skip: Optional = None callback_on_step_end: Optional = None callback_on_step_end_tensor_inputs: List = ['latents'] ) → export const metadata = 'undefined';diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput or tuple
```

参数

+   `prompt`（`Optional[Union[str, List[str]]]`，默认为`None`） - 用于指导图像生成的提示或提示。如果未定义，则需要传递`prompt_embeds`。

+   `num_inference_steps`（`int`，默认为 50） - 去噪步骤的数量。更多的去噪步骤通常会导致更高质量的图像，但会降低推理速度。

+   `original_inference_steps`（`Optional[int]`，默认为`None`） - 用于生成线性间隔时间步骤表的原始推理步骤数，从中我们将均匀间隔地从中抽取`num_inference_steps`个时间步骤作为我们的最终时间步骤表，遵循论文中的 Skipping-Step 方法（参见第 4.3 节）。如果未设置，这将默认为调度器的`original_inference_steps`属性。

+   `guidance_scale`（`float`，默认为 8.5） - 更高的引导比例值鼓励模型生成与文本`prompt`密切相关的图像，但会降低图像质量。当`guidance_scale > 1`时启用引导比例。请注意，原始的潜变量一致性模型论文使用了不同的 CFG 公式，其中引导比例减 1（因此在论文公式中，当`guidance_scale > 0`时启用 CFG）。

+   `num_images_per_prompt`（`int`，默认为 1） - 每个提示生成的图像数量。

+   `generator`（`Optional[Union[torch.Generator, List[torch.Generator]]]`，默认为`None`） - 用于使生成过程确定性的[`torch.Generator`](https://pytorch.org/docs/stable/generated/torch.Generator.html)。

+   `latents`（`Optional[torch.FloatTensor]`，默认为`None`） - 从高斯分布中采样的预生成噪声潜变量，用作图像生成的输入。可以用来使用不同提示微调相同的生成。如果未提供，将使用提供的随机`generator`进行采样生成潜变量张量。

+   `prompt_embeds`（`Optional[torch.FloatTensor]`，默认为`None`） - 预生成的文本嵌入。可以用来轻松调整文本输入（提示加权）。如果未提供，文本嵌入将从`prompt`输入参数生成。

+   `output_type`（`str`，默认为`"pil"`） - 生成图像的输出格式。选择`PIL.Image`或`np.array`之间。

+   `return_dict`（`bool`，默认为`True`） - 是否返回`~pipelines.stable_diffusion.StableDiffusionPipelineOutput`而不是普通元组。

+   `cross_attention_kwargs`（`Optional[Dict[str, Any]]`，默认为`None`） - 如果指定了 kwargs 字典，则将其传递给[`self.processor`](https://github.com/huggingface/diffusers/blob/main/src/diffusers/models/attention_processor.py)中定义的`AttentionProcessor`。

+   `clip_skip`（`Optional[int]`，默认为`None`） - 在计算提示嵌入时要跳过的 CLIP 层数。值为 1 意味着将使用预最终层的输出来计算提示嵌入。

+   `callback_on_step_end`（`Optional[Callable]`，默认为`None`） - 在推理过程中每个去噪步骤结束时调用的函数。该函数将使用以下参数调用：`callback_on_step_end(self: DiffusionPipeline, step: int, timestep: int, callback_kwargs: Dict)`。`callback_kwargs`将包括由`callback_on_step_end_tensor_inputs`指定的所有张量的列表。

+   `callback_on_step_end_tensor_inputs`（`List[str]`，默认为`["latents"]`） - `callback_on_step_end`函数的张量输入列表。列表中指定的张量将作为`callback_kwargs`参数传递。您只能包含在您的管道类的`._callback_tensor_inputs`属性中列出的变量。

返回

`diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput`或`tuple`

如果`return_dict`为`True`，则返回`diffusers.pipelines.stable_diffusion.StableDiffusionPipelineOutput`，否则返回一个`tuple`，其中第一个元素是生成的图像列表，第二个元素是一个`bool`列表，指示相应生成的图像是否包含“不适宜工作”（nsfw）内容。

用于生成的管道的调用函数。

示例：

### 神经元稳定扩散 XL 管道

### `class optimum.neuron.NeuronStableDiffusionXLPipeline`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_diffusion.py#L873)

```py
( text_encoder: ScriptModule unet: ScriptModule vae_decoder: ScriptModule config: Dict tokenizer: CLIPTokenizer scheduler: Union data_parallel_mode: str vae_encoder: Optional = None text_encoder_2: Optional = None tokenizer_2: Optional = None feature_extractor: Optional = None configs: Optional = None neuron_configs: Optional = None model_save_dir: Union = None model_and_config_save_paths: Optional = None add_watermarker: Optional = None )
```

#### `__call__`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/pipelines/diffusers/pipeline_stable_diffusion_xl.py#L38)

```py
( prompt: Union = None prompt_2: Union = None num_inference_steps: int = 50 denoising_end: Optional = None guidance_scale: float = 5.0 negative_prompt: Union = None negative_prompt_2: Union = None num_images_per_prompt: int = 1 eta: float = 0.0 generator: Union = None latents: Optional = None prompt_embeds: Optional = None negative_prompt_embeds: Optional = None pooled_prompt_embeds: Optional = None negative_pooled_prompt_embeds: Optional = None output_type: Optional = 'pil' return_dict: bool = True callback: Optional = None callback_steps: int = 1 cross_attention_kwargs: Optional = None guidance_rescale: float = 0.0 original_size: Optional = None crops_coords_top_left: Tuple = (0, 0) target_size: Optional = None negative_original_size: Optional = None negative_crops_coords_top_left: Tuple = (0, 0) negative_target_size: Optional = None clip_skip: Optional = None ) → export const metadata = 'undefined';diffusers.pipelines.stable_diffusion_xl.StableDiffusionXLPipelineOutput or tuple
```

参数

+   `prompt` (`Optional[Union[str, List[str]]]`, 默认为`None`) — 用于指导图像生成的提示或提示。如果未定义，则必须传递`prompt_embeds`。

+   `prompt_2` (`Optional[Union[str, List[str]]]`, 默认为`None`) — 发送到`tokenizer_2`和`text_encoder_2`的提示或提示。如果未定义，则在两个文本编码器中使用`prompt`。

+   `num_inference_steps` (`int`, 默认为 50) — 去噪步数。更多的去噪步骤通常会导致更高质量的图像，但会降低推理速度。

+   `denoising_end` (`Optional[float]`, 默认为`None`) — 当指定时，确定在故意提前终止之前完成的去噪过程的比例（介于 0.0 和 1.0 之间）。因此，返回的样本仍将保留由调度程序选择的离散时间步确定的大量噪声。当此管道作为“去噪器混合”多管道设置的一部分时，应理想地利用`denoising_end`参数，如[`精细调整图像输出`](https://huggingface.co/docs/diffusers/api/pipelines/stable_diffusion/stable_diffusion_xl#refining-the-image-output)中所详细阐述

+   `guidance_scale` (`float`, 默认为 5.0`) — 在[无分类器扩散指导](https://arxiv.org/abs/2207.12598)中定义的指导比例。`guidance_scale`被定义为[Imagen Paper](https://arxiv.org/pdf/2205.11487.pdf)中方程 2 的`w`。通过设置`guidance_scale > 1`启用指导比例。更高的指导比例鼓励生成与文本`prompt`密切相关的图像，通常以降低图像质量为代价。

+   `negative_prompt` (`Optional[Union[str, List[str]]]`, 默认为`None`) — 不指导图像生成的提示或提示。如果未定义，则必须传递`negative_prompt_embeds`。当不使用指导时（即，如果`guidance_scale`小于`1`，则会被忽略）。

+   `negative_prompt_2` (`Optional[Union[str, List[str]]]`, 默认为`None`) — 不指导图像生成的提示或提示，将发送到`tokenizer_2`和`text_encoder_2`。如果未定义，则在两个文本编码器中使用`negative_prompt`。

+   `num_images_per_prompt` (`int`, 默认为 1`) — 每个提示生成的图像数量。如果与用于编译的批处理大小不同，则将被神经元的静态批处理大小覆盖（除了动态批处理）。

+   `eta` (`float`, 默认为 0.0`) — 对应于 DDIM 论文中的参数 eta（η）：[`arxiv.org/abs/2010.02502`](https://arxiv.org/abs/2010.02502)。仅适用于`schedulers.DDIMScheduler`，对其他情况将被忽略。

+   `generator` (`Optional[Union[torch.Generator, List[torch.Generator]]]`, 默认为`None`) — 一个或多个[torch 生成器](https://pytorch.org/docs/stable/generated/torch.Generator.html)用于使生成过程确定性。

+   `latents` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预生成的噪声潜变量，从高斯分布中采样，用作图像生成的输入。可用于使用不同提示调整相同生成。如果未提供，将使用提供的随机 `generator` 进行采样生成潜变量张量。

+   `prompt_embeds` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预生成的文本嵌入。可用于轻松调整文本输入，例如提示加权。如果未提供，将从 `prompt` 输入参数生成文本嵌入。

+   `negative_prompt_embeds` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预生成的负文本嵌入。可用于轻松调整文本输入，例如提示加权。如果未提供，将从 `negative_prompt` 输入参数生成负 prompt 嵌入。

+   `pooled_prompt_embeds` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预生成的池化文本嵌入。可用于轻松调整文本输入，例如提示加权。如果未提供，将从 `prompt` 输入参数生成池化的文本嵌入。

+   `negative_pooled_prompt_embeds` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预生成的负池化文本嵌入。可用于轻松调整文本输入，例如提示加权。如果未提供，将从 `negative_prompt` 输入参数生成池化的负 prompt 嵌入。

+   `output_type` (`Optional[str]`, 默认为 `"pil"`) — 生成图像的输出格式。选择在 [PIL](https://pillow.readthedocs.io/en/stable/) 中的 `PIL.Image.Image` 或 `np.array` 之间。

+   `return_dict` (`bool`, 默认为 `True`) — 是否返回一个 `diffusers.pipelines.stable_diffusion_xl.StableDiffusionXLPipelineOutput` 而不是一个普通的元组。

+   `callback` (`Optional[Callable]`, 默认为 `None`) — 一个函数，在推理过程中每 `callback_steps` 步会被调用一次。该函数将会以以下参数被调用：`callback(step: int, timestep: int, latents: torch.FloatTensor)`。

+   `callback_steps` (`int`, 默认为 1) — `callback` 函数被调用的频率。如果未指定，将在每一步调用回调函数。

+   `cross_attention_kwargs` (`dict`, 默认为 `None`) — 一个 kwargs 字典，如果指定，将传递给 `AttentionProcessor`，在 [diffusers.models.attention_processor](https://github.com/huggingface/diffusers/blob/main/src/diffusers/models/attention_processor.py) 中由 `self.processor` 定义。

+   `guidance_rescale` (`float`, *可选*, 默认为 0.0) — [Common Diffusion Noise Schedules and Sample Steps are Flawed](https://arxiv.org/pdf/2305.08891.pdf) 提出的指导重缩放因子。`guidance_scale` 在 [Common Diffusion Noise Schedules and Sample Steps are Flawed](https://arxiv.org/pdf/2305.08891.pdf) 的方程式 16 中定义。指导重缩放因子应该在使用零终端 SNR 时修复过曝光问题。

+   `original_size` (`Optional[Tuple[int, int]]`, 默认为 (1024, 1024)) — 如果 `original_size` 与 `target_size` 不同，图像将会被缩小或放大。如果未指定，`original_size` 默认为 `(width, height)`。这是 SDXL 微调的一部分，详见 [`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952) 第 2.2 节。

+   `crops_coords_top_left` (`Tuple[int]`, 默认为 (0, 0)) — `crops_coords_top_left` 可用于生成一个看起来被从位置 `crops_coords_top_left` 向下“裁剪”的图像。通常通过将 `crops_coords_top_left` 设置为 (0, 0) 来获得令人满意、居中的图像。这是 SDXL 微调的一部分，详见 [`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952) 第 2.2 节。

+   `target_size` (`Tuple[int]`, 默认为 (1024, 1024)) — 对于大多数情况，`target_size` 应设置为生成图像的期望高度和宽度。如果未指定，将默认为 `(width, height)`。作为 SDXL 的微调节的一部分，详见 [`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952) 的第 2.2 节。

+   `negative_original_size` (`Tuple[int]`, 默认为 (1024, 1024)) — 根据特定图像分辨率对生成过程进行负面条件。作为 SDXL 的微调节的一部分，详见 [`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952) 的第 2.2 节。更多信息，请参考此问题线程：[`github.com/huggingface/diffusers/issues/4208`](https://github.com/huggingface/diffusers/issues/4208)。

+   `negative_crops_coords_top_left` (`Tuple[int]`, 默认为 (0, 0)) — 根据特定的裁剪坐标对生成过程进行负面条件。作为 SDXL 的微调节的一部分，详见 [`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952) 的第 2.2 节。更多信息，请参考此问题线程：[`github.com/huggingface/diffusers/issues/4208`](https://github.com/huggingface/diffusers/issues/4208)。

+   `negative_target_size` (`Tuple[int]`, 默认为 (1024, 1024)) — 根据目标图像分辨率对生成过程进行负面条件。对于大多数情况，它应与 `target_size` 相同。作为 SDXL 的微调节的一部分，详见 [`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952) 的第 2.2 节。更多信息，请参考此问题线程：[`github.com/huggingface/diffusers/issues/4208`](https://github.com/huggingface/diffusers/issues/4208)。

+   `clip_skip` (`Optional[int]`, 默认为 `None`) — 在计算提示嵌入时，要从 CLIP 中跳过的层数。值为 1 表示将使用预终层的输出来计算提示嵌入。

返回

`diffusers.pipelines.stable_diffusion_xl.StableDiffusionXLPipelineOutput` 或 `tuple`

如果 `return_dict` 为 True，则返回 `diffusers.pipelines.stable_diffusion_xl.StableDiffusionXLPipelineOutput`，否则返回一个 `tuple`。当返回一个 tuple 时，第一个元素是一个包含生成图像的列表。

调用管道进行生成时调用的函数。

示例：

```py
>>> from optimum.neuron import NeuronStableDiffusionXLPipeline

>>> compiler_args = {"auto_cast": "matmul", "auto_cast_type": "bf16"}
>>> input_shapes = {"batch_size": 1, "height": 1024, "width": 1024}

>>> stable_diffusion_xl = NeuronStableDiffusionXLPipeline.from_pretrained(
...     "stabilityai/stable-diffusion-xl-base-1.0", export=True, **compiler_args, **input_shapes)
... )
>>> stable_diffusion_xl.save_pretrained("sd_neuron_xl/")

>>> prompt = "Astronaut in a jungle, cold color palette, muted colors, detailed, 8k"
>>> image = stable_diffusion_xl(prompt).images[0]
```

### NeuronStableDiffusionXLImg2ImgPipeline

### `class optimum.neuron.NeuronStableDiffusionXLImg2ImgPipeline`

[< source >](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_diffusion.py#L877)

```py
( text_encoder: ScriptModule unet: ScriptModule vae_decoder: ScriptModule config: Dict tokenizer: CLIPTokenizer scheduler: Union data_parallel_mode: str vae_encoder: Optional = None text_encoder_2: Optional = None tokenizer_2: Optional = None feature_extractor: Optional = None configs: Optional = None neuron_configs: Optional = None model_save_dir: Union = None model_and_config_save_paths: Optional = None add_watermarker: Optional = None )
```

#### `__call__`

[< source >](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/pipelines/diffusers/pipeline_stable_diffusion_xl_img2img.py#L109)

```py
( prompt: Union = None prompt_2: Union = None image: Optional = None strength: float = 0.3 num_inference_steps: int = 50 denoising_start: Optional = None denoising_end: Optional = None guidance_scale: float = 5.0 negative_prompt: Union = None negative_prompt_2: Union = None num_images_per_prompt: Optional = 1 eta: float = 0.0 generator: Union = None latents: Optional = None prompt_embeds: Optional = None negative_prompt_embeds: Optional = None pooled_prompt_embeds: Optional = None negative_pooled_prompt_embeds: Optional = None output_type: Optional = 'pil' return_dict: bool = True callback: Optional = None callback_steps: int = 1 cross_attention_kwargs: Optional = None guidance_rescale: float = 0.0 original_size: Tuple = None crops_coords_top_left: Tuple = (0, 0) target_size: Tuple = None negative_original_size: Optional = None negative_crops_coords_top_left: Tuple = (0, 0) negative_target_size: Optional = None aesthetic_score: float = 6.0 negative_aesthetic_score: float = 2.5 clip_skip: Optional = None ) → export const metadata = 'undefined';diffusers.pipelines.stable_diffusion.StableDiffusionXLPipelineOutput or tuple
```

参数

+   `prompt` (`Optional[Union[str, List[str]]]`, 默认为 `None`) — 用于引导图像生成的提示或提示。如果未定义，则必须传递 `prompt_embeds`。

+   `prompt_2` (`Optional[Union[str, List[str]]]`, 默认为 `None`) — 要发送到 `tokenizer_2` 和 `text_encoder_2` 的提示或提示。如果未定义，则在两个文本编码器中都使用 `prompt`。

+   `image` (`Optional["PipelineImageInput"]`, 默认为 `None`) — 要使用管道修改的图像。

+   `strength` (`float`, 默认为 0.3) — 在概念上，指示要转换参考 `image` 的程度。必须在 0 和 1 之间。`image` 将被用作起点，添加的噪音越大，`strength` 越大。去噪步骤的数量取决于最初添加的噪音量。当 `strength` 为 1 时，添加的噪音将达到最大，并且去噪过程将运行指定的 `num_inference_steps` 的完整迭代次数。因此，值为 1 的情况下，实际上忽略了 `image`。请注意，在声明 `denoising_start` 为整数的情况下，将忽略 `strength` 的值。

+   `num_inference_steps` (`int`, 默认为 50) — 去噪步骤的数量。更多的去噪步骤通常会导致图像质量更高，但推理速度较慢。

+   `denoising_start` (`Optional[float]`, 默认为 `None`) — 当指定时，指示在启动之前要跳过的总去噪过程的比例（介于 0.0 和 1.0 之间）。因此，将跳过去噪过程的初始部分，并假定传递的 `image` 是部分去噪的图像。请注意，当指定此参数时，将忽略强度。当此流水线被集成到“去噪器混合”多流水线设置中时，`denoising_start` 参数特别有益，如 [`精炼图像输出`](https://huggingface.co/docs/diffusers/api/pipelines/stable_diffusion/stable_diffusion_xl#refining-the-image-output) 中详细说明。

+   `denoising_end` (`Optional[float]`, 默认为 `None`) — 当指定时，确定在意图上提前终止之前完成的去噪过程的比例（介于 0.0 和 1.0 之间）。因此，返回的样本仍将保留相当多的噪音（大约还需要最后 20% 的时间步），应由具有 `denoising_start` 设置为 0.8 的后续流水线进行去噪，以便仅对调度器的最后 20% 进行去噪。当此流水线形成“去噪器混合”多流水线设置的一部分时，应理想地利用 `denoising_end` 参数，如 [`精炼图像输出`](https://huggingface.co/docs/diffusers/api/pipelines/stable_diffusion/stable_diffusion_xl#refining-the-image-output) 中详细说明。

+   `guidance_scale` (`float`, 默认为 7.5) — 在 [无分类器扩散指导](https://arxiv.org/abs/2207.12598) 中定义的指导比例。`guidance_scale` 定义为 [Imagen 论文](https://arxiv.org/pdf/2205.11487.pdf) 中方程 2 的 `w`。通过设置 `guidance_scale > 1` 启用指导比例。更高的指导比例鼓励生成与文本 `prompt` 密切相关的图像，通常以降低图像质量为代价。

+   `negative_prompt` (`Optional[Union[str, List[str]]]`, 默认为 `None`) — 不用来指导图像生成的提示。如果未定义，则必须传递 `negative_prompt_embeds`。当不使用指导时（即，如果 `guidance_scale` 小于 `1`，则忽略）。

+   `negative_prompt_2` (`Optional[Union[str, List[str]]]`, 默认为 `None`) — 不用来指导发送到 `tokenizer_2` 和 `text_encoder_2` 的图像生成的提示。如果未定义，则在两个文本编码器中都使用 `negative_prompt`。

+   `num_images_per_prompt` (`int`, 默认为 1) — 每个提示生成的图像数量。如果与用于编译的批处理大小不同，则将被神经元的静态批处理大小覆盖（除了动态批处理）。

+   `eta` (`float`, 默认为 0.0) — 对应于 DDIM 论文中的参数 eta (η)：[`arxiv.org/abs/2010.02502`](https://arxiv.org/abs/2010.02502)。仅适用于 `schedulers.DDIMScheduler`，对于其他情况将被忽略。

+   `generator` (`Optional[Union[torch.Generator, List[torch.Generator]]]`, 默认为 `None`) — 一个或多个 [torch 生成器](https://pytorch.org/docs/stable/generated/torch.Generator.html) 的列表，用于使生成过程确定性。

+   `latents` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预先生成的嘈杂潜变量，从高斯分布中采样，用作图像生成的输入。可用于使用不同提示微调相同生成。如果未提供，将使用提供的随机 `generator` 进行采样生成潜变量张量。

+   `prompt_embeds` (`Optional[torch.FloatTensor]`, 默认为 `None`) — 预生成的文本嵌入。可用于轻松调整文本输入，例如提示加权。如果未提供，文本嵌入将从 `prompt` 输入参数生成。

+   `negative_prompt_embeds` (`Optional[torch.FloatTensor]`，默认为`None`) — 预生成的负面文本嵌入。可用于轻松调整文本输入，例如提示加权。如果未提供，将从`negative_prompt`输入参数生成负面文本嵌入。

+   `pooled_prompt_embeds` (`Optional[torch.FloatTensor]`，默认为`None`) — 预生成的池化文本嵌入。可用于轻松调整文本输入，例如提示加权。如果未提供，将从`prompt`输入参数生成池化文本嵌入。

+   `negative_pooled_prompt_embeds` (`Optional[torch.FloatTensor]`，默认为`None`) — 预生成的负面池化文本嵌入。可用于轻松调整文本输入，例如提示加权。如果未提供，将从`negative_prompt`输入参数生成负面池化文本嵌入。

+   `output_type` (`Optional[str]`，默认为`"pil"`) — 生成图像的输出格式。选择[PIL](https://pillow.readthedocs.io/en/stable/)：`PIL.Image.Image`或`np.array`之间的选项。

+   `return_dict` (`bool`，默认为`True`) — 是否返回`diffusers.pipelines.stable_diffusion.StableDiffusionXLPipelineOutput`而不是普通元组。

+   `callback` (`Optional[Callable]`，默认为`None`) — 在推断过程中，每`callback_steps`步调用一次的函数。该函数将使用以下参数进行调用：`callback(step: int, timestep: int, latents: torch.FloatTensor)`。

+   `callback_stcallback_steps` (`int`，默认为 1) — `callback`函数将被调用的频率。如果未指定，将在每一步调用回调。

+   `cross_attention_kwargs` (`Optional[Dict[str, Any]]`，默认为`None`) — 如果指定，将传递给`AttentionProcessor`中的`self.processor`定义的 kwargs 字典，在[diffusers.models.attention_processor](https://github.com/huggingface/diffusers/blob/main/src/diffusers/models/attention_processor.py)中有详细定义。

+   `guidance_rescale` (`float`，默认为 0.0）— [Common Diffusion Noise Schedules and Sample Steps are Flawed](https://arxiv.org/pdf/2305.08891.pdf) 提出的指导重缩放因子，`guidance_scale` 在方程式 16 中被定义为`φ`。当使用零终端信噪比时，指导重缩放因子应该修复过曝光问题。

+   `original_size` (`Optional[Tuple[int, int]`，默认为(1024, 1024)) — 如果`original_size`与`target_size`不同，图像将呈现为缩小或放大。如果未指定，`original_size`默认为`(width, height)`。作为 SDXL 微调的一部分，详见[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节。

+   `crops_coords_top_left` (`Tuple[int]`，默认为(0, 0)) — `crops_coords_top_left` 可用于生成一个看起来被“裁剪”到`crops_coords_top_left`位置以下的图像。通常通过将`crops_coords_top_left`设置为(0, 0)来实现有利的、居中的图像。作为 SDXL 微调的一部分，详见[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节。

+   `target_size` (`Tuple[int]`，默认为(1024, 1024)) — 对于大多数情况，`target_size` 应设置为生成图像的期望高度和宽度。如果未指定，将默认为`(width, height)`。作为 SDXL 微调的一部分，详见[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节。

+   `negative_original_size` (`Tuple[int]`, 默认为(1024, 1024)) — 基于特定图像分辨率对生成过程进行负面调节。SDXL 的微调条件的一部分，如[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节所述。有关更多信息，请参考此问题线程：[`github.com/huggingface/diffusers/issues/4208`](https://github.com/huggingface/diffusers/issues/4208)。

+   `negative_crops_coords_top_left` (`Tuple[int]`, 默认为(0, 0)) — 基于特定裁剪坐标对生成过程进行负面调节。SDXL 的微调条件的一部分，如[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节所述。有关更多信息，请参考此问题线程：[`github.com/huggingface/diffusers/issues/4208`](https://github.com/huggingface/diffusers/issues/4208)。

+   `negative_target_size` (`Tuple[int]`, 默认为(1024, 1024)) — 基于目标图像分辨率对生成过程进行负面调节。对于大多数情况，它应与`target_size`相同。SDXL 的微调条件的一部分，如[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节所述。有关更多信息，请参考此问题线程：[`github.com/huggingface/diffusers/issues/4208`](https://github.com/huggingface/diffusers/issues/4208)。

+   `aesthetic_score` (`float`, 默认为 6.0) — 用于通过影响正文条件来模拟生成图像的美学评分。SDXL 的微调条件的一部分，如[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节所述。

+   `negative_aesthetic_score` (`float`, 默认为 2.5) — SDXL 的微调条件的一部分，如[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节所述。可用于通过影响负面文本条件来模拟生成图像的美学评分。

+   `clip_skip` (`Optional[int]`, 默认为`None`) — 在计算提示嵌入时要从 CLIP 中跳过的层数。值为 1 意味着将使用预最终层的输出来计算提示嵌入。

返回

`diffusers.pipelines.stable_diffusion.StableDiffusionXLPipelineOutput`或`tuple`

`diffusers.pipelines.stable_diffusion.StableDiffusionXLPipelineOutput`如果`return_dict`为 True，否则为一个元组。当返回一个元组时，第一个元素是包含生成图像的列表。

调用管道进行生成时调用的函数。

示例：

```py
>>> from optimum.neuron import NeuronStableDiffusionXLImg2ImgPipeline
>>> from diffusers.utils import load_image

>>> url = "https://huggingface.co/datasets/optimum/documentation-images/resolve/main/intel/openvino/sd_xl/castle_friedrich.png"
>>> init_image = load_image(url).convert("RGB")

>>> compiler_args = {"auto_cast": "matmul", "auto_cast_type": "bf16"}
>>> input_shapes = {"batch_size": 1, "height": 512, "width": 512}
>>> pipeline = NeuronStableDiffusionXLImg2ImgPipeline.from_pretrained(
...     "stabilityai/stable-diffusion-xl-base-1.0", export=True, **compiler_args, **input_shapes,
... )
>>> pipeline.save_pretrained("sdxl_img2img/")

>>> prompt = "a dog running, lake, moat"
>>> image = pipeline(prompt=prompt, image=init_image).images[0]
```

### NeuronStableDiffusionXLInpaintPipeline

### `class optimum.neuron.NeuronStableDiffusionXLInpaintPipeline`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/modeling_diffusion.py#L883)

```py
( text_encoder: ScriptModule unet: ScriptModule vae_decoder: ScriptModule config: Dict tokenizer: CLIPTokenizer scheduler: Union data_parallel_mode: str vae_encoder: Optional = None text_encoder_2: Optional = None tokenizer_2: Optional = None feature_extractor: Optional = None configs: Optional = None neuron_configs: Optional = None model_save_dir: Union = None model_and_config_save_paths: Optional = None add_watermarker: Optional = None )
```

#### `__call__`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/pipelines/diffusers/pipeline_stable_diffusion_xl_inpaint.py#L80)

```py
( prompt: Union = None prompt_2: Union = None image: Optional = None mask_image: Optional = None masked_image_latents: Optional = None padding_mask_crop: Optional = None strength: float = 0.9999 num_inference_steps: int = 50 timesteps: Optional = None denoising_start: Optional = None denoising_end: Optional = None guidance_scale: float = 7.5 negative_prompt: Union = None negative_prompt_2: Union = None num_images_per_prompt: Optional = 1 eta: float = 0.0 generator: Union = None latents: Optional = None prompt_embeds: Optional = None negative_prompt_embeds: Optional = None pooled_prompt_embeds: Optional = None negative_pooled_prompt_embeds: Optional = None ip_adapter_image: Union = None output_type: Optional = 'pil' return_dict: bool = True cross_attention_kwargs: Optional = None guidance_rescale: float = 0.0 original_size: Tuple = None crops_coords_top_left: Tuple = (0, 0) target_size: Tuple = None negative_original_size: Optional = None negative_crops_coords_top_left: Tuple = (0, 0) negative_target_size: Optional = None aesthetic_score: float = 6.0 negative_aesthetic_score: float = 2.5 clip_skip: Optional = None callback_on_step_end: Optional = None callback_on_step_end_tensor_inputs: List = ['latents'] **kwargs ) → export const metadata = 'undefined';diffusers.pipelines.stable_diffusion.StableDiffusionXLPipelineOutput or tuple
```

参数

+   `prompt` (`Optional[Union[str, List[str]]]`, 默认为`None`) — 用于引导图像生成的提示或提示。如果未定义，则必须传递`prompt_embeds`。

+   `prompt_2` (`Optional[Union[str, List[str]]]`, 默认为`None`) — 要发送给`tokenizer_2`和`text_encoder_2`的提示或提示。如果未定义，`prompt`将在两个文本编码器中使用

+   `image` (`Optional["PipelineImageInput"]`, 默认为`None`) — `Image`，或表示将被修复的图像批次的张量，即图像的部分将被`mask_image`遮盖并根据`prompt`重新绘制。

+   `mask_image` (`Optional["PipelineImageInput"]`, defaults to `None`) — 用于遮罩`image`的`Image`或表示图像批次的张量。遮罩中的白色像素将被重新绘制，而黑色像素将被保留。如果`mask_image`是 PIL 图像，它将在使用之前转换为单通道（亮度）。如果它是张量，则应包含一个颜色通道（L）而不是 3，因此预期形状将是`(B, H, W, 1)`。

+   `padding_mask_crop` (`Optional[int]`, defaults to `None`) — 要应用于图像和遮罩的裁剪边距大小。如果为`None`，则不对图像和 mask_image 进行裁剪。如果`padding_mask_crop`不是`None`，它将首先找到一个具有与图像相同纵横比且包含所有遮罩区域的矩形区域，然后根据`padding_mask_crop`扩展该区域。然后将根据扩展的区域对图像和 mask_image 进行裁剪，然后将其调整为原始图像大小以进行修复。当遮罩区域较小而图像较大且包含与修复无关的信息（例如背景）时，这是有用的。

+   `strength` (`float`, defaults to 0.9999) — 在概念上，指示要如何转换参考`image`的遮罩部分的程度。必须介于 0 和 1 之间。`image`将被用作起点，添加的噪声越大，`strength`越大。去噪步骤的数量取决于最初添加的噪声量。当`strength`为 1 时，添加的噪声将达到最大，并且去噪过程将运行指定的`num_inference_steps`的完整迭代次数。因此，值为 1 基本上忽略了参考`image`的遮罩部分。请注意，在将`denoising_start`声明为整数的情况下，将忽略`strength`的值。

+   `num_inference_steps` (`int`, defaults to 50) — 去噪步骤的数量。更多的去噪步骤通常会导致图像质量更高，但推理速度较慢。

+   `timesteps` (`Optional[List[int]]`, defaults to `None`) — 用于具有支持`timesteps`参数的调度程序的去噪过程的自定义时间步。如果未定义，则将使用传递`num_inference_steps`时的默认行为。必须按降序排列。

+   `denoising_start` (`Optional[float]`, defaults to `None`) — 当指定时，指示在启动之前要绕过的总去噪过程的比例（介于 0.0 和 1.0 之间）。因此，将跳过去噪过程的初始部分，并假定传递的`image`是部分去噪的图像。请注意，当指定此参数时，将忽略强度。当此流水线集成到“去噪器混合”多流水线设置中时，`denoising_start`参数特别有益，如[`精炼图像输出`](https://huggingface.co/docs/diffusers/api/pipelines/stable_diffusion/stable_diffusion_xl#refining-the-image-output)中详细说明。

+   `denoising_end` (`Optional[float]`, defaults to `None`) — 当指定时，确定在有意提前终止之前完成的总去噪过程的比例（介于 0.0 和 1.0 之间）。因此，返回的样本仍将保留相当多的噪声（大约还需要最后 20%的时间步），应由具有`denoising_start`设置为 0.8 的后续流水线进行去噪，以便仅对调度程序的最后 20%进行去噪。当此流水线形成“去噪器混合”多流水线设置的一部分时，应理想地利用`denoising_end`参数，如[`精炼图像输出`](https://huggingface.co/docs/diffusers/api/pipelines/stable_diffusion/stable_diffusion_xl#refining-the-image-output)中所述。

+   `guidance_scale` (`float`, defaults to 7.5) — 在[Classifier-Free Diffusion Guidance](https://arxiv.org/abs/2207.12598)中定义的指导比例。`guidance_scale`定义为[Imagen Paper](https://arxiv.org/pdf/2205.11487.pdf)中方程 2 的`w`。通过设置`guidance_scale > 1`启用指导比例。更高的指导比例鼓励生成与文本`prompt`密切相关的图像，通常以降低图像质量为代价。

+   `negative_prompt` (`Optional[Union[str, List[str]]]`, defaults to `None`) — 不指导图像生成的提示或提示。如果未定义，则必须传递`negative_prompt_embeds`。在不使用指导时将被忽略（即，如果`guidance_scale`小于`1`，则将被忽略）。

+   `negative_prompt_2` (`Optional[Union[str, List[str]]]`, defaults to `None`) — 不指导图像生成的提示或提示，将发送到`tokenizer_2`和`text_encoder_2`。如果未定义，`negative_prompt`将在两个文本编码器中使用。

+   `prompt_embeds` (`Optional[torch.FloatTensor]`, defaults to `None`) — 预生成的文本嵌入。可用于轻松调整文本输入，例如提示加权。如果未提供，将从`prompt`输入参数生成文本嵌入。

+   `negative_prompt_embeds` (`Optional[torch.FloatTensor]`, defaults to `None`) — 预生成的负文本嵌入。可用于轻松调整文本输入，例如提示加权。如果未提供，将从`negative_prompt`输入参数生成 negative_prompt_embeds。

+   `pooled_prompt_embeds` (`Optional[torch.FloatTensor]`, defaults to `None`) — 预生成的汇集文本嵌入。可用于轻松调整文本输入，例如提示加权。如果未提供，将从`prompt`输入参数生成汇集文本嵌入。

+   `negative_pooled_prompt_embeds` (`Optional[torch.FloatTensor]`, defaults to `None`) — 预生成的负汇集文本嵌入。可用于轻松调整文本输入，例如提示加权。如果未提供，将从`negative_prompt`输入参数生成负汇集文本嵌入。ip_adapter_image — (`Optional[PipelineImageInput]`, defaults to `None`): 可选的图像输入以与 IP 适配器一起使用。

+   `num_images_per_prompt` (`int`, defaults to 1) — 每个提示生成的图像数量。

+   `eta` (`float`, defaults to 0.0) — 对应于 DDIM 论文中参数 eta (η)：[`arxiv.org/abs/2010.02502`](https://arxiv.org/abs/2010.02502)。仅适用于`schedulers.DDIMScheduler`，对其他情况将被忽略。

+   `generator` (`Optional[Union[torch.Generator, List[torch.Generator]]]`, defaults to `None`) — 一个或多个[torch 生成器](https://pytorch.org/docs/stable/generated/torch.Generator.html)，用于使生成过程确定性。

+   `latents` (`Optional[torch.FloatTensor]`, defaults to `None`) — 预生成的嘈杂潜变量，从高斯分布中采样，用作图像生成的输入。可用于使用不同提示调整相同生成。如果未提供，将使用提供的随机`generator`进行采样生成一个潜变量张量。

+   `output_type` (`Optional[str]`, defaults to `"pil"`) — 生成图像的输出格式。选择[PIL](https://pillow.readthedocs.io/en/stable/)之间的格式：`PIL.Image.Image`或`np.array`。

+   `return_dict` (`bool`, defaults to `True`) — 是否返回`~pipelines.stable_diffusion.StableDiffusionPipelineOutput`而不是普通元组。

+   `cross_attention_kwargs` (`Optional[Dict[str, Any]]`, defaults to `None`) — 如果指定，将传递给`AttentionProcessor`的 kwargs 字典，如在[diffusers.models.attention_processor](https://github.com/huggingface/diffusers/blob/main/src/diffusers/models/attention_processor.py)中所定义。

+   `original_size` (`Tuple[int]`, 默认值为(1024, 1024)) — 如果`original_size`与`target_size`不同，图像将呈现为缩小或放大。如果未指定，`original_size`默认为`(height, width)`。作为 SDXL 微调节的一部分，详见[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节。

+   `crops_coords_top_left` (`Tuple[int]`, 默认值为(0, 0)) — `crops_coords_top_left` 可用于生成一个看起来被从位置`crops_coords_top_left`向下“裁剪”的图像。通常通过将`crops_coords_top_left`设置为(0, 0)来实现令人满意、居中的图像。作为 SDXL 微调节的一部分，详见[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节。

+   `target_size` (`Tuple[int]`, 默认值为(1024, 1024)) — 在大多数情况下，`target_size`应设置为生成图像的期望高度和宽度。如果未指定，将默认为`(height, width)`。作为 SDXL 微调节的一部分，详见[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节。

+   `negative_original_size` (`Tuple[int]`, 默认值为(1024, 1024)) — 基于特定图像分辨率负向调节生成过程。作为 SDXL 微调节的一部分，详见[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节。更多信息，请参考此问题线程：[`github.com/huggingface/diffusers/issues/4208`](https://github.com/huggingface/diffusers/issues/4208)。

+   `negative_crops_coords_top_left` (`Tuple[int]`, 默认值为(0, 0)) — 基于特定裁剪坐标来负向调节生成过程。作为 SDXL 微调节的一部分，详见[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节。更多信息，请参考此问题线程：[`github.com/huggingface/diffusers/issues/4208`](https://github.com/huggingface/diffusers/issues/4208)。

+   `negative_target_size` (`Tuple[int]`, 默认值为(1024, 1024)) — 基于目标图像分辨率负向调节生成过程。在大多数情况下，应与`target_size`相同。作为 SDXL 微调节的一部分，详见[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节。更多信息，请参考此问题线程：[`github.com/huggingface/diffusers/issues/4208`](https://github.com/huggingface/diffusers/issues/4208)。

+   `aesthetic_score` (`float`, 默认值为 6.0）— 用于通过影响正文条件来模拟生成图像的审美评分。作为 SDXL 微调节的一部分，详见[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节。

+   `negative_aesthetic_score` (`float`, 默认值为 2.5) — 作为 SDXL 微调节的一部分，详见[`huggingface.co/papers/2307.01952`](https://huggingface.co/papers/2307.01952)第 2.2 节。可用于通过影响负文本条件来模拟生成图像的审美评分。

+   `clip_skip` (`Optional[int]`, 默认值为`None`) — 在计算提示嵌入时，要从 CLIP 中跳过的层数。值为 1 表示将使用前一层的输出来计算提示嵌入。

+   `callback_on_step_end` (`Optional[Callable[[int, int, Dict], None]]`, 默认值为`None`) — 在推断过程中每个去噪步骤结束时调用的函数。该函数将使用以下参数调用：`callback_on_step_end(self: DiffusionPipeline, step: int, timestep: int, callback_kwargs: Dict)`。`callback_kwargs`将包括由`callback_on_step_end_tensor_inputs`指定的所有张量的列表。

+   `callback_on_step_end_tensor_inputs` (`List[str]`，默认为[“latents”]) — `callback_on_step_end` 函数的张量输入列表。列表中指定的张量将作为 `callback_kwargs` 参数传递。您只能包含在管道类的 `._callback_tensor_inputs` 属性中列出的变量。

返回

`diffusers.pipelines.stable_diffusion.StableDiffusionXLPipelineOutput` 或 `tuple`

如果 `return_dict` 为 True，则返回 `diffusers.pipelines.stable_diffusion.StableDiffusionXLPipelineOutput`，否则返回一个 `tuple.` 元组。当返回一个元组时，第一个元素是包含生成图像的列表。

在调用管道生成时调用的函数。

示例：

```py
>>> from optimum.neuron import NeuronStableDiffusionXLInpaintPipeline
>>> from diffusers.utils import load_image

>>> img_url = "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/diffusers/sdxl-text2img.png" (
>>> mask_url = "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/diffusers/sdxl-inpaint-mask.png"

>>> init_image = load_image(img_url).convert("RGB")
>>> mask_image = load_image(mask_url).convert("RGB")

>>> compiler_args = {"auto_cast": "matmul", "auto_cast_type": "bf16"}
>>> input_shapes = {"batch_size": 1, "height": 1024, "width": 1024}
>>> pipeline = NeuronStableDiffusionXLInpaintPipeline.from_pretrained(
...     "stabilityai/stable-diffusion-xl-base-1.0", export=True, **compiler_args, **input_shapes,
... )
>>> pipeline.save_pretrained("sdxl_inpaint/")

>>> prompt = "A deep sea diver floating"
>>> image = pipeline(prompt=prompt, image=init_image, mask_image=mask_image, strength=0.85, guidance_scale=12.5).images[0]
```
