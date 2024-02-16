# NeuronTrainer

> 原文：[https://huggingface.co/docs/optimum-neuron/package_reference/trainer](https://huggingface.co/docs/optimum-neuron/package_reference/trainer)

[`NeuronTrainer`](https://huggingface.co/docs/optimum/neuron/package_reference/trainer#optimum.neuron.NeuronTrainer)类为功能完整的[Transformers Trainer](https://huggingface.co/docs/transformers/main_classes/trainer)提供了扩展API。它在所有的[示例脚本](https://github.com/huggingface/optimum-neuron/tree/main/examples)中使用。

[`NeuronTrainer`](https://huggingface.co/docs/optimum/neuron/package_reference/trainer#optimum.neuron.NeuronTrainer)类针对在AWS Trainium上运行的🤗 Transformers模型进行了优化。

这是一个如何自定义[`NeuronTrainer`](https://huggingface.co/docs/optimum/neuron/package_reference/trainer#optimum.neuron.NeuronTrainer)以使用加权损失的示例（当您有一个不平衡的训练集时很有用）：

```py
from torch import nn
from optimum.neuron import NeuronTrainer

class CustomNeuronTrainer(NeuronTrainer):
    def compute_loss(self, model, inputs, return_outputs=False):
        labels = inputs.get("labels")
        # forward pass
        outputs = model(**inputs)
        logits = outputs.get("logits")
        # compute custom loss (suppose one has 3 labels with different weights)
        loss_fct = nn.CrossEntropyLoss(weight=torch.tensor([1.0, 2.0, 3.0]))
        loss = loss_fct(logits.view(-1, self.model.config.num_labels), labels.view(-1))
        return (loss, outputs) if return_outputs else loss
```

另一种自定义PyTorch [`NeuronTrainer`](https://huggingface.co/docs/optimum/neuron/package_reference/trainer#optimum.neuron.NeuronTrainer)的训练循环行为的方法是使用[callbacks](https://huggingface.co/docs/transformers/main_classes/callback)，这些callbacks可以检查训练循环的状态（用于进度报告、在TensorBoard或其他ML平台上记录日志…）并做出决策（如提前停止）。

## NeuronTrainer

### `class optimum.neuron.NeuronTrainer`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/trainers.py#L1319)

```py
( *args **kwargs )
```

适用于在AWS Tranium实例上进行训练的Trainer。

### `class optimum.neuron.Seq2SeqNeuronTrainer`

[<来源>](https://github.com/huggingface/optimum-neuron/blob/main/optimum/neuron/trainers.py#L1325)

```py
( *args **kwargs )
```

适用于在AWS Tranium实例上进行训练的Seq2SeqTrainer。
