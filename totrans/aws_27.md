# Optimum Neuron Distributed

> 原始文本：[`huggingface.co/docs/optimum-neuron/package_reference/distributed`](https://huggingface.co/docs/optimum-neuron/package_reference/distributed)

`optimum.neuron.distributed`模块提供了一组工具来执行分布式训练和推理。

## 并行化

在分布式训练/推理中的主要任务是能够分片模型权重、梯度和/或优化器状态等内容。我们构建了`Parallelizer`类来处理分片。

### 基础并行化器

`Parallelizer`类是为支持模型并行性的每个模型派生的基础抽象类。它提供了并行化模型、保存和加载分片检查点的方法。

### `class optimum.neuron.distributed.Parallelizer`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/distributed/base.py#L135)

```py
( )
```

处理模型并行性的基础抽象类。

`_parallelize`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/distributed/base.py#L238)

```py
( model: PreTrainedModel device: Optional = None parallelize_embeddings: bool = True sequence_parallel_enabled: bool = False ) → export const metadata = 'undefined';PreTrainedModel
```

参数

+   `model` (`PreTrainedModel`) — 要并行化的模型。

+   `device` (`Optional[torch.device]`，默认为`None`) — 新并行层应该放置的设备。

+   `parallelize_embeddings` (`bool`，默认为`True`) — 是否应该并行化嵌入。当 TP 大小不能整除词汇量大小时，可以禁用此选项。

+   `sequence_parallel_enabled` (`bool`，默认为`False`) — 是否启用序列并行性。

返回

`PreTrainedModel`

并行化后的模型。

通过将常规层转换为其并行化对应层来并行化模型。每个具体类必须实现它。

#### `parallelize`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/distributed/base.py#L264)

```py
( model: PreTrainedModel device: Optional = None parallelize_embeddings: bool = True sequence_parallel_enabled: bool = False pipeline_parallel_input_names: Union = None pipeline_parallel_num_microbatches: int = 1 pipeline_parallel_use_zero1_optimizer: bool = False checkpoint_dir: Union = None ) → export const metadata = 'undefined';PreTrainedModel
```

参数

+   `model` (`PreTrainedModel`) — 要并行化的模型。

+   `device` (`Optional[torch.device]`，默认为`None`) — 新并行层应该放置的设备。

+   `parallelize_embeddings` (`bool`，默认为`True`) — 是否应该并行化嵌入。当 TP 大小不能整除词汇量大小时，可以禁用此选项。

+   `sequence_parallel_enabled` (`bool`，默认为`False`) — 是否启用序列并行性。

+   `pipeline_parallel_num_microbatches` (`int`，默认为 1) — 用于管道执行的微批次数。

+   `pipeline_parallel_use_zero1_optimizer` (`bool`，默认为`False`) — 当使用 zero-1 优化器时，将此选项设置为 True，这样 PP 模型将了解 zero-1 优化器将处理数据并行梯度平均。

+   `checkpoint_dir` (`Optional[Union[str, Path]]`) — 分片检查点的路径。如果指定，检查点权重将被加载到并行化模型中。

返回

`PreTrainedModel`

并行化后的模型。

通过使用`cls._parallelize()`将常规层转换为其并行化对应层来并行化模型。

它还确保每个参数已加载其权重或者如果没有与之关联的预训练权重，则已初始化。

#### `optimizer_for_mp`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/distributed/base.py#L610)

```py
( optimizer: torch.optim.Optimizer orig_param_to_parallel_param_on_xla: Mapping ) → export const metadata = 'undefined';torch.optim.Optimizer
```

参数

+   `optimizer` (`torch.optim.Optimizer`) — 原始优化器。

+   `orig_param_to_parallel_param_on_xla` (`Mapping[int, torch.nn.Parameter]`) — 将`optimizer`中参数的 ID 映射到其在 XLA 设备上的并行化对应项的映射（例如类似于字典）。

返回

`torch.optim.Optimizer`

张量并行准备好的优化器。

从现有的优化器创建一个为并行化模型准备好的优化器。

有两种情况：

1.  优化器是通过`optimum.neuron.distributed.utils.make_optimizer_constructor_lazy`的延迟构造函数创建的，这种情况下，为张量并行性创建了确切的优化器。

1.  使用常规构造函数创建了优化器。在这种情况下，为张量并行性创建的优化器尽可能接近预期，但不能保证完全一致。

#### `save_model_checkpoint`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/distributed/base.py#L757)

```py
( model: PreTrainedModel output_dir: Union as_regular: bool = False as_sharded: bool = True optimizer: Optional = None )
```

#### `load_model_checkpoint`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/distributed/base.py#L789)

```py
( model: PreTrainedModel load_dir: Union )
```

### 选择特定于模型的并行器类

`optimum-neuron` 中支持并行化的每个模型都有自己派生的 `Parallelizer` 类。工厂类 `ParallelizersManager` 允许您轻松检索这些特定于模型的 `Parallelizer`。

### `class optimum.neuron.distributed.ParallelizersManager`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/distributed/parallelizers_manager.py#L52)

```py
( )
```

#### `get_supported_model_types`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/distributed/parallelizers_manager.py#L65)

```py
( )
```

提供支持并行化的模型类型列表。

#### `is_model_supported`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/distributed/parallelizers_manager.py#L85)

```py
( model_type_or_model: Union )
```

参数

+   `model_type_or_model` (`Union[str, PreTrainedModel]`) — 模型类型或模型实例。

如果模型可以并行化，则返回 `True`，否则返回 `False`。

#### `parallelizer_for_model`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/distributed/parallelizers_manager.py#L97)

```py
( model_type_or_model: Union )
```

参数

+   `model_type_or_model` (`Union[str, PreTrainedModel]`) — 模型类型或模型实例。

返回与模型关联的并行器类。

## 工具

### 延迟加载

当模型太大无法适应一个设备时，通常需要分布式训练/推理。因此，需要能够延迟加载模型权重和优化器状态的工具，以避免在并行化之前出现内存不足。

#### `optimum.neuron.distributed.lazy_load_for_parallelism`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/distributed/utils.py#L789)

```py
( tensor_parallel_size: int = 1 pipeline_parallel_size: int = 1 )
```

参数

+   `tensor_parallel_size` (`int`，默认为 1）— 考虑的张量并行大小。

+   `pipeline_parallel_size` (`int`，默认为 1）— 考虑的管道并行大小。

上下文管理器，使模型加载变为延迟加载，用于模型并行性：

+   每个 `torch.nn.Linear` 都放在 `torch.device("meta")` 设备上，这意味着实例化时不占用内存。

+   每个 `torch.nn.Embedding` 也放在 `torch.device("meta")` 设备上。

+   实际上并未加载状态字典，而是创建了一个权重映射并附加到模型上。有关更多信息，请阅读 `optimum.neuron.distributed.utils.from_pretrained_for_mp` 的文档字符串。

如果 `tensor_parallel_size` 和 `pipeline_parallel_size` 都设置为 1，则不执行延迟加载。

`optimum.neuron.distributed.make_optimizer_constructor_lazy`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/distributed/utils.py#L835)

```py
( optimizer_cls: Type )
```

将优化器构造函数（优化器类）转换为延迟加载，不初始化参数。这使得优化器轻量且可用于在模型并行化后创建“真实”优化器。
