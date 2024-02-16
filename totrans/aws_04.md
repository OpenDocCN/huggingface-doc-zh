# 快速入门

> [https://huggingface.co/docs/optimum-neuron/quickstart](https://huggingface.co/docs/optimum-neuron/quickstart)

🤗 Optimum Neuron的设计目标是：**使任何🤗 Transformers用户能够轻松进行训练和推理，同时利用AWS加速器的全部功能**。

## 训练

有两个主要的类需要了解：

+   NeuronArgumentParser：继承了Transformers中原始的[HfArgumentParser](https://huggingface.co/docs/transformers/main/en/internal/trainer_utils#transformers.HfArgumentParser)，并对参数值进行额外检查，以确保它们能够很好地与AWS Trainium实例配合使用。

+   [NeuronTrainer](https://huggingface.co/docs/optimum/neuron/package_reference/trainer)：负责编译和分发模型以在Trainium芯片上运行，并执行训练和评估的训练器类。

NeuronTrainer非常类似于🤗 Transformers Trainer，并且通过使用NeuronTrainer来调整脚本以使其与Trainium一起工作，主要是简单地将`Trainer`类替换为`NeuronTrainer`类。这就是大多数[示例脚本](https://github.com/huggingface/optimum-neuron/tree/main/examples)是如何从它们的[原始对应物](https://github.com/huggingface/transformers/tree/main/examples/pytorch)进行调整的。

修改：

```py
from transformers import TrainingArguments
-from transformers import Trainer
+from optimum.neuron import NeuronTrainer as Trainer
training_args = TrainingArguments(
  # training arguments...
)
# A lot of code here
# Initialize our Trainer
trainer = Trainer(
    model=model,
    args=training_args,  # Original training arguments.
    train_dataset=train_dataset if training_args.do_train else None,
    eval_dataset=eval_dataset if training_args.do_eval else None,
    compute_metrics=compute_metrics,
    tokenizer=tokenizer,
    data_collator=data_collator,
)
```

所有Trainium实例至少配备2个Neuron核心。为了利用这些核心，我们需要使用`torchrun`启动训练。下面是一个示例，演示如何在`trn1.2xlarge`实例上使用`bert-base-uncased`模型启动训练脚本。

```py
torchrun --nproc_per_node=2 huggingface-neuron-samples/text-classification/run_glue.py \
--model_name_or_path bert-base-uncased \
--dataset_name philschmid/emotion \
--do_train \
--do_eval \
--bf16 True \
--per_device_train_batch_size 16 \
--learning_rate 5e-5 \
--num_train_epochs 3 \
--output_dir ./bert-emotion
```

## 推理

您可以在Neuron设备上进行推理之前，将🤗 Transformers模型编译并导出为序列化格式：

```py
optimum-cli export neuron 
  --model distilbert-base-uncased-finetuned-sst-2-english \
  --batch_size 1 \
  --sequence_length 32 \
  --auto_cast matmul \
  --auto_cast_type bf16 \
  distilbert_base_uncased_finetuned_sst2_english_neuron/
```

上面的命令将使用静态形状导出`distilbert-base-uncased-finetuned-sst-2-english`：`batch_size=1`和`sequence_length=32`，并将所有`matmul`操作从FP32转换为BF16。查看[导出器指南](https://huggingface.co/docs/optimum-neuron/guides/export_model#exporting-a-model-to-neuron-using-the-cli)以获取更多编译选项。

然后，您可以使用类似于🤗 Transformers中的`AutoModelForXXX`类的`NeuronModelForXXX`类在Neuron设备上运行导出的Neuron模型：

```py
from transformers import AutoTokenizer
-from transformers import AutoModelForSequenceClassification
+from optimum.neuron import NeuronModelForSequenceClassification

# PyTorch checkpoint
-model = AutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")
+model = NeuronModelForSequenceClassification.from_pretrained("distilbert_base_uncased_finetuned_sst2_english_neuron")

tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")
inputs = tokenizer("Hamilton is considered to be the best musical of past years.", return_tensors="pt")

logits = model(**inputs).logits
print(model.config.id2label[logits.argmax().item()])
# 'POSITIVE'
```
