# 支持的模型和硬件

> 原始文本：[https://huggingface.co/docs/text-embeddings-inference/supported_models](https://huggingface.co/docs/text-embeddings-inference/supported_models)

我们正在不断扩大对其他模型类型的支持，并计划在未来更新中包括它们。

## 支持的嵌入模型

文本嵌入推理目前支持具有绝对位置的 BERT、CamemBERT、XLM-RoBERTa 模型以及具有 Alibi 位置的 JinaBERT 模型。

以下是当前支持的模型的一些示例：

| MTEB 排名 | 模型类型 | 模型 ID |
| --- | --- | --- |
| 1 | Bert | [BAAI/bge-large-en-v1.5](https://hf.co/BAAI/bge-large-en-v1.5) |
| 2 |  | [BAAI/bge-base-en-v1.5](https://hf.co/BAAI/bge-base-en-v1.5) |
| 3 |  | [llmrails/ember-v1](https://hf.co/llmrails/ember-v1) |
| 4 |  | [thenlper/gte-large](https://hf.co/thenlper/gte-large) |
| 5 |  | [thenlper/gte-base](https://hf.co/thenlper/gte-base) |
| 6 |  | [intfloat/e5-large-v2](https://hf.co/intfloat/e5-large-v2) |
| 7 |  | [BAAI/bge-small-en-v1.5](https://hf.co/BAAI/bge-small-en-v1.5) |
| 10 |  | [intfloat/e5-base-v2](https://hf.co/intfloat/e5-base-v2) |
| 11 | XLM-RoBERTa | [intfloat/multilingual-e5-large](https://hf.co/intfloat/multilingual-e5-large) |
| N/A | JinaBERT | [jinaai/jina-embeddings-v2-base-en](https://hf.co/jinaai/jina-embeddings-v2-base-en) |
| N/A | JinaBERT | [jinaai/jina-embeddings-v2-small-en](https://hf.co/jinaai/jina-embeddings-v2-small-en) |

要探索最佳表现的文本嵌入模型列表，请访问[大规模文本嵌入基准（MTEB）排行榜](https://huggingface.co/spaces/mteb/leaderboard)。

## 支持的重新排序器和序列分类模型

文本嵌入推理目前支持具有绝对位置的 CamemBERT 和 XLM-RoBERTa 序列分类模型。

以下是当前支持的模型的一些示例：

| 任务 | 模型类型 | 模型 ID | 修订版本 |
| --- | --- | --- | --- |
| 重新排序 | XLM-RoBERTa | [BAAI/bge-reranker-large](https://huggingface.co/BAAI/bge-reranker-large) | `refs/pr/4` |
| 重新排序 | XLM-RoBERTa | [BAAI/bge-reranker-base](https://huggingface.co/BAAI/bge-reranker-base) | `refs/pr/5` |
| 情感分析 | RoBERTa | [SamLowe/roberta-base-go_emotions](https://huggingface.co/SamLowe/roberta-base-go_emotions) |  |

## 支持的硬件

文本嵌入推理支持可以在 CPU、图灵（T4、RTX 2000 系列，...）、安培 80（A100、A30）、安培 86（A10、A40，...）、艾达·洛维斯（RTX 4000 系列，...）和霍珀（H100）架构上使用。

该库**不**支持 CUDA 计算能力 < 7.5，这意味着 V100、Titan V、GTX 1000 系列等不受支持。为了利用您的 GPU，请确保安装[NVIDIA 容器工具包](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)，并使用 CUDA 版本为 12.2 或更高的 NVIDIA 驱动程序。

在以下表格中找到适合您硬件的 Docker 镜像：

| 架构 | 镜像 |
| --- | --- |
| CPU | ghcr.io/huggingface/text-embeddings-inference:cpu-0.6 |
| Volta | 不支持 |
| 图灵（T4、RTX 2000 系列，...） | ghcr.io/huggingface/text-embeddings-inference:turing-0.6（实验性） |
| 安培 80（A100、A30） | ghcr.io/huggingface/text-embeddings-inference:0.6 |
| 安培 86（A10、A40，...） | ghcr.io/huggingface/text-embeddings-inference:86-0.6 |
| 艾达·洛维斯（RTX 4000 系列，...） | ghcr.io/huggingface/text-embeddings-inference:89-0.6 |
| 霍珀（H100） | ghcr.io/huggingface/text-embeddings-inference:hopper-0.4.0（实验性） |

**警告**：由于存在精度问题，图灵镜像默认关闭了 Flash Attention。您可以通过使用`USE_FLASH_ATTENTION=True`环境变量将 Flash Attention v1 打开。
