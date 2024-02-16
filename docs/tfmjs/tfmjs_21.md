# processors

> 原始文本：[`huggingface.co/docs/transformers.js/api/processors`](https://huggingface.co/docs/transformers.js/api/processors)

Processors are used to prepare non-textual inputs (e.g., image or audio) for a model.

**Example:** 使用`WhisperProcessor`准备音频输入以供模型使用。

```py
import { AutoProcessor, read_audio } from '@xenova/transformers';

let processor = await AutoProcessor.from_pretrained('openai/whisper-tiny.en');
let audio = await read_audio('https://huggingface.co/datasets/Narsil/asr_dummy/resolve/main/mlk.flac', 16000);
let { input_features } = await processor(audio);
// Tensor {
//   data: Float32Array(240000) [0.4752984642982483, 0.5597258806228638, 0.56434166431427, ...],
//   dims: [1, 80, 3000],
//   type: 'float32',
//   size: 240000,
// }
```

+   processors

    +   *static*

        +   .FeatureExtractor ⇐ `Callable`

            +   `new FeatureExtractor(config)`

        +   .ImageFeatureExtractor ⇐ `FeatureExtractor`

            +   `new ImageFeatureExtractor(config)`

            +   [`.thumbnail(image, size, [resample])`](#module_processors.ImageFeatureExtractor+thumbnail) ⇒ `Promise.<RawImage>`

            +   `.crop_margin(image, gray_threshold)` ⇒ `Promise.<RawImage>`

            +   `.pad_image(pixelData, imgDims, padSize, options)` ⇒ `*`

            +   `.rescale(pixelData)` ⇒ `void`

            +   `.get_resize_output_image_size(image, size)` ⇒ `*`

            +   `.resize(image)` ⇒ `Promise.<RawImage>`

            +   `.preprocess(image, overrides)` ⇒ `Promise.<PreprocessedImage>`

            +   `._call(images, ...args)` ⇒ `Promise.<ImageFeatureExtractorResult>`

        +   .DetrFeatureExtractor ⇐ `ImageFeatureExtractor`

            +   `._call(images)` ⇒ `Promise.<DetrFeatureExtractorResult>`

            +   `.post_process_object_detection()` : `post_process_object_detection`

            +   `.remove_low_and_no_objects(class_logits, mask_logits, object_mask_threshold, num_labels)` ⇒ `*`

            +   `.check_segment_validity(mask_labels, mask_probs, k, mask_threshold, overlap_mask_area_threshold)` ⇒ `*`

            +   `.compute_segments(mask_probs, pred_scores, pred_labels, mask_threshold, overlap_mask_area_threshold, label_ids_to_fuse, target_size)` ⇒ `*`

            +   [`.post_process_panoptic_segmentation(outputs, [threshold], [mask_threshold], [overlap_mask_area_threshold], [label_ids_to_fuse], [target_sizes])`](#module_processors.DetrFeatureExtractor+post_process_panoptic_segmentation) ⇒ `Array.<{segmentation: Tensor, segments_info: Array<{id: number, label_id: number, score: number}>}>`

        +   .Processor ⇐ `Callable`

            +   `new Processor(feature_extractor)`

            +   `._call(input, ...args)` ⇒ `Promise.<any>`

        +   .WhisperProcessor ⇐ `Processor`

            +   `._call(audio)` ⇒ `Promise.<any>`

        +   .AutoProcessor

            +   `.from_pretrained(pretrained_model_name_or_path, options)` ⇒ `Promise.<Processor>`

    +   *inner*

        +   `~center_to_corners_format(arr)` ⇒ `Array.<number>`

        +   `~enforce_size_divisibility(size, divisor)` ⇒ `*`

        +   `~HeightWidth` : `*`

        +   `~ImageFeatureExtractorResult` : `object`

        +   `~PreprocessedImage` : `object`

        +   `~DetrFeatureExtractorResult` : `object`

        +   `~SamImageProcessorResult` : `object`

* * *

## processors.FeatureExtractor ⇐ <code> Callable </code>

特征提取器的基类。

**Kind**: 静态类 `processors`

**扩展**: `Callable`

* * *

### new FeatureExtractor(config)

构造一个新的 FeatureExtractor 实例。

| Param | Type | Description |
| --- | --- | --- |
| config | `Object` | 特征提取器的配置。 |

* * *

## processors.ImageFeatureExtractor ⇐ <code> FeatureExtractor </code>

用于图像模型的特征提取器。

**Kind**: 静态类 `processors`

**扩展**: `FeatureExtractor`

+   .ImageFeatureExtractor ⇐ `FeatureExtractor`

    +   `new ImageFeatureExtractor(config)`

    +   [`.thumbnail(image, size, [resample])`](#module_processors.ImageFeatureExtractor+thumbnail) ⇒ `Promise.<RawImage>`

    +   `.crop_margin(image, gray_threshold)` ⇒ `Promise.<RawImage>`

    +   `.pad_image(pixelData, imgDims, padSize, options)` ⇒ `*`

    +   `.rescale(pixelData)` ⇒ `void`

    +   `.get_resize_output_image_size(image, size)` ⇒ `*`

    +   `.resize(image)` ⇒ `Promise.<RawImage>`

    +   `.preprocess(image, overrides)` ⇒ `Promise.<PreprocessedImage>`

    +   `._call(images, ...args)` ⇒ `Promise.<ImageFeatureExtractorResult>`

* * *

### new ImageFeatureExtractor(config)

构造一个新的 ImageFeatureExtractor 实例。

| Param | Type | Description |
| --- | --- | --- |
| config | `Object` | 特征提取器的配置。 |
| config.image_mean | `Array.<number>` | 图像归一化的均值。 |
| config.image_std | `Array.<number>` | 图像归一化的标准差值。 |
| config.do_rescale | `boolean` | 是否将图像像素值重新缩放到[0,1]范围内。 |
| config.rescale_factor | `number` | 用于重新缩放图像像素值的因子。 |
| config.do_normalize | `boolean` | 是否对图像像素值进行归一化。 |
| config.do_resize | `boolean` | 是否调整图像大小。 |
| config.resample | `number` | 用于重采样的方法。 |
| config.size | `number` &#124; `Object` | 要调整图像大小的尺寸。 |

* * *

### imageFeatureExtractor.thumbnail(image, size, [resample]) ⇒ <code> Promise. < RawImage > </code>

将图像调整大小以制作缩略图。调整图像大小，使得没有任何维度大于指定尺寸的相应维度。

**Kind**: `ImageFeatureExtractor`的实例方法

**返回**: `Promise.<RawImage>` - 调整大小后的图像。

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| image | `RawImage` |  | 要调整大小的图像。 |
| size | `Object` |  | 调整图像大小的尺寸 `{"height": h, "width": w}`。 |
| [resample] | `string` &#124; `0` &#124; `1` &#124; `2` &#124; `3` &#124; `4` &#124; `5` | `2` | 要使用的重采样滤波器。 |

* * *

### imageFeatureExtractor.crop_margin(image, gray_threshold) ⇒ <code> Promise. < RawImage > </code>

裁剪图像的边缘。灰色像素被视为边缘（即像素值低于阈值）。

**Kind**: `ImageFeatureExtractor`的实例方法

**返回**: `Promise.<RawImage>` - 裁剪后的图像。

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| image | `RawImage` |  | 要裁剪的图像。 |
| gray_threshold | `number` | `200` | 被视为边缘的像素值下限。 |

* * *

### imageFeatureExtractor.pad_image(pixelData, imgDims, padSize, options) ⇒ <code> * </code>

通过一定量进行图像填充。

**Kind**: `ImageFeatureExtractor`的实例方法

**返回**：`*` - 填充后的像素数据和图像尺寸。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| pixelData | `Float32Array` |  | 要填充的像素数据。 |
| imgDims | `Array.<number>` |  | 图像的尺寸。 |
| padSize | `*` |  | 填充后图像的尺寸。 |
| 选项 | `Object` |  | 填充选项。 |
| [options.mode] | `'constant'` &#124; `'symmetric'` | `'constant'` | 要添加的填充类型。 |
| [options.center] | `boolean` | `false` | 是否居中图像。 |
| [options.constant_values] | `number` | `0` | 用于填充的常量值。 |

* * *

### imageFeatureExtractor.rescale(pixelData) ⇒ <code> void </code>

通过`this.rescale_factor`重新调整图像的像素值。

**类型**：`ImageFeatureExtractor`的实例方法

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| pixelData | `Float32Array` | 要调整大小的像素数据。 |

* * *

### imageFeatureExtractor.get_resize_output_image_size(image, size) ⇒ <code> * </code>

查找给定输入图像和所需尺寸的目标（宽度、高度）调整大小后的输出图像维度。

**类型**：`ImageFeatureExtractor`的实例方法

**返回**：`*` - 调整大小后输出图像的目标（宽度、高度）维度。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 图像 | `RawImage` | 要调整大小的图像。 |
| 尺寸 | `any` | 用于调整图像大小的尺寸。 |

* * *

### imageFeatureExtractor.resize(image) ⇒ <code> Promise. < RawImage > </code>

调整图像大小。

**类型**：`ImageFeatureExtractor`的实例方法

**返回**：`Promise.<RawImage>` - 调整大小后的图像。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 图像 | `RawImage` | 要调整大小的图像。 |

* * *

### imageFeatureExtractor.preprocess(image, overrides) ⇒ <code> Promise. < PreprocessedImage > </code>

预处理给定的图像。

**类型**：`ImageFeatureExtractor`的实例方法

**返回**：`Promise.<PreprocessedImage>` - 预处理后的图像。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 图像 | `RawImage` | 要预处理的图像。 |
| 覆盖 | `Object` | 预处理选项的覆盖。 |

* * *

### imageFeatureExtractor._call(images, ...args) ⇒ <code> Promise. < ImageFeatureExtractorResult > </code>

对图像数组调用特征提取过程，预处理每个图像，并将生成的特征连接成单个张量。

**类型**：`ImageFeatureExtractor`的实例方法

**返回**：`Promise.<ImageFeatureExtractorResult>` - 包含预处理图像的连接像素值（和其他元数据）的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| images | `Array.<RawImage>` | 要从中提取特征的图像。 |
| ...args | `any` | 附加参数。 |

* * *

## processors.DetrFeatureExtractor ⇐ <code> ImageFeatureExtractor </code>

Detr 特征提取器。

**类型**：`processors`的静态类

**扩展自**：`ImageFeatureExtractor`

+   .DetrFeatureExtractor ⇐ `ImageFeatureExtractor`

    +   `._call(images)` ⇒ `Promise.<DetrFeatureExtractorResult>`

    +   `.post_process_object_detection()` : `post_process_object_detection`

    +   `.remove_low_and_no_objects(class_logits, mask_logits, object_mask_threshold, num_labels)` ⇒ `*`

    +   `.check_segment_validity(mask_labels, mask_probs, k, mask_threshold, overlap_mask_area_threshold)` ⇒ `*`

    +   `.compute_segments(mask_probs, pred_scores, pred_labels, mask_threshold, overlap_mask_area_threshold, label_ids_to_fuse, target_size)` ⇒ `*`

    +   [`.post_process_panoptic_segmentation(outputs, [threshold], [mask_threshold], [overlap_mask_area_threshold], [label_ids_to_fuse], [target_sizes])`](#module_processors.DetrFeatureExtractor+post_process_panoptic_segmentation) ⇒ `Array.<{segmentation: Tensor, segments_info: Array<{id: number, label_id: number, score: number}>}>`

* * *

### detrFeatureExtractor._call(images) ⇒ <code> Promise. < DetrFeatureExtractorResult > </code>

对图像数组调用特征提取过程，预处理每个图像，并将生成的特征连接成单个张量。

**类型**：`DetrFeatureExtractor`的实例方法

**返回**：`Promise.<DetrFeatureExtractorResult>` - 包含预处理图像的像素值的对象。

| Param | Type | Description |
| --- | --- | --- |
| images | `Array.<RawImage>` | 从中提取特征的图像。 |

* * *

### detrFeatureExtractor.post_process_object_detection() : <code> post_process_object_detection </code>

**类型**：`DetrFeatureExtractor`的实例方法

* * *

### detrFeatureExtractor.remove_low_and_no_objects(class_logits, mask_logits, object_mask_threshold, num_labels) ⇒ <code> * </code>

使用`object_mask_threshold`对给定的掩模进行二值化，返回`masks`、`scores`和`labels`的相关值。

**类型**：`DetrFeatureExtractor`的实例方法

**返回**：`*` - 二值化的掩模、分数和标签。

| Param | Type | Description |
| --- | --- | --- |
| class_logits | `Tensor` | 类别对数。 |
| mask_logits | `Tensor` | 掩模对数。 |
| object_mask_threshold | `number` | 用于二值化掩模的介于 0 和 1 之间的数字。 |
| num_labels | `number` | 标签数量。 |

* * *

### detrFeatureExtractor.check_segment_validity(mask_labels, mask_probs, k, mask_threshold, overlap_mask_area_threshold) ⇒ <code> * </code>

检查分段是否有效。

**类型**：`DetrFeatureExtractor`的实例方法

**返回**：`*` - 分段是否有效以及有效标签的索引。

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| mask_labels | `Int32Array` |  | 掩模中每个像素的标签。 |
| mask_probs | `Array.<Tensor>` |  | 掩模中每个像素的概率。 |
| k | `number` |  | 分段的类别 ID。 |
| mask_threshold | `number` | `0.5` | 掩模阈值。 |
| overlap_mask_area_threshold | `number` | `0.8` | 重叠掩模区域阈值。 |

* * *

### detrFeatureExtractor.compute_segments(mask_probs, pred_scores, pred_labels, mask_threshold, overlap_mask_area_threshold, label_ids_to_fuse, target_size) ⇒ <code> * </code>

计算分段。

**类型**：`DetrFeatureExtractor`的实例方法

**返回**：`*` - 计算得到的分段。

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| mask_probs | `Array.<Tensor>` |  | 掩模概率。 |
| pred_scores | `Array.<number>` |  | 预测的分数。 |
| pred_labels | `Array.<number>` |  | 预测的标签。 |
| mask_threshold | `number` |  | 掩模阈值。 |
| overlap_mask_area_threshold | `number` |  | 重叠掩模区域阈值。 |
| label_ids_to_fuse | `Set.<number>` |  | 要融合的标签 ID。 |
| target_size | `Array.<number>` |  | 图像的目标尺寸。 |

* * *

### detrFeatureExtractor.post_process_panoptic_segmentation(outputs, [threshold], [mask_threshold], [overlap_mask_area_threshold], [label_ids_to_fuse], [target_sizes]) ⇒ <code> Array. < {segmentation: Tensor, segments_info: Array < {id: number, label_id: number, score: number} > } > </code>

后处理模型输出以生成最终的全景分割。

**种类**: `DetrFeatureExtractor`的实例方法

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| 输出 | `*` |  | 要进行后处理的模型输出 |
| [threshold] | `number` | `0.5` | 保留预测实例掩码的概率分数阈值。 |
| [mask_threshold] | `number` | `0.5` | 在将预测的掩码转换为二进制值时使用的阈值。 |
| [overlap_mask_area_threshold] | `number` | `0.8` | 合并或丢弃每个二进制实例掩码中的小断开部分的重叠掩码区域阈值。 |
| [label_ids_to_fuse] | `Set.<number>` |  | 此状态中的标签将使其所有实例被融合在一起。 |
| [target_sizes] | `Array.<Array<number>>` |  | 调整掩码大小的目标尺寸。 |

* * *

## 处理器。Processor ⇐ <code> Callable </code>

表示从输入中提取特征的处理器。

**种类**: `processors`的静态类

**扩展**: `Callable`

+   .Processor ⇐ `Callable`

    +   `new Processor(feature_extractor)`

    +   `._call(input, ...args)` ⇒ `Promise.<any>`

* * *

### 新处理器(feature_extractor)

使用给定的特征提取器创建一个新的处理器。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| feature_extractor | `FeatureExtractor` | 用于从输入中提取特征的函数。 |

* * *

### processor._call(input, ...args) ⇒ <code> Promise. < any > </code>

调用 feature_extractor 函数并提供输入。

**种类**: `Processor`的实例方法

**返回**: `Promise.<any>` - 一个解析为提取特征的 Promise。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 输入 | `any` | 要从中提取特征的输入。 |
| ...args | `any` | 附加参数。 |

* * *

## 处理器。WhisperProcessor ⇐ <code>处理器</code>

表示从音频输入提取特征的 WhisperProcessor。

**种类**: `processors`的静态类

**扩展**: `Processor`

* * *

### whisperProcessor._call(audio) ⇒ <code> Promise. < any > </code>

调用 feature_extractor 函数并提供音频输入。

**种类**: `WhisperProcessor`的实例方法

**返回**: `Promise.<any>` - 一个解析为提取特征的 Promise。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 音频 | `any` | 从中提取特征的音频输入。 |

* * *

## processors.AutoProcessor

用于使用`from_pretrained`函数实例化预训练处理器的辅助类。所选的处理器类由处理器配置中指定的类型确定。

**示例:** 使用`from_pretrained`加载处理器。

```py
let processor = await AutoProcessor.from_pretrained('openai/whisper-tiny.en');
```

**示例:** 运行图像通过处理器。

```py
let processor = await AutoProcessor.from_pretrained('Xenova/clip-vit-base-patch16');
let image = await RawImage.read('https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/football-match.jpg');
let image_inputs = await processor(image);
// {
//   "pixel_values": {
//     "dims": [ 1, 3, 224, 224 ],
//     "type": "float32",
//     "data": Float32Array [ -1.558687686920166, -1.558687686920166, -1.5440893173217773, ... ],
//     "size": 150528
//   },
//   "original_sizes": [
//     [ 533, 800 ]
//   ],
//   "reshaped_input_sizes": [
//     [ 224, 224 ]
//   ]
// }
```

**种类**: `processors`的静态类

* * *

### AutoProcessor.from_pretrained(pretrained_model_name_or_path, options) ⇒ <code> Promise. < Processor > </code>

从预训练模型实例化库中的处理器类之一。

要实例化的处理器类是根据配置对象的`feature_extractor_type`属性选择的（如果可能，可以作为参数传递或从`pretrained_model_name_or_path`加载）

**种类**: `AutoProcessor`的静态方法

**返回**: `Promise.<处理器>` - 处理器类的一个新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |

| pretrained_model_name_or_path | `string` | 预训练模型的名称或路径。可以是：

+   一个字符串，预训练处理器的*模型 ID*，托管在 huggingface.co 上的模型存储库中。有效的模型 ID 可以位于根级别，如`bert-base-uncased`，或者在用户或组织名称下命名空间，如`dbmdz/bert-base-german-cased`。

+   一个包含处理器文件的*目录*的路径，例如`./my_model_directory/`。

|

| options | `*` | 加载处理器的附加选项。 |
| --- | --- | --- |

* * *

## 处理器~center_to_corners_format(arr) ⇒ `<code> Array. < number > </code>`

将边界框从中心格式转换为角格式。

**种类**：`processors`的内部方法

**返回**：`Array.<number>` - 盒子的左上角和右下角的坐标（top_left_x，top_left_y，bottom_right_x，bottom_right_y）

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| arr | `Array.<number>` | 盒子中心及其宽度、高度尺寸的坐标（center_x，center_y，width，height） |

* * *

## 处理器~enforce_size_divisibility(size, divisor) ⇒ `<code> * </code>`

将高度和宽度四舍五入到最接近的 size_divisibility 的倍数

**种类**：`processors`的内部方法

**返回**：`*` - 四舍五入的大小。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| size | `*` | 图像的大小 |
| divisor | `number` | 要使用的除数。 |

* * *

## 处理器~HeightWidth：`<code> * </code>`

命名元组指示我们使用的顺序是（高度 x 宽度），即使图形行业标准是（宽度 x 高度）。

**种类**：`processors`的内部 typedef

* * *

## 处理器~ImageFeatureExtractorResult：`<code> object </code>`

**种类**：`processors`的内部 typedef

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| pixel_values | `Tensor` | 批处理预处理图像的像素值。 |
| original_sizes | `Array.<HeightWidth>` | 类似[[480, 640]]的二维元组数组。 |
| reshaped_input_sizes | `Array.<HeightWidth>` | 类似[[1000, 1330]]的二维元组数组。 |

* * *

## 处理器~PreprocessedImage：`<code> object </code>`

**种类**：`processors`的内部 typedef

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| original_size | `HeightWidth` | 图像的原始大小。 |
| reshaped_input_size | `HeightWidth` | 图像的重新调整输入大小。 |
| pixel_values | `Tensor` | 预处理图像的像素值。 |

* * *

## 处理器~DetrFeatureExtractorResult：`<code> object </code>`

**种类**：`processors`的内部 typedef

**属性**

| 名称 | 类型 |
| --- | --- |
| pixel_mask | `Tensor` |

* * *

## 处理器~SamImageProcessorResult：`<code> object </code>`

**种类**：`processors`的内部 typedef

**属性**

| 名称 | 类型 |
| --- | --- |
| pixel_values | `Tensor` |
| original_sizes | `Array.<HeightWidth>` |
| reshaped_input_sizes | `Array.<HeightWidth>` |
| [input_points] | `Tensor` |
| [input_labels] | `Tensor` |

* * *
