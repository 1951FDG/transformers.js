# Transformers.js

[![npm](https://img.shields.io/npm/v/@xenova/transformers)](https://www.npmjs.com/package/@xenova/transformers)
[![downloads](https://img.shields.io/npm/dw/@xenova/transformers)](https://www.npmjs.com/package/@xenova/transformers)
[![license](https://img.shields.io/github/license/xenova/transformers.js)](https://github.com/xenova/transformers.js/blob/main/LICENSE)


State-of-the-art Machine Learning for the web. Run 🤗 Transformers directly in your browser, with no need for a server!

Transformers.js is designed to be functionally equivalent to Hugging Face's [transformers](https://github.com/huggingface/transformers) python library, meaning you can run the same pretrained models using a very similar API. These models support common tasks in different modalities, such as:
  - 📝 **Natural Language Processing**: text classification, named entity recognition, question answering, language modeling, summarization, translation, multiple choice, and text generation.
  - 🖼️ **Computer Vision**: image classification, object detection, and segmentation.
  - 🗣️ **Audio**: automatic speech recognition and audio classification.
  - 🐙 **Multimodal**: zero-shot image classification.

Transformers.js uses [ONNX Runtime](https://onnxruntime.ai/) to run models in the browser. The best part about it, is that you can easily [convert](#convert-your-models-to-onnx) your pretrained PyTorch, TensorFlow, or JAX models to ONNX using [🤗 Optimum](https://github.com/huggingface/optimum#onnx--onnx-runtime). 


For more information, check out the full [documentation](https://huggingface.co/docs/transformers.js).


## Quick tour

It's super simple to translate from existing code! Just like the python library, we support the `pipeline` API. Pipelines group together a pretrained model with preprocessing of inputs and postprocessing of outputs, making it the easiest way to run models with the library.

<table>
<tr>
<th width="440px"><b>Python (original)</b></th>
<th width="440px"><b>Javascript (ours)</b></th>
</tr>
<tr>
<td>

```python
from transformers import pipeline

# Allocate a pipeline for sentiment-analysis
pipe = pipeline('sentiment-analysis')

out = pipe('I love transformers!')
# [{'label': 'POSITIVE', 'score': 0.999806941}]
```

</td>
<td>

```javascript
import { pipeline } from "@xenova/transformers";

// Allocate a pipeline for sentiment-analysis
let pipe = await pipeline('sentiment-analysis');

let out = await pipe('I love transformers!');
// [{'label': 'POSITIVE', 'score': 0.999817686}]
```

</td>
</tr>
</table>


You can also use a different model by specifying the model id or path as the second argument to the `pipeline` function. For example:
```javascript
// Use a different model for sentiment-analysis
let pipe = await pipeline('sentiment-analysis', 'nlptown/bert-base-multilingual-uncased-sentiment');
```


## Installation
To install via [NPM](https://www.npmjs.com/package/@xenova/transformers), run:
```bash
npm i @xenova/transformers
```

Alternatively, you can use it in vanilla JS, without any bundler, by using a CDN or static hosting. For example, using [ES Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules), you can import the library with:
```html
<script type="module">
    import { pipeline } from 'https://cdn.jsdelivr.net/npm/@xenova/transformers@2.0.0/';
</script>
```

## Examples
**[TODO]** Want to jump straight in? Get started with one of our sample applications/templates:

| Platform          | Description | Source code                   |
|-------------------|-------------|-------------------------------|
| Vanilla JS        | TODO        | [link](./examples/demo-site/) |
| Browser extension | TODO        | [link](./examples/extension/) |
| React             | TODO        | [link](./examples/react/)     |
| Electron          | TODO        | [link](./examples/electron/)  |
| Next.js           | TODO        | [link](./examples/next/)      |
| Node.js           | TODO        | [link](./examples/node/)      |



## Custom usage
By default, Transformers.js uses [hosted pretrained models](https://huggingface.co/models) and [precompiled WASM binaries](https://cdn.jsdelivr.net/npm/@xenova/transformers/dist/), which should work out-of-the-box. You can customize this as follows:


### Settings

```javascript
import { env } from "@xenova/transformers";

// Specify a custom location for models (defaults to '/models/').
env.localModelPath = '/path/to/models/';

// Disable the loading of remote models from the Hugging Face Hub:
env.allowRemoteModels = false;

// Set location of .wasm files. Defaults to use a CDN.
env.backends.onnx.wasm.wasmPaths = '/path/to/files/';
```

For a full list of available settings, check out the [docs](https://huggingface.co/docs/transformers.js).

### Convert your models to ONNX
We recommend using our [conversion script](./scripts/convert.py) to convert your PyTorch, TensorFlow, or JAX models to ONNX. Behind the scenes, it uses [🤗 Optimum](https://huggingface.co/docs/optimum) to perform conversion and quantization of your model in a single command:

```bash
python -m scripts.convert --quantize --model_id <model_name_or_path>
```

For example, convert and quantize [bert-base-uncased](https://huggingface.co/bert-base-uncased) using:
```bash
python -m scripts.convert --quantize --model_id bert-base-uncased
```

This will save the following files to `./models/`:

```
bert-base-uncased/
├── config.json
├── tokenizer.json
├── tokenizer_config.json
└── onnx/
    ├── model.onnx
    └── model_quantized.onnx
```

Check out the [documentation](https://huggingface.co/docs/transformers.js) for the full list of options.

## Supported tasks/models

Here is the list of all tasks and models currently supported by Transformers.js. If you don't see your task/model listed here or it is not yet supported, feel free to open up a feature request [here](https://github.com/xenova/transformers.js/issues/new/choose).


### Tasks
#### Natual Language Processing

| Task                     | ID | Description | Supported? |
|--------------------------|----|-------------|------------|
| [Conversational](https://huggingface.co/tasks/conversational)           | `conversational`  |           | ❌ |
| [Fill-Mask](https://huggingface.co/tasks/fill-mask)                     | `fill-mask`   |               | ✅ |
| [Question Answering](https://huggingface.co/tasks/question-answering)   | `question-answering`   |      | ✅ |
| [Sentence Similarity](https://huggingface.co/tasks/sentence-similarity) | `sentence-similarity`  |      | ✅ |
| [Summarization](https://huggingface.co/tasks/summarization)             |  `summarization`  |           | ✅ |
| [Table Question Answering](https://huggingface.co/tasks/table-question-answering) |  `table-question-answering`  |             | ❌ |
| [Text Classification](https://huggingface.co/tasks/text-classification)      | `text-classification` or `sentiment-analysis`  |             | ✅ |
| [Text Generation](https://huggingface.co/tasks/text-generation#completion-generation-models)          | `text-generation`  |  | ✅ |
| [Text-to-text Generation](https://huggingface.co/tasks/text-generation#text-to-text-generation-models)  | `text2text-generation`  |             | ✅ |
| [Token Classification](https://huggingface.co/tasks/token-classification)     | `token-classification` or `ner`  |             | ✅ |
| [Translation](https://huggingface.co/tasks/translation)              |  `translation`  |             | ✅ |
| [Zero-Shot Classification](https://huggingface.co/tasks/zero-shot-classification) | `zero-shot-classification`  |             | ✅ |

#### Vision

| Task                     | ID | Description | Supported? |
|--------------------------|----|-------------|------------|
| [Depth Estimation](https://huggingface.co/tasks/depth-estimation)         |  `depth-estimation`  |             | ❌ |
| [Image Classification](https://huggingface.co/tasks/image-classification)                | `image-classification`   |             | ✅ |
| [Image Segmentation](https://huggingface.co/tasks/image-segmentation)       | `image-segmentation`   |             | ✅ |
| [Image-to-Image](https://huggingface.co/tasks/image-to-image)      |  `image-to-image` |             | ❌ |
| [Mask Generation](https://huggingface.co/tasks/mask-generation)            |  `mask-generation`  |             | ❌ |
| [Object Detection](https://huggingface.co/tasks/object-detection)            | `object-detection`   |             | ✅ |
| [Video Classification](https://huggingface.co/tasks/video-classification) |  n/a  |             | ❌ |
| [Unconditional Image Generation](https://huggingface.co/tasks/unconditional-image-generation)      |  n/a   |             | ❌ |

#### Audio
| Task                     | ID | Description | Supported? |
|--------------------------|----|-------------|------------|
| [Audio Classification](https://huggingface.co/tasks/audio-classification)         |  `audio-classification`  |             | ❌ |
| [Audio-to-Audio](https://huggingface.co/tasks/audio-to-audio)         |  n/a  |             | ❌ |
| [Automatic Speech Recognition](https://huggingface.co/tasks/automatic-speech-recognition)         | `automatic-speech-recognition`  |    | ✅ |
| [Text-to-Speech](https://huggingface.co/tasks/text-to-speech)         |  n/a  |             | ❌ |


#### Tabular
| Task                     | ID | Description | Supported? |
|--------------------------|----|-------------|------------|
| [Tabular Classification](https://huggingface.co/tasks/tabular-classification)         |  n/a  |             | ❌ |
| [Tabular Regression](https://huggingface.co/tasks/tabular-regression)         |  n/a  |             | ❌ |


#### Multimodal
| Task                     | ID | Description | Supported? |
|--------------------------|----|-------------|------------|
| [Document Question Answering](https://huggingface.co/tasks/document-question-answering)         | `document-question-answering`  |             | ❌ |
| [Feature Extraction](https://huggingface.co/tasks/feature-extraction)         |  `feature-extraction`  |             | ✅ |
| [Image-to-Text](https://huggingface.co/tasks/image-to-text)         |  `image-to-text`  |             | ✅ |
| [Text-to-Image](https://huggingface.co/tasks/text-to-image)         |  `text-to-image`  |             | ❌ |
| [Visual Question Answering](https://huggingface.co/tasks/visual-question-answering)         |  `visual-question-answering`  |             | ❌ |
| [Zero-Shot Image Classification](https://huggingface.co/tasks/zero-shot-image-classification) | `zero-shot-image-classification`  |             | ✅ |


#### Reinforcement Learning
| Task                     | ID | Description | Supported? |
|--------------------------|----|-------------|------------|
| Reinforcement Learning   |  n/a  |             | ❌ |


### Models
1. **[ALBERT](https://huggingface.co/docs/transformers/model_doc/albert)** (from Google Research and the Toyota Technological Institute at Chicago) released with the paper [ALBERT: A Lite BERT for Self-supervised Learning of Language Representations](https://arxiv.org/abs/1909.11942), by Zhenzhong Lan, Mingda Chen, Sebastian Goodman, Kevin Gimpel, Piyush Sharma, Radu Soricut.
1. **[BART](https://huggingface.co/docs/transformers/model_doc/bart)** (from Facebook) released with the paper [BART: Denoising Sequence-to-Sequence Pre-training for Natural Language Generation, Translation, and Comprehension](https://arxiv.org/abs/1910.13461) by Mike Lewis, Yinhan Liu, Naman Goyal, Marjan Ghazvininejad, Abdelrahman Mohamed, Omer Levy, Ves Stoyanov and Luke Zettlemoyer.
1. **[BERT](https://huggingface.co/docs/transformers/model_doc/bert)** (from Google) released with the paper [BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding](https://arxiv.org/abs/1810.04805) by Jacob Devlin, Ming-Wei Chang, Kenton Lee and Kristina Toutanova.
1. **[CLIP](https://huggingface.co/docs/transformers/model_doc/clip)** (from OpenAI) released with the paper [Learning Transferable Visual Models From Natural Language Supervision](https://arxiv.org/abs/2103.00020) by Alec Radford, Jong Wook Kim, Chris Hallacy, Aditya Ramesh, Gabriel Goh, Sandhini Agarwal, Girish Sastry, Amanda Askell, Pamela Mishkin, Jack Clark, Gretchen Krueger, Ilya Sutskever.
1. **[CodeGen](https://huggingface.co/docs/transformers/model_doc/codegen)** (from Salesforce) released with the paper [A Conversational Paradigm for Program Synthesis](https://arxiv.org/abs/2203.13474) by Erik Nijkamp, Bo Pang, Hiroaki Hayashi, Lifu Tu, Huan Wang, Yingbo Zhou, Silvio Savarese, Caiming Xiong.
1. **[DETR](https://huggingface.co/docs/transformers/model_doc/detr)** (from Facebook) released with the paper [End-to-End Object Detection with Transformers](https://arxiv.org/abs/2005.12872) by Nicolas Carion, Francisco Massa, Gabriel Synnaeve, Nicolas Usunier, Alexander Kirillov, Sergey Zagoruyko.
1. **[DistilBERT](https://huggingface.co/docs/transformers/model_doc/distilbert)** (from HuggingFace), released together with the paper [DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter](https://arxiv.org/abs/1910.01108) by Victor Sanh, Lysandre Debut and Thomas Wolf. The same method has been applied to compress GPT2 into [DistilGPT2](https://github.com/huggingface/transformers/tree/main/examples/research_projects/distillation), RoBERTa into [DistilRoBERTa](https://github.com/huggingface/transformers/tree/main/examples/research_projects/distillation), Multilingual BERT into [DistilmBERT](https://github.com/huggingface/transformers/tree/main/examples/research_projects/distillation) and a German version of DistilBERT.
1. **[FLAN-T5](https://huggingface.co/docs/transformers/model_doc/flan-t5)** (from Google AI) released in the repository [google-research/t5x](https://github.com/google-research/t5x/blob/main/docs/models.md#flan-t5-checkpoints) by Hyung Won Chung, Le Hou, Shayne Longpre, Barret Zoph, Yi Tay, William Fedus, Eric Li, Xuezhi Wang, Mostafa Dehghani, Siddhartha Brahma, Albert Webson, Shixiang Shane Gu, Zhuyun Dai, Mirac Suzgun, Xinyun Chen, Aakanksha Chowdhery, Sharan Narang, Gaurav Mishra, Adams Yu, Vincent Zhao, Yanping Huang, Andrew Dai, Hongkun Yu, Slav Petrov, Ed H. Chi, Jeff Dean, Jacob Devlin, Adam Roberts, Denny Zhou, Quoc V. Le, and Jason Wei
1. **[GPT Neo](https://huggingface.co/docs/transformers/model_doc/gpt_neo)** (from EleutherAI) released in the repository [EleutherAI/gpt-neo](https://github.com/EleutherAI/gpt-neo) by Sid Black, Stella Biderman, Leo Gao, Phil Wang and Connor Leahy.
1. **[GPT-2](https://huggingface.co/docs/transformers/model_doc/gpt2)** (from OpenAI) released with the paper [Language Models are Unsupervised Multitask Learners](https://blog.openai.com/better-language-models/) by Alec Radford*, Jeffrey Wu*, Rewon Child, David Luan, Dario Amodei** and Ilya Sutskever**.
1. **[MarianMT](https://huggingface.co/docs/transformers/model_doc/marian)** Machine translation models trained using [OPUS](http://opus.nlpl.eu/) data by Jörg Tiedemann. The [Marian Framework](https://marian-nmt.github.io/) is being developed by the Microsoft Translator Team.
1. **[MobileBERT](https://huggingface.co/docs/transformers/model_doc/mobilebert)** (from CMU/Google Brain) released with the paper [MobileBERT: a Compact Task-Agnostic BERT for Resource-Limited Devices](https://arxiv.org/abs/2004.02984) by Zhiqing Sun, Hongkun Yu, Xiaodan Song, Renjie Liu, Yiming Yang, and Denny Zhou.
1. **[MT5](https://huggingface.co/docs/transformers/model_doc/mt5)** (from Google AI) released with the paper [mT5: A massively multilingual pre-trained text-to-text transformer](https://arxiv.org/abs/2010.11934) by Linting Xue, Noah Constant, Adam Roberts, Mihir Kale, Rami Al-Rfou, Aditya Siddhant, Aditya Barua, Colin Raffel.
1. **[SqueezeBERT](https://huggingface.co/docs/transformers/model_doc/squeezebert)** (from Berkeley) released with the paper [SqueezeBERT: What can computer vision teach NLP about efficient neural networks?](https://arxiv.org/abs/2006.11316) by Forrest N. Iandola, Albert E. Shaw, Ravi Krishna, and Kurt W. Keutzer.
1. **[T5](https://huggingface.co/docs/transformers/model_doc/t5)** (from Google AI) released with the paper [Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer](https://arxiv.org/abs/1910.10683) by Colin Raffel and Noam Shazeer and Adam Roberts and Katherine Lee and Sharan Narang and Michael Matena and Yanqi Zhou and Wei Li and Peter J. Liu.
1. **[T5v1.1](https://huggingface.co/docs/transformers/model_doc/t5v1.1)** (from Google AI) released in the repository [google-research/text-to-text-transfer-transformer](https://github.com/google-research/text-to-text-transfer-transformer/blob/main/released_checkpoints.md#t511) by Colin Raffel and Noam Shazeer and Adam Roberts and Katherine Lee and Sharan Narang and Michael Matena and Yanqi Zhou and Wei Li and Peter J. Liu.
1. **[Vision Transformer (ViT)](https://huggingface.co/docs/transformers/model_doc/vit)** (from Google AI) released with the paper [An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale](https://arxiv.org/abs/2010.11929) by Alexey Dosovitskiy, Lucas Beyer, Alexander Kolesnikov, Dirk Weissenborn, Xiaohua Zhai, Thomas Unterthiner, Mostafa Dehghani, Matthias Minderer, Georg Heigold, Sylvain Gelly, Jakob Uszkoreit, Neil Houlsby.
1. **[Whisper](https://huggingface.co/docs/transformers/model_doc/whisper)** (from OpenAI) released with the paper [Robust Speech Recognition via Large-Scale Weak Supervision](https://cdn.openai.com/papers/whisper.pdf) by Alec Radford, Jong Wook Kim, Tao Xu, Greg Brockman, Christine McLeavey, Ilya Sutskever.

