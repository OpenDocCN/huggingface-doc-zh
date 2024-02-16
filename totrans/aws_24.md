# 为新架构添加支持

> 原文链接: [https://huggingface.co/docs/optimum-neuron/community/contributing](https://huggingface.co/docs/optimum-neuron/community/contributing)
> 
> ***注意:*** ❗这一部分不适用于通过`transformers-neuronx`进行自回归采样的解码器模型的推断。如果您想为这些模型添加支持，请在Optimum Neuron GitHub存储库上开启一个问题，并@维护者寻求帮助。

您想在AWS Inferentia或Trainium上导出并运行新模型吗？查看指南，并向[🤗 Optimum Neuron的GitHub存储库](https://github.com/huggingface/optimum-neuron/)提交拉取请求！

为了在Optimum Neuron库中支持新的模型架构，以下是一些要遵循的步骤：

1.  实现自定义神经元配置。

1.  导出并验证模型。

1.  贡献到GitHub存储库。

## 实现自定义神经元配置

为了支持将新模型导出为Neuron兼容格式，首先要做的是定义一个神经元配置，描述如何通过指定来导出PyTorch模型：

1.  输入名称。

1.  输出名称。

1.  用于跟踪模型的虚拟输入：Neuron编译器通过跟踪记录计算图，并在生成的`TorchScript`模块上工作。

1.  用于控制硬件效率（延迟、吞吐量）和准确性之间权衡的编译参数。

根据模型和任务的选择，我们使用配置类表示上面的数据。每个配置类与特定的模型架构相关联，并遵循命名约定`ArchitectureNameNeuronConfig`。例如，指定BERT模型的Neuron导出的配置是`BertNeuronConfig`。

由于许多架构共享类似的神经元配置属性，🤗 Optimum采用了3级类层次结构：

1.  抽象和通用的基类。这些处理所有基本特性，同时对模态（文本、图像、音频等）保持不可知。

1.  中间类。这些了解模态。对于同一模态，可能存在多个配置类，取决于它们支持的输入。它们指定应使用哪些输入生成器来生成虚拟输入，但保持与模型无关。

1.  像上面提到的`BertNeuronConfig`这样的特定于模型的类。这些实际上是用于导出模型的类。

### 示例：为ESM模型添加支持

在这里，我们以[ESM模型](https://huggingface.co/docs/transformers/model_doc/esm#esm)为例。让我们在`optimum/exporters/neuron/model_configs.py`中创建一个`EsmNeuronConfig`类。

当一个Esm模型被解释为文本编码器时，我们可以从中间类[`TextEncoderNeuronConfig`](https://github.com/huggingface/optimum-neuron/blob/v0.0.18/optimum/exporters/neuron/config.py#L36)继承。由于当Esm被解释为编码器时，建模和配置几乎与BERT相同，我们可以使用`NormalizedConfigManager`和`model_type=bert`来规范化配置，以生成用于跟踪模型的虚拟输入。

最后一步，由于`optimum-neuron`是`optimum`的扩展，我们需要将创建的神经元配置注册到[TasksManager](https://huggingface.co/docs/optimum/main/en/exporters/task_manager#optimum.exporters.TasksManager)中，通过指定模型类型和支持的任务使用`register_in_tasks_manager`装饰器。

```py

@register_in_tasks_manager("esm", *["feature-extraction", "fill-mask", "text-classification", "token-classification"])
class EsmNeuronConfig(TextEncoderNeuronConfig):
    NORMALIZED_CONFIG_CLASS = NormalizedConfigManager.get_normalized_config_class("bert")
    ATOL_FOR_VALIDATION = 1e-3  # absolute tolerance to compare for comparing model on CPUs

 @property
    def inputs(self) -> List[str]:
        return ["input_ids", "attention_mask"]

```

## 导出并验证模型

使用您实现的神经元配置类，现在快速测试一下是否按预期工作：

+   导出

```py
optimum-cli export neuron --model facebook/esm2_t33_650M_UR50D --task text-classification --batch_size 1 --sequence_length 16 esm_neuron/
```

在导出期间，将调用[`validate_model_outputs`](https://github.com/huggingface/optimum-neuron/blob/7b18de9ddfa5c664c94051304c651eaf855c3e0b/optimum/exporters/neuron/convert.py#L136)来验证导出的Neuron模型的输出，通过将其与PyTorch在CPU上的结果进行比较。您也可以手动验证模型。

```py
from optimum.exporters.neuron import validate_model_outputs

validate_model_outputs(
    neuron_config, base_model, neuron_model_path, neuron_named_outputs, neuron_config.ATOL_FOR_VALIDATION
)
```

+   推断（可选）

```py
from transformers import AutoTokenizer
from optimum.neuron import NeuronModelForSequenceClassification

model = NeuronModelForSequenceClassification.from_pretrained("esm_neuron/")
tokenizer = AutoTokenizer.from_pretrained("esm_neuron/")
inputs = tokenizer("Hello, my dog is cute", return_tensors="pt")
logits = model(**inputs).logits
```

## 贡献到GitHub仓库

我们几乎已经准备就绪。现在提交一个拉取请求，使您的工作对所有社区成员可访问！

+   在[Optimum Neuron GitHub仓库](https://github.com/huggingface/optimum-neuron/issues)中开启一个问题来描述新功能，并让Optimum Neuron的维护者们看到。

+   将模型添加到[`optimum-neuron/tests/exporters/exporters_utils.py`](https://github.com/huggingface/optimum-neuron/blob/v0.0.18/tests/exporters/exporters_utils.py)中的导出器测试和[`optimum-neuron/tests/inference/inference_utils.py`](https://github.com/huggingface/optimum-neuron/blob/v0.0.18/tests/inference/inference_utils.py)中的推理测试。

+   开启一个拉取请求！（不要忘记将其链接到您开启的问题，以便维护者在需要时更好地跟踪并提供帮助。）

通常我们会测试较小的检查点以加速CI，您可以在[`Hugging Face Internal Testing Organization`](https://huggingface.co/hf-internal-testing)下找到用于测试的微型模型。

您已经在Neuron上为社区提供了一个新的模型！感谢您加入我们努力使优秀的机器学习民主化的行动🤗。
