# 量化

> 原文链接：[https://huggingface.co/docs/text-generation-inference/conceptual/quantization](https://huggingface.co/docs/text-generation-inference/conceptual/quantization)

TGI提供了GPTQ和bits-and-bytes量化，以量化大型语言模型。

## 使用GPTQ进行量化

GPTQ是一种后训练量化方法，可以使模型更小。它通过找到该权重的压缩版本来量化层，这将产生一个最小均方误差，如下所示👇

给定一个带有权重矩阵<math><semantics><mrow><msub><mi>W</mi><mi>l</mi></msub></mrow><annotation encoding="application/x-tex">W_{l}</annotation></semantics></math>Wl和层输入<math><semantics><mrow><msub><mi>X</mi><mi>l</mi></msub></mrow><annotation encoding="application/x-tex">X_{l}</annotation></semantics></math>Xl的层<math><semantics><mrow><mi>l</mi></mrow><annotation encoding="application/x-tex">l</annotation></semantics></math>l，找到量化权重<math><semantics><mrow><mi>h</mi><mi>a</mi><mi>t</mi><msub><mi>W</mi><mi>l</mi></msub></mrow><annotation encoding="application/x-tex">\\hat{W}_{l}</annotation></semantics></math>hatWl： <math display="block"><semantics><mrow><mo stretchy="false">(</mo><msup><msub><mover accent="true"><mi>W</mi><mo>^</mo></mover><mi>l</mi></msub><mo lspace="0em" rspace="0em">∗</mo></msup><mo>=</mo><mi>a</mi><mi>r</mi><mi>g</mi><mi>m</mi><mi>i</mi><msub><mi>n</mi><mover accent="true"><msub><mi>W</mi><mi>l</mi></msub><mo>^</mo></mover></msub><mi mathvariant="normal">∣</mi><mi mathvariant="normal">∣</mi><msub><mi>W</mi><mi>l</mi></msub><mi>X</mi><mo>−</mo><msub><mover accent="true"><mi>W</mi><mo>^</mo></mover><mi>l</mi></msub><mi>X</mi><mi mathvariant="normal">∣</mi><msubsup><mi mathvariant="normal">∣</mi><mn>2</mn><mn>2</mn></msubsup><mo stretchy="false">)</mo></mrow><annotation encoding="application/x-tex">({\hat{W}_{l}}^{*} = argmin_{\hat{W_{l}}} ||W_{l}X-\hat{W}_{l}X||^{2}_{2})</annotation></semantics></math>(W^l​∗=argminWl​^​​∣∣Wl​X−W^l​X∣∣22​)

TGI允许您运行已经GPTQ量化的模型（查看可用模型[此处](https://huggingface.co/models?search=gptq)）或使用量化脚本量化您选择的模型。您可以通过简单地传递—quantize来运行一个量化模型，如下所示👇

```py
docker run --gpus all --shm-size 1g -p 8080:80 -v $volume:/data ghcr.io/huggingface/text-generation-inference:latest --model-id $model --quantize gptq
```

请注意，TGI的GPTQ实现不使用[AutoGPTQ](https://github.com/PanQiWei/AutoGPTQ)作为底层。但是，使用AutoGPTQ或Optimum量化的模型仍然可以被TGI提供服务。

要使用GPTQ对给定模型进行量化，只需运行

```py
text-generation-server quantize tiiuae/falcon-40b /data/falcon-40b-gptq
# Add --upload-to-model-id MYUSERNAME/falcon-40b to push the created model to the hub directly
```

这将创建一个包含量化文件的新目录，您可以使用以下命令进行操作，

```py
text-generation-launcher --model-id /data/falcon-40b-gptq/ --sharded true --num-shard 2 --quantize gptq
```

您可以通过运行`text-generation-server quantize --help`来了解更多有关量化选项的信息。

如果您希望对GPTQ模型进行更多操作（例如在顶部训练一个适配器），您可以阅读有关transformers GPTQ集成的信息[此处](https://huggingface.co/blog/gptq-integration)。您可以从[论文](https://arxiv.org/pdf/2210.17323.pdf)中了解更多关于GPTQ的信息。

## 使用bitsandbytes进行量化

bitsandbytes是一个用于将8位和4位量化应用于模型的库。与GPTQ量化不同，bitsandbytes不需要校准数据集或任何后处理-权重会在加载时自动量化。但是，使用bitsandbytes进行推断比GPTQ或FP16精度要慢。

8位量化使得多十亿参数规模的模型可以适应更小的硬件而不会太大程度地降低性能。在TGI中，您可以通过添加`--quantize bitsandbytes`来使用8位量化，如下所示👇

```py
docker run --gpus all --shm-size 1g -p 8080:80 -v $volume:/data ghcr.io/huggingface/text-generation-inference:latest --model-id $model --quantize bitsandbytes
```

4位量化也可以使用bitsandbytes。您可以选择以下4位数据类型之一：4位浮点数（`fp4`）或4位`NormalFloat`（`nf4`）。这些数据类型是在参数高效微调的背景下引入的，但您可以通过在加载时自动转换模型权重来将其应用于推断。

在 TGI 中，您可以通过添加 `--quantize bitsandbytes-nf4` 或 `--quantize bitsandbytes-fp4` 来使用 4 位量化，就像下面这样👇

```py
docker run --gpus all --shm-size 1g -p 8080:80 -v $volume:/data ghcr.io/huggingface/text-generation-inference:latest --model-id $model --quantize bitsandbytes-nf4
```

您可以通过阅读这篇[博客文章](https://huggingface.co/blog/hf-bitsandbytes-integration)了解更多关于 8 位量化的信息，以及通过阅读[这篇博客文章](https://huggingface.co/blog/4bit-transformers-bitsandbytes)了解 4 位量化。
