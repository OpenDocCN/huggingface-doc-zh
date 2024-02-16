# 模型

> 原始文本：[https://huggingface.co/docs/transformers.js/api/models](https://huggingface.co/docs/transformers.js/api/models)

Transformers.js 中所有可用模型的定义。

**示例：** 加载并运行一个 `AutoModel`。

```py
import { AutoModel, AutoTokenizer } from '@xenova/transformers';

let tokenizer = await AutoTokenizer.from_pretrained('Xenova/bert-base-uncased');
let model = await AutoModel.from_pretrained('Xenova/bert-base-uncased');

let inputs = await tokenizer('I love transformers!');
let { logits } = await model(inputs);
// Tensor {
//     data: Float32Array(183132) [-7.117443084716797, -7.107812881469727, -7.092104911804199, ...]
//     dims: (3) [1, 6, 30522],
//     type: "float32",
//     size: 183132,
// }
```

我们还提供其他 `AutoModel`（如下所列），您可以像使用 Python 库一样使用它们。例如：

**示例：** 加载并运行一个 `AutoModelForSeq2SeqLM`。

```py
import { AutoModelForSeq2SeqLM, AutoTokenizer } from '@xenova/transformers';

let tokenizer = await AutoTokenizer.from_pretrained('Xenova/t5-small');
let model = await AutoModelForSeq2SeqLM.from_pretrained('Xenova/t5-small');

let { input_ids } = await tokenizer('translate English to German: I love transformers!');
let outputs = await model.generate(input_ids);
let decoded = tokenizer.decode(outputs[0], { skip_special_tokens: true });
// 'Ich liebe Transformatoren!'
```

+   模型

    +   *static*

        +   [.PreTrainedModel](#module_models.PreTrainedModel)

            +   [`new PreTrainedModel(config, session)`](#new_module_models.PreTrainedModel_new)

            +   *instance*

                +   [`.dispose()`](#module_models.PreTrainedModel+dispose) ⇒ `Promise.<Array<unknown>>`

                +   [`._call(model_inputs)`](#module_models.PreTrainedModel+_call) ⇒ `Promise.<Object>`

                +   [`.forward(model_inputs)`](#module_models.PreTrainedModel+forward) ⇒ `Promise.<Object>`

                +   [`._get_generation_config(generation_config)`](#module_models.PreTrainedModel+_get_generation_config) ⇒ `*`

                +   [`.groupBeams(beams)`](#module_models.PreTrainedModel+groupBeams) ⇒ `Array`

                +   [`.getPastKeyValues(decoderResults, pastKeyValues)`](#module_models.PreTrainedModel+getPastKeyValues) ⇒ `Object`

                +   [`.getAttentions(decoderResults)`](#module_models.PreTrainedModel+getAttentions) ⇒ `Object`

                +   [`.addPastKeyValues(decoderFeeds, pastKeyValues)`](#module_models.PreTrainedModel+addPastKeyValues)

            +   *static*

                +   [`.from_pretrained(pretrained_model_name_or_path, options)`](#module_models.PreTrainedModel.from_pretrained) ⇒ `Promise.<PreTrainedModel>`

        +   [.BaseModelOutput](#module_models.BaseModelOutput)

            +   [`new BaseModelOutput(output)`](#new_module_models.BaseModelOutput_new)

        +   [.BertForMaskedLM](#module_models.BertForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.BertForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.BertForSequenceClassification](#module_models.BertForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.BertForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.BertForTokenClassification](#module_models.BertForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.BertForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.BertForQuestionAnswering](#module_models.BertForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.BertForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.RoFormerModel](#module_models.RoFormerModel)

        +   [.RoFormerForMaskedLM](#module_models.RoFormerForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.RoFormerForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.RoFormerForSequenceClassification](#module_models.RoFormerForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.RoFormerForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.RoFormerForTokenClassification](#module_models.RoFormerForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.RoFormerForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.RoFormerForQuestionAnswering](#module_models.RoFormerForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.RoFormerForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.ConvBertModel](#module_models.ConvBertModel)

        +   [.ConvBertForMaskedLM](#module_models.ConvBertForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.ConvBertForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.ConvBertForSequenceClassification](#module_models.ConvBertForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.ConvBertForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.ConvBertForTokenClassification](#module_models.ConvBertForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.ConvBertForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.ConvBertForQuestionAnswering](#module_models.ConvBertForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.ConvBertForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.ElectraModel](#module_models.ElectraModel)

        +   [.ElectraForMaskedLM](#module_models.ElectraForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.ElectraForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.ElectraForSequenceClassification](#module_models.ElectraForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.ElectraForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.ElectraForTokenClassification](#module_models.ElectraForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.ElectraForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.ElectraForQuestionAnswering](#module_models.ElectraForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.ElectraForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.CamembertModel](#module_models.CamembertModel)

        +   [.CamembertForMaskedLM](#module_models.CamembertForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.CamembertForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.CamembertForSequenceClassification](#module_models.CamembertForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.CamembertForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.CamembertForTokenClassification](#module_models.CamembertForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.CamembertForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.CamembertForQuestionAnswering](#module_models.CamembertForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.CamembertForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.DebertaModel](#module_models.DebertaModel)

        +   [.DebertaForMaskedLM](#module_models.DebertaForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.DebertaForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.DebertaForSequenceClassification](#module_models.DebertaForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.DebertaForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.DebertaForTokenClassification](#module_models.DebertaForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.DebertaForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.DebertaForQuestionAnswering](#module_models.DebertaForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.DebertaForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.DebertaV2Model](#module_models.DebertaV2Model)

        +   [.DebertaV2ForMaskedLM](#module_models.DebertaV2ForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.DebertaV2ForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.DebertaV2ForSequenceClassification](#module_models.DebertaV2ForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.DebertaV2ForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.DebertaV2ForTokenClassification](#module_models.DebertaV2ForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.DebertaV2ForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.DebertaV2ForQuestionAnswering](#module_models.DebertaV2ForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.DebertaV2ForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.DistilBertForSequenceClassification](#module_models.DistilBertForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.DistilBertForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.DistilBertForTokenClassification](#module_models.DistilBertForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.DistilBertForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.DistilBertForQuestionAnswering](#module_models.DistilBertForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.DistilBertForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.DistilBertForMaskedLM](#module_models.DistilBertForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.DistilBertForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.EsmModel](#module_models.EsmModel)

        +   [.EsmForMaskedLM](#module_models.EsmForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.EsmForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.EsmForSequenceClassification](#module_models.EsmForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.EsmForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.EsmForTokenClassification](#module_models.EsmForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.EsmForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.MobileBertForMaskedLM](#module_models.MobileBertForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.MobileBertForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.MobileBertForSequenceClassification](#module_models.MobileBertForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.MobileBertForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.MobileBertForQuestionAnswering](#module_models.MobileBertForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.MobileBertForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.MPNetModel](#module_models.MPNetModel)

        +   [.MPNetForMaskedLM](#module_models.MPNetForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.MPNetForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.MPNetForSequenceClassification](#module_models.MPNetForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.MPNetForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.MPNetForTokenClassification](#module_models.MPNetForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.MPNetForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.MPNetForQuestionAnswering](#module_models.MPNetForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.MPNetForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.T5ForConditionalGeneration](#module_models.T5ForConditionalGeneration)

            +   [`new T5ForConditionalGeneration(config, session, decoder_merged_session, generation_config)`](#new_module_models.T5ForConditionalGeneration_new)

        +   [.LongT5PreTrainedModel](#module_models.LongT5PreTrainedModel)

        +   [.LongT5Model](#module_models.LongT5Model)

        +   [.LongT5ForConditionalGeneration](#module_models.LongT5ForConditionalGeneration)

            +   [`new LongT5ForConditionalGeneration(config, session, decoder_merged_session, generation_config)`](#new_module_models.LongT5ForConditionalGeneration_new)

        +   [.MT5ForConditionalGeneration](#module_models.MT5ForConditionalGeneration)

            +   [`new MT5ForConditionalGeneration(config, session, decoder_merged_session, generation_config)`](#new_module_models.MT5ForConditionalGeneration_new)

        +   [.BartModel](#module_models.BartModel)

        +   [.BartForConditionalGeneration](#module_models.BartForConditionalGeneration)

            +   [`new BartForConditionalGeneration(config, session, decoder_merged_session, generation_config)`](#new_module_models.BartForConditionalGeneration_new)

        +   [.BartForSequenceClassification](#module_models.BartForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.BartForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.MBartModel](#module_models.MBartModel)

        +   [.MBartForConditionalGeneration](#module_models.MBartForConditionalGeneration)

            +   [`new MBartForConditionalGeneration(config, session, decoder_merged_session, generation_config)`](#new_module_models.MBartForConditionalGeneration_new)

        +   [.MBartForSequenceClassification](#module_models.MBartForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.MBartForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.MBartForCausalLM](#module_models.MBartForCausalLM)

            +   [`new MBartForCausalLM(config, decoder_merged_session, generation_config)`](#new_module_models.MBartForCausalLM_new)

        +   [.BlenderbotModel](#module_models.BlenderbotModel)

        +   [.BlenderbotForConditionalGeneration](#module_models.BlenderbotForConditionalGeneration)

            +   [`new BlenderbotForConditionalGeneration(config, session, decoder_merged_session, generation_config)`](#new_module_models.BlenderbotForConditionalGeneration_new)

        +   [.BlenderbotSmallModel](#module_models.BlenderbotSmallModel)

        +   [.BlenderbotSmallForConditionalGeneration](#module_models.BlenderbotSmallForConditionalGeneration)

            +   [`new BlenderbotSmallForConditionalGeneration(config, session, decoder_merged_session, generation_config)`](#new_module_models.BlenderbotSmallForConditionalGeneration_new)

        +   [.RobertaForMaskedLM](#module_models.RobertaForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.RobertaForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.RobertaForSequenceClassification](#module_models.RobertaForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.RobertaForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.RobertaForTokenClassification](#module_models.RobertaForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.RobertaForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.RobertaForQuestionAnswering](#module_models.RobertaForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.RobertaForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.XLMPreTrainedModel](#module_models.XLMPreTrainedModel)

        +   [.XLMModel](#module_models.XLMModel)

        +   [.XLMWithLMHeadModel](#module_models.XLMWithLMHeadModel)

            +   [`._call(model_inputs)`](#module_models.XLMWithLMHeadModel+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.XLMForSequenceClassification](#module_models.XLMForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.XLMForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.XLMForTokenClassification](#module_models.XLMForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.XLMForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.XLMForQuestionAnswering](#module_models.XLMForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.XLMForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.XLMRobertaForMaskedLM](#module_models.XLMRobertaForMaskedLM)

            +   [`._call(model_inputs)`](#module_models.XLMRobertaForMaskedLM+_call) ⇒ `Promise.<MaskedLMOutput>`

        +   [.XLMRobertaForSequenceClassification](#module_models.XLMRobertaForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.XLMRobertaForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.XLMRobertaForTokenClassification](#module_models.XLMRobertaForTokenClassification)

            +   [`._call(model_inputs)`](#module_models.XLMRobertaForTokenClassification+_call) ⇒ `Promise.<TokenClassifierOutput>`

        +   [.XLMRobertaForQuestionAnswering](#module_models.XLMRobertaForQuestionAnswering)

            +   [`._call(model_inputs)`](#module_models.XLMRobertaForQuestionAnswering+_call) ⇒ `Promise.<QuestionAnsweringModelOutput>`

        +   [.ASTModel](#module_models.ASTModel)

        +   [.ASTForAudioClassification](#module_models.ASTForAudioClassification)

        +   [.WhisperModel](#module_models.WhisperModel)

        +   [.WhisperForConditionalGeneration](#module_models.WhisperForConditionalGeneration)

            +   [`new WhisperForConditionalGeneration(config, session, decoder_merged_session, generation_config)`](#new_module_models.WhisperForConditionalGeneration_new)

            +   [`.generate(inputs, generation_config, logits_processor)`](#module_models.WhisperForConditionalGeneration+generate) ⇒ `Promise.<Object>`

            +   [`._extract_token_timestamps(generate_outputs, alignment_heads, [num_frames], [time_precision])`](#module_models.WhisperForConditionalGeneration+_extract_token_timestamps) ⇒ `Tensor`

        +   [.VisionEncoderDecoderModel](#module_models.VisionEncoderDecoderModel)

            +   [`new VisionEncoderDecoderModel(config, session, decoder_merged_session, generation_config)`](#new_module_models.VisionEncoderDecoderModel_new)

        +   [.CLIPModel](#module_models.CLIPModel)

        +   [.CLIPTextModelWithProjection](#module_models.CLIPTextModelWithProjection)

            +   [`.from_pretrained()`](#module_models.CLIPTextModelWithProjection.from_pretrained) : `PreTrainedModel.from_pretrained`

        +   [.CLIPVisionModelWithProjection](#module_models.CLIPVisionModelWithProjection)

            +   [`.from_pretrained()`](#module_models.CLIPVisionModelWithProjection.from_pretrained) : `PreTrainedModel.from_pretrained`

        +   [.SiglipModel](#module_models.SiglipModel)

        +   [.SiglipTextModel](#module_models.SiglipTextModel)

            +   [`.from_pretrained()`](#module_models.SiglipTextModel.from_pretrained) : `PreTrainedModel.from_pretrained`

        +   [.SiglipVisionModel](#module_models.SiglipVisionModel)

            +   [`.from_pretrained()`](#module_models.SiglipVisionModel.from_pretrained) : `PreTrainedModel.from_pretrained`

        +   [.CLIPSegForImageSegmentation](#module_models.CLIPSegForImageSegmentation)

        +   [.GPT2PreTrainedModel](#module_models.GPT2PreTrainedModel)

            +   [`new GPT2PreTrainedModel(config, session, generation_config)`](#new_module_models.GPT2PreTrainedModel_new)

        +   [.GPT2LMHeadModel](#module_models.GPT2LMHeadModel)

        +   [.GPTNeoPreTrainedModel](#module_models.GPTNeoPreTrainedModel)

            +   [`new GPTNeoPreTrainedModel(config, session, generation_config)`](#new_module_models.GPTNeoPreTrainedModel_new)

        +   [.GPTNeoXPreTrainedModel](#module_models.GPTNeoXPreTrainedModel)

            +   [`new GPTNeoXPreTrainedModel(config, session, generation_config)`](#new_module_models.GPTNeoXPreTrainedModel_new)

        +   [.GPTJPreTrainedModel](#module_models.GPTJPreTrainedModel)

            +   [`new GPTJPreTrainedModel(config, session, generation_config)`](#new_module_models.GPTJPreTrainedModel_new)

        +   [.GPTBigCodePreTrainedModel](#module_models.GPTBigCodePreTrainedModel)

            +   [`new GPTBigCodePreTrainedModel(config, session, generation_config)`](#new_module_models.GPTBigCodePreTrainedModel_new)

        +   [.CodeGenPreTrainedModel](#module_models.CodeGenPreTrainedModel)

            +   [`new CodeGenPreTrainedModel(config, session, generation_config)`](#new_module_models.CodeGenPreTrainedModel_new)

        +   [.CodeGenModel](#module_models.CodeGenModel)

        +   [.CodeGenForCausalLM](#module_models.CodeGenForCausalLM)

        +   [.LlamaPreTrainedModel](#module_models.LlamaPreTrainedModel)

            +   [`new LlamaPreTrainedModel(config, session, generation_config)`](#new_module_models.LlamaPreTrainedModel_new)

        +   [.LlamaModel](#module_models.LlamaModel)

        +   [.Qwen2PreTrainedModel](#module_models.Qwen2PreTrainedModel)

            +   [`new Qwen2PreTrainedModel(config, session, generation_config)`](#new_module_models.Qwen2PreTrainedModel_new)

        +   [.Qwen2Model](#module_models.Qwen2Model)

        +   [.PhiPreTrainedModel](#module_models.PhiPreTrainedModel)

            +   [`new PhiPreTrainedModel(config, session, generation_config)`](#new_module_models.PhiPreTrainedModel_new)

        +   [.PhiModel](#module_models.PhiModel)

        +   [.BloomPreTrainedModel](#module_models.BloomPreTrainedModel)

            +   [`new BloomPreTrainedModel(config, session, generation_config)`](#new_module_models.BloomPreTrainedModel_new)

        +   [.BloomModel](#module_models.BloomModel)

        +   [.BloomForCausalLM](#module_models.BloomForCausalLM)

        +   [.MptPreTrainedModel](#module_models.MptPreTrainedModel)

            +   [`new MptPreTrainedModel(config, session, generation_config)`](#new_module_models.MptPreTrainedModel_new)

        +   [.MptModel](#module_models.MptModel)

        +   [.MptForCausalLM](#module_models.MptForCausalLM)

        +   [.OPTPreTrainedModel](#module_models.OPTPreTrainedModel)

            +   [`new OPTPreTrainedModel(config, session, generation_config)`](#new_module_models.OPTPreTrainedModel_new)

        +   [.OPTModel](#module_models.OPTModel)

        +   [.OPTForCausalLM](#module_models.OPTForCausalLM)

        +   [.VitMatteForImageMatting](#module_models.VitMatteForImageMatting)

            +   [`._call(model_inputs)`](#module_models.VitMatteForImageMatting+_call)

        +   [.DetrObjectDetectionOutput](#module_models.DetrObjectDetectionOutput)

            +   [`new DetrObjectDetectionOutput(output)`](#new_module_models.DetrObjectDetectionOutput_new)

        +   [.DetrSegmentationOutput](#module_models.DetrSegmentationOutput)

            +   [`new DetrSegmentationOutput(output)`](#new_module_models.DetrSegmentationOutput_new)

        +   [.TableTransformerModel](#module_models.TableTransformerModel)

        +   [.TableTransformerForObjectDetection](#module_models.TableTransformerForObjectDetection)

            +   [`._call(model_inputs)`](#module_models.TableTransformerForObjectDetection+_call)

        +   [.ResNetPreTrainedModel](#module_models.ResNetPreTrainedModel)

        +   [.ResNetModel](#module_models.ResNetModel)

        +   [.ResNetForImageClassification](#module_models.ResNetForImageClassification)

            +   [`._call(model_inputs)`](#module_models.ResNetForImageClassification+_call)

        +   [.Swin2SRModel](#module_models.Swin2SRModel)

        +   [.Swin2SRForImageSuperResolution](#module_models.Swin2SRForImageSuperResolution)

        +   [.DPTModel](#module_models.DPTModel)

        +   [.DPTForDepthEstimation](#module_models.DPTForDepthEstimation)

        +   [.DepthAnythingForDepthEstimation](#module_models.DepthAnythingForDepthEstimation)

        +   [.GLPNModel](#module_models.GLPNModel)

        +   [.GLPNForDepthEstimation](#module_models.GLPNForDepthEstimation)

        +   [.DonutSwinModel](#module_models.DonutSwinModel)

        +   [.ConvNextModel](#module_models.ConvNextModel)

        +   [.ConvNextForImageClassification](#module_models.ConvNextForImageClassification)

            +   [`._call(model_inputs)`](#module_models.ConvNextForImageClassification+_call)

        +   [.ConvNextV2Model](#module_models.ConvNextV2Model)

        +   [.ConvNextV2ForImageClassification](#module_models.ConvNextV2ForImageClassification)

            +   [`._call(model_inputs)`](#module_models.ConvNextV2ForImageClassification+_call)

        +   [.Dinov2Model](#module_models.Dinov2Model)

        +   [.Dinov2ForImageClassification](#module_models.Dinov2ForImageClassification)

            +   [`._call(model_inputs)`](#module_models.Dinov2ForImageClassification+_call)

        +   [.YolosObjectDetectionOutput](#module_models.YolosObjectDetectionOutput)

            +   [`new YolosObjectDetectionOutput(output)`](#new_module_models.YolosObjectDetectionOutput_new)

        +   [.SamModel](#module_models.SamModel)

            +   [`new SamModel(config, vision_encoder, prompt_encoder_mask_decoder)`](#new_module_models.SamModel_new)

            +   [`.get_image_embeddings(model_inputs)`](#module_models.SamModel+get_image_embeddings) ⇒ `Promise.<{image_embeddings: Tensor, image_positional_embeddings: Tensor}>`

            +   [`.forward(model_inputs)`](#module_models.SamModel+forward) ⇒ `Promise.<Object>`

            +   [`._call(model_inputs)`](#module_models.SamModel+_call) ⇒ `Promise.<SamImageSegmentationOutput>`

        +   [.SamImageSegmentationOutput](#module_models.SamImageSegmentationOutput)

            +   [`new SamImageSegmentationOutput(output)`](#new_module_models.SamImageSegmentationOutput_new)

        +   [.MarianMTModel](#module_models.MarianMTModel)

            +   [`new MarianMTModel(config, session, decoder_merged_session, generation_config)`](#new_module_models.MarianMTModel_new)

        +   [.M2M100ForConditionalGeneration](#module_models.M2M100ForConditionalGeneration)

            +   [`new M2M100ForConditionalGeneration(config, session, decoder_merged_session, generation_config)`](#new_module_models.M2M100ForConditionalGeneration_new)

        +   [.Wav2Vec2Model](#module_models.Wav2Vec2Model)

        +   [.Wav2Vec2BertModel](#module_models.Wav2Vec2BertModel)

        +   [.Wav2Vec2BertForCTC](#module_models.Wav2Vec2BertForCTC)

            +   [`._call(model_inputs)`](#module_models.Wav2Vec2BertForCTC+_call)

        +   [.Wav2Vec2BertForSequenceClassification](#module_models.Wav2Vec2BertForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.Wav2Vec2BertForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.HubertModel](#module_models.HubertModel)

        +   [.HubertForCTC](#module_models.HubertForCTC)

            +   [`._call(model_inputs)`](#module_models.HubertForCTC+_call)

        +   [.HubertForSequenceClassification](#module_models.HubertForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.HubertForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.WavLMPreTrainedModel](#module_models.WavLMPreTrainedModel)

        +   [.WavLMModel](#module_models.WavLMModel)

        +   [.WavLMForCTC](#module_models.WavLMForCTC)

            +   [`._call(model_inputs)`](#module_models.WavLMForCTC+_call)

        +   [.WavLMForSequenceClassification](#module_models.WavLMForSequenceClassification)

            +   [`._call(model_inputs)`](#module_models.WavLMForSequenceClassification+_call) ⇒ `Promise.<SequenceClassifierOutput>`

        +   [.SpeechT5PreTrainedModel](#module_models.SpeechT5PreTrainedModel)

        +   [.SpeechT5Model](#module_models.SpeechT5Model)

        +   [.SpeechT5ForSpeechToText](#module_models.SpeechT5ForSpeechToText)

        +   [.SpeechT5ForTextToSpeech](#module_models.SpeechT5ForTextToSpeech)

            +   [`new SpeechT5ForTextToSpeech(config, session, decoder_merged_session, generation_config)`](#new_module_models.SpeechT5ForTextToSpeech_new)：新的SpeechT5文本转语音模型  

            +   [`.generate_speech(input_values, speaker_embeddings, options)`](#module_models.SpeechT5ForTextToSpeech+generate_speech) ⇒ `Promise.<SpeechOutput>`  

        +   [.SpeechT5HifiGan](#module_models.SpeechT5HifiGan)：SpeechT5HifiGan  

        +   [.TrOCRPreTrainedModel](#module_models.TrOCRPreTrainedModel)：TrOCR预训练模型  

            +   [`new TrOCRPreTrainedModel(config, session, generation_config)`](#new_module_models.TrOCRPreTrainedModel_new)：新的TrOCR预训练模型  

        +   [.TrOCRForCausalLM](#module_models.TrOCRForCausalLM)：用于因果LM的TrOCR  

        +   [.MistralPreTrainedModel](#module_models.MistralPreTrainedModel)：Mistral预训练模型  

            +   [`new MistralPreTrainedModel(config, session, generation_config)`](#new_module_models.MistralPreTrainedModel_new)：新的Mistral预训练模型  

        +   [.FalconPreTrainedModel](#module_models.FalconPreTrainedModel)：Falcon预训练模型  

            +   [`new FalconPreTrainedModel(config, session, generation_config)`](#new_module_models.FalconPreTrainedModel_new)：新的Falcon预训练模型  

        +   [.ClapTextModelWithProjection](#module_models.ClapTextModelWithProjection)：带投影的Clap文本模型  

            +   [`.from_pretrained()`](#module_models.ClapTextModelWithProjection.from_pretrained)：`PreTrainedModel.from_pretrained`  

        +   [.ClapAudioModelWithProjection](#module_models.ClapAudioModelWithProjection)：带投影的Clap音频模型  

            +   [`.from_pretrained()`](#module_models.ClapAudioModelWithProjection.from_pretrained)：`PreTrainedModel.from_pretrained`  

        +   [.VitsModel](#module_models.VitsModel)：Vits模型  

            +   [`._call(model_inputs)`](#module_models.VitsModel+_call) ⇒ `Promise.<VitsModelOutput>`  

        +   [.SegformerModel](#module_models.SegformerModel)：Segformer模型  

        +   [.SegformerForImageClassification](#module_models.SegformerForImageClassification)：用于图像分类的Segformer  

        +   [.SegformerForSemanticSegmentation](#module_models.SegformerForSemanticSegmentation)：用于语义分割的Segformer  

        +   [.PretrainedMixin](#module_models.PretrainedMixin)：预训练混合模型  

            +   *instance*  

                +   [`.MODEL_CLASS_MAPPINGS`](#module_models.PretrainedMixin+MODEL_CLASS_MAPPINGS)：`*`  

                +   [`.BASE_IF_FAIL`](#module_models.PretrainedMixin+BASE_IF_FAIL)：`*`  

            +   *static*  

                +   [`.from_pretrained()`](#module_models.PretrainedMixin.from_pretrained)：`PreTrainedModel.from_pretrained`  

        +   [.AutoModel](#module_models.AutoModel)：自动模型  

            +   [`.MODEL_CLASS_MAPPINGS`](#module_models.AutoModel+MODEL_CLASS_MAPPINGS)：`*`  

        +   [.AutoModelForSequenceClassification](#module_models.AutoModelForSequenceClassification)：用于序列分类的自动模型  

        +   [.AutoModelForTokenClassification](#module_models.AutoModelForTokenClassification)：用于标记分类的自动模型  

        +   [.AutoModelForSeq2SeqLM](#module_models.AutoModelForSeq2SeqLM)：用于Seq2SeqLM的自动模型  

        +   [.AutoModelForSpeechSeq2Seq](#module_models.AutoModelForSpeechSeq2Seq)：用于语音Seq2Seq的自动模型  

        +   [.AutoModelForTextToSpectrogram](#module_models.AutoModelForTextToSpectrogram)：用于文本到频谱图的自动模型  

        +   [.AutoModelForTextToWaveform](#module_models.AutoModelForTextToWaveform)：用于文本到波形的自动模型  

        +   [.AutoModelForCausalLM](#module_models.AutoModelForCausalLM)：用于因果LM的自动模型  

        +   [.AutoModelForMaskedLM](#module_models.AutoModelForMaskedLM)：用于MaskedLM的自动模型  

        +   [.AutoModelForQuestionAnswering](#module_models.AutoModelForQuestionAnswering)：用于问答的自动模型  

        +   [.AutoModelForVision2Seq](#module_models.AutoModelForVision2Seq)：用于Vision2Seq的自动模型  

        +   [.AutoModelForImageClassification](#module_models.AutoModelForImageClassification)：用于图像分类的自动模型  

        +   [.AutoModelForImageSegmentation](#module_models.AutoModelForImageSegmentation)：用于图像分割的自动模型  

        +   [.AutoModelForSemanticSegmentation](#module_models.AutoModelForSemanticSegmentation)：用于语义分割的自动模型  

        +   [.AutoModelForObjectDetection](#module_models.AutoModelForObjectDetection)：用于目标检测的自动模型  

        +   [.AutoModelForMaskGeneration](#module_models.AutoModelForMaskGeneration)：用于Mask生成的自动模型  

        +   [.Seq2SeqLMOutput](#module_models.Seq2SeqLMOutput)：Seq2SeqLM输出  

            +   [`new Seq2SeqLMOutput(output)`](#new_module_models.Seq2SeqLMOutput_new)：新的Seq2SeqLM输出  

        +   [.SequenceClassifierOutput](#module_models.SequenceClassifierOutput)：序列分类器输出

            +   [`new SequenceClassifierOutput(output)`](#new_module_models.SequenceClassifierOutput_new)：新的序列分类器输出  

        +   [.TokenClassifierOutput](#module_models.TokenClassifierOutput)：标记分类器输出  

            +   [`new TokenClassifierOutput(output)`](#new_module_models.TokenClassifierOutput_new)：新的标记分类器输出  

        +   [.MaskedLMOutput](#module_models.MaskedLMOutput)：MaskedLM输出  

            +   [`new MaskedLMOutput(output)`](#new_module_models.MaskedLMOutput_new)：新的MaskedLM输出  

        +   [.QuestionAnsweringModelOutput](#module_models.QuestionAnsweringModelOutput)：问答模型输出  

            +   [`new QuestionAnsweringModelOutput(output)`](#new_module_models.QuestionAnsweringModelOutput_new)：新的问答模型输出  

        +   [.CausalLMOutput](#module_models.CausalLMOutput)：因果LM输出  

            +   [`new CausalLMOutput(output)`](#new_module_models.CausalLMOutput_new)

        +   [.CausalLMOutputWithPast](#module_models.CausalLMOutputWithPast)

            +   [`new CausalLMOutputWithPast(output)`](#new_module_models.CausalLMOutputWithPast_new)

        +   [.ImageMattingOutput](#module_models.ImageMattingOutput)

            +   [`new ImageMattingOutput(output)`](#new_module_models.ImageMattingOutput_new)

        +   [.VitsModelOutput](#module_models.VitsModelOutput)

            +   [`new VitsModelOutput(output)`](#new_module_models.VitsModelOutput_new)

    +   *内部*

        +   [`~InferenceSession`](#module_models..InferenceSession) : `*`

        +   [`~TypedArray`](#module_models..TypedArray) : `*`

        +   [`~DecoderOutput`](#module_models..DecoderOutput) ⇒ `Promise.<(Array<Array<number>>|EncoderDecoderOutput|DecoderOutput)>`

        +   [`~WhisperGenerationConfig`](#module_models..WhisperGenerationConfig) : `Object`

        +   [`~SamModelInputs`](#module_models..SamModelInputs) : `Object`

        +   [`~SpeechOutput`](#module_models..SpeechOutput) : `Object`

* * *

## models.PreTrainedModel

提供模型配置和ONNX会话的预训练模型的基类。

**类型**: [`models`](#module_models)的静态类

+   [.PreTrainedModel](#module_models.PreTrainedModel)

    +   [`new PreTrainedModel(config, session)`](#new_module_models.PreTrainedModel_new)

    +   *实例*

        +   [`.dispose()`](#module_models.PreTrainedModel+dispose) ⇒ `Promise.<Array<unknown>>`

        +   [`._call(model_inputs)`](#module_models.PreTrainedModel+_call) ⇒ `Promise.<Object>`

        +   [`.forward(model_inputs)`](#module_models.PreTrainedModel+forward) ⇒ `Promise.<Object>`

        +   [`._get_generation_config(generation_config)`](#module_models.PreTrainedModel+_get_generation_config) ⇒ `*`

        +   [`.groupBeams(beams)`](#module_models.PreTrainedModel+groupBeams) ⇒ `Array`

        +   [`.getPastKeyValues(decoderResults, pastKeyValues)`](#module_models.PreTrainedModel+getPastKeyValues) ⇒ `Object`

        +   [`.getAttentions(decoderResults)`](#module_models.PreTrainedModel+getAttentions) ⇒ `Object`

        +   [`.addPastKeyValues(decoderFeeds, pastKeyValues)`](#module_models.PreTrainedModel+addPastKeyValues)

    +   *静态*

        +   [`.from_pretrained(pretrained_model_name_or_path, options)`](#module_models.PreTrainedModel.from_pretrained) ⇒ `Promise.<PreTrainedModel>`

* * *

### new PreTrainedModel(config, session)

创建`PreTrainedModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 模型配置。 |
| session | `any` | 模型的会话。 |

* * *

### preTrainedModel.dispose() ⇒ <code> Promise. < Array < unknown > > </code>

处理在推断期间创建的所有ONNX会话。

**类型**: [`PreTrainedModel`](#module_models.PreTrainedModel)的实例方法

**返回值**: `Promise.<Array<unknown>>` - 一个包含每个正在处理的ONNX会话的Promise数组。

**待办事项**

+   使用[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/FinalizationRegistry](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/FinalizationRegistry)

* * *

### preTrainedModel._call(model_inputs) ⇒ <code> Promise. < Object > </code>

运行提供的输入的模型

**类型**: [`PreTrainedModel`](#module_models.PreTrainedModel)的实例方法

**返回值**: `Promise.<Object>` - 包含输出张量的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 包含输入张量的对象 |

* * *

### preTrainedModel.forward(model_inputs) ⇒ <code> Promise. < Object > </code>

预训练模型的前向方法。如果子类没有覆盖，将根据模型类型选择正确的前向方法。

**类型**: [`PreTrainedModel`](#module_models.PreTrainedModel)的实例方法

**返回值**: `Promise.<Object>` - 模型中指定格式的输出数据。

**抛出**:

+   `错误` 这个方法必须在子类中实现。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型中指定格式的输入数据。 |

* * *

### preTrainedModel._get_generation_config(generation_config) ⇒ <code> * </code>

此函数将多个生成配置合并在一起，形成最终的生成配置，供模型用于文本生成。首先创建一个空的`GenerationConfig`对象，然后将模型自己的`generation_config`属性应用于它。最后，如果在参数中传递了一个`generation_config`对象，则用传递的配置对象的属性覆盖最终配置中的相应属性。

**类型**：[`PreTrainedModel`](#module_models.PreTrainedModel)的实例方法

**返回**：`*` - 用于文本生成的最终生成配置对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| generation_config | `*` | 包含生成参数的`GenerationConfig`对象。 |

* * *

### preTrainedModel.groupBeams(beams) ⇒ <code> Array </code>

按照它们的id将beam对象数组分组。

**类型**：[`PreTrainedModel`](#module_models.PreTrainedModel)的实例方法

**返回**：`Array` - 一个数组，其中每个内部数组包含具有相同id的beam对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| beams | `Array` | 要分组的beam对象数组。 |

* * *

### preTrainedModel.getPastKeyValues(decoderResults, pastKeyValues) ⇒ <code> Object </code>

从给定的解码器结果对象返回一个包含过去键值的对象。

**类型**：[`PreTrainedModel`](#module_models.PreTrainedModel)的实例方法

**返回**：`Object` - 一个包含过去键值的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| decoderResults | `Object` | 解码器结果对象。 |
| pastKeyValues | `Object` | 先前的过去键值。 |

* * *

### preTrainedModel.getAttentions(decoderResults) ⇒ <code> Object </code>

从给定的解码器结果对象返回一个包含注意力的对象。

**类型**：[`PreTrainedModel`](#module_models.PreTrainedModel)的实例方法

**返回**：`Object` - 一个包含注意力的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| decoderResults | `Object` | 解码器结果对象。 |

* * *

### preTrainedModel.addPastKeyValues(decoderFeeds, pastKeyValues)

将过去键值添加到解码器feeds对象。如果pastKeyValues为null，则为过去键值创建新张量。

**类型**：[`PreTrainedModel`](#module_models.PreTrainedModel)的实例方法

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| decoderFeeds | `Object` | 要添加过去键值的解码器feeds对象。 |
| pastKeyValues | `Object` | 包含过去键值的对象。 |

* * *

### PreTrainedModel.from_pretrained(pretrained_model_name_or_path, options) ⇒ <code> Promise. < PreTrainedModel > </code>

从预训练模型实例化库中的一个模型类。

根据配置对象的`model_type`属性选择要实例化的模型类（如果可能，作为参数传递或从`pretrained_model_name_or_path`加载）

**类型**：[`PreTrainedModel`](#module_models.PreTrainedModel)的静态方法

**返回**：`Promise.<PreTrainedModel>` - `PreTrainedModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |

| pretrained_model_name_or_path | `string` | 预训练模型的名称或路径。可以是：

+   一个字符串，托管在huggingface.co模型存储库中的预训练模型的*模型id*。有效的模型id可以位于根级别，如`bert-base-uncased`，或者在用户或组织名称下命名空间化，如`dbmdz/bert-base-german-cased`。

+   包含模型权重的*目录*的路径，例如`./my_model_directory/`。

|

| options | `*` | 加载模型的附加选项。 |
| --- | --- | --- |

* * *

## models.BaseModelOutput

模型输出的基类，具有潜在的隐藏状态和注意力。

**类型**：[`models`](#module_models)的静态类

* * *

### new BaseModelOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.last_hidden_state | `Tensor` | 模型最后一层输出的隐藏状态序列。 |
| [output.hidden_states] | `Tensor` | 模型在每一层输出的隐藏状态加上可选的初始嵌入输出。 |
| [output.attentions] | `Tensor` | 注意力softmax后的注意力权重，用于计算自注意力头中的加权平均值。 |

* * *

## models.BertForMaskedLM

BertForMaskedLM是表示用于掩码语言建模的BERT模型的类。

**类型**: [`models`](#module_models)的静态类

* * *

### bertForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

在新输入上调用模型。

**类型**: [`BertForMaskedLM`](#module_models.BertForMaskedLM)的实例方法

**返回**: `Promise.<MaskedLMOutput>` - 包含模型对掩码语言建模的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.BertForSequenceClassification

BertForSequenceClassification是表示用于序列分类的BERT模型的类。

**类型**: [`models`](#module_models)的静态类

* * *

### bertForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

在新输入上调用模型。

**类型**: [`BertForSequenceClassification`](#module_models.BertForSequenceClassification)的实例方法

**返回**: `Promise.<SequenceClassifierOutput>` - 包含模型对序列分类的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.BertForTokenClassification

BertForTokenClassification是表示用于标记分类的BERT模型的类。

**类型**: [`models`](#module_models)的静态类

* * *

### bertForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>

在新输入上调用模型。

**类型**: [`BertForTokenClassification`](#module_models.BertForTokenClassification)的实例方法

**返回**: `Promise.<TokenClassifierOutput>` - 包含模型对标记分类的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.BertForQuestionAnswering

BertForQuestionAnswering是表示用于问题回答的BERT模型的类。

**类型**: [`models`](#module_models)的静态类

* * *

### bertForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>

在新输入上调用模型。

**类型**: [`BertForQuestionAnswering`](#module_models.BertForQuestionAnswering)的实例方法

**返回**: `Promise.<QuestionAnsweringModelOutput>` - 包含模型对问题回答的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.RoFormerModel

裸RoFormer模型变换器输出没有特定头部的原始隐藏状态。

**类型**: [`models`](#module_models)的静态类

* * *

## models.RoFormerForMaskedLM

带有顶部`语言建模`头部的RoFormer模型。

**类型**: [`models`](#module_models)的静态类

* * *

### roFormerForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

在新输入上调用模型。

**类型**: [`RoFormerForMaskedLM`](#module_models.RoFormerForMaskedLM)的实例方法

**返回**: `Promise.<MaskedLMOutput>` - 包含模型对掩码语言建模的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.RoFormerForSequenceClassification

带有顶部的序列分类/回归头部的RoFormer模型变换器（在汇总输出的顶部有一个线性层）

**类型**: [`models`](#module_models)的静态类

* * *

### roFormerForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

在新输入上调用模型。

**类型**：[`RoFormerForSequenceClassification`](#module_models.RoFormerForSequenceClassification)的实例方法

**返回**：`Promise.<SequenceClassifierOutput>` - 包含序列分类模型输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.RoFormerForTokenClassification

带有标记分类头部的RoFormer模型（隐藏状态输出的顶部线性层），例如用于命名实体识别（NER）任务。

**类型**：[`models`](#module_models)的静态类

* * *

### roFormerForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>

在新输入上调用模型。

**类型**：[`RoFormerForTokenClassification`](#module_models.RoFormerForTokenClassification)的实例方法

**返回**：`Promise.<TokenClassifierOutput>` - 包含标记分类模型输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.RoFormerForQuestionAnswering

带有用于提取式问答任务的跨度分类头部的RoFormer模型，例如SQuAD（隐藏状态输出的顶部线性层来计算`跨度开始logits`和`跨度结束logits`）。

**类型**：[`models`](#module_models)的静态类

* * *

### roFormerForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>

在新输入上调用模型。

**类型**：[`RoFormerForQuestionAnswering`](#module_models.RoFormerForQuestionAnswering)的实例方法

**返回**：`Promise.<QuestionAnsweringModelOutput>` - 包含问题回答模型输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.ConvBertModel

裸的ConvBERT模型变换器，输出原始的隐藏状态，没有特定的头部。

**类型**：[`models`](#module_models)的静态类

* * *

## models.ConvBertForMaskedLM

带有语言建模头部的ConvBERT模型。

**类型**：[`models`](#module_models)的静态类

* * *

### convBertForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

在新输入上调用模型。

**类型**：[`ConvBertForMaskedLM`](#module_models.ConvBertForMaskedLM)的实例方法

**返回**：`Promise.<MaskedLMOutput>` - 包含遮蔽语言建模模型输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.ConvBertForSequenceClassification

带有序列分类/回归头部的ConvBERT模型变换器（汇总输出的顶部线性层）

**类型**：[`models`](#module_models)的静态类

* * *

### convBertForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

在新输入上调用模型。

**类型**：[`ConvBertForSequenceClassification`](#module_models.ConvBertForSequenceClassification)的实例方法

**返回**：`Promise.<SequenceClassifierOutput>` - 包含序列分类模型输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.ConvBertForTokenClassification

带有标记分类头部的ConvBERT模型（隐藏状态输出的顶部线性层），例如用于命名实体识别（NER）任务。

**类型**：[`models`](#module_models)的静态类

* * *

### convBertForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>

在新输入上调用模型。

**类型**：[`ConvBertForTokenClassification`](#module_models.ConvBertForTokenClassification)的实例方法

**返回**：`Promise.<TokenClassifierOutput>` - 包含模型对标记分类的输出logits的对象。  

| 参数 | 类型 | 描述 |   |
| --- | --- | --- | --- |
| --- | --- | --- |   |
| model_inputs | `Object` | 模型的输入。   |

* * *  

## models.ConvBertForQuestionAnswering  

带有顶部用于提取式问答任务（如SQuAD）的跨度分类头的ConvBERT模型（在隐藏状态输出的顶部有线性层，用于计算`跨度开始logits`和`跨度结束logits`）。

**类型**：[`models`](#module_models)的静态类  

* * *  

### convBertForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>  

对新输入调用模型。  

**类型**：[`ConvBertForQuestionAnswering`](#module_models.ConvBertForQuestionAnswering)的实例方法  

**返回**：`Promise.<QuestionAnsweringModelOutput>` - 包含模型对问题回答的输出logits的对象。  

| 参数 | 类型 | 描述 |   |
| --- | --- | --- | --- |
| --- | --- | --- |   |
| model_inputs | `Object` | 模型的输入。   |

* * *  

## models.ElectraModel  

裸的ELECTRA模型变换器，输出原始隐藏状态，没有特定的顶部。与BERT模型相同，只是在嵌入层和编码器之间使用额外的线性层，如果隐藏大小和嵌入大小不同。  

**类型**：[`models`](#module_models)的静态类  

* * *  

## models.ElectraForMaskedLM  

带有顶部语言建模头的ELECTRA模型。  

**类型**：[`models`](#module_models)的静态类  

* * *  

### electraForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>  

对新输入调用模型。  

**类型**：[`ElectraForMaskedLM`](#module_models.ElectraForMaskedLM)的实例方法  

**返回**：`Promise.<MaskedLMOutput>` - 包含模型对掩码语言建模的输出logits的对象。  

| 参数 | 类型 | 描述 |   |
| --- | --- | --- | --- |
| --- | --- | --- |   |
| model_inputs | `Object` | 模型的输入。   |

* * *  

## models.ElectraForSequenceClassification  

带有顶部序列分类/回归头的ELECTRA模型变换器（在池化输出的顶部有一个线性层）  

**类型**：[`models`](#module_models)的静态类  

* * *  

### electraForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>  

对新输入调用模型。  

**类型**：[`ElectraForSequenceClassification`](#module_models.ElectraForSequenceClassification)的实例方法  

**返回**：`Promise.<SequenceClassifierOutput>` - 包含模型对序列分类的输出logits的对象。  

| 参数 | 类型 | 描述 |   |
| --- | --- | --- | --- |
| --- | --- | --- |   |
| model_inputs | `Object` | 模型的输入。   |

* * *  

## models.ElectraForTokenClassification  

带有顶部标记分类头的ELECTRA模型。  

**类型**：[`models`](#module_models)的静态类  

* * *  

### electraForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>  

对新输入调用模型。  

**类型**：[`ElectraForTokenClassification`](#module_models.ElectraForTokenClassification)的实例方法  

**返回**：`Promise.<TokenClassifierOutput>` - 包含模型对标记分类的输出logits的对象。  

| 参数 | 类型 | 描述 |   |
| --- | --- | --- | --- |
| --- | --- | --- |   |
| model_inputs | `Object` | 模型的输入。   |

* * *  

## models.ElectraForQuestionAnswering  

带有顶部用于提取式问答任务（如SQuAD）的跨度分类头的LECTRA模型（在隐藏状态输出的顶部有线性层，用于计算`跨度开始logits`和`跨度结束logits`）。  

**类型**：[`models`](#module_models)的静态类  

* * *  

### electraForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>  

对新输入调用模型。  

**类型**：[`ElectraForQuestionAnswering`](#module_models.ElectraForQuestionAnswering)的实例方法  

**返回**：`Promise.<QuestionAnsweringModelOutput>` - 包含模型对问题回答的输出logits的对象。  

| 参数 | 类型 | 描述 |   |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.CamembertModel

裸CamemBERT模型变换器输出原始隐藏状态，没有特定的头部。

**类型**：[`models`](#module_models)的静态类

* * *

## models.CamembertForMaskedLM

带有`语言建模`头部的CamemBERT模型。

**类型**：[`models`](#module_models)的静态类

* * *

### camembertForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

对新输入调用模型。

**类型**：[`CamembertForMaskedLM`](#module_models)的实例方法

**返回**：`Promise.<MaskedLMOutput>` - 包含用于掩码语言建模的模型输出对数的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.CamembertForSequenceClassification

带有顺序分类/回归头部（池化输出顶部的线性层）的CamemBERT模型，例如用于GLUE任务。

**类型**：[`models`](#module_models)的静态类

* * *

### camembertForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

对新输入调用模型。

**类型**：[`CamembertForSequenceClassification`](#module_models.CamembertForSequenceClassification)的实例方法

**返回**：`Promise.<SequenceClassifierOutput>` - 包含用于序列分类的模型输出对数的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.CamembertForTokenClassification

带有标记分类头部（隐藏状态输出顶部的线性层）的CamemBERT模型，例如用于命名实体识别（NER）任务。

**类型**：[`models`](#module_models)的静态类

* * *

### camembertForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>

对新输入调用模型。

**类型**：[`CamembertForTokenClassification`](#module_models.CamembertForTokenClassification)的实例方法

**返回**：`Promise.<TokenClassifierOutput>` - 包含用于标记分类的模型输出对数的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.CamembertForQuestionAnswering

带有抽取式问答任务的跨度分类头部的CamemBERT模型

**类型**：[`models`](#module_models)的静态类

* * *

### camembertForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>

对新输入调用模型。

**类型**：[`CamembertForQuestionAnswering`](#module_models.CamembertForQuestionAnswering)的实例方法

**返回**：`Promise.<QuestionAnsweringModelOutput>` - 包含用于问题回答的模型输出对数的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.DebertaModel

裸DeBERTa模型变换器输出原始隐藏状态，没有特定的头部。

**类型**：[`models`](#module_models)的静态类

* * *

## models.DebertaForMaskedLM

带有`语言建模`头部的DeBERTa模型。

**类型**：[`models`](#module_models)的静态类

* * *

### debertaForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

对新输入调用模型。

**类型**：[`DebertaForMaskedLM`](#module_models.DebertaForMaskedLM)的实例方法

**返回**：`Promise.<MaskedLMOutput>` - 包含用于掩码语言建模的模型输出对数的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.DebertaForSequenceClassification

带有顺序分类/回归头部（池化输出顶部的线性层）的DeBERTa模型变换器

**类型**：[`models`](#module_models)的静态类

* * *

### debertaForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

对新输入调用模型。

**种类**: [`DebertaForSequenceClassification`](#module_models.DebertaForSequenceClassification)的实例方法

**返回**: `Promise.<SequenceClassifierOutput>` - 包含模型对序列分类的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.DebertaForTokenClassification

在顶部带有标记分类头的DeBERTa模型（隐藏状态输出顶部的线性层），例如用于命名实体识别（NER）任务。

**种类**: [`models`](#module_models)的静态类

* * *

### debertaForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>

对新输入调用模型。

**种类**: [`DebertaForTokenClassification`](#module_models.DebertaForTokenClassification)的实例方法

**返回**: `Promise.<TokenClassifierOutput>` - 包含模型对标记分类的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.DebertaForQuestionAnswering

带有顶部的跨度分类头的DeBERTa模型，用于提取式问答任务，如SQuAD（在隐藏状态输出顶部的线性层上计算`跨度开始logits`和`跨度结束logits`）。

**种类**: [`models`](#module_models)的静态类

* * *

### debertaForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>

对新输入调用模型。

**种类**: [`DebertaForQuestionAnswering`](#module_models.DebertaForQuestionAnswering)的实例方法

**返回**: `Promise.<QuestionAnsweringModelOutput>` - 包含模型对问题回答的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.DebertaV2Model

裸的DeBERTa-V2模型变换器，输出原始的隐藏状态，没有特定的头部。

**种类**: [`models`](#module_models)的静态类

* * *

## models.DebertaV2ForMaskedLM

在顶部带有`语言建模`头的DeBERTa-V2模型。

**种类**: [`models`](#module_models)的静态类

* * *

### debertaV2ForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

对新输入调用模型。

**种类**: [`DebertaV2ForMaskedLM`](#module_models.DebertaV2ForMaskedLM)的实例方法

**返回**: `Promise.<MaskedLMOutput>` - 包含模型对掩码语言建模的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.DebertaV2ForSequenceClassification

带有顶部的序列分类/回归头的DeBERTa-V2模型变换器（在汇总输出的顶部的线性层）

**种类**: [`models`](#module_models)的静态类

* * *

### debertaV2ForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

对新输入调用模型。

**种类**: [`DebertaV2ForSequenceClassification`](#module_models.DebertaV2ForSequenceClassification)的实例方法

**返回**: `Promise.<SequenceClassifierOutput>` - 包含模型对序列分类的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.DebertaV2ForTokenClassification

在顶部带有标记分类头的DeBERTa-V2模型（隐藏状态输出顶部的线性层），例如用于命名实体识别（NER）任务。

**种类**: [`models`](#module_models)的静态类

* * *

### debertaV2ForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>

对新输入调用模型。

**种类**: [`DebertaV2ForTokenClassification`](#module_models.DebertaV2ForTokenClassification)的实例方法

**返回**: `Promise.<TokenClassifierOutput>` - 包含模型用于标记分类的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## 模型.DebertaV2ForQuestionAnswering

带有用于提取式问答任务（如SQuAD）的跨度分类头部的DeBERTa-V2模型（在隐藏状态输出的顶部有线性层来计算`跨度起始logits`和`跨度结束logits`）。

**种类**: [`models`](#module_models)的静态类

* * *

### debertaV2ForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>

对新输入调用模型。

**种类**: [`DebertaV2ForQuestionAnswering`](#module_models.DebertaV2ForQuestionAnswering)的实例方法

**返回**: `Promise.<QuestionAnsweringModelOutput>` - 包含模型用于问答的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## 模型.DistilBertForSequenceClassification

DistilBertForSequenceClassification是代表用于序列分类的DistilBERT模型的类。

**种类**: [`models`](#module_models)的静态类

* * *

### distilBertForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

对新输入调用模型。

**种类**: [`DistilBertForSequenceClassification`](#module_models.DistilBertForSequenceClassification)的实例方法

**返回**: `Promise.<SequenceClassifierOutput>` - 包含模型用于序列分类的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## 模型.DistilBertForTokenClassification

DistilBertForTokenClassification是代表用于标记分类的DistilBERT模型的类。

**种类**: [`models`](#module_models)的静态类

* * *

### distilBertForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>

对新输入调用模型。

**种类**: [`DistilBertForTokenClassification`](#module_models.DistilBertForTokenClassification)的实例方法

**返回**: `Promise.<TokenClassifierOutput>` - 包含模型用于标记分类的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## 模型.DistilBertForQuestionAnswering

DistilBertForQuestionAnswering是代表用于问答的DistilBERT模型的类。

**种类**: [`models`](#module_models)的静态类

* * *

### distilBertForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>

对新输入调用模型。

**种类**: [`DistilBertForQuestionAnswering`](#module_models.DistilBertForQuestionAnswering)的实例方法

**返回**: `Promise.<QuestionAnsweringModelOutput>` - 包含模型用于问答的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## 模型.DistilBertForMaskedLM

DistilBertForMaskedLM是代表用于掩码任务的DistilBERT模型的类。

**种类**: [`models`](#module_models)的静态类

* * *

### distilBertForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

对新输入调用模型。

**种类**: [`DistilBertForMaskedLM`](#module_models.DistilBertForMaskedLM)的实例方法

**返回**: `Promise.<MaskedLMOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## 模型.EsmModel

ESM模型裸的变压器输出原始隐藏状态，没有特定头部。

**种类**: [`models`](#module_models)的静态类

* * *

## 模型.EsmForMaskedLM

带有`语言建模`头部的ESM模型。

**种类**: [`models`](#module_models)的静态类

* * *

### esmForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

调用模型对新输入进行操作。

**类型**：[`EsmForMaskedLM`](#module_models.EsmForMaskedLM)的实例方法

**返回**：`Promise.<MaskedLMOutput>` - 包含模型对掩码语言建模的输出对数的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.EsmForSequenceClassification

ESM模型变换器，顶部带有一个序列分类/回归头（汇总输出顶部的线性层）

**类型**：[`models`](#module_models)的静态类

* * *

### esmForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

调用模型对新输入进行操作。

**类型**：[`EsmForSequenceClassification`](#module_models.EsmForSequenceClassification)的实例方法

**返回**：`Promise.<SequenceClassifierOutput>` - 包含模型对序列分类的输出对数的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.EsmForTokenClassification

ESM模型，顶部带有一个标记分类头（隐藏状态输出顶部的线性层），例如用于命名实体识别（NER）任务。

**类型**：[`models`](#module_models)的静态类

* * *

### esmForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>

调用模型对新输入进行操作。

**类型**：[`EsmForTokenClassification`](#module_models.EsmForTokenClassification)的实例方法

**返回**：`Promise.<TokenClassifierOutput>` - 包含模型对标记分类的输出对数的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.MobileBertForMaskedLM

MobileBertForMaskedLM是表示用于掩码任务的MobileBERT模型的类。

**类型**：[`models`](#module_models)的静态类

* * *

### mobileBertForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

调用模型对新输入进行操作。

**类型**：[`MobileBertForMaskedLM`](#module_models.MobileBertForMaskedLM)的实例方法

**返回**：`Promise.<MaskedLMOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.MobileBertForSequenceClassification

MobileBert模型变换器，顶部带有一个序列分类/回归头（汇总输出顶部的线性层）

**类型**：[`models`](#module_models)的静态类

* * *

### mobileBertForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

调用模型对新输入进行操作。

**类型**：[`MobileBertForSequenceClassification`](#module_models.MobileBertForSequenceClassification)的实例方法

**返回**：`Promise.<SequenceClassifierOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.MobileBertForQuestionAnswering

MobileBert模型，顶部带有一个用于提取性问答任务的跨度分类头

**类型**：[`models`](#module_models)的静态类

* * *

### mobileBertForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>

调用模型对新输入进行操作。

**类型**：[`MobileBertForQuestionAnswering`](#module_models.MobileBertForQuestionAnswering)的实例方法

**返回**：`Promise.<QuestionAnsweringModelOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.MPNetModel

裸MPNet模型变换器，输出原始隐藏状态，没有特定的头部。

**类型**：[`models`](#module_models)的静态类

* * *

## models.MPNetForMaskedLM

MPNetForMaskedLM是表示用于掩码语言建模的MPNet模型的类。

**类型**：[`models`](#module_models) 的静态类

* * *

### mpNetForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

调用模型的新输入。

**类型**：[`MPNetForMaskedLM`](#module_models.MPNetForMaskedLM) 的实例方法

**返回**：`Promise.<MaskedLMOutput>` - 包含遮蔽语言建模模型输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.MPNetForSequenceClassification

MPNetForSequenceClassification 是表示用于序列分类的 MPNet 模型的类。

**类型**：[`models`](#module_models) 的静态类

* * *

### mpNetForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

调用模型的新输入。

**类型**：[`MPNetForSequenceClassification`](#module_models.MPNetForSequenceClassification) 的实例方法

**返回**：`Promise.<SequenceClassifierOutput>` - 包含序列分类模型输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.MPNetForTokenClassification

MPNetForTokenClassification 是表示用于标记分类的 MPNet 模型的类。

**类型**：[`models`](#module_models) 的静态类

* * *

### mpNetForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>

调用模型的新输入。

**类型**：[`MPNetForTokenClassification`](#module_models) 的实例方法

**返回**：`Promise.<TokenClassifierOutput>` - 包含标记分类模型输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.MPNetForQuestionAnswering

MPNetForQuestionAnswering 是表示用于问答的 MPNet 模型的类。

**类型**：[`models`](#module_models) 的静态类

* * *

### mpNetForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>

调用模型的新输入。

**类型**：[`MPNetForQuestionAnswering`](#module_models.MPNetForQuestionAnswering) 的实例方法

**返回**：`Promise.<QuestionAnsweringModelOutput>` - 包含问答模型输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.T5ForConditionalGeneration

T5Model 是表示用于条件生成的 T5 模型的类。

**类型**：[`models`](#module_models) 的静态类

* * *

### new T5ForConditionalGeneration(config, session, decoder_merged_session, generation_config)

创建一个新的 `T5ForConditionalGeneration` 类的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 模型配置。 |
| session | `any` | 模型的会话。 |
| decoder_merged_session | `any` | 解码器的会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.LongT5PreTrainedModel

一个抽象类，用于处理权重初始化和一个简单的接口，用于下载和加载预训练模型。

**类型**：[`models`](#module_models) 的静态类

* * *

## models.LongT5Model

裸 LONGT5 模型变压器，输出原始隐藏状态，没有特定的头部。

**类型**：[`models`](#module_models) 的静态类

* * *

## models.LongT5ForConditionalGeneration

带有`语言建模`头部的 LONGT5 模型。

**类型**：[`models`](#module_models) 的静态类

* * *

### new LongT5ForConditionalGeneration(config, session, decoder_merged_session, generation_config)

创建一个新的 `LongT5ForConditionalGeneration` 类的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 模型配置。 |
| session | `any` | 模型的会话。 |
| decoder_merged_session | `any` | 解码器的会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.MT5ForConditionalGeneration

基于MT5架构的条件序列到序列模型的类。

**种类**: [`models`](#module_models)的静态类

* * *

### new MT5ForConditionalGeneration(config, session, decoder_merged_session, generation_config)

创建`MT5ForConditionalGeneration`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `any` | 模型配置。 |
| session | `any` | 包含编码器权重的ONNX会话。 |
| decoder_merged_session | `any` | 包含合并的解码器权重的ONNX会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.BartModel

裸BART模型，输出原始隐藏状态，顶部没有特定的头。

**种类**: [`models`](#module_models)的静态类

* * *

## models.BartForConditionalGeneration

带有语言建模头的BART模型。可用于摘要。

**种类**: [`models`](#module_models)的静态类

* * *

### new BartForConditionalGeneration(config, session, decoder_merged_session, generation_config)

创建`BartForConditionalGeneration`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | Bart模型的配置对象。 |
| session | `Object` | 用于执行模型的ONNX会话。 |
| decoder_merged_session | `Object` | 用于执行解码器的ONNX会话。 |
| generation_config | `Object` | 生成配置对象。 |

* * *

## models.BartForSequenceClassification

带有序列分类/头（汇总输出的顶部线性层）的Bart模型

**种类**: [`models`](#module_models)的静态类

* * *

### bartForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

在新输入上调用模型。

**种类**: [`BartForSequenceClassification`](#module_models.BartForSequenceClassification)的实例方法

**返回**: `Promise.<SequenceClassifierOutput>` - 包含用于序列分类的模型输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.MBartModel

裸MBART模型，顶部没有特定的头。

**种类**: [`models`](#module_models)的静态类

* * *

## models.MBartForConditionalGeneration

带有语言建模头的MBART模型。在微调预训练模型后可用于摘要。

**种类**: [`models`](#module_models)的静态类

* * *

### new MBartForConditionalGeneration(config, session, decoder_merged_session, generation_config)

创建`MBartForConditionalGeneration`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | Bart模型的配置对象。 |
| session | `Object` | 用于执行模型的ONNX会话。 |
| decoder_merged_session | `Object` | 用于执行解码器的ONNX会话。 |
| generation_config | `Object` | 生成配置对象。 |

* * *

## models.MBartForSequenceClassification

MBart模型，顶部带有序列分类/头（汇总输出的顶部线性层）。

**种类**: [`models`](#module_models)的静态类

* * *

### mBartForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

在新输入上调用模型。

**种类**: [`MBartForSequenceClassification`](#module_models.MBartForSequenceClassification)的实例方法

**返回**: `Promise.<SequenceClassifierOutput>` - 包含用于序列分类的模型输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.MBartForCausalLM

**种类**: [`models`](#module_models)的静态类

* * *

### new MBartForCausalLM(config, decoder_merged_session, generation_config)

创建`MBartForCausalLM`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 模型的配置对象。 |
| decoder_merged_session | `Object` | 用于解码器的ONNX会话对象。 |
| generation_config | `Object` | 生成过程的配置对象。 |

* * *

## models.BlenderbotModel

裸的Blenderbot模型，输出原始隐藏状态，没有特定的头部。

静态类[`models`](#module_models)的Kind：

* * *

## models.BlenderbotForConditionalGeneration

带有语言建模头部的Blenderbot模型。可用于摘要。

**Kind**: 静态类[`models`](#module_models)

* * *

### new BlenderbotForConditionalGeneration(config, session, decoder_merged_session, generation_config)

创建`BlenderbotForConditionalGeneration`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `any` | 模型配置。 |
| session | `any` | 包含编码器权重的ONNX会话。 |
| decoder_merged_session | `any` | 包含合并的解码器权重的ONNX会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.BlenderbotSmallModel

裸的BlenderbotSmall模型，输出原始隐藏状态，没有特定的头部。

**Kind**: 静态类[`models`](#module_models)

* * *

## models.BlenderbotSmallForConditionalGeneration

带有语言建模头部的BlenderbotSmall模型。可用于摘要。

**Kind**: 静态类[`models`](#module_models)

* * *

### new BlenderbotSmallForConditionalGeneration(config, session, decoder_merged_session, generation_config)

创建`BlenderbotForConditionalGeneration`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `any` | 模型配置。 |
| session | `any` | 包含编码器权重的ONNX会话。 |
| decoder_merged_session | `any` | 包含合并的解码器权重的ONNX会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.RobertaForMaskedLM

RobertaForMaskedLM类用于在Roberta模型上执行掩码语言建模。

静态类[`models`](#module_models)的Kind：

* * *

### robertaForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

对新输入调用模型。

**Kind**: [`RobertaForMaskedLM`](#module_models.RobertaForMaskedLM)的实例方法

**返回**: `Promise.<MaskedLMOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.RobertaForSequenceClassification

RobertaForSequenceClassification类用于在Roberta模型上执行序列分类。

**Kind**: 静态类[`models`](#module_models)

* * *

### robertaForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

对新输入调用模型。

**Kind**: [`RobertaForSequenceClassification`](#module_models.RobertaForSequenceClassification)的实例方法

**返回**: `Promise.<SequenceClassifierOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.RobertaForTokenClassification

RobertaForTokenClassification类用于在Roberta模型上执行标记分类。

**Kind**: 静态类[`models`](#module_models)

* * *

### robertaForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>

对新输入调用模型。

**Kind**: [`RobertaForTokenClassification`](#module_models.RobertaForTokenClassification)的实例方法

**返回**: `Promise.<TokenClassifierOutput>` - 包含用于标记分类的模型输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.RobertaForQuestionAnswering

在Roberta模型上执行问答的RobertaForQuestionAnswering类。

**种类**：[`models`](#module_models)的静态类

* * *

### robertaForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>

在新输入上调用模型。

**种类**：[`RobertaForQuestionAnswering`](#module_models.RobertaForQuestionAnswering)的实例方法

**返回**：`Promise.<QuestionAnsweringModelOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.XLMPreTrainedModel

一个处理权重初始化和一个简单接口用于下载和加载预训练模型的抽象类。

**种类**：[`models`](#module_models)的静态类

* * *

## models.XLMModel

裸的XLM模型变换器输出原始隐藏状态，没有特定的头部在顶部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.XLMWithLMHeadModel

带有语言建模头部的XLM模型变换器（线性层，权重与输入嵌入相关联）。

**种类**：[`models`](#module_models)的静态类

* * *

### xlmWithLMHeadModel._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

在新输入上调用模型。

**种类**：[`XLMWithLMHeadModel`](#module_models.XLMWithLMHeadModel)的实例方法

**返回**：`Promise.<MaskedLMOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.XLMForSequenceClassification

在顶部有一个序列分类/回归头的XLM模型（在汇总输出的顶部有一个线性层）

**种类**：[`models`](#module_models)的静态类

* * *

### xlmForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

在新输入上调用模型。

**种类**：[`XLMForSequenceClassification`](#module_models.XLMForSequenceClassification)的实例方法

**返回**：`Promise.<SequenceClassifierOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.XLMForTokenClassification

在顶部有一个标记分类头的XLM模型（在隐藏状态输出的顶部有一个线性层）

**种类**：[`models`](#module_models)的静态类

* * *

### xlmForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>

在新输入上调用模型。

**种类**：[`XLMForTokenClassification`](#module_models.XLMForTokenClassification)的实例方法

**返回**：`Promise.<TokenClassifierOutput>` - 包含模型对标记分类的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.XLMForQuestionAnswering

在顶部有一个用于提取问答任务的跨度分类头的XLM模型

**种类**：[`models`](#module_models)的静态类

* * *

### xlmForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>

在新输入上调用模型。

**种类**：[`XLMForQuestionAnswering`](#module_models.XLMForQuestionAnswering)的实例方法

**返回**：`Promise.<QuestionAnsweringModelOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.XLMRobertaForMaskedLM

用于在XLMRoberta模型上执行掩码语言建模的XLMRobertaForMaskedLM类。

**种类**：[`models`](#module_models)的静态类

* * *

### xlmRobertaForMaskedLM._call(model_inputs) ⇒ <code> Promise. < MaskedLMOutput > </code>

在新输入上调用模型。

**种类**：[`XLMRobertaForMaskedLM`](#module_models.XLMRobertaForMaskedLM)的实例方法

**返回**：`Promise.<MaskedLMOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.XLMRobertaForSequenceClassification

XLMRobertaForSequenceClassification类用于在XLMRoberta模型上执行序列分类。

**类型**: [`models`](#module_models)的静态类

* * *

### xlmRobertaForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

对新输入调用模型。

**类型**: [`XLMRobertaForSequenceClassification`](#module_models.XLMRobertaForSequenceClassification)的实例方法

**返回值**: `Promise.<SequenceClassifierOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.XLMRobertaForTokenClassification

用于在XLMRoberta模型上执行标记分类的XLMRobertaForTokenClassification类。

**类型**: [`models`](#module_models)的静态类

* * *

### xlmRobertaForTokenClassification._call(model_inputs) ⇒ <code> Promise. < TokenClassifierOutput > </code>

对新输入调用模型。

**类型**: [`XLMRobertaForTokenClassification`](#module_models.XLMRobertaForTokenClassification)的实例方法

**返回值**: `Promise.<TokenClassifierOutput>` - 包含模型用于标记分类的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.XLMRobertaForQuestionAnswering

用于在XLMRoberta模型上执行问题回答的XLMRobertaForQuestionAnswering类。

**类型**: [`models`](#module_models)的静态类

* * *

### xlmRobertaForQuestionAnswering._call(model_inputs) ⇒ <code> Promise. < QuestionAnsweringModelOutput > </code>

对新输入调用模型。

**类型**: [`XLMRobertaForQuestionAnswering`](#module_models.XLMRobertaForQuestionAnswering)的实例方法

**返回值**: `Promise.<QuestionAnsweringModelOutput>` - 返回的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.ASTModel

输出原始隐藏状态而不在顶部具有特定头部的裸AST模型变换器。

**类型**: [`models`](#module_models)的静态类

* * *

## models.ASTForAudioClassification

在顶部具有音频分类头部的音频频谱变换器模型（在池化输出的顶部有一个线性层），例如用于AudioSet、Speech Commands v2等数据集。

**类型**: [`models`](#module_models)的静态类

* * *

## models.WhisperModel

用于训练没有语言模型头的Whisper模型的WhisperModel类。

**类型**: [`models`](#module_models)的静态类

* * *

## models.WhisperForConditionalGeneration

用于从Whisper模型生成条件输出的WhisperForConditionalGeneration类。

**类型**: [`models`](#module_models)的静态类

+   [.WhisperForConditionalGeneration](#module_models.WhisperForConditionalGeneration)

    +   [`new WhisperForConditionalGeneration(config, session, decoder_merged_session, generation_config)`](#new_module_models.WhisperForConditionalGeneration_new)

    +   [`.generate(inputs, generation_config, logits_processor)`](#module_models.WhisperForConditionalGeneration+generate) ⇒ `Promise.<Object>`

    +   [`._extract_token_timestamps(generate_outputs, alignment_heads, [num_frames], [time_precision])`](#module_models.WhisperForConditionalGeneration+_extract_token_timestamps) ⇒ `Tensor`

* * *

### new WhisperForConditionalGeneration(config, session, decoder_merged_session, generation_config)

创建一个`WhisperForConditionalGeneration`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 模型的配置对象。 |
| session | `Object` | 模型的ONNX会话对象。 |
| decoder_merged_session | `Object` | 解码器的ONNX会话对象。 |
| generation_config | `Object` | 生成过程的配置对象。 |

* * *

### whisperForConditionalGeneration.generate(inputs, generation_config, logits_processor) ⇒ <code> Promise. < Object > </code>

根据输入和生成配置生成输出。

**种类**：[`WhisperForConditionalGeneration`](#module_models)的实例方法

**返回**：`Promise.<Object>` - Promise对象表示生成的输出。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| 输入 | `Object` |  | 模型的输入数据。 |
| 生成配置 | `WhisperGenerationConfig` |  | 生成过程的配置对象。 |
| logits_processor | `Object` |  | 可选的logits处理器对象。 |

* * *

### whisperForConditionalGeneration._extract_token_timestamps(generate_outputs, alignment_heads, [num_frames], [time_precision]) ⇒ <code> Tensor </code>

使用编码器-解码器交叉注意力和动态时间规整（DTW）计算标记级时间戳，将每个输出标记映射到输入音频中的位置。

**种类**：[`WhisperForConditionalGeneration`](#module_models)的实例方法

**返回**：`Tensor` - 包含每个预测标记的秒数时间戳的张量

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| 生成输出 | `Object` |  | 模型生成的输出 |
| generate_outputs.cross_attentions | `Array.<Array<Array<Tensor>>>` |  | 模型输出的交叉注意力 |
| generate_outputs.decoder_attentions | `Array.<Array<Array<Tensor>>>` |  | 模型输出的解码器注意力 |
| generate_outputs.sequences | `Array.<Array<number>>` |  | 模型输出的序列 |
| alignment_heads | `Array.<Array<number>>` |  | 模型的对齐头部 |
| [num_frames] | `number` |  | 输入音频中的帧数。 |
| [时间精度] | `number` | `0.02` | 秒数时间戳的精度 |

* * *

## models.VisionEncoderDecoderModel

基于OpenAI的GPT架构的视觉编码器-解码器模型，用于图像字幕和其他视觉任务

**种类**：[`models`](#module_models)的静态类

* * *

### new VisionEncoderDecoderModel(config, session, decoder_merged_session, generation_config)

创建`VisionEncoderDecoderModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 指定超参数和其他模型设置的配置对象。 |
| 会话 | `Object` | 包含编码器模型的ONNX会话。 |
| decoder_merged_session | `any` | 包含合并解码器模型的ONNX会话。 |
| 生成配置 | `Object` | 生成过程的配置对象。 |

* * *

## models.CLIPModel

带有投影层的CLIP文本和视觉模型

**示例：** 使用`CLIPModel`进行零样本图像分类。

```py
import { AutoTokenizer, AutoProcessor, CLIPModel, RawImage } from '@xenova/transformers';

// Load tokenizer, processor, and model
let tokenizer = await AutoTokenizer.from_pretrained('Xenova/clip-vit-base-patch16');
let processor = await AutoProcessor.from_pretrained('Xenova/clip-vit-base-patch16');
let model = await CLIPModel.from_pretrained('Xenova/clip-vit-base-patch16');

// Run tokenization
let texts = ['a photo of a car', 'a photo of a football match']
let text_inputs = tokenizer(texts, { padding: true, truncation: true });

// Read image and run processor
let image = await RawImage.read('https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/football-match.jpg');
let image_inputs = await processor(image);

// Run model with both text and pixel inputs
let output = await model({ ...text_inputs, ...image_inputs });
// {
//   logits_per_image: Tensor {
//     dims: [ 1, 2 ],
//     data: Float32Array(2) [ 18.579734802246094, 24.31830596923828 ],
//   },
//   logits_per_text: Tensor {
//     dims: [ 2, 1 ],
//     data: Float32Array(2) [ 18.579734802246094, 24.31830596923828 ],
//   },
//   text_embeds: Tensor {
//     dims: [ 2, 512 ],
//     data: Float32Array(1024) [ ... ],
//   },
//   image_embeds: Tensor {
//     dims: [ 1, 512 ],
//     data: Float32Array(512) [ ... ],
//   }
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

## models.CLIPTextModelWithProjection

带有投影层的CLIP文本模型（在池化输出的顶部有一个线性层）

**示例：** 使用`CLIPTextModelWithProjection`计算文本嵌入。

```py
import { AutoTokenizer, CLIPTextModelWithProjection } from '@xenova/transformers';

// Load tokenizer and text model
const tokenizer = await AutoTokenizer.from_pretrained('Xenova/clip-vit-base-patch16');
const text_model = await CLIPTextModelWithProjection.from_pretrained('Xenova/clip-vit-base-patch16');

// Run tokenization
let texts = ['a photo of a car', 'a photo of a football match'];
let text_inputs = tokenizer(texts, { padding: true, truncation: true });

// Compute embeddings
const { text_embeds } = await text_model(text_inputs);
// Tensor {
//   dims: [ 2, 512 ],
//   type: 'float32',
//   data: Float32Array(1024) [ ... ],
//   size: 1024
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

### CLIPTextModelWithProjection.from_pretrained()：`PreTrainedModel.from_pretrained`

**种类**：[`CLIPTextModelWithProjection`](#module_models.CLIPTextModelWithProjection)的静态方法

* * *

## models.CLIPVisionModelWithProjection

带有投影层的CLIP视觉模型（在池化输出的顶部有一个线性层）

**示例：** 使用`CLIPVisionModelWithProjection`计算视觉嵌入。

```py
import { AutoProcessor, CLIPVisionModelWithProjection, RawImage} from '@xenova/transformers';

// Load processor and vision model
const processor = await AutoProcessor.from_pretrained('Xenova/clip-vit-base-patch16');
const vision_model = await CLIPVisionModelWithProjection.from_pretrained('Xenova/clip-vit-base-patch16');

// Read image and run processor
let image = await RawImage.read('https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/football-match.jpg');
let image_inputs = await processor(image);

// Compute embeddings
const { image_embeds } = await vision_model(image_inputs);
// Tensor {
//   dims: [ 1, 512 ],
//   type: 'float32',
//   data: Float32Array(512) [ ... ],
//   size: 512
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

### CLIPVisionModelWithProjection.from_pretrained()：`PreTrainedModel.from_pretrained`

**种类**：[`CLIPVisionModelWithProjection`](#module_models.CLIPVisionModelWithProjection)的静态方法

* * *

## models.SiglipModel

带有投影层的SigLIP文本和视觉模型

**示例：** 使用`SiglipModel`进行零样本图像分类。

```py
import { AutoTokenizer, AutoProcessor, SiglipModel, RawImage } from '@xenova/transformers';

// Load tokenizer, processor, and model
const tokenizer = await AutoTokenizer.from_pretrained('Xenova/siglip-base-patch16-224');
const processor = await AutoProcessor.from_pretrained('Xenova/siglip-base-patch16-224');
const model = await SiglipModel.from_pretrained('Xenova/siglip-base-patch16-224');

// Run tokenization
const texts = ['a photo of 2 cats', 'a photo of 2 dogs'];
const text_inputs = tokenizer(texts, { padding: 'max_length', truncation: true });

// Read image and run processor
const image = await RawImage.read('http://images.cocodataset.org/val2017/000000039769.jpg');
const image_inputs = await processor(image);

// Run model with both text and pixel inputs
const output = await model({ ...text_inputs, ...image_inputs });
// {
//   logits_per_image: Tensor {
//     dims: [ 1, 2 ],
//     data: Float32Array(2) [ -1.6019744873046875, -10.720091819763184 ],
//   },
//   logits_per_text: Tensor {
//     dims: [ 2, 1 ],
//     data: Float32Array(2) [ -1.6019744873046875, -10.720091819763184 ],
//   },
//   text_embeds: Tensor {
//     dims: [ 2, 768 ],
//     data: Float32Array(1536) [ ... ],
//   },
//   image_embeds: Tensor {
//     dims: [ 1, 768 ],
//     data: Float32Array(768) [ ... ],
//   }
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

## models.SiglipTextModel

SigLIP文本模型，没有任何头部或投影层。

**示例：** 使用`SiglipTextModel`计算文本嵌入。

```py
import { AutoTokenizer, SiglipTextModel } from '@xenova/transformers';

// Load tokenizer and text model
const tokenizer = await AutoTokenizer.from_pretrained('Xenova/siglip-base-patch16-224');
const text_model = await SiglipTextModel.from_pretrained('Xenova/siglip-base-patch16-224');

// Run tokenization
const texts = ['a photo of 2 cats', 'a photo of 2 dogs'];
const text_inputs = tokenizer(texts, { padding: 'max_length', truncation: true });

// Compute embeddings
const { pooler_output } = await text_model(text_inputs);
// Tensor {
//   dims: [ 2, 768 ],
//   type: 'float32',
//   data: Float32Array(1536) [ ... ],
//   size: 1536
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

### SiglipTextModel.from_pretrained() : <code> PreTrainedModel.from_pretrained </code>

**种类**：[`SiglipTextModel`](#module_models.SiglipTextModel)的静态方法

* * *

## models.SiglipVisionModel

来自SigLIP的视觉模型，没有头部或顶部投影。

**示例：** 使用`SiglipVisionModel`进行视觉嵌入计算。

```py
import { AutoProcessor, SiglipVisionModel, RawImage} from '@xenova/transformers';

// Load processor and vision model
const processor = await AutoProcessor.from_pretrained('Xenova/siglip-base-patch16-224');
const vision_model = await SiglipVisionModel.from_pretrained('Xenova/siglip-base-patch16-224');

// Read image and run processor
const image = await RawImage.read('https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/football-match.jpg');
const image_inputs = await processor(image);

// Compute embeddings
const { pooler_output } = await vision_model(image_inputs);
// Tensor {
//   dims: [ 1, 768 ],
//   type: 'float32',
//   data: Float32Array(768) [ ... ],
//   size: 768
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

### SiglipVisionModel.from_pretrained() : <code> PreTrainedModel.from_pretrained </code>

**种类**：[`models`](#module_models)的静态方法

* * *

## models.CLIPSegForImageSegmentation

使用基于Transformer的解码器的CLIPSeg模型进行零样本和一次性图像分割。

**示例：** 使用`CLIPSegForImageSegmentation`模型执行零样本图像分割。

```py
import { AutoTokenizer, AutoProcessor, CLIPSegForImageSegmentation, RawImage } from '@xenova/transformers';

// Load tokenizer, processor, and model
const tokenizer = await AutoTokenizer.from_pretrained('Xenova/clipseg-rd64-refined');
const processor = await AutoProcessor.from_pretrained('Xenova/clipseg-rd64-refined');
const model = await CLIPSegForImageSegmentation.from_pretrained('Xenova/clipseg-rd64-refined');

// Run tokenization
const texts = ['a glass', 'something to fill', 'wood', 'a jar'];
const text_inputs = tokenizer(texts, { padding: true, truncation: true });

// Read image and run processor
const image = await RawImage.read('https://github.com/timojl/clipseg/blob/master/example_image.jpg?raw=true');
const image_inputs = await processor(image);

// Run model with both text and pixel inputs
const { logits } = await model({ ...text_inputs, ...image_inputs });
// logits: Tensor {
//   dims: [4, 352, 352],
//   type: 'float32',
//   data: Float32Array(495616) [ ... ],
//   size: 495616
// }
```

您可以按以下方式可视化预测：

```py
const preds = logits
  .unsqueeze_(1)
  .sigmoid_()
  .mul_(255)
  .round_()
  .to('uint8');

for (let i = 0; i < preds.dims[0]; ++i) {
  const img = RawImage.fromTensor(preds[i]);
  img.save(`prediction_${i}.png`);
}
```

**种类**：[`models`](#module_models)的静态类

* * *

## models.GPT2PreTrainedModel

**种类**：[`models`](#module_models)的静态类

* * *

### 创建一个新的GPT2PreTrainedModel类的实例。

创建一个新的GPT2PreTrainedModel类的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 模型的配置。 |
| session | `any` | 包含模型权重的ONNX会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.GPT2LMHeadModel

GPT-2语言模型头部在GPT-2基础模型之上。该模型适用于文本生成任务。

**种类**：[`models`](#module_models)的静态类

* * *

## models.GPTNeoPreTrainedModel

**种类**：[`models`](#module_models)的静态类

* * *

### 创建一个新的GPTNeoPreTrainedModel实例。

创建一个新的GPTNeoPreTrainedModel类的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 模型的配置。 |
| session | `any` | 包含模型权重的ONNX会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.GPTNeoXPreTrainedModel

**种类**：[`models`](#module_models)的静态类

* * *

### 创建一个新的GPTNeoXPreTrainedModel实例。

创建一个新的GPTNeoXPreTrainedModel类的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 模型的配置。 |
| session | `any` | 包含模型权重的ONNX会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.GPTJPreTrainedModel

**种类**：[`models`](#module_models)的静态类

* * *

### 创建一个新的GPTJPreTrainedModel实例。

创建一个新的GPTJPreTrainedModel类的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 模型的配置。 |
| session | `any` | 包含模型权重的ONNX会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.GPTBigCodePreTrainedModel

**种类**：[`models`](#module_models)的静态类

* * *

### 创建一个新的GPTBigCodePreTrainedModel实例。

创建一个新的GPTBigCodePreTrainedModel类的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 模型的配置。 |
| session | `any` | 包含模型权重的ONNX会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.CodeGenPreTrainedModel

**种类**：[`models`](#module_models)的静态类

* * *

### 创建一个新的CodeGenPreTrainedModel实例。

创建一个新的CodeGenPreTrainedModel类的实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 模型配置对象。 |
| session | `Object` | ONNX会话对象。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.CodeGenModel

CodeGenModel是一个代表没有语言模型头部的代码生成模型的类。

**种类**：[`models`](#module_models)的静态类

* * *

## models.CodeGenForCausalLM

CodeGenForCausalLM是基于GPT-2架构的代码生成模型的类。它扩展了`CodeGenPreTrainedModel`类。

**种类**：[`models`](#module_models)的静态类

* * *

## models.LlamaPreTrainedModel

裸LLama模型输出原始隐藏状态，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

### 新的LlamaPreTrainedModel(config, session, generation_config)

创建`LlamaPreTrainedModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 模型配置对象。 |
| 会话 | `Object` | ONNX会话对象。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.LlamaModel

裸LLaMA模型输出原始隐藏状态，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.Qwen2PreTrainedModel

裸Qwen2模型输出原始隐藏状态，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

### 新的Qwen2PreTrainedModel(config, session, generation_config)

创建`Qwen2PreTrainedModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 模型配置对象。 |
| 会话 | `Object` | ONNX会话对象。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.Qwen2Model

裸Qwen2模型输出原始隐藏状态，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.PhiPreTrainedModel

**种类**：[`models`](#module_models)的静态类

* * *

### 新的PhiPreTrainedModel(config, session, generation_config)

创建`PhiPreTrainedModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 模型配置对象。 |
| 会话 | `Object` | ONNX会话对象。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.PhiModel

裸Phi模型输出原始隐藏状态，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.BloomPreTrainedModel

具有语言建模头部的Bloom模型变换器（线性层，其权重与输入嵌入相关联）。

**种类**：[`models`](#module_models)的静态类

* * *

### 新的BloomPreTrainedModel(config, session, generation_config)

创建`BloomPreTrainedModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 模型配置。 |
| 会话 | `any` | 包含模型权重的ONNX会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.BloomModel

裸Bloom模型变换器输出原始隐藏状态，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.BloomForCausalLM

具有语言建模头部的Bloom模型变换器（线性层，其权重与输入嵌入相关联）。

**种类**：[`models`](#module_models)的静态类

* * *

## models.MptPreTrainedModel

**种类**：[`models`](#module_models)的静态类

* * *

### 新的MptPreTrainedModel(config, session, generation_config)

创建`MptPreTrainedModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 模型配置对象。 |
| 会话 | `Object` | ONNX会话对象。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.MptModel

裸Mpt模型变换器输出原始隐藏状态，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.MptForCausalLM

带有语言建模头部的MPT模型变换器（线性层，权重与输入嵌入绑定）。

**种类**: [`models`](#module_models)的静态类

* * *

## models.OPTPreTrainedModel

**种类**: [`models`](#module_models)的静态类

* * *

### new OPTPreTrainedModel(config, session, generation_config)

创建`OPTPreTrainedModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 模型配置对象。 |
| session | `Object` | ONNX会话对象。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## models.OPTModel

裸的OPT模型输出原始隐藏状态，顶部没有任何特定的头部。

**种类**: [`models`](#module_models)的静态类

* * *

## models.OPTForCausalLM

带有语言建模头部的OPT模型变换器（线性层，权重与输入嵌入绑定）。

**种类**: [`models`](#module_models)的静态类

* * *

## models.VitMatteForImageMatting

利用任何视觉骨干（例如ADE20k，CityScapes）的ViTMatte框架。

**示例:** 使用`VitMatteForImageMatting`模型执行图像抠图。

```py
import { AutoProcessor, VitMatteForImageMatting, RawImage } from '@xenova/transformers';

// Load processor and model
const processor = await AutoProcessor.from_pretrained('Xenova/vitmatte-small-distinctions-646');
const model = await VitMatteForImageMatting.from_pretrained('Xenova/vitmatte-small-distinctions-646');

// Load image and trimap
const image = await RawImage.fromURL('https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/vitmatte_image.png');
const trimap = await RawImage.fromURL('https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/vitmatte_trimap.png');

// Prepare image + trimap for the model
const inputs = await processor(image, trimap);

// Predict alpha matte
const { alphas } = await model(inputs);
// Tensor {
//   dims: [ 1, 1, 640, 960 ],
//   type: 'float32',
//   size: 614400,
//   data: Float32Array(614400) [ 0.9894027709960938, 0.9970508813858032, ... ]
// }
```

您可以按以下方式可视化alpha遮罩：

```py
import { Tensor, cat } from '@xenova/transformers';

// Visualize predicted alpha matte
const imageTensor = new Tensor(
  'uint8',
  new Uint8Array(image.data),
  [image.height, image.width, image.channels]
).transpose(2, 0, 1);

// Convert float (0-1) alpha matte to uint8 (0-255)
const alphaChannel = alphas
  .squeeze(0)
  .mul_(255)
  .clamp_(0, 255)
  .round_()
  .to('uint8');

// Concatenate original image with predicted alpha
const imageData = cat([imageTensor, alphaChannel], 0);

// Save output image
const outputImage = RawImage.fromTensor(imageData);
outputImage.save('output.png');
```

**种类**: [`models`](#module_models)的静态类

* * *

### vitMatteForImageMatting._call(model_inputs)

**种类**: [`VitMatteForImageMatting`](#module_models.VitMatteForImageMatting)的实例方法

| 参数 | 类型 |
| --- | --- |
| model_inputs | `any` |

* * *

## models.DetrObjectDetectionOutput

**种类**: [`models`](#module_models)的静态类

* * *

### new DetrObjectDetectionOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.logits | `Tensor` | 所有查询的分类logits（包括无对象）。 |
| output.pred_boxes | `Tensor` | 所有查询的标准化框坐标，表示为（中心_x，中心_y，宽度，高度）。这些值在[0, 1]范围内标准化，相对于批处理中每个单独图像的大小（忽略可能的填充）。 |

* * *

## models.DetrSegmentationOutput

**种类**: [`models`](#module_models)的静态类

* * *

### new DetrSegmentationOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.logits | `Tensor` | 模型的输出logits。 |
| output.pred_boxes | `Tensor` | 预测的框。 |
| output.pred_masks | `Tensor` | 预测的掩模。 |

* * *

## models.TableTransformerModel

裸的Table Transformer模型（由骨干和编码器-解码器Transformer组成），输出顶部没有任何特定头部的原始隐藏状态。

**种类**: [`models`](#module_models)的静态类

* * *

## models.TableTransformerForObjectDetection

带有对象检测头部的Table Transformer模型（由骨干和编码器-解码器Transformer组成），用于诸如COCO检测之类的任务。

**种类**: [`models`](#module_models)的静态类

* * *

### tableTransformerForObjectDetection._call(model_inputs)

**种类**: [`TableTransformerForObjectDetection`](#module_models.TableTransformerForObjectDetection)的实例方法

| 参数 | 类型 |
| --- | --- |
| model_inputs | `any` |

* * *

## models.ResNetPreTrainedModel

一个处理权重初始化和简单接口以下载和加载预训练模型的抽象类。

**种类**: [`models`](#module_models)的静态类

* * *

## models.ResNetModel

裸的ResNet模型输出原始特征，顶部没有任何特定的头。

**种类**: [`models`](#module_models)的静态类

* * *

## models.ResNetForImageClassification

带有图像分类头部（在池化特征的顶部有一个线性层），例如用于ImageNet的ResNet模型。

**种类**: [`models`](#module_models)的静态类

* * *

### resNetForImageClassification._call(model_inputs)

**种类**: [`models`](#module_models.ResNetForImageClassification)的实例方法

| 参数 | 类型 |
| --- | --- |
| model_inputs | `any` |

* * *

## models.Swin2SRModel

裸的Swin2SR模型转换器，输出原始的隐藏状态，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.Swin2SRForImageSuperResolution

带有上采样头部的Swin2SR模型转换器，用于图像超分辨率和恢复。

**示例**：使用`Xenova/swin2SR-classical-sr-x2-64`进行超分辨率。

```py
import { AutoProcessor, Swin2SRForImageSuperResolution, RawImage } from '@xenova/transformers';

// Load processor and model
const model_id = 'Xenova/swin2SR-classical-sr-x2-64';
const processor = await AutoProcessor.from_pretrained(model_id);
const model = await Swin2SRForImageSuperResolution.from_pretrained(model_id);

// Prepare model inputs
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/butterfly.jpg';
const image = await RawImage.fromURL(url);
const inputs = await processor(image);

// Run model
const outputs = await model(inputs);

// Convert Tensor to RawImage
const output = outputs.reconstruction.squeeze().clamp_(0, 1).mul_(255).round_().to('uint8');
const outputImage = RawImage.fromTensor(output);
// RawImage {
//   data: Uint8Array(786432) [ 41, 31, 24, ... ],
//   width: 512,
//   height: 512,
//   channels: 3
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

## models.DPTModel

裸的DPT模型转换器，输出原始的隐藏状态，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.DPTForDepthEstimation

带有深度估计头部的DPT模型（由3个卷积层组成），例如用于KITTI、NYUv2。

**示例**：使用`Xenova/dpt-hybrid-midas`进行深度估计。

```py
import { DPTForDepthEstimation, AutoProcessor, RawImage, interpolate, max } from '@xenova/transformers';

// Load model and processor
const model_id = 'Xenova/dpt-hybrid-midas';
const model = await DPTForDepthEstimation.from_pretrained(model_id);
const processor = await AutoProcessor.from_pretrained(model_id);

// Load image from URL
const url = 'http://images.cocodataset.org/val2017/000000039769.jpg';
const image = await RawImage.fromURL(url);

// Prepare image for the model
const inputs = await processor(image);

// Run model
const { predicted_depth } = await model(inputs);

// Interpolate to original size
const prediction = interpolate(predicted_depth, image.size.reverse(), 'bilinear', false);

// Visualize the prediction
const formatted = prediction.mul_(255 / max(prediction.data)[0]).to('uint8');
const depth = RawImage.fromTensor(formatted);
// RawImage {
//   data: Uint8Array(307200) [ 85, 85, 84, ... ],
//   width: 640,
//   height: 480,
//   channels: 1
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

## models.DepthAnythingForDepthEstimation

带有深度估计头部的深度任意模型（由3个卷积层组成），例如用于KITTI、NYUv2。

**种类**：[`models`](#module_models)的静态类

* * *

## models.GLPNModel

裸的GLPN编码器（Mix-Transformer），输出原始的隐藏状态，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.GLPNForDepthEstimation

带有轻量级深度估计头部的GLPN模型转换器，例如用于KITTI、NYUv2。

**示例**：使用`Xenova/glpn-kitti`进行深度估计。

```py
import { GLPNForDepthEstimation, AutoProcessor, RawImage, interpolate, max } from '@xenova/transformers';

// Load model and processor
const model_id = 'Xenova/glpn-kitti';
const model = await GLPNForDepthEstimation.from_pretrained(model_id);
const processor = await AutoProcessor.from_pretrained(model_id);

// Load image from URL
const url = 'http://images.cocodataset.org/val2017/000000039769.jpg';
const image = await RawImage.fromURL(url);

// Prepare image for the model
const inputs = await processor(image);

// Run model
const { predicted_depth } = await model(inputs);

// Interpolate to original size
const prediction = interpolate(predicted_depth, image.size.reverse(), 'bilinear', false);

// Visualize the prediction
const formatted = prediction.mul_(255 / max(prediction.data)[0]).to('uint8');
const depth = RawImage.fromTensor(formatted);
// RawImage {
//   data: Uint8Array(307200) [ 207, 169, 154, ... ],
//   width: 640,
//   height: 480,
//   channels: 1
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

## models.DonutSwinModel

裸的Donut Swin模型转换器，输出原始的隐藏状态，没有特定的头部。

**示例**：逐步文档解析。

```py
import { AutoProcessor, AutoTokenizer, AutoModelForVision2Seq, RawImage } from '@xenova/transformers';

// Choose model to use
const model_id = 'Xenova/donut-base-finetuned-cord-v2';

// Prepare image inputs
const processor = await AutoProcessor.from_pretrained(model_id);
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/receipt.png';
const image = await RawImage.read(url);
const image_inputs = await processor(image);

// Prepare decoder inputs
const tokenizer = await AutoTokenizer.from_pretrained(model_id);
const task_prompt = '<s_cord-v2>';
const decoder_input_ids = tokenizer(task_prompt, {
  add_special_tokens: false,
}).input_ids;

// Create the model
const model = await AutoModelForVision2Seq.from_pretrained(model_id);

// Run inference
const output = await model.generate(image_inputs.pixel_values, {
  decoder_input_ids,
  max_length: model.config.decoder.max_position_embeddings,
});

// Decode output
const decoded = tokenizer.batch_decode(output)[0];
// <s_cord-v2><s_menu><s_nm> CINNAMON SUGAR</s_nm><s_unitprice> 17,000</s_unitprice><s_cnt> 1 x</s_cnt><s_price> 17,000</s_price></s_menu><s_sub_total><s_subtotal_price> 17,000</s_subtotal_price></s_sub_total><s_total><s_total_price> 17,000</s_total_price><s_cashprice> 20,000</s_cashprice><s_changeprice> 3,000</s_changeprice></s_total></s>
```

**示例**：逐步文档视觉问答（DocVQA）

```py
import { AutoProcessor, AutoTokenizer, AutoModelForVision2Seq, RawImage } from '@xenova/transformers';

// Choose model to use
const model_id = 'Xenova/donut-base-finetuned-docvqa';

// Prepare image inputs
const processor = await AutoProcessor.from_pretrained(model_id);
const url = 'https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/invoice.png';
const image = await RawImage.read(url);
const image_inputs = await processor(image);

// Prepare decoder inputs
const tokenizer = await AutoTokenizer.from_pretrained(model_id);
const question = 'What is the invoice number?';
const task_prompt = `<s_docvqa><s_question>${question}</s_question><s_answer>`;
const decoder_input_ids = tokenizer(task_prompt, {
  add_special_tokens: false,
}).input_ids;

// Create the model
const model = await AutoModelForVision2Seq.from_pretrained(model_id);

// Run inference
const output = await model.generate(image_inputs.pixel_values, {
  decoder_input_ids,
  max_length: model.config.decoder.max_position_embeddings,
});

// Decode output
const decoded = tokenizer.batch_decode(output)[0];
// <s_docvqa><s_question> What is the invoice number?</s_question><s_answer> us-001</s_answer></s>
```

**种类**：[`models`](#module_models)的静态类

* * *

## models.ConvNextModel

裸的ConvNext模型，输出原始特征，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.ConvNextForImageClassification

带有图像分类头部的ConvNext模型（在池化特征的顶部有一个线性层），例如用于ImageNet。

**种类**：[`models`](#module_models)的静态类

* * *

### convNextForImageClassification._call(model_inputs)

**种类**：[`models`](#module_models)的实例方法

| 参数 | 类型 |
| --- | --- |
| model_inputs | `any` |

* * *

## models.ConvNextV2Model

裸的ConvNextV2模型，输出原始特征，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.ConvNextV2ForImageClassification

带有图像分类头部的ConvNextV2模型（在池化特征的顶部有一个线性层），例如用于ImageNet。

**种类**：[`models`](#module_models)的静态类

* * *

### convNextV2ForImageClassification._call(model_inputs)

**种类**：[`models`](#module_models)的`ConvNextV2ForImageClassification`实例方法

| 参数 | 类型 |
| --- | --- |
| model_inputs | `any` |

* * *

## models.Dinov2Model

裸的DINOv2模型转换器，输出原始的隐藏状态，没有特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.Dinov2ForImageClassification

带有图像分类头部的Dinov2模型转换器（在[CLS]标记的最终隐藏状态的顶部有一个线性层），例如用于ImageNet。

**种类**：[`models`](#module_models)的静态类

* * *

### dinov2ForImageClassification._call(model_inputs)

**种类**：[`models`](#module_models)的`Dinov2ForImageClassification`实例方法

| 参数 | 类型 |
| --- | --- |
| model_inputs | `any` |

* * *

## models.YolosObjectDetectionOutput

**种类**：[`models`](#module_models)的静态类

* * *

### 新的YolosObjectDetectionOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.logits | `Tensor` | 所有查询的分类logits（包括无对象）。 |
| output.pred_boxes | `Tensor` | 所有查询的标准化框坐标，表示为（中心_x，中心_y，宽度，高度）。这些值在[0, 1]范围内标准化，相对于批处理中每个单独图像的大小（忽略可能的填充）。 |

* * *

## models.SamModel

Segment Anything Model (SAM)用于生成分割蒙版，给定输入图像和可选的2D位置和边界框。

**示例:** 使用`Xenova/sam-vit-base`执行蒙版生成。

```py
import { SamModel, AutoProcessor, RawImage } from '@xenova/transformers';

const model = await SamModel.from_pretrained('Xenova/sam-vit-base');
const processor = await AutoProcessor.from_pretrained('Xenova/sam-vit-base');

const img_url = 'https://huggingface.co/ybelkada/segment-anything/resolve/main/assets/car.png';
const raw_image = await RawImage.read(img_url);
const input_points = [[[450, 600]]] // 2D localization of a window

const inputs = await processor(raw_image, input_points);
const outputs = await model(inputs);

const masks = await processor.post_process_masks(outputs.pred_masks, inputs.original_sizes, inputs.reshaped_input_sizes);
// [
//   Tensor {
//     dims: [ 1, 3, 1764, 2646 ],
//     type: 'bool',
//     data: Uint8Array(14002632) [ ... ],
//     size: 14002632
//   }
// ]
const scores = outputs.iou_scores;
// Tensor {
//   dims: [ 1, 1, 3 ],
//   type: 'float32',
//   data: Float32Array(3) [
//     0.8892380595207214,
//     0.9311248064041138,
//     0.983696699142456
//   ],
//   size: 3
// }
```

**类型**: [`models`](#module_models)的静态类

+   [.SamModel](#module_models.SamModel)

    +   [`new SamModel(config, vision_encoder, prompt_encoder_mask_decoder)`](#new_module_models.SamModel_new)

    +   [`.get_image_embeddings(model_inputs)`](#module_models.SamModel+get_image_embeddings) ⇒ `Promise.<{image_embeddings: Tensor, image_positional_embeddings: Tensor}>`

    +   [`.forward(model_inputs)`](#module_models.SamModel+forward) ⇒ `Promise.<Object>`

    +   [`._call(model_inputs)`](#module_models.SamModel+_call) ⇒ `Promise.<SamImageSegmentationOutput>`

* * *

### new SamModel(config, vision_encoder, prompt_encoder_mask_decoder)

创建`SamModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 指定超参数和其他模型设置的配置对象。 |
| vision_encoder | `Object` | 包含视觉编码器模型的ONNX会话。 |
| prompt_encoder_mask_decoder | `any` | 包含提示编码器和蒙版解码器模型的ONNX会话。 |

* * *

### samModel.get_image_embeddings(model_inputs) ⇒ <code> Promise. < {image_embeddings: Tensor, image_positional_embeddings: Tensor} > </code>

计算图像嵌入和位置图像嵌入，给定图像的像素值。

**类型**: [`SamModel`](#module_models)的实例方法

**返回**: `Promise.<{image_embeddings: Tensor, image_positional_embeddings: Tensor}>` - 图像嵌入和位置图像嵌入。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 包含模型输入的对象。 |
| model_inputs.pixel_values | `Tensor` | 使用`SamProcessor`获取的像素值。 |

* * *

### samModel.forward(model_inputs) ⇒ <code> Promise. < Object > </code>

**类型**: [`SamModel`](#module_models.SamModel)的实例方法

**返回**: `Promise.<Object>` - 模型的输出。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `SamModelInputs` | 包含模型输入的对象。 |

* * *

### samModel._call(model_inputs) ⇒ <code> Promise. < SamImageSegmentationOutput > </code>

运行提供的输入的模型

**类型**: [`SamModel`](#module_models.SamModel)的实例方法

**返回**: `Promise.<SamImageSegmentationOutput>` - 包含分割输出的对象

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型输入 |

* * *

## models.SamImageSegmentationOutput

Segment-Anything模型输出的基类。

**类型**: [`models`](#module_models)的静态类

* * *

### new SamImageSegmentationOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.iou_scores | `Tensor` | 模型的输出对数。 |
| output.pred_masks | `Tensor` | 预测的框。 |

* * *

## models.MarianMTModel

**类型**: [`models`](#module_models)的静态类

* * *

### new MarianMTModel(config, session, decoder_merged_session, generation_config)

创建`MarianMTModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| config | `Object` | 模型配置对象。 |
| session | `Object` | ONNX会话对象。 |
| decoder_merged_session | `any` |  |
| generation_config | `any` |  |

* * *

## models.M2M100ForConditionalGeneration

**类型**: [`models`](#module_models)的静态类

* * *

### new M2M100ForConditionalGeneration(config, session, decoder_merged_session, generation_config)

创建`M2M100ForConditionalGeneration`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `Object` | 模型配置对象。 |
| 会话 | `Object` | ONNX会话对象。 |
| decoder_merged_session | `any` |  |
| generation_config | `any` |  |

* * *

## models.Wav2Vec2Model

裸的Wav2Vec2模型变压器输出原始隐藏状态，没有特定的头部。

**示例：**加载并运行`Wav2Vec2Model`进行特征提取。

```py
import { AutoProcessor, AutoModel, read_audio } from '@xenova/transformers';

// Read and preprocess audio
const processor = await AutoProcessor.from_pretrained('Xenova/mms-300m');
const audio = await read_audio('https://huggingface.co/datasets/Narsil/asr_dummy/resolve/main/mlk.flac', 16000);
const inputs = await processor(audio);

// Run model with inputs
const model = await AutoModel.from_pretrained('Xenova/mms-300m');
const output = await model(inputs);
// {
//   last_hidden_state: Tensor {
//     dims: [ 1, 1144, 1024 ],
//     type: 'float32',
//     data: Float32Array(1171456) [ ... ],
//     size: 1171456
//   }
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

## models.Wav2Vec2BertModel

裸的Wav2Vec2Bert模型变压器输出原始隐藏状态，没有任何特定的头部。

**种类**：[`models`](#module_models)的静态类

* * *

## models.Wav2Vec2BertForCTC

Wav2Vec2Bert模型，顶部带有用于连接主义时间分类（CTC）的`语言建模`头。

**种类**：[`models`](#module_models)的静态类

* * *

### wav2Vec2BertForCTC._call(model_inputs)

**种类**：[`Wav2Vec2BertForCTC`](#module_models.Wav2Vec2BertForCTC)的实例方法

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` |  |
| model_inputs.input_features | `Tensor` | 输入梅尔频谱的浮点值。 |
| model_inputs.attention_mask | `Tensor` | 避免在填充标记索引上执行卷积和注意力的掩码。掩码值选择在[0, 1]中 |

* * *

## models.Wav2Vec2BertForSequenceClassification

Wav2Vec2Bert模型，顶部带有序列分类头（在汇总输出上的线性层）。

**种类**：[`models`](#module_models)的静态类

* * *

### wav2Vec2BertForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

在新输入上调用模型。

**种类**：[`Wav2Vec2BertForSequenceClassification`](#module_models.Wav2Vec2BertForSequenceClassification)

**返回**：`Promise.<SequenceClassifierOutput>` - 包含模型用于序列分类的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.HubertModel

裸的Hubert模型变压器输出原始隐藏状态，没有特定的头部。

**示例：**加载并运行`HubertModel`进行特征提取。

```py
import { AutoProcessor, AutoModel, read_audio } from '@xenova/transformers';

// Read and preprocess audio
const processor = await AutoProcessor.from_pretrained('Xenova/hubert-base-ls960');
const audio = await read_audio('https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/jfk.wav', 16000);
const inputs = await processor(audio);

// Load and run model with inputs
const model = await AutoModel.from_pretrained('Xenova/hubert-base-ls960');
const output = await model(inputs);
// {
//   last_hidden_state: Tensor {
//     dims: [ 1, 549, 768 ],
//     type: 'float32',
//     data: Float32Array(421632) [0.0682469978928566, 0.08104046434164047, -0.4975186586380005, ...],
//     size: 421632
//   }
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

## models.HubertForCTC

Hubert模型，顶部带有用于连接主义时间分类（CTC）的`语言建模`头。

**种类**：[`models`](#module_models)的静态类

* * *

### hubertForCTC._call(model_inputs)

**种类**：[`HubertForCTC`](#module_models.HubertForCTC)的实例方法

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` |  |
| model_inputs.input_values | `Tensor` | 输入原始语音波形的浮点值。 |
| model_inputs.attention_mask | `Tensor` | 避免在填充标记索引上执行卷积和注意力的掩码。掩码值选择在[0, 1]中 |

* * *

## models.HubertForSequenceClassification

Hubert模型，顶部带有序列分类头（在汇总输出上的线性层），用于任务如SUPERB关键词检测。

**种类**：[`models`](#module_models)的静态类

* * *

### hubertForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

在新输入上调用模型。

**种类**：[`HubertForSequenceClassification`](#module_models.HubertForSequenceClassification)的实例方法

**返回**：`Promise.<SequenceClassifierOutput>` - 包含模型用于序列分类的输出logits的对象。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `Object` | 模型的输入。 |

* * *

## models.WavLMPreTrainedModel

一个抽象类，用于处理权重初始化和一个简单的接口来下载和加载预训练模型。

**种类**：[`models`](#module_models)

* * *

## models.WavLMModel

裸的WavLM模型变压器输出原始隐藏状态，没有特定的头部。

**示例：**加载并运行`WavLMModel`进行特征提取。

```py
import { AutoProcessor, AutoModel, read_audio } from '@xenova/transformers';

// Read and preprocess audio
const processor = await AutoProcessor.from_pretrained('Xenova/wavlm-base');
const audio = await read_audio('https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/jfk.wav', 16000);
const inputs = await processor(audio);

// Run model with inputs
const model = await AutoModel.from_pretrained('Xenova/wavlm-base');
const output = await model(inputs);
// {
//   last_hidden_state: Tensor {
//     dims: [ 1, 549, 768 ],
//     type: 'float32',
//     data: Float32Array(421632) [-0.349443256855011, -0.39341306686401367,  0.022836603224277496, ...],
//     size: 421632
//   }
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

## models.WavLMForCTC

带有`语言建模`头部的WavLM模型，用于连接主义时间分类（CTC）。

**种类**：[`models`](#module_models)的静态类

* * *

### wavLMForCTC._call(model_inputs)

**种类**：[`WavLMForCTC`](#module_models.WavLMForCTC)的实例方法

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `对象` |  |
| model_inputs.input_values | `张量` | 输入原始语音波形的浮点值。 |
| model_inputs.attention_mask | `张量` | 避免在填充标记索引上执行卷积和注意力的掩码。掩码值选择在[0, 1]中 |

* * *

## models.WavLMForSequenceClassification

WavLM模型，顶部带有序列分类头（池化输出上的线性层）。

**种类**：[`models`](#module_models)的静态类

* * *

### wavLMForSequenceClassification._call(model_inputs) ⇒ <code> Promise. < SequenceClassifierOutput > </code>

对新输入调用模型。

**种类**：[`WavLMForSequenceClassification`](#module_models.WavLMForSequenceClassification)的实例方法

**返回**：`Promise.<SequenceClassifierOutput>` - 包含模型输出logits的对象，用于序列分类。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| model_inputs | `对象` | 模型的输入。 |

* * *

## models.SpeechT5PreTrainedModel

一个抽象类，用于处理权重初始化和下载和加载预训练模型的简单接口。

**种类**：[`models`](#module_models)的静态类

* * *

## models.SpeechT5Model

SpeechT5编码器-解码器模型裸输出原始隐藏状态，没有任何特定的前置或后置网络。

**种类**：[`models`](#module_models)的静态类

* * *

## models.SpeechT5ForSpeechToText

带有语音编码器和文本解码器的SpeechT5模型。

**示例：**使用`SpeechT5ForSpeechToText`从文本生成语音。

```py
import { AutoTokenizer, AutoProcessor, SpeechT5ForTextToSpeech, SpeechT5HifiGan, Tensor } from '@xenova/transformers';

// Load the tokenizer and processor
const tokenizer = await AutoTokenizer.from_pretrained('Xenova/speecht5_tts');
const processor = await AutoProcessor.from_pretrained('Xenova/speecht5_tts');

// Load the models
// NOTE: We use the unquantized versions as they are more accurate
const model = await SpeechT5ForTextToSpeech.from_pretrained('Xenova/speecht5_tts', { quantized: false });
const vocoder = await SpeechT5HifiGan.from_pretrained('Xenova/speecht5_hifigan', { quantized: false });

// Load speaker embeddings from URL
const speaker_embeddings_data = new Float32Array(
    await (await fetch('https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/speaker_embeddings.bin')).arrayBuffer()
);
const speaker_embeddings = new Tensor(
    'float32',
    speaker_embeddings_data,
    [1, speaker_embeddings_data.length]
)

// Run tokenization
const { input_ids } = tokenizer('Hello, my dog is cute');

// Generate waveform
const { waveform } = await model.generate_speech(input_ids, speaker_embeddings, { vocoder });
console.log(waveform)
// Tensor {
//   dims: [ 26112 ],
//   type: 'float32',
//   size: 26112,
//   data: Float32Array(26112) [ -0.00043630177970044315, -0.00018082228780258447, ... ],
// }
```

**种类**：[`models`](#module_models)的静态类

* * *

## models.SpeechT5ForTextToSpeech

带有文本编码器和语音解码器的SpeechT5模型。

**种类**：[`models`](#module_models)的静态类

+   [.SpeechT5ForTextToSpeech](#module_models.SpeechT5ForTextToSpeech)

    +   [`new SpeechT5ForTextToSpeech(config, session, decoder_merged_session, generation_config)`](#new_module_models.SpeechT5ForTextToSpeech_new)

    +   [`.generate_speech(input_values, speaker_embeddings, options)`](#module_models.SpeechT5ForTextToSpeech+generate_speech) ⇒ `Promise.<SpeechOutput>`

* * *

### new SpeechT5ForTextToSpeech(config, session, decoder_merged_session, generation_config)

创建`SpeechT5ForTextToSpeech`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `对象` | 模型配置。 |
| 会话 | `任意` | 模型的会话。 |
| decoder_merged_session | `任意` | 解码器的会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

### speechT5ForTextToSpeech.generate_speech(input_values, speaker_embeddings, options) ⇒ <code> Promise. < SpeechOutput > </code>

将一系列输入标记转换为一系列梅尔频谱图，随后使用声码器将其转换为语音波形。

**种类**：[`SpeechT5ForTextToSpeech`](#module_models.SpeechT5ForTextToSpeech)的实例方法

**返回**：`Promise.<SpeechOutput>` - 一个包含频谱图、波形和交叉注意张量的对象的Promise。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| input_values | `张量` |  | 词汇表中输入序列标记的索引。 |
| speaker_embeddings | `张量` |  | 包含说话者嵌入的张量。 |
| 选项 | `对象` |  | 生成语音的可选参数。 |
| [选项.threshold] | `数字` | `0.5` | 当预测的停止标记概率超过此值时，生成的序列结束。 |
| [选项.minlenratio] | `数字` | `0.0` | 用于计算输出序列所需的最小长度。 |
| [选项.maxlenratio] | `数字` | `20.0` | 用于计算输出序列的最大允许长度。 |
| [选项.声码器] | `对象` | 将梅尔频谱图转换为语音波形的声码器。如果为`null`，则输出为梅尔频谱图。 |
| [选项.输出交叉注意力] | `布尔` | `false` | 是否返回解码器的交叉注意力层的注意力张量。 |

* * *

## 模型.SpeechT5HifiGan

HiFi-GAN声码器。

查看[SpeechT5ForSpeechToText](./models#module_models.SpeechT5ForSpeechToText)以获取示例用法。

**类型**：[`models`](#module_models)的静态类

* * *

## 模型.TrOCRPreTrainedModel

**类型**：[`models`](#module_models)的静态类

* * *

### 新的TrOCRPreTrainedModel(config, session, generation_config)

创建`TrOCRPreTrainedModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `对象` | 模型的配置。 |
| 会话 | `任意` | 包含模型权重的ONNX会话。 |
| generation_config | `GenerationConfig` | 生成配置。 |

* * *

## 模型.TrOCRForCausalLM

带有语言建模头的TrOCR解码器。

**类型**：[`models`](#module_models)的静态类

* * *

## 模型.MistralPreTrainedModel

裸Mistral模型输出原始隐藏状态，没有特定的顶部头。

**类型**：[`models`](#module_models)的静态类

* * *

### 新的MistralPreTrainedModel(config, session, generation_config)

创建`MistralPreTrainedModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `对象` | 模型的配置。 |
| 会话 | `任意` | 包含模型权重的ONNX会话。 |
| 生成配置 | `GenerationConfig` | 生成配置。 |

* * *

## 模型.FalconPreTrainedModel

裸Falcon模型输出原始隐藏状态，没有特定的顶部头。

**类型**：[`models`](#module_models)的静态类

* * *

### 新的FalconPreTrainedModel(config, session, generation_config)

创建`FalconPreTrainedModel`类的新实例。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 配置 | `对象` | 模型的配置。 |
| 会话 | `任意` | 包含模型权重的ONNX会话。 |
| 生成配置 | `GenerationConfig` | 生成配置。 |

* * *

## 模型.ClapTextModelWithProjection

CLAP文本模型，顶部带有投影层（在池化输出的顶部有一个线性层）。

**示例：** 使用`ClapTextModelWithProjection`计算文本嵌入。

```py
import { AutoTokenizer, ClapTextModelWithProjection } from '@xenova/transformers';

// Load tokenizer and text model
const tokenizer = await AutoTokenizer.from_pretrained('Xenova/clap-htsat-unfused');
const text_model = await ClapTextModelWithProjection.from_pretrained('Xenova/clap-htsat-unfused');

// Run tokenization
const texts = ['a sound of a cat', 'a sound of a dog'];
const text_inputs = tokenizer(texts, { padding: true, truncation: true });

// Compute embeddings
const { text_embeds } = await text_model(text_inputs);
// Tensor {
//   dims: [ 2, 512 ],
//   type: 'float32',
//   data: Float32Array(1024) [ ... ],
//   size: 1024
// }
```

**类型**：[`models`](#module_models)的静态类

* * *

### ClapTextModelWithProjection.from_pretrained()：`PreTrainedModel.from_pretrained`

**类型**：[`ClapTextModelWithProjection`](#module_models.ClapTextModelWithProjection)的静态方法

* * *

## 模型.ClapAudioModelWithProjection

带有投影层的CLAP音频模型（在池化输出的顶部有一个线性层）。

**示例：** 使用`ClapAudioModelWithProjection`计算音频嵌入。

```py
import { AutoProcessor, ClapAudioModelWithProjection, read_audio } from '@xenova/transformers';

// Load processor and audio model
const processor = await AutoProcessor.from_pretrained('Xenova/clap-htsat-unfused');
const audio_model = await ClapAudioModelWithProjection.from_pretrained('Xenova/clap-htsat-unfused');

// Read audio and run processor
const audio = await read_audio('https://huggingface.co/datasets/Xenova/transformers.js-docs/resolve/main/cat_meow.wav');
const audio_inputs = await processor(audio);

// Compute embeddings
const { audio_embeds } = await audio_model(audio_inputs);
// Tensor {
//   dims: [ 1, 512 ],
//   type: 'float32',
//   data: Float32Array(512) [ ... ],
//   size: 512
// }
```

**类型**：[`models`](#module_models)的静态类

* * *

### ClapAudioModelWithProjection.from_pretrained()：`PreTrainedModel.from_pretrained`

**类型**：[`ClapAudioModelWithProjection`](#module_models.ClapAudioModelWithProjection)的静态方法

* * *

## 模型.VitsModel

完整的VITS模型，用于文本到语音合成。

**示例：** 使用`VitsModel`从文本生成语音。

```py
import { AutoTokenizer, VitsModel } from '@xenova/transformers';

// Load the tokenizer and model
const tokenizer = await AutoTokenizer.from_pretrained('Xenova/mms-tts-eng');
const model = await VitsModel.from_pretrained('Xenova/mms-tts-eng');

// Run tokenization
const inputs = tokenizer('I love transformers');

// Generate waveform
const { waveform } = await model(inputs);
// Tensor {
//   dims: [ 1, 35328 ],
//   type: 'float32',
//   data: Float32Array(35328) [ ... ],
//   size: 35328,
// }
```

**类型**：[`models`](#module_models)的静态类

* * *

### vitsModel._call(model_inputs) ⇒ `Promise.<VitsModelOutput>`

在新输入上调用模型。

**类型**：[`VitsModel`](#module_models.VitsModel)的实例方法

**返回**：`Promise.<VitsModelOutput>` - VITS模型的输出。

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| 模型输入 | `对象` | 模型的输入。 |

* * *

## 模型.SegformerModel

裸SegFormer编码器（Mix-Transformer）输出原始隐藏状态，没有特定的顶部头。

**类型**：[`models`](#module_models)的静态类

* * *

## models.SegformerForImageClassification

SegFormer模型变压器，顶部带有图像分类头（最终隐藏状态顶部的线性层），例如用于ImageNet。

**种类**：[`models`](#module_models)的静态类

* * *

## models.SegformerForSemanticSegmentation

SegFormer模型变压器，顶部带有全MLP解码头，例如用于ADE20k，CityScapes。

**种类**：[`models`](#module_models)的静态类

* * *

## models.PretrainedMixin

所有AutoModels的基类。包含用于实例化预训练模型的`from_pretrained`函数。

**种类**：[`models`](#module_models)的静态类

+   [.PretrainedMixin](#module_models.PretrainedMixin)

    +   *实例*

        +   [`.MODEL_CLASS_MAPPINGS`](#module_models.PretrainedMixin+MODEL_CLASS_MAPPINGS)：`*`

        +   [`.BASE_IF_FAIL`](#module_models.PretrainedMixin+BASE_IF_FAIL)

    +   *静态*

        +   [`.from_pretrained()`](#module_models.PretrainedMixin.from_pretrained)：`PreTrainedModel.from_pretrained`

* * *

### pretrainedMixin.MODEL_CLASS_MAPPINGS：`*`

从模型类型到模型类的映射。

**种类**：[`PretrainedMixin`](#module_models.PretrainedMixin)的实例属性

* * *

### pretrainedMixin.BASE_IF_FAIL

是否尝试实例化基类（`PretrainedModel`）如果在映射中找不到模型类型。

**种类**：[`PretrainedMixin`](#module_models.PretrainedMixin)的实例属性

* * *

### PretrainedMixin.from_pretrained()：`PreTrainedModel.from_pretrained`

**种类**：[`PretrainedMixin`](#module_models.PretrainedMixin)的静态方法

* * *

## models.AutoModel

用于使用`from_pretrained`函数实例化预训练模型的辅助类。所选的模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

### autoModel.MODEL_CLASS_MAPPINGS：`*`

**种类**：[`AutoModel`](#module_models.AutoModel)的实例属性

* * *

## models.AutoModelForSequenceClassification

用于使用`from_pretrained`函数实例化预训练序列分类模型的辅助类。所选的模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForTokenClassification

用于使用`from_pretrained`函数实例化预训练标记分类模型的辅助类。所选的模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForSeq2SeqLM

用于使用`from_pretrained`函数实例化预训练序列到序列模型的辅助类。所选的模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForSpeechSeq2Seq

用于使用`from_pretrained`函数实例化预训练序列到序列语音到文本模型的辅助类。所选的模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForTextToSpectrogram

用于使用`from_pretrained`函数实例化预训练序列到序列文本到频谱图模型的辅助类。所选的模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForTextToWaveform

用于使用`from_pretrained`函数实例化预训练文本到波形模型的辅助类。所选的模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForCausalLM

用于使用`from_pretrained`函数实例化预训练因果语言模型的辅助类。所选的模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForMaskedLM

用于使用`from_pretrained`函数实例化预训练掩蔽语言模型的辅助类。所选模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForQuestionAnswering

用于使用`from_pretrained`函数实例化预训练问答模型的辅助类。所选模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForVision2Seq

用于使用`from_pretrained`函数实例化预训练视觉到序列模型的辅助类。所选模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForImageClassification

用于使用`from_pretrained`函数实例化预训练图像分类模型的辅助类。所选模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForImageSegmentation

用于使用`from_pretrained`函数实例化预训练图像分割模型的辅助类。所选模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForSemanticSegmentation

用于使用`from_pretrained`函数实例化预训练图像分割模型的辅助类。所选模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForObjectDetection

用于使用`from_pretrained`函数实例化预训练目标检测模型的辅助类。所选模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.AutoModelForMaskGeneration

用于使用`from_pretrained`函数实例化预训练掩蔽生成模型的辅助类。所选模型类由模型配置中指定的类型确定。

**种类**：[`models`](#module_models)的静态类

* * *

## models.Seq2SeqLMOutput

**种类**：[`models`](#module_models)的静态类

* * *

### 新的Seq2SeqLMOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.logits | `Tensor` | 模型的输出logits。 |
| output.past_key_values | `Tensor` | 代表模型先前状态的键/值对张量。 |
| output.encoder_outputs | `Tensor` | 序列到序列模型中编码器的输出。 |
| [output.decoder_attentions] | `Tensor` | 解码器的注意力权重，在注意力softmax之后，用于计算自注意力头中的加权平均。 |
| [output.cross_attentions] | `Tensor` | 解码器交叉注意力层的注意力权重，在注意力softmax之后，用于计算交叉注意力头中的加权平均。 |

* * *

## models.SequenceClassifierOutput

用于句子分类模型输出的基类。

**种类**：[`models`](#module_models)的静态类

* * *

### 新的SequenceClassifierOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.logits | `Tensor` | 分类（如果config.num_labels==1则为回归）得分（SoftMax之前）。 |

* * *

## models.TokenClassifierOutput

用于标记分类模型输出的基类。

**种类**：[`models`](#module_models)的静态类

* * *

### 新的TokenClassifierOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.logits | `Tensor` | 分类得分（SoftMax之前）。 |

* * *

## models.MaskedLMOutput

用于掩蔽语言模型输出的基类。

**种类**：[`models`](#module_models)的静态类

* * *

### new MaskedLMOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.logits | `Tensor` | 语言建模头的预测分数（SoftMax之前每个词汇标记的分数）。 |

* * *

## models.QuestionAnsweringModelOutput

用于问答模型输出的基类。

**种类**：[`models`](#module_models)的静态类

* * *

### new QuestionAnsweringModelOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.start_logits | `Tensor` | 跨度开始分数（SoftMax之前）。 |
| output.end_logits | `Tensor` | 跨度结束分数（SoftMax之前）。 |

* * *

## models.CausalLMOutput

用于因果语言模型（或自回归）输出的基类。

**种类**：[`models`](#module_models)的静态类

* * *

### new CausalLMOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.logits | `Tensor` | 语言建模头的预测分数（SoftMax之前每个词汇标记的分数）。 |

* * *

## models.CausalLMOutputWithPast

用于因果语言模型（或自回归）输出的基类。

**种类**：[`models`](#module_models)的静态类

* * *

### new CausalLMOutputWithPast(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.logits | `Tensor` | 语言建模头的预测分数（SoftMax之前每个词汇标记的分数）。 |
| output.past_key_values | `Tensor` | 包含预先计算的隐藏状态（自注意力块中的键和值），可用于加速顺序解码（参见`past_key_values`输入）。 |

* * *

## models.ImageMattingOutput

**种类**：[`models`](#module_models)的静态类

* * *

### new ImageMattingOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.alphas | `Tensor` | 估计的alpha值，形状为`(batch_size, num_channels, height, width)`。 |

* * *

## models.VitsModelOutput

描述VITS模型的输出。

**种类**：[`models`](#module_models)的静态类

* * *

### new VitsModelOutput(output)

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| output | `Object` | 模型的输出。 |
| output.waveform | `Tensor` | 模型预测的最终音频波形，形状为`(batch_size, sequence_length)`。 |
| output.spectrogram | `Tensor` | 在流模型输出处预测的对数梅尔频谱图。将此频谱图传递给Hi-Fi GAN解码器模型以获得最终音频波形。 |

* * *

## models~InferenceSession： <code> * </code>

**种类**：[`models`](#module_models)的内部类型定义

* * *

## models~TypedArray： <code> * </code>

**种类**：[`models`](#module_models)的内部类型定义

* * *

## models~DecoderOutput ⇒ <code> Promise. < (Array < Array < number > > |EncoderDecoderOutput|DecoderOutput) > </code>

根据给定的输入和生成配置使用模型生成文本。

**种类**：[`models`](#module_models)的内部类型定义

**返回**：`Promise.<(Array<Array<number>>|EncoderDecoderOutput|DecoderOutput)>` - 生成的输出序列的数组，其中每个序列都是一个标记ID数组。

**抛出**：

+   `Error` 如果输入数组为空，则抛出错误。

| 参数 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| 输入 | `Tensor` &#124; `Array` &#124; `TypedArray` |  | 输入标记ID的数组。 |
| generation_config | `Object` &#124; `GenerationConfig` &#124; `null` |  | 要使用的生成配置。如果为null，将使用默认配置。 |
| logits_processor | `Object` &#124; `null` |  | 要使用的可选logits处理器。如果为null，将创建一个新的LogitsProcessorList实例。 |
| 选项 | `Object` |  | 选项 |
| [options.inputs_attention_mask] | `对象` |  | 输入的可选注意力掩码。 |

* * *

## models~WhisperGenerationConfig： <code>对象</code>

**种类**：[`models`](#module_models)的内部类型定义

**扩展**：`GenerationConfig`

**属性**

| 名称 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| [return_timestamps] | `boolean` |  | 是否返回时间戳与文本。这使得`WhisperTimestampsLogitsProcessor`生效。 |
| [return_token_timestamps] | `boolean` |  | 是否返回标记级时间戳与文本。这可以与`return_timestamps`选项一起使用。要获得单词级时间戳，请使用分词器将标记分组成单词。 |
| [num_frames] | `number` |  | 此块中可用的音频帧数。这仅用于生成单词级时间戳。 |

* * *

## models~SamModelInputs： <code>对象</code>

包含模型输入的对象。

**种类**：[`models`](#module_models)的内部类型定义

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| pixel_values | `Tensor` | 像素值作为形状为`(batch_size, num_channels, height, width)`的张量。这些可以使用`SamProcessor`获得。 |
| input_points | `Tensor` | 具有形状`(batch_size, num_points, 2)`的输入2D空间点。这由提示编码器用于编码提示。 |

| [input_labels] | `Tensor` | 点的输入标签，作为形状为`(batch_size, point_batch_size, num_points)`的张量。这由提示编码器用于编码提示。有4种类型的标签：

+   `1`：该点是包含感兴趣对象的点

+   `0`：该点是不包含感兴趣对象的点

+   `-1`：该点对应于背景

+   `-10`：该点是填充点，因此应该被提示编码器忽略

|

| [image_embeddings] | `Tensor` | 被掩码解码器使用的图像嵌入。 |
| --- | --- | --- |
| [image_positional_embeddings] | `Tensor` | 被掩码解码器使用的图像位置嵌入。 |

* * *

## models~SpeechOutput： <code>对象</code>

**种类**：[`models`](#module_models)的内部类型定义

**属性**

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| [spectrogram] | `Tensor` | 形状为`(output_sequence_length, config.num_mel_bins)`的预测对数梅尔频谱图。在没有提供`vocoder`时返回 |
| [waveform] | `Tensor` | 形状为`(num_frames,)`的预测波形。在提供`vocoder`时返回。 |
| [cross_attentions] | `Tensor` | 形状为`(config.decoder_layers, config.decoder_attention_heads, output_sequence_length, input_sequence_length)`的解码器交叉注意力层的输出。当`output_cross_attentions`为`true`时返回。 |

* * *
