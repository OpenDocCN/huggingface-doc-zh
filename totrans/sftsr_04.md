# Torch 共享张量

> 原始文本：[`huggingface.co/docs/safetensors/torch_shared_tensors`](https://huggingface.co/docs/safetensors/torch_shared_tensors)

## TL;DR

使用特定函数，这应该在大多数情况下为您工作。这并非没有副作用。

```py
from safetensors.torch import load_model, save_model

save_model(model, "model.safetensors")
# Instead of save_file(model.state_dict(), "model.safetensors")

load_model(model, "model.safetensors")
# Instead of model.load_state_dict(load_file("model.safetensors"))
```

## 什么是共享张量？

Pytorch 在某些计算中使用共享张量。这对于通常减少内存使用非常有趣。

一个非常经典的用例是在 transformers 中，`embeddings`与`lm_head`共享。通过使用相同的矩阵，模型使用更少的参数，并且梯度更好地流向`embeddings`（这是模型的起始点，所以它们不容易流向那里，而`lm_head`在模型的末尾，所以梯度在那里非常好，因为它们是相同的张量，它们都受益）

```py
from torch import nn

class Model(nn.Module):
    def __init__(self):
        super().__init__()
        self.a = nn.Linear(100, 100)
        self.b = self.a

    def forward(self, x):
        return self.b(self.a(x))

model = Model()
print(model.state_dict())
# odict_keys(['a.weight', 'a.bias', 'b.weight', 'b.bias'])
torch.save(model.state_dict(), "model.bin")
# This file is now 41k instead of ~80k, because A and B are the same weight hence only 1 is saved on disk with both `a` and `b` pointing to the same buffer
```

## 为什么共享张量没有保存在 safetensors 中？

有多个原因：

+   *并非所有框架都支持*，例如`tensorflow`不支持。因此，如果有人在 torch 中保存共享张量，就没有办法以类似的方式加载它们，因此我们无法保持相同的`Dict[str, Tensor]` API。

+   *它使惰性加载非常快速*。惰性加载是仅加载给定文件的一些张量或张量部分的能力。这在没有共享张量的情况下是微不足道的，但是通过张量共享

    ```py
    with safe_open("model.safetensors", framework="pt") as f:
        a = f.get_tensor("a")
        b = f.get_tensor("b")
    ```

    现在使用这个给定的代码“重新共享”缓冲区是不可能的。一旦我们给出了`a`张量，当您要求`b`时，我们无法将相同的内存还给您。（在这个特定的示例中，我们可以跟踪给定的缓冲区，但这在一般情况下并非如此，因为您可以对`a`执行任意工作，比如在请求`b`之前将其发送到另一个设备）

+   *可能导致比必要更大的文件*。如果您保存的是一个只是较大张量的一部分的共享张量，那么使用 pytorch 保存它会导致保存整个缓冲区，而不仅仅保存所需的部分。

    ```py
    a = torch.zeros((100, 100))
    b = a[:1, :]
    torch.save({"b": b}, "model.bin")
    # File is 41k instead of the expected 400 bytes
    # In practice it could happen that you save several 10GB instead of 1GB.
    ```

现在提到所有这些原因，没有什么是一成不变的。共享张量不会导致不安全性或拒绝服务潜力，因此如果当前的解决方法不令人满意，这个决定可以重新审视。

## 它是如何工作的？

设计相当简单。我们将寻找所有共享张量，然后寻找覆盖整个缓冲区的所有张量（可能有多个这样的张量）。这给我们提供了可以保存的多个名称，我们只是选择第一个

在`load_model`期间，我们加载类似于`load_state_dict`的方式，除了我们查看模型本身，以检查共享缓冲区，并忽略“丢失的键”，这些键实际上是由于缓冲区共享而被覆盖的（它们已经正确加载，因为在幕后加载了一个缓冲区）。其他任何错误都会按原样引发

**注意**：这意味着我们在文件中删除了一些键。这意味着如果您正在检查磁盘上保存的键，您将看到一些“丢失的张量”，或者如果您正在使用`load_state_dict`。除非我们开始直接支持共享张量的格式，否则没有真正的解决方法。
