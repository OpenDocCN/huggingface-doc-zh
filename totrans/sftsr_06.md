# 将权重转换为 safetensors

> 原文链接：[`huggingface.co/docs/safetensors/convert-weights`](https://huggingface.co/docs/safetensors/convert-weights)

PyTorch 模型权重通常以`.bin`文件的形式保存和存储，使用 Python 的[`pickle`](https://docs.python.org/3/library/pickle.html)工具。为了以更安全的`safetensor`格式保存和存储您的模型权重，我们建议将您的权重转换为`.safetensors`。

将模型权重转换为`.safetensors`文件的最简单方法是使用[Convert Space](https://huggingface.co/spaces/diffusers/convert)，假设您的模型权重已经存储在 Hub 上。Convert Space 会下载 pickled 权重，将其转换，并打开一个 Pull Request 来上传新转换的`.safetensors`文件到您的存储库。

对于较大的模型，由于其资源被用于转换其他模型，Space 可能会慢一些。您也可以尝试在本地运行[convert.py](https://github.com/huggingface/safetensors/blob/main/bindings/python/convert.py)脚本（这就是 Space 正在运行的内容）来转换您的权重。

如有任何与 Space 相关的问题，请随时联系[@Narsil](https://huggingface.co/Narsil)。
