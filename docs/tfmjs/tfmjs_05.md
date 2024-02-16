# 使用自定义模型

> 原始文本：[`huggingface.co/docs/transformers.js/custom_usage`](https://huggingface.co/docs/transformers.js/custom_usage)

默认情况下，Transformers.js 使用[托管的预训练模型](https://huggingface.co/models?library=transformers.js)和[预编译的 WASM 二进制文件](https://cdn.jsdelivr.net/npm/@xenova/transformers@2.15.0/dist/)，应该可以直接使用。您可以按照以下方式自定义：

### 设置

```py
import { env } from '@xenova/transformers';

// Specify a custom location for models (defaults to '/models/').
env.localModelPath = '/path/to/models/';

// Disable the loading of remote models from the Hugging Face Hub:
env.allowRemoteModels = false;

// Set location of .wasm files. Defaults to use a CDN.
env.backends.onnx.wasm.wasmPaths = '/path/to/files/';
```

要查看所有可用设置的完整列表，请查看 API 参考。

### 将您的模型转换为 ONNX

我们建议使用我们的[转换脚本](https://github.com/xenova/transformers.js/blob/main/scripts/convert.py)来将您的 PyTorch、TensorFlow 或 JAX 模型一次性转换为 ONNX。在幕后，它使用[🤗 Optimum](https://huggingface.co/docs/optimum)来执行模型的转换和量化。

```py
python -m scripts.convert --quantize --model_id <model_name_or_path>
```

例如，使用以下命令转换和量化[bert-base-uncased](https://huggingface.co/bert-base-uncased)：

```py
python -m scripts.convert --quantize --model_id bert-base-uncased
```

这将保存以下文件到`./models/`：

```py
bert-base-uncased/
├── config.json
├── tokenizer.json
├── tokenizer_config.json
└── onnx/
    ├── model.onnx
    └── model_quantized.onnx
```

要查看支持的所有架构的完整列表，请参阅[Optimum 文档](https://huggingface.co/docs/optimum/main/en/exporters/onnx/overview)。
