# configs

> 原文链接: [https://huggingface.co/docs/transformers.js/api/configs](https://huggingface.co/docs/transformers.js/api/configs)

用于使用模型配置的辅助模块。更多信息，请参阅相应的[Python文档](https://huggingface.co/docs/transformers/main/en/model_doc/auto#transformers.AutoConfig)。

**示例:** 加载一个`AutoConfig`。

```py
import { AutoConfig } from '@xenova/transformers';
let config = await AutoConfig.from_pretrained('bert-base-uncased');
console.log(config);
// PretrainedConfig {
//   "model_type": "bert",
//   "is_encoder_decoder": false,
//   "architectures": [
//       "BertForMaskedLM"
//   ],
//   "vocab_size": 30522
//   "num_attention_heads": 12,
//   "num_hidden_layers": 12,
//   "hidden_size": 768,
//   "max_position_embeddings": 512,
//   ...
// }
```

+   [configs](#module_configs)

    +   *静态*

        +   [.PretrainedConfig](#module_configs.PretrainedConfig)

            +   [`new PretrainedConfig(configJSON)`](#new_module_configs.PretrainedConfig_new)

            +   [`.from_pretrained(pretrained_model_name_or_path, options)`](#module_configs.PretrainedConfig.from_pretrained) ⇒ `Promise.<PretrainedConfig>`

        +   [.AutoConfig](#module_configs.AutoConfig)

            +   [`.from_pretrained()`](#module_configs.AutoConfig.from_pretrained) : `PretrainedConfig.from_pretrained`

    +   *内部*

        +   [`~loadConfig(pretrained_model_name_or_path, options)`](#module_configs..loadConfig) ⇒ `Promise.<Array>`

        +   [`~PretrainedOptions`](#module_configs..PretrainedOptions) : `*`

* * *

## configs.PretrainedConfig

所有配置类的基类。更多信息，请参阅相应的[Python文档](https://huggingface.co/docs/transformers/main/en/main_classes/configuration#transformers.PretrainedConfig)。

**种类**: [`configs`](#module_configs) 的静态类

+   [.PretrainedConfig](#module_configs.PretrainedConfig)

    +   [`new PretrainedConfig(configJSON)`](#new_module_configs.PretrainedConfig_new)

    +   [`.from_pretrained(pretrained_model_name_or_path, options)`](#module_configs.PretrainedConfig.from_pretrained) ⇒ `Promise.<PretrainedConfig>`

* * *

### new PretrainedConfig(configJSON)

创建一个新的 PreTrainedTokenizer 实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| configJSON | `Object` | 配置的 JSON 对象。 |

* * *

### PretrainedConfig.from_pretrained(pretrained_model_name_or_path, options) ⇒ <code> Promise. < PretrainedConfig > </code>

从给定的`pretrained_model_name_or_path`加载预训练配置。

**种类**: [`PretrainedConfig`](#module_configs.PretrainedConfig) 的静态方法

**返回**: `Promise.<PretrainedConfig>` - `PretrainedConfig` 类的新实例。

**抛出**:

+   如果在`pretrained_model_name_or_path`中找不到`config.json`，则抛出`Error`。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| pretrained_model_name_or_path | `string` | 预训练配置的路径。 |
| options | `PretrainedOptions` | 加载配置时的额外选项。 |

* * *

## configs.AutoConfig

用于使用`from_pretrained`函数实例化预训练配置的辅助类。

**种类**: [`configs`](#module_configs) 的静态类

* * *

### AutoConfig.from_pretrained() : <code> PretrainedConfig.from_pretrained </code>

**种类**: [`AutoConfig`](#module_configs.AutoConfig) 的静态方法

* * *

## configs~loadConfig(pretrained_model_name_or_path, options) ⇒ <code> Promise. < Array > </code>

从指定路径加载配置。

**种类**: [`configs`](#module_configs) 的内部方法

**返回**: `Promise.<Array>` - 一个解析为加载的配置信息的 Promise。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| pretrained_model_name_or_path | `string` | 配置目录的路径。 |
| options | `PretrainedOptions` | 加载配置时的额外选项。 |

* * *

## configs~PretrainedOptions : <code> * </code>

**种类**: [`configs`](#module_configs) 的内部类型

* * *
