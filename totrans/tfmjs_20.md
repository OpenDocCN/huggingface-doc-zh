# tokenizers

> 原始文本: [`huggingface.co/docs/transformers.js/api/tokenizers`](https://huggingface.co/docs/transformers.js/api/tokenizers)

分词器用于为模型准备文本输入。

**示例:** 创建一个`AutoTokenizer`并使用它对句子进行分词。这将根据`tokenizer.json`中定义的分词器类自动检测分词器类型。

```py
import { AutoTokenizer } from '@xenova/transformers';

const tokenizer = await AutoTokenizer.from_pretrained('Xenova/bert-base-uncased');
const { input_ids } = await tokenizer('I love transformers!');
// Tensor {
//   data: BigInt64Array(6) [101n, 1045n, 2293n, 19081n, 999n, 102n],
//   dims: [1, 6],
//   type: 'int64',
//   size: 6,
// }
```

+   tokenizers

    +   *static*

        +   .TokenizerModel ⇐ `Callable`

            +   `new TokenizerModel(config)`

            +   *instance*

                +   `.vocab` : `Array.<string>`

                +   `.tokens_to_ids` : `Map.<string, number>`

                +   `.fuse_unk` : `boolean`

                +   `._call(tokens)` ⇒ `Array.<string>`

                +   `.encode(tokens)` ⇒ `Array.<string>`

                +   `.convert_tokens_to_ids(tokens)` ⇒ `Array.<number>`

                +   `.convert_ids_to_tokens(ids)` ⇒ `Array.<string>`

            +   *static*

                +   `.fromConfig(config, ...args)` ⇒ `TokenizerModel`

        +   .PreTrainedTokenizer

            +   `new PreTrainedTokenizer(tokenizerJSON, tokenizerConfig)`

            +   *instance*

                +   `.added_tokens` : `Array.<AddedToken>`

                +   `.remove_space` : `boolean`

                +   `.padding_side` : `’right’` | `’left’`

                +   `.getToken(...keys)` ⇒ `string` | `null`

                +   `._call(text, options)` ⇒ `BatchEncoding`

                +   `._encode_text(text)` ⇒ `Array<string>` | `null`

                +   `.encode(text, text_pair, options)` ⇒ `Array.<number>`

                +   `.batch_decode(batch, decode_args)` ⇒ `Array.<string>`

                +   [`.decode(token_ids, [decode_args])`](#module_tokenizers.PreTrainedTokenizer+decode) ⇒ `string`

                +   `.decode_single(token_ids, decode_args)` ⇒ `string`

                +   `.apply_chat_template(conversation, options)` ⇒ `string` | `Tensor` | `Array<number>` | `Array<Array<number>>`

            +   *static*

                +   `.from_pretrained(pretrained_model_name_or_path, options)` ⇒ `Promise.<PreTrainedTokenizer>`

        +   .BertTokenizer ⇐ `PreTrainedTokenizer`

        +   .AlbertTokenizer ⇐ `PreTrainedTokenizer`

        +   .NllbTokenizer

            +   `._build_translation_inputs(raw_inputs, tokenizer_options, generate_kwargs)` ⇒ `Object`

        +   .M2M100Tokenizer

            +   `._build_translation_inputs(raw_inputs, tokenizer_options, generate_kwargs)` ⇒ `Object`

        +   .WhisperTokenizer ⇐ `PreTrainedTokenizer`

            +   `._decode_asr(sequences, options)` ⇒ `*`

            +   `.decode()` : `*`

            +   `.get_decoder_prompt_ids(options)` ⇒ `Array.<Array<number>>`

        +   .MarianTokenizer

            +   `new MarianTokenizer(tokenizerJSON, tokenizerConfig)`

            +   `._encode_text(text)` ⇒ `Array`

        +   .AutoTokenizer

            +   `.from_pretrained(pretrained_model_name_or_path, options)` ⇒ `Promise.<PreTrainedTokenizer>`

    +   *inner*

        +   ~AddedToken

            +   `new AddedToken(config)`

        +   ~WordPieceTokenizer ⇐ `TokenizerModel`

            +   `new WordPieceTokenizer(config)`

            +   `.tokens_to_ids` : `Map.<string, number>`

            +   `.unk_token_id` : `number`

            +   `.unk_token` : `string`

            +   `.max_input_chars_per_word` : `number`

            +   `.vocab` : `Array.<string>`

            +   `.encode(tokens)` ⇒ `Array.<string>`

        +   ~Unigram ⇐ `TokenizerModel`

            +   `new Unigram(config, moreConfig)`

            +   `.populateNodes(lattice)`

            +   `.tokenize(normalized)` ⇒ `Array.<string>`

            +   `.encode(tokens)` ⇒ `Array.<string>`

        +   ~BPE ⇐ `TokenizerModel`

            +   `new BPE(config)`

            +   `.tokens_to_ids` : `Map.<string, number>`

            +   `.cache` : `Map.<string, Array<string>>`

            +   `.bpe(token)` ⇒ `Array.<string>`

            +   `.encode(tokens)` ⇒ `Array.<string>`

        +   ~LegacyTokenizerModel

            +   `new LegacyTokenizerModel(config, moreConfig)`

            +   `.tokens_to_ids` : `Map.<string, number>`

        +   *~Normalizer*

            +   *`new Normalizer(config)`*

            +   *instance*

                +   **`.normalize(text)` ⇒ `string`**

                +   *`._call(text)` ⇒ `string`*

            +   *static*

                +   *`.fromConfig(config)` ⇒ `Normalizer`*

        +   ~Replace ⇐ `Normalizer`

            +   `.normalize(text)` ⇒ `string`

        +   ~NFC ⇐ `Normalizer`

            +   `.normalize(text)` ⇒ `string`

        +   ~NFKC ⇐ `Normalizer`

            +   `.normalize(text)` ⇒ `string`

        +   ~NFKD ⇐ `Normalizer`

            +   `.normalize(text)` ⇒ `string`

        +   ~StripNormalizer

            +   `.normalize(text)` ⇒ `string`

        +   ~StripAccents ⇐ `Normalizer`

            +   `.normalize(text)` ⇒ `string`

        +   ~Lowercase ⇐ `Normalizer`

            +   `.normalize(text)` ⇒ `string`

        +   ~Prepend ⇐ `Normalizer`

            +   `.normalize(text)` ⇒ `string`

        +   ~NormalizerSequence ⇐ `Normalizer`

            +   `new NormalizerSequence(config)`

            +   `.normalize(text)` ⇒ `string`

        +   ~BertNormalizer ⇐ `Normalizer`

            +   `._tokenize_chinese_chars(text)` ⇒ `string`

            +   `._is_chinese_char(cp)` ⇒ `boolean`

            +   `.stripAccents(text)` ⇒ `string`

            +   `.normalize(text)` ⇒ `string`

        +   ~PreTokenizer ⇐ `Callable`

            +   *instance*

                +   *[`.pre_tokenize_text(text, [options])`](#module_tokenizers..PreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`*

                +   [`.pre_tokenize(text, [options])`](#module_tokenizers..PreTokenizer+pre_tokenize) ⇒ `Array.<string>`

                +   [`._call(text, [options])`](#module_tokenizers..PreTokenizer+_call) ⇒ `Array.<string>`

            +   *static*

                +   `.fromConfig(config)` ⇒ `PreTokenizer`

        +   ~BertPreTokenizer ⇐ `PreTokenizer`

            +   `new BertPreTokenizer(config)`

            +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..BertPreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

        +   ~ByteLevelPreTokenizer ⇐ `PreTokenizer`

            +   `new ByteLevelPreTokenizer(config)`

            +   `.add_prefix_space` : `boolean`

            +   `.trim_offsets` : `boolean`

            +   `.use_regex` : `boolean`

            +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..ByteLevelPreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

        +   ~SplitPreTokenizer ⇐ `PreTokenizer`

            +   `new SplitPreTokenizer(config)`

            +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..SplitPreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

        +   ~PunctuationPreTokenizer ⇐ `PreTokenizer`

            +   `new PunctuationPreTokenizer(config)`

            +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..PunctuationPreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

        +   ~DigitsPreTokenizer ⇐ `PreTokenizer`

            +   `new DigitsPreTokenizer(config)`

            +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..DigitsPreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

        +   ~PostProcessor ⇐ `Callable`

            +   `new PostProcessor(config)`

            +   *instance*

                +   `.post_process(tokens, ...args)` ⇒ `PostProcessedOutput`

                +   `._call(tokens, ...args)` ⇒ `PostProcessedOutput`

            +   *static*

                +   `.fromConfig(config)` ⇒ `PostProcessor`

        +   ~BertProcessing

            +   `new BertProcessing(config)`

            +   [`.post_process(tokens, [tokens_pair])`](#module_tokenizers..BertProcessing+post_process) ⇒ `PostProcessedOutput`

        +   ~TemplateProcessing ⇐ `PostProcessor`

            +   `new TemplateProcessing(config)`

            +   [`.post_process(tokens, [tokens_pair])`](#module_tokenizers..TemplateProcessing+post_process) ⇒ `PostProcessedOutput`

        +   ~ByteLevelPostProcessor ⇐ `PostProcessor`

            +   [`.post_process(tokens, [tokens_pair])`](#module_tokenizers..ByteLevelPostProcessor+post_process) ⇒ `PostProcessedOutput`

        +   ~Decoder ⇐ `Callable`

            +   `new Decoder(config)`

            +   *instance*

                +   `.added_tokens` : `Array.<AddedToken>`

                +   `._call(tokens)` ⇒ `string`

                +   `.decode(tokens)` ⇒ `string`

                +   `.decode_chain(tokens)` ⇒ `Array.<string>`

            +   *static*

                +   `.fromConfig(config)` ⇒ `Decoder`

        +   ~FuseDecoder

            +   `.decode_chain()` : `*`

        +   ~WordPieceDecoder ⇐ `Decoder`

            +   `new WordPieceDecoder(config)`

            +   `.decode_chain()` : `*`

        +   ~ByteLevelDecoder ⇐ `Decoder`

            +   `new ByteLevelDecoder(config)`

            +   `.convert_tokens_to_string(tokens)` ⇒ `string`

            +   `.decode_chain()` : `*`

        +   ~CTCDecoder

            +   `.convert_tokens_to_string(tokens)` ⇒ `string`

            +   `.decode_chain()` : `*`

        +   ~DecoderSequence ⇐ `Decoder`

            +   `new DecoderSequence(config)`

            +   `.decode_chain()` : `*`

        +   ~MetaspacePreTokenizer ⇐ `PreTokenizer`

            +   `new MetaspacePreTokenizer(config)` : `Object`

            +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..MetaspacePreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

        +   ~MetaspaceDecoder ⇐ `Decoder`

            +   `new MetaspaceDecoder(config)`

            +   `.decode_chain()` : `*`

        +   ~Precompiled ⇐ `Normalizer`

            +   `new Precompiled(config)`

            +   `.normalize(text)` ⇒ `string`

        +   ~PreTokenizerSequence ⇐ `PreTokenizer`

            +   `new PreTokenizerSequence(config)`

            +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..PreTokenizerSequence+pre_tokenize_text) ⇒ `Array.<string>`

        +   ~WhitespacePreTokenizer

            +   `new WhitespacePreTokenizer(config)`

            +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..WhitespacePreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

        +   ~WhitespaceSplit ⇐ `PreTokenizer`

            +   `new WhitespaceSplit(config)`

            +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..WhitespaceSplit+pre_tokenize_text) ⇒ `Array.<string>`

        +   ~ReplacePreTokenizer

            +   `new ReplacePreTokenizer(config)`

            +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..ReplacePreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

        +   `~BYTES_TO_UNICODE` ⇒ `Object`

        +   `~loadTokenizer(pretrained_model_name_or_path, options)` ⇒ `Promise.<Array<any>>`

        +   `~regexSplit(text, regex)` ⇒ `Array.<string>`

        +   `~createPattern(pattern, invert)` ⇒ `RegExp` | `null`

        +   `~objectToMap(obj)` ⇒ `Map.<string, any>`

        +   `~prepareTensorForDecode(tensor)` ⇒ `Array.<number>`

        +   `~clean_up_tokenization(text)` ⇒ `string`

        +   `~remove_accents(text)` ⇒ `string`

        +   `~lowercase_and_remove_accent(text)` ⇒ `string`

        +   `~fuse(arr, value, mapping)`

        +   `~whitespace_split(text)` ⇒ `Array.<string>`

        +   `~PretrainedTokenizerOptions` : `Object`

        +   `~BPENode` : `Object`

        +   `~SplitDelimiterBehavior`：`’removed’` | `’isolated’` | `’mergedWithPrevious’` | `’mergedWithNext’` | `’contiguous’`

        +   `~PostProcessedOutput`：`Object`

        +   `~EncodingSingle`：`Object`

        +   `~BatchEncoding`：`Array<number>` | `Array<Array<number>>` | `Tensor`

        +   `~Message`：`Object`

* * *

## tokenizers.TokenizerModel ⇐ <code> Callable </code>

tokenizer models 的抽象基类。

**类型**：`tokenizers`的静态类

**扩展**：`Callable`

+   .TokenizerModel ⇐ `Callable`

    +   `new TokenizerModel(config)`

    +   *实例*

        +   `.vocab`：`Array.<string>`

        +   `.tokens_to_ids`：`Map.<string, number>`

        +   `.fuse_unk`：`boolean`

        +   `._call(tokens)` ⇒ `Array.<string>`

        +   `.encode(tokens)` ⇒ `Array.<string>`

        +   `.convert_tokens_to_ids(tokens)` ⇒ `Array.<number>`

        +   `.convert_ids_to_tokens(ids)` ⇒ `Array.<string>`

    +   *静态*

        +   `.fromConfig(config, ...args)` ⇒ `TokenizerModel`

* * *

### new TokenizerModel(config)

创建一个新的 TokenizerModel 实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | TokenizerModel 的配置对象。 |

* * *

### tokenizerModel.vocab：<code> Array. < string > </code>

**类型**：`TokenizerModel`的实例属性

* * *

### tokenizerModel.tokens_to_ids：<code> Map. < string, number > </code>

将 tokens 映射到 ids。

**类型**：`TokenizerModel`的实例属性

* * *

### tokenizerModel.fuse_unk：<code> boolean </code>

在编码时是否融合未知 tokens。默认为 false。

**类型**：`TokenizerModel`的实例属性

* * *

### tokenizerModel._call(tokens) ⇒ <code> Array. < string > </code>

调用 TokenizerModel 实例的内部函数。

**类型**：`TokenizerModel`的实例方法

**返回**: `Array.<string>` - 编码后的 token IDs。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array.<string>` | 要编码的 tokens。 |

* * *

### tokenizerModel.encode(tokens) ⇒ <code> Array. < string > </code>

将一组 tokens 编码为一组 token IDs。

**类型**：`TokenizerModel`的实例方法

**返回**：`Array.<string>` - 编码后的 tokens。

**抛出**：

+   如果在子类中未实现，将会抛出错误。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array.<string>` | 要编码的 tokens。 |

* * *

### tokenizerModel.convert_tokens_to_ids(tokens) ⇒ <code> Array. < number > </code>

将一组 tokens 转换为一组 token IDs。

**类型**：`TokenizerModel`的实例方法

**返回**：`Array.<number>` - 转换后的 token IDs。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array.<string>` | 要转换的 tokens。 |

* * *

### tokenizerModel.convert_ids_to_tokens(ids) ⇒ <code> Array. < string > </code>

将一组 token IDs 转换为一组 tokens。

**类型**：`TokenizerModel`的实例方法

**返回**：`Array.<string>` - 转换后的 tokens。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| ids | `Array.<number>` | 要转换的 token IDs。 |

* * *

### TokenizerModel.fromConfig(config, ...args) ⇒ <code> TokenizerModel </code>

根据提供的配置对象实例化一个新的 TokenizerModel 实例。

**类型**：`TokenizerModel`的静态方法

**返回**：`TokenizerModel` - 一个 TokenizerModel 的新实例。

**抛出**：

+   如果配置中的 TokenizerModel 类型未被识别，则会抛出错误。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | TokenizerModel 的配置对象。 |
| ...args | `*` | 传递给特定 TokenizerModel 构造函数的可选参数。 |

* * *

## tokenizers.PreTrainedTokenizer

**类型**：`tokenizers`的静态类

+   .PreTrainedTokenizer

    +   `new PreTrainedTokenizer(tokenizerJSON, tokenizerConfig)`

    +   *实例*

        +   `.added_tokens`：`Array.<AddedToken>`

        +   `.remove_space`：`boolean`

        +   `.padding_side`：`’right’` | `’left’`

        +   `.getToken(...keys)`⇒`string` | `null`

        +   `._call(text, options)`⇒`BatchEncoding`

        +   `._encode_text(text)`⇒`Array<string>` | `null`

        +   `.encode(text, text_pair, options)`⇒`Array.<number>`

        +   `.batch_decode(batch, decode_args)`⇒`Array.<string>`

        +   [`.decode(token_ids, [decode_args])`](#module_tokenizers.PreTrainedTokenizer+decode)⇒`string`

        +   `.decode_single(token_ids, decode_args)`⇒`string`

        +   `.apply_chat_template(conversation, options)`⇒`string` | `Tensor` | `Array<number>` | `Array<Array<number>>`

    +   *静态*

        +   `.from_pretrained(pretrained_model_name_or_path, options)`⇒`Promise.<PreTrainedTokenizer>`

* * *

### new PreTrainedTokenizer(tokenizerJSON, tokenizerConfig)

创建一个新的 PreTrainedTokenizer 实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokenizerJSON | `Object` | tokenizer 的 JSON。 |
| tokenizerConfig | `Object` | tokenizer 的配置。 |

* * *

### preTrainedTokenizer.added_tokens：<code>Array.<AddedToken></code>

**类型**：`PreTrainedTokenizer`的实例属性

* * *

### preTrainedTokenizer.remove_space：<code>boolean</code>

在标记化时是否去除文本（删除字符串前后的多余空格）。

**类型**：`PreTrainedTokenizer`的实例属性

* * *

### preTrainedTokenizer.padding_side : <code>’right’</code> | <code>’left’</code>

**类型**：`PreTrainedTokenizer`的实例属性

* * *

### preTrainedTokenizer.getToken(...keys)⇒<code>string</code> | <code>null</code>

返回 tokenizer 配置对象中第一个匹配键的值。

**类型**：`PreTrainedTokenizer`的实例方法

**返回**：`string` | `null` - 与第一个匹配键关联的值，如果找不到匹配项则返回 null。

**抛出**：

+   `Error` 如果找到匹配键的对象且其 __type 属性不是"AddedToken"。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| ...keys | `string` | 要在 tokenizer 配置对象中搜索的一个或多个键。 |

* * *

### preTrainedTokenizer._call(text, options)⇒<code>BatchEncoding</code>

对给定的文本进行编码/标记化。

**类型**：`PreTrainedTokenizer`的实例方法

**返回**：`BatchEncoding` - 传递给模型的对象。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| text | `string` &#124; `Array<string>` |  | 要进行标记化的文本。 |
| options | `Object` |  | 包含以下属性的可选对象： |
| [options.text_pair] | `string` &#124; `Array<string>` | `null` | 要编码的可选第二个序列。如果设置，必须与文本相同类型。 |
| [options.padding] | `boolean` &#124; `'max_length'` | `false` | 是否填充输入序列。 |
| [options.add_special_tokens] | `boolean` | `true` | 是否添加与相应模型相关的特殊标记。 |
| [options.truncation] | `boolean` |  | 是否截断输入序列。 |
| [options.max_length] | `number` |  | 返回列表的最大长度和可选填充长度。 |
| [options.return_tensor] | `boolean` | `true` | 是否将结果返回为张量或数组。 |

* * *

### preTrainedTokenizer._encode_text(text) ⇒ <code> Array < string > </code> | <code> null </code>

使用分词器的预处理流程对单个文本进行编码。

**种类**: `PreTrainedTokenizer`的实例方法

**返回**: `Array<string>` | `null` - 编码的标记。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` &#124; `null` | 要编码的文本。 |

* * *

### preTrainedTokenizer.encode(text, text_pair, options) ⇒ <code> Array. < number > </code>

使用模型的分词器对单个文本或一对文本进行编码。

**种类**: `PreTrainedTokenizer`的实例方法

**返回**: `Array.<number>` - 代表编码文本的标记 ID 的数组。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| text | `string` |  | 要编码的文本。 |
| text_pair | `string` &#124; `null` | `null` | 要编码的可选第二个文本。 |
| options | `Object` |  | 包含以下属性的可选对象： |
| [options.add_special_tokens] | `boolean` | `true` | 是否添加与相应模型相关的特殊标记。 |

* * *

### preTrainedTokenizer.batch_decode(batch, decode_args) ⇒ <code> Array. < string > </code>

解码一批标记化序列。

**种类**: `PreTrainedTokenizer`的实例方法

**返回**: `Array.<string>` - 解码序列的列表。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| batch | `Array<Array<number>>` &#124; `Tensor` | 标记化输入序列的列表/张量。 |
| decode_args | `Object` | (可选) 解码参数的对象。 |

* * *

### preTrainedTokenizer.decode(token_ids, [decode_args]) ⇒ <code> string </code>

将一系列标记 ID 解码回字符串。

**种类**: `PreTrainedTokenizer`的实例方法

**返回**: `string` - 解码后的字符串。

**抛出**:

+   `错误` 如果`token_ids`不是非空整数数组。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| token_ids | `Array<number>` &#124; `Tensor` |  | 要解码的标记 ID 列表/张量。 |
| [decode_args] | `Object` | `{}` |  |
| [decode_args.skip_special_tokens] | `boolean` | `false` | 如果为 true，则从输出字符串中删除特殊标记。 |
| [decode_args.clean_up_tokenization_spaces] | `boolean` | `true` | 如果为 true，则删除标点符号和缩写形式之前的空格。 |

* * *

### preTrainedTokenizer.decode_single(token_ids, decode_args) ⇒ <code> string </code>

将单个标记 id 列表解码为字符串。

**种类**: `PreTrainedTokenizer`的实例方法

**返回**: `string` - 解码后的字符串

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| token_ids | `Array.<number>` |  | 要解码的标记 id 列表 |
| decode_args | `Object` |  | 解码的可选参数 |
| [decode_args.skip_special_tokens] | `boolean` | `false` | 解码过程中是否跳过特殊标记 |
| [decode_args.clean_up_tokenization_spaces] | `boolean` |  | 是否在解码期间清除标记化空格。如果为 null，则值将设置为`this.decoder.cleanup`（如果存在），否则将回退到`this.clean_up_tokenization_spaces`（如果存在），最后回退到`true`。 |

* * *

### preTrainedTokenizer.apply_chat_template(conversation, options) ⇒ <code> string </code> | <code> Tensor </code> | <code> Array < number > </code> | <code> Array < Array < number > > </code>

将具有``"role"``和``"content"``键的消息对象列表转换为标记 ID 列表。此方法旨在与聊天模型一起使用，并将读取分词器的`chat_template`属性以确定在转换时要使用的格式和控制标记。当`chat_template`为`None`时，将退回到在类级别指定的`default_chat_template`。

查看更多信息，请访问[这里](https://huggingface.co/docs/transformers/chat_templating)。

**示例:** 应用聊天模板到对话。

```py
import { AutoTokenizer } from "@xenova/transformers";

const tokenizer = await AutoTokenizer.from_pretrained("mistralai/Mistral-7B-Instruct-v0.1");

const chat = [
  { "role": "user", "content": "Hello, how are you?" },
  { "role": "assistant", "content": "I'm doing great. How can I help you today?" },
  { "role": "user", "content": "I'd like to show off how chat templating works!" },
]

const text = tokenizer.apply_chat_template(chat, { tokenize: false });
// "<s>[INST] Hello, how are you? [/INST]I'm doing great. How can I help you today?</s> [INST] I'd like to show off how chat templating works! [/INST]"

const input_ids = tokenizer.apply_chat_template(chat, { tokenize: true, return_tensor: false });
// [1, 733, 16289, 28793, 22557, 28725, 910, 460, 368, 28804, 733, 28748, 16289, 28793, 28737, 28742, 28719, 2548, 1598, 28723, 1602, 541, 315, 1316, 368, 3154, 28804, 2, 28705, 733, 16289, 28793, 315, 28742, 28715, 737, 298, 1347, 805, 910, 10706, 5752, 1077, 3791, 28808, 733, 28748, 16289, 28793]
```

**类型**: `PreTrainedTokenizer`的实例方法

**返回**: `string` | `Tensor` | `Array<number>` | `Array<Array<number>>` - 标记化的输出。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| conversation | `Array.<Message>` |  | 具有``"role"``和``"content"``键的消息对象列表。 |
| options | `Object` |  | 包含以下属性的可选对象: |
| [options.chat_template] | `string` | `null` | 用于此转换的 Jinja 模板。如果未传递此参数，则将使用模型的默认聊天模板。 |
| [options.add_generation_prompt] | `boolean` | `false` | 是否以指示助手消息开始的标记结束提示。当您想要从模型生成响应时，这很有用。请注意，此参数将传递给聊天模板，因此模板必须支持此参数才能产生任何效果。 |
| [options.tokenize] | `boolean` | `true` | 是否对输出进行标记化。如果为 false，则输出将是一个字符串。 |
| [options.padding] | `boolean` | `false` | 是否将序列填充到最大长度。如果 tokenize 为 false，则无效。 |
| [options.truncation] | `boolean` | `false` | 是否将序列截断到最大长度。如果 tokenize 为 false，则无效。 |
| [options.max_length] | `number` |  | 用于填充或截断的最大长度（以标记为单位）。如果未指定，则将使用分词器的`max_length`属性作为默认值。 |
| [options.return_tensor] | `boolean` | `true` | 是否将输出作为张量或数组返回。如果 tokenize 为 false，则无效。 |

* * *

### PreTrainedTokenizer.from_pretrained(pretrained_model_name_or_path, options) ⇒ <code> Promise. < PreTrainedTokenizer > </code>

从给定的`pretrained_model_name_or_path`加载预训练的分词器。

**类型**: `PreTrainedTokenizer`的静态方法

**返回**: `Promise.<PreTrainedTokenizer>` - `PreTrainedTokenizer`类的新实例。

**抛出**:

+   如果在`pretrained_model_name_or_path`中找不到`tokenizer.json`或`tokenizer_config.json`文件，则抛出`Error`。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| pretrained_model_name_or_path | `string` | 预训练分词器的路径。 |
| options | `PretrainedTokenizerOptions` | 加载分词器的附加选项。 |

* * *

## tokenizers.BertTokenizer ⇐ <code> PreTrainedTokenizer </code>

BertTokenizer 是用于为 BERT 模型标记化文本的类。

**类型**: `tokenizers`的静态类

**扩展**: `PreTrainedTokenizer`

* * *

## tokenizers.AlbertTokenizer ⇐ <code> PreTrainedTokenizer </code>

Albert 分词器

**类型**: `tokenizers`的静态类

**扩展**: `PreTrainedTokenizer`

* * *

## tokenizers.NllbTokenizer

NllbTokenizer 类用于为 NLLB（“没有语言被遗忘”）模型对文本进行标记。

没有语言被遗忘（NLLB）是一项首创性的人工智能项目，开源模型能够直接在 200 多种语言之间提供高质量的翻译 — 包括 Asturian、Luganda、Urdu 等低资源语言。它旨在帮助人们与任何人在任何地方进行交流，无论他们的语言偏好如何。有关更多信息，请查看他们的[论文](https://arxiv.org/abs/2207.04672)。

有关支持的语言列表（以及它们的语言代码），

**种类**：`tokenizers`的静态类

**查看**：[`github.com/facebookresearch/flores/blob/main/flores200/README.md#languages-in-flores-200`](https://github.com/facebookresearch/flores/blob/main/flores200/README.md#languages-in-flores-200)

* * *

### nllbTokenizer._build_translation_inputs(raw_inputs, tokenizer_options, generate_kwargs) ⇒ <code> Object </code>

用于为`NllbTokenizer`构建翻译输入的辅助函数。

**种类**：`NllbTokenizer`的实例方法

**返回**：`Object` - 传递给模型的对象。

| Param | Type | 描述 |
| --- | --- | --- |
| raw_inputs | `string` &#124; `Array<string>` | 要标记的文本。 |
| tokenizer_options | `Object` | 发送到标记器的选项 |
| generate_kwargs | `Object` | 生成选项。 |

* * *

## tokenizers.M2M100Tokenizer

M2M100Tokenizer 类用于为 M2M100（“多对多”）模型对文本进行标记。

M2M100 是一个多语言编码器-解码器（seq-to-seq）模型，用于训练多对多多语言翻译。它在这篇[论文](https://arxiv.org/abs/2010.11125)中介绍，并首次在[此](https://github.com/pytorch/fairseq/tree/master/examples/m2m_100)存储库中发布。

有关支持的语言列表（以及它们的语言代码），

**种类**：`tokenizers`的静态类

**查看**：[`huggingface.co/facebook/m2m100_418M#languages-covered`](https://huggingface.co/facebook/m2m100_418M#languages-covered)

* * *

### m2M100Tokenizer._build_translation_inputs(raw_inputs, tokenizer_options, generate_kwargs) ⇒ <code> Object </code>

用于为`M2M100Tokenizer`构建翻译输入的辅助函数。

**种类**：`M2M100Tokenizer`的实例方法

**返回**：`Object` - 传递给模型的对象。

| Param | Type | 描述 |
| --- | --- | --- |
| raw_inputs | `string` &#124; `Array<string>` | 要标记的文本。 |
| tokenizer_options | `Object` | 发送到标记器的选项 |
| generate_kwargs | `Object` | 生成选项。 |

* * *

## tokenizers.WhisperTokenizer ⇐ <code> PreTrainedTokenizer </code>

WhisperTokenizer 标记器

**种类**：`tokenizers`的静态类

**扩展**：`PreTrainedTokenizer`

+   .WhisperTokenizer ⇐ `PreTrainedTokenizer`

    +   `._decode_asr(sequences, options)` ⇒ `*`

    +   `.decode()` : `*`

    +   `.get_decoder_prompt_ids(options)` ⇒ `Array.<Array<number>>`

* * *

### whisperTokenizer._decode_asr(sequences, options) ⇒ <code> * </code>

解码自动语音识别（ASR）序列。

**种类**：`WhisperTokenizer`的实例方法

**返回**：`*` - 解码后的序列。

| Param | Type | 描述 |
| --- | --- | --- |
| sequences | `*` | 要解码的序列。 |
| options | `Object` | 用于解码的选项。 |

* * *

### whisperTokenizer.decode() : <code> * </code>

**种类**：`WhisperTokenizer`的实例方法

* * *

### whisperTokenizer.get_decoder_prompt_ids(options) ⇒ <code> Array. < Array < number > > </code>

构建`WhisperTokenizer`的翻译输入的辅助函数，取决于语言、任务以及是否要预测时间戳标记。

用于覆盖附加到标签序列开头的前缀标记。

**示例：获取语言的 id**

```py
// instantiate the tokenizer and set the prefix token to Spanish
const tokenizer = await WhisperTokenizer.from_pretrained('Xenova/whisper-tiny');
const forced_decoder_ids = tokenizer.get_decoder_prompt_ids({ language: 'spanish' });
// [(1, 50262), (2, 50363)]
```

**类型**: `WhisperTokenizer`的实例方法

**返回**: `Array.<Array<number>>` - 解码器提示的 id。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 选项 | `Object` | 生成解码器提示的选项。 |
| [options.language] | `string` | 转录文本的语言。对于多语言语音识别和语音翻译任务，相应的语言 id 标记会附加到序列的开头，例如对于"西班牙语"，标记"< | es | >"会附加到序列的开头。 |
| [options.task] | `string` | 要附加到序列开头的任务标识符（如果有）。这应该用于多语言微调，使用"transcribe"进行语音识别和"translate"进行语音翻译。 |
| [options.no_timestamps] | `boolean` | 是否在序列开头添加< | notimestamps | >标记。 |

* * *

## tokenizers.MarianTokenizer

**类型**: `tokenizers`的静态类

**待办事项**

+   Hugging Face 的“快速”分词器库尚不支持此模型（[`github.com/huggingface/tokenizers`](https://github.com/huggingface/tokenizers)）。因此，这个基于快速分词器的实现可能会产生略微不准确的结果。

+   MarianTokenizer

    +   `new MarianTokenizer(tokenizerJSON, tokenizerConfig)`

    +   `._encode_text(text)` ⇒ `Array`

* * *

### new MarianTokenizer(tokenizerJSON, tokenizerConfig)

创建一个新的 MarianTokenizer 实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokenizerJSON | `Object` | 分词器的 JSON。 |
| tokenizerConfig | `Object` | 分词器的配置。 |

* * *

### marianTokenizer._encode_text(text) ⇒ <code> Array </code>

编码单个文本。必须重写此方法，因为在使用 sentencepiece 模型进行编码之前必须删除语言代码。

**类型**: `MarianTokenizer`的实例方法

**返回**: `Array` - 编码后的标记。

**查看**: [`github.com/huggingface/transformers/blob/12d51db243a00726a548a43cc333390ebae731e3/src/transformers/models/marian/tokenization_marian.py#L204-L213`](https://github.com/huggingface/transformers/blob/12d51db243a00726a548a43cc333390ebae731e3/src/transformers/models/marian/tokenization_marian.py#L204-L213)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` &#124; `null` | 要编码的文本。 |

* * *

## tokenizers.AutoTokenizer

用于使用`from_pretrained`函数实例化预训练分词器的辅助类。所选的分词器类由分词器配置中指定的类型确定。

**类型**: `tokenizers`的静态类

* * *

### AutoTokenizer.from_pretrained(pretrained_model_name_or_path, options) ⇒ <code> Promise. < PreTrainedTokenizer > </code>

从预训练模型实例化库中的一个分词器类。

要实例化的分词器类是根据配置对象的`tokenizer_class`属性选择的（如果可能，作为参数传递或从`pretrained_model_name_or_path`加载）

**类型**: `AutoTokenizer`的静态方法

**返回**: `Promise.<PreTrainedTokenizer>` - PreTrainedTokenizer 类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |

| pretrained_model_name_or_path | `string` | 预训练模型的名称或路径。可以是：

+   一个字符串，预训练分词器的*模型 id*，托管在 huggingface.co 上的模型仓库中。有效的模型 id 可以位于根级别，如`bert-base-uncased`，或者在用户或组织名称下命名空间化，如`dbmdz/bert-base-german-cased`。

+   一个包含分词器文件的*目录*的路径，例如，`./my_model_directory/`。

|

| options | `PretrainedTokenizerOptions` | 加载分词器的附加选项。 |
| --- | --- | --- |

* * *

## tokenizers~AddedToken

代表用户在现有模型词汇表之上添加的 token。AddedToken 可以配置以指定它们在各种情况下应该具有的行为，比如：

+   它们是否只匹配单词

+   是否在其左侧或右侧包含任何空格

**类型**：`tokenizers`的内部类

* * *

### new AddedToken(config)

创建一个 AddedToken 的新实例。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| config | `Object` |  | Added token 的配置对象。 |
| config.content | `string` |  | 添加 token 的内容。 |
| config.id | `number` |  | 添加 token 的 id。 |
| [config.single_word] | `boolean` | `false` | 这个 token 是否必须是一个单词或者可以打断单词。 |
| [config.lstrip] | `boolean` | `false` | 这个 token 是否应该去除左侧的空格。 |
| [config.rstrip] | `boolean` | `false` | 这个 token 是否应该去除右侧的空格。 |
| [config.normalized] | `boolean` | `false` | 这个 token 是否应该被规范化。 |
| [config.special] | `boolean` | `false` | 这个 token 是否是特殊的。 |

* * *

## tokenizers~WordPieceTokenizer ⇐ <code> TokenizerModel </code>

一个使用 WordPiece 编码来编码 token 的 TokenizerModel 的子类。

**类型**：`tokenizers`的内部类

**扩展**：`TokenizerModel`

+   ~WordPieceTokenizer ⇐ `TokenizerModel`

    +   `new WordPieceTokenizer(config)`

    +   `.tokens_to_ids` : `Map.<string, number>`

    +   `.unk_token_id` : `number`

    +   `.unk_token` : `string`

    +   `.max_input_chars_per_word` : `number`

    +   `.vocab` : `Array.<string>`

    +   `.encode(tokens)` ⇒ `Array.<string>`

* * *

### new WordPieceTokenizer(config)

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| config | `Object` |  | 配置对象。 |
| config.vocab | `Object` |  | 一个将 token 映射到 id 的映射。 |
| config.unk_token | `string` |  | 未知 token 的字符串。 |
| config.continuing_subword_prefix | `string` |  | 用于继续子词的前缀。 |
| [config.max_input_chars_per_word] | `number` | `100` | 每个单词的最大字符数。 |

* * *

### wordPieceTokenizer.tokens_to_ids : <code> Map. < string, number > </code>

一个将 token 映射到 id 的映射。

**类型**：`WordPieceTokenizer`的实例属性

* * *

### wordPieceTokenizer.unk_token_id : <code> number </code>

未知 token 的 id。

**类型**：`WordPieceTokenizer`的实例属性

* * *

### wordPieceTokenizer.unk_token : <code> string </code>

未知 token 的字符串。

**类型**：`WordPieceTokenizer`的实例属性

* * *

### wordPieceTokenizer.max_input_chars_per_word : <code> number </code>

每个单词允许的最大字符数。

**类型**：`WordPieceTokenizer`的实例属性

* * *

### wordPieceTokenizer.vocab : <code> Array. < string > </code>

一个 token 数组。

**类型**：`WordPieceTokenizer`的实例属性

* * *

### wordPieceTokenizer.encode(tokens) ⇒ <code> Array. < string > </code>

使用 WordPiece 编码对标记数组进行编码。

**类型**：`WordPieceTokenizer` 的实例方法

**返回**：`Array.<string>` - 编码的标记数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array.<string>` | 要编码的标记。 |

* * *

## tokenizers~Unigram ⇐ <code> TokenizerModel </code>

表示 Unigram 分词器模型的类。

**类型**：`tokenizers` 的内部类

**扩展**：`TokenizerModel`

+   【~Unigram】(#module_tokenizers..Unigram) ⇐ `TokenizerModel`

    +   `new Unigram(config, moreConfig)`

    +   `.populateNodes(lattice)`

    +   `.tokenize(normalized)` ⇒ `Array.<string>`

    +   `.encode(tokens)` ⇒ `Array.<string>`

* * *

### new Unigram(config, moreConfig)

创建一个新的 Unigram 分词器模型。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | Unigram 模型的配置对象。 |
| config.unk_id | `number` | 未知标记的 ID |
| config.vocab | `Array.<Array<any>>` | 表示标记到分数的映射的二维数组。 |
| moreConfig | `Object` | Unigram 模型的附加配置对象。 |

* * *

### unigram.populateNodes(lattice)

填充格节点。

**类型**：`Unigram` 的实例方法

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| lattice | `TokenLattice` | 要填充节点的标记格。 |

* * *

### unigram.tokenize(normalized) ⇒ <code> Array. < string > </code>

使用 unigram 模型将标记数组编码为子标记数组。

**类型**：`Unigram` 的实例方法

**返回**：`Array.<string>` - 通过使用 unigram 模型对输入标记进行编码获得的子标记数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| normalized | `string` | 规范化的字符串。 |

* * *

### unigram.encode(tokens) ⇒ <code> Array. < string > </code>

使用 Unigram 编码对标记数组进行编码。

**类型**：`Unigram` 的实例方法

**返回**：`Array.<string>` - 编码的标记数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array.<string>` | 要编码的标记。 |

* * *

## tokenizers~BPE ⇐ <code> TokenizerModel </code>

用于将文本编码为字节对编码（BPE）标记的 BPE 类。

**类型**：`tokenizers` 的内部类

**扩展**：`TokenizerModel`

+   【~BPE】(#module_tokenizers..BPE) ⇐ `TokenizerModel`

    +   `new BPE(config)`

    +   `.tokens_to_ids` : `Map.<string, number>`

    +   `.cache` : `Map.<string, Array<string>>`

    +   `.bpe(token)` ⇒ `Array.<string>`

    +   `.encode(tokens)` ⇒ `Array.<string>`

* * *

### new BPE(config)

创建一个 BPE 实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | BPE 的配置对象。 |
| config.vocab | `Object` | 将标记映射到 ID 的映射。 |
| config.unk_token | `string` | 用于词汇表外单词的未知标记。 |
| config.end_of_word_suffix | `string` | 放在每个单词末尾的后缀。 |
| [config.continuing_subword_suffix] | `string` | 插入在单词之间的后缀。 |
| config.merges | `Array` | 作为字符串的 BPE 合并数组。 |

* * *

### bpE.tokens_to_ids : <code> Map. < string, number > </code>

**类型**：`BPE` 的实例属性

* * *

### bpE.cache : <code> Map. < string, Array < string > > </code>

**类型**：`BPE` 的实例属性

* * *

### bpE.bpe(token) ⇒ <code> Array. < string > </code>

将字节对编码（BPE）应用于给定的标记。高效的基于堆的优先级队列实现，改编自[`github.com/belladoreai/llama-tokenizer-js`](https://github.com/belladoreai/llama-tokenizer-js)。

**种类**：`BPE`的实例方法

**返回**：`Array.<string>` - BPE 编码的标记。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| token | `string` | 要编码的标记。 |

* * *

### bpE.encode(tokens) ⇒ <code> Array. < string > </code>

对输入的标记序列使用 BPE 算法进行编码，并返回生成的子词标记。

**种类**：`BPE`的实例方法

**返回**：`Array.<string>` - 对输入的标记序列应用 BPE 算法后得到的子词标记。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array.<string>` | 要编码的输入标记序列。 |

* * *

## tokenizers~LegacyTokenizerModel

用于仅具有词汇表的标记器的传统标记器类。

**种类**：`tokenizers`的内部类

+   ~LegacyTokenizerModel

    +   `new LegacyTokenizerModel(config, moreConfig)`

    +   `.tokens_to_ids` : `Map.<string, number>`

* * *

### 新的 LegacyTokenizerModel(config, moreConfig)

创建一个 LegacyTokenizerModel 实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | LegacyTokenizerModel 的配置对象。 |
| config.vocab | `Object` | 一个（可能是嵌套的）将标记映射到 ID 的映射。 |
| moreConfig | `Object` | 为 LegacyTokenizerModel 模型提供额外的配置对象。 |

* * *

### legacyTokenizerModel.tokens_to_ids : <code> Map. < string, number > </code>

**种类**：`LegacyTokenizerModel`的实例属性

* * *

## tokenizers~Normalizer

文本规范化的基类。

**种类**：`tokenizers`的内部抽象类

+   *~Normalizer*

    +   *`new Normalizer(config)`*

    +   *实例*

        +   **`.normalize(text)` ⇒ `string`**

        +   *`._call(text)` ⇒ `string`*

    +   *静态*

        +   *`.fromConfig(config)` ⇒ `Normalizer`*

* * *

### 新的 Normalizer(config)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 规范化器的配置对象。 |

* * *

### normalizer.normalize(text) ⇒ <code> string </code>

规范化输入文本。

**种类**：`Normalizer`的实例抽象方法

**返回**：`string` - 规范化后的文本。

**抛出**：

+   `错误` 如果在子类中未实现此方法。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要规范化的文本。 |

* * *

### normalizer._call(text) ⇒ <code> string </code>

别名为 Normalizer#normalize。

**种类**：`Normalizer`的实例方法

**返回**：`string` - 规范化后的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要规范化的文本。 |

* * *

### Normalizer.fromConfig(config) ⇒ <code> Normalizer </code>

从配置对象创建规范化器的工厂方法。

**种类**：`Normalizer`的静态方法

**返回**：`Normalizer` - 一个 Normalizer 对象。

**抛出**：

+   `错误` 如果在配置中指定了未知的规范化器类型。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 规范化器的配置对象。 |

* * *

## tokenizers~Replace ⇐ <code> Normalizer </code>

替换规范化器，用给定的字符串或正则表达式替换模式的出现。

**种类**：`tokenizers`的内部类

**扩展**：`Normalizer`

* * *

### replace.normalize(text) ⇒ <code> string </code>

通过用内容替换模式来规范化输入文本。

**种类**: `Replace`的实例方法

**返回**: `string` - 用内容替换模式后的规范化文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要规范化的输入文本。 |

* * *

## tokenizers~NFC ⇐ <code> Normalizer </code>

一个将输入文本应用 Unicode 规范化形式 C（NFC）的规范化器。

**种类**: `tokenizers`的内部类

**扩展**: `Normalizer`

* * *

### nfC.normalize(text) ⇒ <code> string </code>

通过应用 Unicode 规范化形式 C（NFC）来规范化输入文本。

**种类**: `NFC`的实例方法

**返回**: `string` - 规范化后的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要规范化的输入文本。 |

* * *

## tokenizers~NFKC ⇐ <code> Normalizer </code>

NFKC 规范化器。

**种类**: `tokenizers`的内部类

**扩展**: `Normalizer`

* * *

### nfkC.normalize(text) ⇒ <code> string </code>

使用 NFKC 规范化文本。

**种类**: `NFKC`的实例方法

**返回**: `string` - 规范化后的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要规范化的文本。 |

* * *

## tokenizers~NFKD ⇐ <code> Normalizer </code>

NFKD 规范化器。

**种类**: `tokenizers`的内部类

**扩展**: `Normalizer`

* * *

### nfkD.normalize(text) ⇒ <code> string </code>

使用 NFKD 规范化文本。

**种类**: `NFKD`的实例方法

**返回**: `string` - 规范化后的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要规范化的文本。 |

* * *

## tokenizers~StripNormalizer

从输入文本中去除前导和/或尾随空格的规范化器。

**种类**: `tokenizers`的内部类

* * *

### stripNormalizer.normalize(text) ⇒ <code> string </code>

从输入文本中去除前导和/或尾随空格。

**种类**: `StripNormalizer`的实例方法

**返回**: `string` - 规范化后的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 输入文本。 |

* * *

## tokenizers~StripAccents ⇐ <code> Normalizer </code>

StripAccents 规范化器从文本中删除所有重音符号。

**种类**: `tokenizers`的内部类

**扩展**: `Normalizer`

* * *

### stripAccents.normalize(text) ⇒ <code> string </code>

从文本中删除所有重音符号。

**种类**: `StripAccents`的实例方法

**返回**: `string` - 没有重音符号的规范化文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 输入文本。 |

* * *

## tokenizers~Lowercase ⇐ <code> Normalizer </code>

一个将输入字符串转换为小写的规范化器。

**种类**: `tokenizers`的内部类

**扩展**: `Normalizer`

* * *

### lowercase.normalize(text) ⇒ <code> string </code>

将输入字符串转换为小写。

**种类**: `Lowercase`的实例方法

**返回**: `string` - 规范化后的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要规范化的文本。 |

* * *

## tokenizers~Prepend ⇐ <code> Normalizer </code>

一个在输入字符串前添加字符串的规范化器。

**种类**: `tokenizers`的内部类

**扩展**: `Normalizer`

* * *

### prepend.normalize(text) ⇒ <code> string </code>

在输入字符串前添加内容。

**种类**: `Prepend`的实例方法

**返回**: `string` - 规范化后的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要规范化的文本。 |

* * *

## tokenizers~NormalizerSequence ⇐ <code> Normalizer </code>

一个应用一系列规范化器的规范化器。

**种类**: `tokenizers`的内部类

**扩展自**：`Normalizer`

+   ~NormalizerSequence ⇐ `Normalizer`

    +   `new NormalizerSequence(config)`

    +   `.normalize(text)` ⇒ `string`

* * *

### 新的 NormalizerSequence(config)

创建一个新的 NormalizerSequence 实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 配置对象。 |
| config.normalizers | `Array.<Object>` | 一组规范化器配置对象。 |

* * *

### normalizerSequence.normalize(text) ⇒ <code> string </code>

将一系列规范化器应用于输入文本。

**类型**：`NormalizerSequence`的实例方法

**返回值**：`string` - 规范化后的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要规范化的文本。 |

* * *

## tokenizers~BertNormalizer ⇐ <code> Normalizer </code>

表示 BERT 标记化中使用的规范化器的类。

**类型**：`tokenizers`的内部类

**扩展自**：`Normalizer`

+   ~BertNormalizer ⇐ `Normalizer`

    +   `._tokenize_chinese_chars(text)` ⇒ `string`

    +   `._is_chinese_char(cp)` ⇒ `boolean`

    +   `.stripAccents(text)` ⇒ `string`

    +   `.normalize(text)` ⇒ `string`

* * *

### bertNormalizer._tokenize_chinese_chars(text) ⇒ <code> string </code>

在输入文本中的任何 CJK（中文、日文或韩文）字符周围添加空格。

**类型**：`BertNormalizer`的实例方法

**返回值**：`string` - 在 CJK 字符周围添加空格的标记文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要标记化的输入文本。 |

* * *

### bertNormalizer._is_chinese_char(cp) ⇒ <code> boolean </code>

检查给定的 Unicode 代码点是否表示 CJK（中文、日文或韩文）字符。

“中文字符”被定义为 CJK Unicode 块中的任何内容：[`en.wikipedia.org/wiki/CJK_Unified_Ideographs_(Unicode_block)`](https://en.wikipedia.org/wiki/CJK_Unified_Ideographs_(Unicode_block))

请注意，CJK Unicode 块并不是所有日语和韩语字符，尽管其名称如此。现代韩语 Hangul 字母表是一个不同的块，日语平假名和片假名也是如此。这些字母表用于书写以空格分隔的单词，因此它们不会被特殊处理，而是像其他所有语言一样处理。

**类型**：`BertNormalizer`的实例方法

**返回值**：`boolean` - 如果代码点表示 CJK 字符，则为 True，否则为 False。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| cp | `number` | 要检查的 Unicode 代码点。 |

* * *

### bertNormalizer.stripAccents(text) ⇒ <code> string </code>

从给定文本中去除重音符号。

**类型**：`BertNormalizer`

**返回值**：`string` - 去除重音符号的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要去除重音符号的文本。 |

* * *

### bertNormalizer.normalize(text) ⇒ <code> string </code>

根据配置规范化给定的文本。

**类型**：`BertNormalizer`的实例方法

**返回值**：`string` - 规范化后的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要规范化的文本。 |

* * *

## tokenizers~PreTokenizer ⇐ <code> Callable </code>

表示在标记化中使用的预标记器的可调用类。子类应该实现`pre_tokenize_text`方法来定义特定的预标记逻辑。

**类型**：`tokenizers`的内部类

**扩展自**：`Callable`

+   ~PreTokenizer ⇐ `Callable`

    +   *实例*

        +   *[`.pre_tokenize_text(text, [options])`](#module_tokenizers..PreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`*

        +   [`.pre_tokenize(text, [options])`](#module_tokenizers..PreTokenizer+pre_tokenize) ⇒ `Array.<string>`

        +   [`._call(text, [options])`](#module_tokenizers..PreTokenizer+_call) ⇒ `Array.<string>`

    +   *静态*

        +   `.fromConfig(config)` ⇒ `PreTokenizer`

* * *

### preTokenizer.pre_tokenize_text(text, [options]) ⇒ <code> Array. < string > </code>

应该由子类实现的方法来定义特定的预分词逻辑。

**种类**: `PreTokenizer`的实例抽象方法

**返回**: `Array.<string>` - 预标记的文本。

**抛出**:

+   `Error` 如果子类中未实现该方法。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要预标记的文本。 |
| [options] | `Object` | 用于预分词逻辑的额外选项。 |

* * *

### preTokenizer.pre_tokenize(text, [options]) ⇒ <code> Array. < string > </code>

将给定的文本标记为预标记。

**种类**: `PreTokenizer`的实例方法

**返回**: `Array.<string>` - 预标记的数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` &#124; `Array<string>` | 要预标记的文本或文本数组。 |
| [options] | `Object` | 用于预分词逻辑的额外选项。 |

* * *

### preTokenizer._call(text, [options]) ⇒ <code> Array. < string > </code>

别名为 PreTokenizer#pre_tokenize。

**种类**: `PreTokenizer`的实例方法

**返回**: `Array.<string>` - 预标记的数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` &#124; `Array<string>` | 要预标记的文本或文本数组。 |
| [options] | `Object` | 用于预分词逻辑的额外选项。 |

* * *

### PreTokenizer.fromConfig(config) ⇒ <code> PreTokenizer </code>

工厂方法，根据提供的配置返回`PreTokenizer`的子类的实例。

**种类**: `PreTokenizer`的静态方法

**返回**: `PreTokenizer` - `PreTokenizer`的子类的实例。

**抛出**:

+   `Error` 如果提供的配置对象与任何已知的预分词器不对应。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 用于预分词器的配置对象。 |

* * *

## tokenizers~BertPreTokenizer ⇐ <code> PreTokenizer </code>

**种类**: `tokenizers`的内部类

**扩展**: `PreTokenizer`

+   ~BertPreTokenizer ⇐ `PreTokenizer`

    +   `new BertPreTokenizer(config)`

    +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..BertPreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

* * *

### new BertPreTokenizer(config)

一个将文本拆分为单词片段的预分词器，使用类似于 BERT 原始实现中使用的基本分词方案。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 配置对象。 |

* * *

### bertPreTokenizer.pre_tokenize_text(text, [options]) ⇒ <code> Array. < string > </code>

使用 BERT 预标记方案对单个文本进行标记。

**种类**: `BertPreTokenizer`的实例方法

**返回**: `Array.<string>` - 令牌数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要标记的文本。 |
| [options] | `Object` | 用于预分词逻辑的额外选项。 |

* * *

## tokenizers~ByteLevelPreTokenizer ⇐ <code> PreTokenizer </code>

一个将文本拆分为字节对编码（BPE）子词的预分词器。

**种类**: `tokenizers`的内部类

**扩展**: `PreTokenizer`

+   ~ByteLevelPreTokenizer ⇐ `PreTokenizer`

    +   `new ByteLevelPreTokenizer(config)`

    +   `.add_prefix_space` : `boolean`

    +   `.trim_offsets` : `boolean`

    +   `.use_regex` : `boolean`

    +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..ByteLevelPreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

* * *

### new ByteLevelPreTokenizer(config)

创建`ByteLevelPreTokenizer`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 配置对象。 |

* * *

### byteLevelPreTokenizer.add_prefix_space : <code> boolean </code>

是否在第一个单词前添加一个空格。这样可以将第一个单词视为任何其他单词。

**种类**：`ByteLevelPreTokenizer`的实例属性

* * *

### byteLevelPreTokenizer.trim_offsets : <code> boolean </code>

是否在后处理步骤中修剪偏移量以避免包含空格。

**种类**：`ByteLevelPreTokenizer`的实例属性

**待办事项**

+   在预标记步骤中使用此功能。

* * *

### byteLevelPreTokenizer.use_regex : <code> boolean </code>

是否使用标准的 GPT2 正则表达式进行空格拆分。如果要使用自己的拆分，请将其设置为 False。默认为 true。

**种类**：`ByteLevelPreTokenizer`的实例属性

* * *

### byteLevelPreTokenizer.pre_tokenize_text(text, [options]) ⇒ <code> Array. < string > </code>

使用字节级标记化对单个文本片段进行标记化。

**种类**：`ByteLevelPreTokenizer`的实例方法

**返回**：`Array.<string>` - 一个标记数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要标记的文本。 |
| [options] | `Object` | 预标记逻辑的其他选项。 |

* * *

## tokenizers~SplitPreTokenizer ⇐ <code> PreTokenizer </code>

使用给定模式拆分文本。

**种类**：`tokenizers`的内部类

**扩展**：`PreTokenizer`

+   ~SplitPreTokenizer ⇐ `PreTokenizer`

    +   `new SplitPreTokenizer(config)`

    +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..SplitPreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

* * *

### new SplitPreTokenizer(config)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 预标记器的配置选项。 |
| config.pattern | `Object` | 用于分割文本的模式。可以是字符串或正则表达式对象。 |
| config.pattern.String | `string` &#124; `undefined` | 用于拆分的字符串。仅在模式为字符串时定义。 |
| config.pattern.Regex | `string` &#124; `undefined` | 用于拆分的正则表达式。仅在模式为正则表达式时定义。 |
| config.behavior | `SplitDelimiterBehavior` | 分割时使用的行为。 |
| config.invert | `boolean` | 是否拆分（invert=false）或匹配（invert=true）模式。 |

* * *

### splitPreTokenizer.pre_tokenize_text(text, [options]) ⇒ <code> Array. < string > </code>

通过使用给定模式拆分文本进行标记化。

**种类**：`SplitPreTokenizer`的实例方法

**返回**：`Array.<string>` - 一个标记数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要标记的文本。 |
| [options] | `Object` | 预标记逻辑的其他选项。 |

* * *

## tokenizers~PunctuationPreTokenizer ⇐ <code> PreTokenizer </code>

根据标点符号拆分文本。

**种类**：`tokenizers`的内部类

**扩展**：`PreTokenizer`

+   ~PunctuationPreTokenizer ⇐ `PreTokenizer`

    +   `new PunctuationPreTokenizer(config)`

    +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..PunctuationPreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

* * *

### new PunctuationPreTokenizer(config)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 预标记器的配置选项。 |
| 配置.behavior | `SplitDelimiterBehavior` | 拆分时要使用的行为。 |

* * *

### punctuationPreTokenizer.pre_tokenize_text(text, [options]) ⇒ <code> Array. < string > </code>

使用给定的模式拆分文本进行标记化。

**类型**: `PunctuationPreTokenizer` 的实例方法

**返回**: `Array.<string>` - 一个标记数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要进行标记化的文本。 |
| [options] | `Object` | 预标记逻辑的附加选项。 |

* * *

## tokenizers~DigitsPreTokenizer ⇐ <code> PreTokenizer </code>

根据数字拆分文本。

**类型**: `tokenizers` 的内部类

**扩展**: `PreTokenizer`

+   ~DigitsPreTokenizer ⇐ `PreTokenizer`

    +   `new DigitsPreTokenizer(config)`

    +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..DigitsPreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

* * *

### new DigitsPreTokenizer(config)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 预标记器的配置选项。 |
| 配置.individual_digits | `boolean` | 是否拆分单个数字。 |

* * *

### digitsPreTokenizer.pre_tokenize_text(text, [options]) ⇒ <code> Array. < string > </code>

使用给定的模式拆分文本进行标记化。

**类型**: `DigitsPreTokenizer` 的实例方法

**返回**: `Array.<string>` - 一个标记数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要进行标记化的文本。 |
| [options] | `Object` | 预标记逻辑的附加选项。 |

* * *

## tokenizers~PostProcessor ⇐ <code> Callable </code>

**类型**: `tokenizers` 的内部类

**扩展**: `Callable`

+   ~PostProcessor ⇐ `Callable`

    +   `new PostProcessor(config)`

    +   *实例*

        +   `.post_process(tokens, ...args)` ⇒ `PostProcessedOutput`

        +   `._call(tokens, ...args)` ⇒ `PostProcessedOutput`

    +   *静态*

        +   `.fromConfig(config)` ⇒ `PostProcessor`

* * *

### new PostProcessor(config)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 后处理器的配置。 |

* * *

### postProcessor.post_process(tokens, ...args) ⇒ <code> PostProcessedOutput </code>

要在子类中实现的方法，以在给定的标记上应用后处理。

**类型**: `PostProcessor` 的实例方法

**返回**: `PostProcessedOutput` - 后处理后的标记。

**抛出**:

+   `Error` 如果在子类中未实现该方法。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array` | 要进行后处理的输入标记。 |
| ...args | `*` | 后处理逻辑所需的其他参数。 |

* * *

### postProcessor._call(tokens, ...args) ⇒ <code> PostProcessedOutput </code>

别名为 PostProcessor#post_process。

**类型**: `PostProcessor` 的实例方法

**返回**: `PostProcessedOutput` - 后处理后的标记。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array` | 要进行后处理的文本或文本数组。 |
| ...args | `*` | 后处理逻辑所需的其他参数。 |

* * *

### PostProcessor.fromConfig(config) ⇒ <code> PostProcessor </code>

从配置对象创建一个 PostProcessor 对象的工厂方法。

**种类**：`PostProcessor`的静态方法

**返回**：`PostProcessor` - 从给定配置创建的 PostProcessor 对象。

**抛出**：

+   `错误` 如果遇到未知的后处理器类型。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 表示后处理器的配置对象。 |

* * *

## tokenizers~BertProcessing

一个后处理器，将特殊标记添加到输入的开头和结尾。

**种类**：`tokenizers`的内部类

+   ~BertProcessing

    +   `new BertProcessing(config)`

    +   [`.post_process(tokens, [tokens_pair])`](#module_tokenizers..BertProcessing+post_process) ⇒ `PostProcessedOutput`

* * *

### new BertProcessing(config)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 后处理器的配置。 |
| config.cls | `Array.<string>` | 要添加到输入开头的特殊标记。 |
| config.sep | `Array.<string>` | 要添加到输入末尾的特殊标记。 |

* * *

### bertProcessing.post_process(tokens, [tokens_pair]) ⇒ <code> PostProcessedOutput </code>

将特殊标记添加到输入的开头和结尾。

**种类**：`BertProcessing`的实例方法

**返回**：`PostProcessedOutput` - 添加了特殊标记的后处理标记的开头和结尾。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| tokens | `Array.<string>` |  | 输入标记。 |
| [tokens_pair] | `Array.<string>` |  | 可选的第二组输入标记。 |

* * *

## tokenizers~TemplateProcessing ⇐ <code> PostProcessor </code>

用实际标记替换模板中的特殊标记的后处理器。

**种类**：`tokenizers`的内部类

**扩展**：`PostProcessor`

+   ~TemplateProcessing ⇐ `PostProcessor`

    +   `new TemplateProcessing(config)`

    +   [`.post_process(tokens, [tokens_pair])`](#module_tokenizers..TemplateProcessing+post_process) ⇒ `PostProcessedOutput`

* * *

### new TemplateProcessing(config)

创建一个新的`TemplateProcessing`实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 后处理器的配置选项。 |
| config.single | `Array` | 单个标记序列的模板。 |
| config.pair | `Array` | 一对标记序列的模板。 |

* * *

### templateProcessing.post_process(tokens, [tokens_pair]) ⇒ <code> PostProcessedOutput </code>

用实际标记替换模板中的特殊标记。

**种类**：`TemplateProcessing`的实例方法

**返回**：`PostProcessedOutput` - 包含用实际标记替换的特殊标记的标记列表的对象。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| tokens | `Array.<string>` |  | 第一个序列的标记列表。 |
| [tokens_pair] | `Array.<string>` |  | 第二个序列的标记列表（可选）。 |

* * *

## tokenizers~ByteLevelPostProcessor ⇐ <code> PostProcessor </code>

一个将给定标记原样返回的 PostProcessor。

**种类**：`tokenizers`的内部类

**扩展**：`PostProcessor`

* * *

### byteLevelPostProcessor.post_process(tokens, [tokens_pair]) ⇒ <code> PostProcessedOutput </code>

后处理给定的标记。

**种类**：`ByteLevelPostProcessor`的实例方法

**返回**：`PostProcessedOutput` - 包含后处理的标记的对象。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| tokens | `Array.<字符串>` |  | 第一个序列的标记列表。 |
| [tokens_pair] | `Array.<字符串>` |  | 第二个序列的标记列表（可选）。 |

* * *

## tokenizers~Decoder ⇐ <code>Callable</code>

标记解码器的基类。

**种类**：`tokenizers`的内部类

**扩展**：`Callable`

+   ~Decoder ⇐ `Callable`

    +   `new Decoder(config)`

    +   *实例*

        +   `.added_tokens`：`数组.<AddedToken>`

        +   `._call(tokens)` ⇒ `字符串`

        +   `.decode(tokens)` ⇒ `字符串`

        +   `.decode_chain(tokens)` ⇒ `数组.<字符串>`

    +   *静态*

        +   `.fromConfig(config)` ⇒ `解码器`

* * *

### 新的 Decoder(config)

创建`Decoder`的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `对象` | 配置对象。 |

* * *

### decoder.added_tokens： <code>数组.<AddedToken></code>

**种类**：`Decoder`的实例属性

* * *

### decoder._call(tokens) ⇒ <code>字符串</code>

调用`decode`方法。

**种类**：`Decoder`的实例方法

**返回**：`字符串` - 解码后的字符串。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array.<字符串>` | 标记列表。 |

* * *

### decoder.decode(tokens) ⇒ <code>字符串</code>

解码一组标记。

**种类**：`Decoder`的实例方法

**返回**：`字符串` - 解码后的字符串。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array.<字符串>` | 标记列表。 |

* * *

### decoder.decode_chain(tokens) ⇒ <code>数组.<字符串></code>

将解码器应用于一组标记。

**种类**：`Decoder`的实例方法

**返回**：`数组.<字符串>` - 解码后的标记列表。

**抛出**：

+   `错误` 如果子类中未实现`decode_chain`方法。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array.<字符串>` | 标记列表。 |

* * *

### Decoder.fromConfig(config) ⇒ <code>解码器</code>

根据提供的配置创建一个解码器实例。

**种类**：`Decoder`的静态方法

**返回**：`Decoder` - 一个解码器实例。

**抛出**：

+   `错误` 如果提供了未知的解码器类型。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `对象` | 配置对象。 |

* * *

## 分词器~FuseDecoder

Fuse 简单地将所有标记融合成一个大字符串。通常这是最后的解码步骤，但如果在该步骤之后需要进行一些解码器，则存在这个解码器

**种类**：`tokenizers`的内部类

* * *

### fuseDecoder.decode_chain()： <code>*</code>

**种类**：`FuseDecoder`的实例方法

* * *

## tokenizers~WordPieceDecoder ⇐ <code>解码器</code>

一个解码器，将一组 WordPiece 标记解码为一个字符串。

**种类**：`tokenizers`的内部类

**扩展**：`Decoder`

+   ~WordPieceDecoder ⇐ `Decoder`

    +   `new WordPieceDecoder(config)`

    +   `.decode_chain()`：`*`

* * *

### 新的 WordPieceDecoder(config)

创建一个新的 WordPieceDecoder 实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `对象` | 配置对象。 |
| config.prefix | `字符串` | 用于 WordPiece 编码的前缀。 |
| config.cleanup | `布尔值` | 是否清理解码后的字符串。 |

* * *

### wordPieceDecoder.decode_chain()： <code>*</code>

**种类**：`WordPieceDecoder`的实例方法

* * *

## tokenizers~ByteLevelDecoder ⇐ <code>解码器</code>

用于标记化输出的字节级解码器。继承自`Decoder`类。

**类型**：`tokenizers`的内部类

**扩展**：`Decoder`

+   ~ByteLevelDecoder ⇐ `Decoder`

    +   `new ByteLevelDecoder(config)`

    +   `.convert_tokens_to_string(tokens)` ⇒ `string`

    +   `.decode_chain()`：`*`

* * *

### new ByteLevelDecoder(config)

创建一个`ByteLevelDecoder`对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 配置对象。 |

* * *

### byteLevelDecoder.convert_tokens_to_string(tokens) ⇒ <code> string </code>

通过解码每个字节将标记数组转换为字符串。

**类型**：`ByteLevelDecoder`的实例方法

**返回**：`string` - 解码后的字符串。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array.<string>` | 要解码的标记数组。 |

* * *

### byteLevelDecoder.decode_chain()： <code> * </code>

**类型**：`ByteLevelDecoder`的实例方法

* * *

## tokenizers~CTCDecoder

CTC（Connectionist Temporal Classification）解码器。查看[`github.com/huggingface/tokenizers/blob/bb38f390a61883fc2f29d659af696f428d1cda6b/tokenizers/src/decoders/ctc.rs`](https://github.com/huggingface/tokenizers/blob/bb38f390a61883fc2f29d659af696f428d1cda6b/tokenizers/src/decoders/ctc.rs)

**类型**：`tokenizers`的内部类

+   ~CTCDecoder

    +   `.convert_tokens_to_string(tokens)` ⇒ `string`

    +   `.decode_chain()`：`*`

* * *

### ctcDecoder.convert_tokens_to_string(tokens) ⇒ <code> string </code>

将连接主时间分类（CTC）输出标记转换为单个字符串。

**类型**：`CTCDecoder`的实例方法

**返回**：`string` - 解码后的字符串。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array.<string>` | 要解码的标记数组。 |

* * *

### ctcDecoder.decode_chain()： <code> * </code>

**类型**：`CTCDecoder`的实例方法

* * *

## tokenizers~DecoderSequence ⇐ <code> Decoder </code>

应用一系列解码器。

**类型**：`tokenizers`的内部类

**扩展**：`Decoder`

+   ~DecoderSequence ⇐ `Decoder`

    +   `new DecoderSequence(config)`

    +   `.decode_chain()`：`*`

* * *

### new DecoderSequence(config)

创建一个新的 DecoderSequence 实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 配置对象。 |
| config.decoders | `Array.<Decoder>` | 要应用的解码器列表。 |

* * *

### decoderSequence.decode_chain()： <code> * </code>

**类型**：`DecoderSequence`的实例方法

* * *

## tokenizers~MetaspacePreTokenizer ⇐ <code> PreTokenizer </code>

这个 PreTokenizer 将空格替换为给定的替换字符，如果需要则添加前缀空格，并返回一个标记列表。

**类型**：`tokenizers`的内部类

**扩展**：`PreTokenizer`

+   ~MetaspacePreTokenizer ⇐ `PreTokenizer`

    +   `new MetaspacePreTokenizer(config)`

    +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..MetaspacePreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

* * *

### new MetaspacePreTokenizer(config)

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| config | `Object` |  | MetaspacePreTokenizer 的配置对象。 |
| config.add_prefix_space | `boolean` |  | 是否在第一个标记前添加前缀空格。 |
| config.replacement | `string` |  | 用来替换空格的字符。 |
| [config.str_rep] | `string` | `"config.replacement"` | 替换字符的可选字符串表示。 |
| [config.prepend_scheme] | `'first'` &#124; `'never'` &#124; `'always'` | `'always'` | metaspace 前置方案。 |

* * *

### metaspacePreTokenizer.pre_tokenize_text(text, [options]) ⇒ <code> Array. < string > </code>

该方法接受一个字符串，用替换字符替换空格，如果需要则添加前缀空格，并返回一个新的标记列表。

**种类**：`MetaspacePreTokenizer`的实例方法

**返回**：`Array.<string>` - 一个新的预标记化的标记列表。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要预标记的文本。 |
| [options] | `Object` | 预标记化的选项。 |
| [options.section_index] | `number` | 要预标记的部分的索引。 |

* * *

## tokenizers~MetaspaceDecoder ⇐ <code> Decoder </code>

MetaspaceDecoder 类扩展了 Decoder 类并解码 Metaspace 标记化。

**种类**：`tokenizers`的内部类

**扩展自**：`Decoder`

+   ~MetaspaceDecoder ⇐ `Decoder`

    +   `new MetaspaceDecoder(config)`

    +   `.decode_chain()` : `*`

* * *

### new MetaspaceDecoder(config)

构造一个新的 MetaspaceDecoder 对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | MetaspaceDecoder 的配置对象。 |
| config.add_prefix_space | `boolean` | 是否在解码后的字符串前添加前缀空格。 |
| config.replacement | `string` | 用来替换空格的字符串。 |

* * *

### metaspaceDecoder.decode_chain() : <code> * </code>

**种类**：`MetaspaceDecoder`的实例方法

* * *

## tokenizers~Precompiled ⇐ <code> Normalizer </code>

一个应用预编译 charsmap 的规范化器。这对于在 C++中应用复杂的规范化并将其暴露给 JavaScript 非常有用。

**种类**：`tokenizers`的内部类

**扩展自**：`Normalizer`

+   ~Precompiled ⇐ `Normalizer`

    +   `new Precompiled(config)`

    +   `.normalize(text)` ⇒ `string`

* * *

### new Precompiled(config)

创建一个新的 Precompiled 规范化器的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 预编译规范化器的配置对象。 |
| config.precompiled_charsmap | `Object` | 预编译的 charsmap 对象。 |

* * *

### precompiled.normalize(text) ⇒ <code> string </code>

通过应用预编译的 charsmap 对给定文本进行规范化。

**种类**：`Precompiled`的实例方法

**返回**：`string` - 规范化后的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| text | `string` | 要规范化的文本。 |

* * *

## tokenizers~PreTokenizerSequence ⇐ <code> PreTokenizer </code>

一个预标记器，将一系列预标记器应用于输入文本。

**种类**：`tokenizers`的内部类

**扩展自**：`PreTokenizer`

+   ~PreTokenizerSequence ⇐ `PreTokenizer`

    +   `new PreTokenizerSequence(config)`

    +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..PreTokenizerSequence+pre_tokenize_text) ⇒ `Array.<string>`

* * *

### new PreTokenizerSequence(config)

创建一个 PreTokenizerSequence 的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 预标记序列的配置对象。 |
| config.pretokenizers | `Array.<Object>` | 预标记器配置的数组。 |

* * *

### preTokenizerSequence.pre_tokenize_text(text, [options]) ⇒ <code> Array. < string > </code>

将序列中的每个预分词器依次应用于输入文本。

**种类**：`PreTokenizerSequence`的实例方法

**返回**：`Array.<string>` - 预分词文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `字符串` | 要进行预分词的文本。 |
| [选项] | `对象` | 预分词逻辑的其他选项。 |

* * *

## tokenizers〜WhitespacePreTokenizer

通过单词边界拆分（使用以下正则表达式：`\w+|[^\w\s]+`）。

**种类**：`tokenizers`的内部类

+   〜WhitespacePreTokenizer

    +   `new WhitespacePreTokenizer(config)`

    +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..WhitespacePreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

* * *

### 新的 WhitespacePreTokenizer(config)

创建 WhitespacePreTokenizer 的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `对象` | 预分词器的配置对象。 |

* * *

### 空白字符预分词器。预分词文本（文本，[选项]）⇒ <code>数组。<字符串></code>

通过单词边界拆分输入文本的预分词。

**种类**：`WhitespacePreTokenizer`的实例方法

**返回**：`Array.<string>` - 通过在空白字符上拆分输入文本生成的令牌数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `字符串` | 要进行预分词的文本。 |
| [选项] | `对象` | 预分词逻辑的其他选项。 |

* * *

## tokenizers〜WhitespaceSplit ⇐ <code>预分词器</code>

通过空白字符将文本字符串拆分为单独的令牌。

**种类**：`tokenizers`的内部类

**扩展**：`预分词器`

+   〜WhitespaceSplit ⇐ `PreTokenizer`

    +   `new WhitespaceSplit(config)`

    +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..WhitespaceSplit+pre_tokenize_text) ⇒ `Array.<string>`

* * *

### 新的 WhitespaceSplit(config)

创建 WhitespaceSplit 的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `对象` | 预分词器的配置对象。 |

* * *

### whitespaceSplit。pre_tokenize_text（文本，[选项]）⇒ <code>数组。<字符串></code>

通过在空白字符上拆分输入文本来预分词。

**种类**：`WhitespaceSplit`的实例方法

**返回**：`Array.<string>` - 通过在空白字符上拆分输入文本生成的令牌数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `字符串` | 要进行预分词的文本。 |
| [选项] | `对象` | 预分词逻辑的其他选项。 |

* * *

## tokenizers〜ReplacePreTokenizer

**种类**：`tokenizers`的内部类

+   〜ReplacePreTokenizer

    +   `new ReplacePreTokenizer(config)`

    +   [`.pre_tokenize_text(text, [options])`](#module_tokenizers..ReplacePreTokenizer+pre_tokenize_text) ⇒ `Array.<string>`

* * *

### 新的 ReplacePreTokenizer(config)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `对象` | 预分词器的配置选项。 |
| 配置。模式 | `对象` | 用于拆分文本的模式。可以是字符串或正则表达式对象。 |
| 配置内容 | `字符串` | 用于替换模式的内容。 |

* * *

### replacePreTokenizer。pre_tokenize_text（文本，[选项]）⇒ <code>数组。<字符串></code>

通过替换某些字符对输入文本进行预分词。

**种类**：`ReplacePreTokenizer`的实例方法

**返回**：`Array.<string>` - 通过替换某些字符生成的令牌数组。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `字符串` | 要进行预分词的文本。 |
| [选项] | `对象` | 预分词逻辑的附加选项。 |

* * *

## tokenizers~BYTES_TO_UNICODE ⇒ <code> 对象 </code>

返回 utf-8 字节列表和到 unicode 字符串的映射。特别避免映射到 BPE 代码无法处理的空格/控制字符。

**种类**: `tokenizers`的内部常量

**返回**: `对象` - 具有 utf-8 字节键和 unicode 字符串值的对象。

* * *

## tokenizers~loadTokenizer(pretrained_model_name_or_path, options) ⇒ <code> Promise. < 数组 < 任意 > > </code>

从指定路径加载分词器。

**种类**: `tokenizers`的内部方法

**返回**: `Promise.<数组<任意>>` - 一个解析为有关加载的分词器信息的承诺。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| pretrained_model_name_or_path | `字符串` | 分词器目录的路径。 |
| 选项 | `PretrainedTokenizerOptions` | 加载分词器的附加选项。 |

* * *

## tokenizers~regexSplit(text, regex) ⇒ <code> 数组. < 字符串 > </code>

将字符串根据正则表达式拆分，但保留分隔符的辅助函数。这是必需的，因为 JavaScript 的`.split()`方法不会保留分隔符，并且将其包装在捕获组中会导致现有捕获组出现问题（由于嵌套）。

**种类**: `tokenizers`的内部方法

**返回**: `Array.<字符串>` - 拆分的字符串。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `字符串` | 要拆分的文本。 |
| 正则表达式 | `正则表达式` | 要拆分的正则表达式。 |

* * *

## tokenizers~createPattern(pattern, invert) ⇒ <code> 正则表达式 </code> | <code> null </code>

从配置对象构造模式的辅助方法。

**种类**: `tokenizers`的内部方法

**返回**: `正则表达式` | `null` - 编译后的模式。

| 参数 | 类型 | 默认 | 描述 |
| --- | --- | --- | --- |
| 模式 | `对象` |  | 模式对象。 |
| 反转 | `布尔值` | `true` | 是否反转模式。 |

* * *

## tokenizers~objectToMap(obj) ⇒ <code> 映射. < 字符串, 任意 > </code>

将对象转换为映射的辅助函数

**种类**: `tokenizers`的内部方法

**返回**: `映射.<字符串, 任意>` - 映射。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 对象 | `对象` | 要转换的对象。 |

* * *

## tokenizers~prepareTensorForDecode(tensor) ⇒ <code> 数组. < 数字 > </code>

将张量转换为解码前的列表的辅助函数。

**种类**: `tokenizers`的内部方法

**返回**: `Array.<数字>` - 作为列表的张量。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 张量 | `张量` | 要转换的张量。 |

* * *

## tokenizers~clean_up_tokenization(text) ⇒ <code> 字符串 </code>

清理简单英语分词工件列表，如标点符号前的空格和缩写形式

**种类**: `tokenizers`的内部方法

**返回**: `字符串` - 清理后的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `字符串` | 要清理的文本。 |

* * *

## tokenizers~remove_accents(text) ⇒ <code> 字符串 </code>

从字符串中去除重音符号的辅助函数。

**种类**: `tokenizers`的内部方法

**返回**: `字符串` - 去除重音符号的文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `字符串` | 要去除重音符号的文本。 |

* * *

## tokenizers~lowercase_and_remove_accent(text) ⇒ <code> 字符串 </code>

将字符串小写并去除重音符号的辅助函数。

**种类**: `tokenizers`的内部方法

**返回**: `字符串` - 去除重音符号的小写文本。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `字符串` | 要小写并去除重音符号的文本。 |

* * *

## tokenizers~fuse(arr, value, mapping)

将数组中连续值等于指定值的值合并的辅助函数。

**种类**: `tokenizers`的内部方法

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| arr | `Array.<string>` | 输入数组 |
| 值 | `any` | 融合的值。 |
| 映射 | `Map.<string, any>` | 从输入域到值的映射。 |

* * *

## tokenizers~whitespace_split(text) ⇒ <code> Array. < string > </code>

在空格上拆分字符串。

**种类**：`tokenizers`的内部方法

**返回**：`Array.<string>` - 拆分的字符串。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 文本 | `string` | 要拆分的文本。 |

* * *

## tokenizers~PretrainedTokenizerOptions : <code> Object </code>

额外的特定于分词器的属性。

**种类**：`tokenizers`的内部 typedef

**属性**

| 名称 | 类型 | 默认 | 描述 |
| --- | --- | --- | --- |
| [legacy] | `boolean` | `false` | 是否应使用分词器的`legacy`行为。 |

* * *

## tokenizers~BPENode : <code> Object </code>

**种类**：`tokenizers`的内部 typedef

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| token | `string` | 与节点关联的令牌 |
| 偏差 | `number` | 节点的位置偏差。 |
| [score] | `number` | 节点的分数。 |
| [prev] | `BPENode` | 链表中的前一个节点。 |
| [next] | `BPENode` | 链表中的下一个节点。 |

* * *

## tokenizers~SplitDelimiterBehavior : <code> ’ removed ’ </code> | <code> ’ isolated ’ </code> | <code> ’ mergedWithPrevious ’ </code> | <code> ’ mergedWithNext ’ </code> | <code> ’ contiguous ’ </code>

**种类**：`tokenizers`的内部 typedef

* * *

## tokenizers~PostProcessedOutput : <code> Object </code>

**种类**：`tokenizers`的内部 typedef

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| tokens | `Array.<string>` | 后处理器生成的令牌列表。 |
| [token_type_ids] | `Array.<number>` | 后处理器生成的令牌类型 id 列表。 |

* * *

## tokenizers~EncodingSingle : <code> Object </code>

**种类**：`tokenizers`的内部 typedef

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| input_ids | `Array.<number>` | 要提供给模型的令牌 id 列表。 |
| attention_mask | `Array.<number>` | 要提供给模型的令牌类型 id 列表 |
| [token_type_ids] | `Array.<number>` | 指定哪些令牌应该被模型关注的索引列表 |

* * *

## tokenizers~BatchEncoding : <code> Array < number > </code> | <code> Array < Array < number > > </code> | <code> Tensor </code>

保存分词器调用函数的输出。

**种类**：`tokenizers`的内部 typedef

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| input_ids | `BatchEncodingItem` | 要提供给模型的令牌 id 列表。 |
| attention_mask | `BatchEncodingItem` | 指定模型应关注的令牌的索引列表。 |
| [token_type_ids] | `BatchEncodingItem` | 要提供给模型的令牌类型 id 列表。 |

* * *

## tokenizers~Message : <code> Object </code>

**种类**：`tokenizers`的内部 typedef

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| 角色 | `string` | 消息的角色（例如，“用户”或“助手”或“系统”）。 |
| 内容 | `string` | 消息的内容。 |

* * *
