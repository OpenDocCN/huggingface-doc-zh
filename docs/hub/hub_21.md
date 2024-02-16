# 模型下载统计

> 原文链接：[`huggingface.co/docs/hub/models-download-stats`](https://huggingface.co/docs/hub/models-download-stats)

## 模型的下载统计是如何生成的？

对模型的下载次数进行计数并不是一项简单的任务，因为单个模型存储库可能包含多个文件，包括多个模型权重文件（例如，具有分片模型），并且取决于库的不同格式。为了避免重复计算下载次数（例如，将模型的单次下载计为多次下载），Hub 使用一组用于下载计数的查询文件。用户不会发送任何信息，也不会为此进行额外的调用。计数是在服务器端完成的，因为我们为下载提供文件。

每个对这些文件的 HTTP 请求，包括`GET`和`HEAD`，都将被计为一次下载。默认情况下，当未指定库时，Hub 会将`config.json`作为默认查询文件。否则，查询文件取决于每个库，Hub 可能会检查诸如`pytorch_model.bin`和`adapter_config.json`之类的文件。

## 不同库的查询文件是哪些？

默认情况下，Hub 会查看`config.json`、`config.yaml`、`hyperparams.yaml`和`meta.yaml`。对于以下一组库，有特定的查询文件。

```py
{
    "adapter-transformers": {
        filter: [
            {
                term: { path: "adapter_config.json" },
            },
        ],
    },
    "asteroid": {
        filter: [
            {
                term: { path: "pytorch_model.bin" },
            },
        ],
    },
    "flair": {
        filter: [
            {
                term: { path: "pytorch_model.bin" },
            },
        ],
    },
    "keras": {
        filter: [
            {
                term: { path: "saved_model.pb" },
            },
        ],
    },
    "ml-agents": {
        filter: [
            {
                wildcard: { path: "*.onnx" },
            },
        ],
    },
    "nemo": {
        filter: [
            {
                wildcard: { path: "*.nemo" },
            },
        ],
    },
    "open_clip": {
        filter: [
            {
                wildcard: { path: "*pytorch_model.bin" },
            },
        ],
    },
    "sample-factory": {
        filter: [
            {
                term: { path: "cfg.json" },
            },
        ],
    },
    "paddlenlp": {
        filter: [
            {
                term: { path: "model_config.json" },
            },
        ],
    },
    "speechbrain": {
        filter: [
            {
                term: { path: "hyperparams.yaml" },
            },
        ],
    },
    "sklearn": {
        filter: [
            {
                term: { path: "sklearn_model.joblib" },
            },
        ],
    },
    "spacy": {
        filter: [
            {
                wildcard: { path: "*.whl" },
            },
        ],
    },
    "stanza": {
        filter: [
            {
                term: { path: "models/default.zip" },
            },
        ],
    },
    "stable-baselines3": {
        filter: [
            {
                wildcard: { path: "*.zip" },
            },
        ],
    },
    "timm": {
        filter: [
            {
                terms: { path: ["pytorch_model.bin", "model.safetensors"] },
            },
        ],
    },
    "diffusers": {
        /// Filter out nested safetensors and pickle weights to avoid double counting downloads from the diffusers lib
        must_not: [
            {
                wildcard: { path: "*/*.safetensors" },
            },
            {
                wildcard: { path: "*/*.bin" },
            },
        ],
        /// Include documents that match at least one of the following rules
        should: [
            /// Downloaded from diffusers lib
            {
                term: { path: "model_index.json" },
            },
            /// Direct downloads (LoRa, Auto1111 and others)
            {
                wildcard: { path: "*.safetensors" },
            },
            {
                wildcard: { path: "*.ckpt" },
            },
            {
                wildcard: { path: "*.bin" },
            },
        ],
        minimum_should_match: 1,
    },
    "peft": {
        filter: [
            {
                term: { path: "adapter_config.json" },
            },
        ],
    }
}
```
