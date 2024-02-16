# 音频数据的流式加载

> 原文：[`huggingface.co/learn/audio-course/zh-CN/chapter1/streaming`](https://huggingface.co/learn/audio-course/zh-CN/chapter1/streaming)

            

使用音频数据集的最大挑战之一是它们庞大的规模。仅仅一分钟的 CD 音质音频（44.1 千赫兹，16 比特）就需要占用约 5MB 的存储。一般的音频数据集会含有数个小时的音频录音。

在上一小节中，我们使用了 MINDS-14 的一个非常小的子集，但常见的音频数据集往往要大得多。举个例子[GigaSpeech from SpeechColab](https://huggingface.co/datasets/speechcolab/gigaspeech)的`xs`（最小）子集仅有 10 小时的训练数据，但需要占用超过 13GB 的存储空间。当我们选择更大的子集时会发生什么呢？其最大的`xl`子集含有 10000 小时的训练数据，需要占用超过 1TB 的存储空间。对大多数人来说，这已经远远超过了我们的硬盘所能容纳的容量。我们需要剁手购买额外的硬盘吗？还是说有其他方法可以让我们绕过存储空间的限制呢？

🤗 Datasets 为你提供了解决方法：流式处理模式。流式处理可以让我们在遍历数据集的过程中渐进地加载数据。与其一次性地下载完整数据集，不如将样本一个个单独加载。在遍历数据集时，我们实时地在有需要时加载和准备好每个样本。通过这样的方式，我们永远只加载我们正在使用的数据，而不加载我们暂时不需要的数据。当我们使用完一个样本时，就继续遍历数据集，加载下一个样本。

相较于全量加载，流式处理模式主要有三个优点：

*   硬盘空间：样本是在遍历数据集的过程中被依次加载到内存中的。由于我们不需要将数据下载到本地，因此也没有任何对硬盘空间的要求，你可以使用任意大小的数据集。
*   下载和处理时间：音频数据集极其庞大并且需要大量的数据下载和处理时间。在流式处理模式下，数据会进行实时的加载和处理，因此当第一个样本准备完成时我们就可以开始训练。
*   测试简单：无需下载完整的数据集，仅用少量的样本就可以测试你的训练脚本。

在使用流式处理模式时，有一点需要警惕：在全量下载数据集时，所有的原始数据和处理后的数据都会在硬盘内本地存储。如果我们想要重复利用这些数据，我们可以跳过下载和处理过程，直接从硬盘中加载数据。这意味着我们在重复利用数据时，仅在第一次时需要下载和处理数据。

但在流式处理模式中，数据不会被下载到硬盘内。因此，我们的下载和预处理过程都不会被保存。在重复利用数据集时，我们必须要重复进行下载和预处理才做。因此，如果你计划重复使用数据集，我们还是推荐将数据集下载到本地硬盘。

如何启用流式处理模式呢？很简单！只需要在加载数据集时传入`streaming=True`参数即可。我们会帮你做好其他工作：

```py
gigaspeech = load_dataset("speechcolab/gigaspeech", "xs", streaming=True)
```

与我们在全量下载 MINDS-14 子集时进行的预处理操作相同，你也可以在流式数据集上使用同样的预处理操作。

唯一的区别是我们现在无法使用 Python 的索引功能访问单个的样本（例如`gigaspeech["train"][sample_idx]`）。相反地，我们需要遍历整个数据集。你可以在流式数据集这这样访问单个样本：

```py
next(iter(gigaspeech["train"]))
```

**输出：**

```py
{
    "segment_id": "YOU0000000315_S0000660",
    "speaker": "N/A",
    "text": "AS THEY'RE LEAVING <COMMA> CAN KASH PULL ZAHRA ASIDE REALLY QUICKLY <QUESTIONMARK>",
    "audio": {
        "path": "xs_chunks_0000/YOU0000000315_S0000660.wav",
        "array": array(
            [0.0005188, 0.00085449, 0.00012207, ..., 0.00125122, 0.00076294, 0.00036621]
        ),
        "sampling_rate": 16000,
    },
    "begin_time": 2941.89,
    "end_time": 2945.07,
    "audio_id": "YOU0000000315",
    "title": "Return to Vasselheim | Critical Role: VOX MACHINA | Episode 43",
    "url": "https://www.youtube.com/watch?v=zr2n1fLVasU",
    "source": 2,
    "category": 24,
    "original_full_path": "audio/youtube/P0004/YOU0000000315.opus",
}
```

你可以使用`take()`函数来预览数据集的几个样本。让我们看看 gigaspeech 数据集的前两个样本：

```py
gigaspeech_head = gigaspeech["train"].take(2)
list(gigaspeech_head)
```

**输出：**

```py
[
    {
        "segment_id": "YOU0000000315_S0000660",
        "speaker": "N/A",
        "text": "AS THEY'RE LEAVING <COMMA> CAN KASH PULL ZAHRA ASIDE REALLY QUICKLY <QUESTIONMARK>",
        "audio": {
            "path": "xs_chunks_0000/YOU0000000315_S0000660.wav",
            "array": array(
                [
                    0.0005188,
                    0.00085449,
                    0.00012207,
                    ...,
                    0.00125122,
                    0.00076294,
                    0.00036621,
                ]
            ),
            "sampling_rate": 16000,
        },
        "begin_time": 2941.89,
        "end_time": 2945.07,
        "audio_id": "YOU0000000315",
        "title": "Return to Vasselheim | Critical Role: VOX MACHINA | Episode 43",
        "url": "https://www.youtube.com/watch?v=zr2n1fLVasU",
        "source": 2,
        "category": 24,
        "original_full_path": "audio/youtube/P0004/YOU0000000315.opus",
    },
    {
        "segment_id": "AUD0000001043_S0000775",
        "speaker": "N/A",
        "text": "SIX TOMATOES <PERIOD>",
        "audio": {
            "path": "xs_chunks_0000/AUD0000001043_S0000775.wav",
            "array": array(
                [
                    1.43432617e-03,
                    1.37329102e-03,
                    1.31225586e-03,
                    ...,
                    -6.10351562e-05,
                    -1.22070312e-04,
                    -1.83105469e-04,
                ]
            ),
            "sampling_rate": 16000,
        },
        "begin_time": 3673.96,
        "end_time": 3675.26,
        "audio_id": "AUD0000001043",
        "title": "Asteroid of Fear",
        "url": "http//www.archive.org/download/asteroid_of_fear_1012_librivox/asteroid_of_fear_1012_librivox_64kb_mp3.zip",
        "source": 0,
        "category": 28,
        "original_full_path": "audio/audiobook/P0011/AUD0000001043.opus",
    },
]
```

流式处理模式可以助力你的研究进入更高的阶段：现在你不仅可以利用庞大的数据集，而且可以在无需担忧硬盘空间的情况下使用多个数据集来评估你的系统。与使用单个数据集进行评估相比，多数据集评估可以更好地体现系统的泛化性能。