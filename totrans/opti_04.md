# 快速导览

> 原文：[https://huggingface.co/docs/optimum/quicktour](https://huggingface.co/docs/optimum/quicktour)

这个快速导览适用于准备深入代码并查看如何将🤗 Optimum集成到他们的模型训练和推理工作流程中的开发人员。

## 加速推理

#### OpenVINO

要加载模型并使用OpenVINO Runtime进行推理，您只需将您的`AutoModelForXxx`类替换为相应的`OVModelForXxx`类。如果要加载PyTorch检查点，请设置`export=True`以将模型转换为OpenVINO IR（中间表示）。

```py
- from transformers import AutoModelForSequenceClassification
+ from optimum.intel.openvino import OVModelForSequenceClassification
  from transformers import AutoTokenizer, pipeline

  # Download a tokenizer and model from the Hub and convert to OpenVINO format
  tokenizer = AutoTokenizer.from_pretrained(model_id)
  model_id = "distilbert-base-uncased-finetuned-sst-2-english"
- model = AutoModelForSequenceClassification.from_pretrained(model_id)
+ model = OVModelForSequenceClassification.from_pretrained(model_id, export=True)

  # Run inference!
  classifier = pipeline("text-classification", model=model, tokenizer=tokenizer)
  results = classifier("He's a dreadful magician.")
```

您可以在[文档](https://huggingface.co/docs/optimum/intel/inference)和[示例](https://github.com/huggingface/optimum-intel/tree/main/examples/openvino)中找到更多示例。

#### ONNX Runtime

为了加速使用ONNX Runtime进行推理，🤗 Optimum使用*配置对象*来定义图优化和量化的参数。然后使用这些对象来实例化专用的*优化器*和*量化器*。

在应用量化或优化之前，首先我们需要加载我们的模型。要加载模型并使用ONNX Runtime进行推理，您只需将经典的Transformers [`AutoModelForXxx`](https://huggingface.co/docs/transformers/model_doc/auto#transformers.AutoModel)类替换为相应的[`ORTModelForXxx`](https://huggingface.co/docs/optimum/onnxruntime/package_reference/modeling_ort#optimum.onnxruntime.ORTModel)类。如果要从PyTorch检查点加载，请设置`export=True`以将模型导出为ONNX格式。

```py
>>> from optimum.onnxruntime import ORTModelForSequenceClassification
>>> from transformers import AutoTokenizer

>>> model_checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
>>> save_directory = "tmp/onnx/"

>>> # Load a model from transformers and export it to ONNX
>>> tokenizer = AutoTokenizer.from_pretrained(model_checkpoint)
>>> ort_model = ORTModelForSequenceClassification.from_pretrained(model_checkpoint, export=True)

>>> # Save the ONNX model and tokenizer
>>> ort_model.save_pretrained(save_directory)
>>> tokenizer.save_pretrained(save_directory)
```

现在让我们看看如何使用ONNX Runtime应用动态量化：

```py
>>> from optimum.onnxruntime.configuration import AutoQuantizationConfig
>>> from optimum.onnxruntime import ORTQuantizer

>>> # Define the quantization methodology
>>> qconfig = AutoQuantizationConfig.arm64(is_static=False, per_channel=False)
>>> quantizer = ORTQuantizer.from_pretrained(ort_model)

>>> # Apply dynamic quantization on the model
>>> quantizer.quantize(save_dir=save_directory, quantization_config=qconfig)
```

在这个例子中，我们对来自Hugging Face Hub的模型进行了量化，以相同的方式，我们可以通过提供包含模型权重的目录路径来对本地托管的模型进行量化。应用`quantize()`方法的结果是一个`model_quantized.onnx`文件，可用于运行推理。这里是如何加载一个ONNX Runtime模型并生成预测的示例：

```py
>>> from optimum.onnxruntime import ORTModelForSequenceClassification
>>> from transformers import pipeline, AutoTokenizer

>>> model = ORTModelForSequenceClassification.from_pretrained(save_directory, file_name="model_quantized.onnx")
>>> tokenizer = AutoTokenizer.from_pretrained(save_directory)
>>> classifier = pipeline("text-classification", model=model, tokenizer=tokenizer)
>>> results = classifier("I love burritos!")
```

您可以在[文档](https://huggingface.co/docs/optimum/onnxruntime/quickstart)和[示例](https://github.com/huggingface/optimum/tree/main/examples/onnxruntime)中找到更多示例。

## 加速训练

#### Habana

为了在Habana的Gaudi处理器上训练transformers，🤗 Optimum提供了一个`GaudiTrainer`，它与🤗 Transformers [Trainer](https://huggingface.co/docs/transformers/main_classes/trainer)非常相似。这里是一个简单的例子：

```py
- from transformers import Trainer, TrainingArguments
+ from optimum.habana import GaudiTrainer, GaudiTrainingArguments

  # Download a pretrained model from the Hub
  model = AutoModelForXxx.from_pretrained("bert-base-uncased")

  # Define the training arguments
- training_args = TrainingArguments(
+ training_args = GaudiTrainingArguments(
      output_dir="path/to/save/folder/",
+     use_habana=True,
+     use_lazy_mode=True,
+     gaudi_config_name="Habana/bert-base-uncased",
      ...
  )

  # Initialize the trainer
- trainer = Trainer(
+ trainer = GaudiTrainer(
      model=model,
      args=training_args,
      train_dataset=train_dataset,
      ...
  )

  # Use Habana Gaudi processor for training!
  trainer.train()
```

您可以在[文档](https://huggingface.co/docs/optimum/habana/quickstart)和[示例](https://github.com/huggingface/optimum-habana/tree/main/examples)中找到更多示例。

#### ONNX Runtime

使用ONNX Runtime的加速功能来训练transformers，🤗 Optimum提供了一个`ORTTrainer`，它与🤗 Transformers [Trainer](https://huggingface.co/docs/transformers/main_classes/trainer)非常相似。这里是一个简单的例子：

```py
- from transformers import Trainer, TrainingArguments
+ from optimum.onnxruntime import ORTTrainer, ORTTrainingArguments

  # Download a pretrained model from the Hub
  model = AutoModelForSequenceClassification.from_pretrained("bert-base-uncased")

  # Define the training arguments
- training_args = TrainingArguments(
+ training_args = ORTTrainingArguments(
      output_dir="path/to/save/folder/",
      optim="adamw_ort_fused",
      ...
  )

  # Create a ONNX Runtime Trainer
- trainer = Trainer(
+ trainer = ORTTrainer(
      model=model,
      args=training_args,
      train_dataset=train_dataset,
+     feature="text-classification", # The model type to export to ONNX
      ...
  )

  # Use ONNX Runtime for training!
  trainer.train()
```

您可以在[文档](https://huggingface.co/docs/optimum/onnxruntime/usage_guides/trainer)和[示例](https://github.com/huggingface/optimum/tree/main/examples/onnxruntime/training)中找到更多示例。

## 开箱即用的ONNX导出

Optimum库可以直接处理Transformers和Diffusers模型的ONNX导出！

将模型导出到ONNX就像这样简单

```py
optimum-cli export onnx --model gpt2 gpt2_onnx/
```

查看帮助以获取更多选项：

```py
optimum-cli export onnx --help
```

查看[文档](https://huggingface.co/docs/optimum/exporters/onnx/usage_guides/export_a_model)获取更多信息。

## PyTorch的BetterTransformer支持

[BetterTransformer](https://pytorch.org/blog/a-better-transformer-for-fast-transformer-encoder-inference/) 是一个免费的 PyTorch 本地优化，可在基于 Transformer 的模型推理中获得 x1.25 - x4 的加速。它已在[PyTorch 1.13](https://pytorch.org/blog/PyTorch-1.13-release/)中标记为稳定。我们将 BetterTransformer 与 🤗 Transformers 库中最常用的模型集成，使用集成就像这样简单：

```py
>>> from optimum.bettertransformer import BetterTransformer
>>> from transformers import AutoModelForSequenceClassification

>>> model = AutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")
>>> model = BetterTransformer.transform(model)
```

查看更多详细信息，请查看[文档](https://huggingface.co/docs/optimum/bettertransformer/overview)，并查看[PyTorch Medium 上的博文](https://medium.com/pytorch/bettertransformer-out-of-the-box-performance-for-huggingface-transformers-3fbe27d50ab2)以了解更多关于集成的信息！

## torch.fx 集成

Optimum 与 `torch.fx` 集成，提供了几个图形转换的一行代码。我们旨在通过 `torch.fx` 支持更好的[量化](https://huggingface.co/docs/optimum/concept_guides/quantization)管理，包括量化感知训练（QAT）和训练后量化（PTQ）。

查看更多[文档](https://huggingface.co/docs/optimum/torch_fx/usage_guides/optimization)和[参考资料](https://huggingface.co/docs/optimum/torch_fx/package_reference/optimization)！
