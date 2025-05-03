---
layout: page
title: "Bridging the digital divide: Where do we stand?"
description: Internship — XRI Global
img: assets/img/test400.jpg
# 12.jpg
importance: 1
category: internship
related_publications: true
---

## 1. Introduction

The purpose of my internship at [XRI Global](https://www.xriglobal.ai/) was to provide support for low-resource languages via data extraction for training data used in machine translation (MT) systems. The accumulation of my efforts spanned a range of sources for bitexts[^1] and provided an initial survey of the available resources needed for work on MT. A translation system was also trained between Spanish (es) and Asturian (ast), which achieved above state-of-the-art performance, as far as I know.

The first step in bridging the digital divide starts by cataloging data and models from popular hubs (e.g., Hugging Face, GitHub, Common Voice) to assess the amount of support needed to develop these language technologies, in particular for those classified as low-resource. In the context of natural language processing (NLP), the relevant modalities include audio- and text-based resources. My focus throughout the internship was on machine translation (i.e., text-based resources).

My role at XRI Global was akin to a junior Data Engineer or ML/NLP Engineer and was largely independent from the other interns because I was the sole contributor for data extraction/modeling for MT. However I often interacted with nearly all interns to share my findings. I interacted with Jennifer, whose interactions indirectly helped streamline the attributes for each collection. I briefly worked with Vanessa to distribute annotation and visualization duties because at the time of her arrival, the team already had most duties fulfilled. Prior to the conference, I worked with Daniel which improved my understanding of the company's mission statement. Lastly, I worked with Zach (intern lead) by creating a master list of results from experiments done by other interns.

The work was also presented during a poster session at the International Conference on Language Technologies for All (LT4All 2025) held at the UNESCO Headquarters. The abstract is shown below:

<div class="row justify-content-center">
    <div class="col-md-8 mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/image.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

> :rocket: GitHub repository is available [here](https://github.com/XRILLC/inclusiveai) under the `text` folder <br>
> :page_facing_up: Poster abstract is available [here](https://www.lt4all2025.eu/2025/02/24/lt4all-2025-book-of-abstracts-now-available/) under `Bridging the digital divide: Where do we stand?` <br>
> :computer: XRI Global and students at UA produced a map that is live [here](https://inclusiveai-app.vercel.app/)

**NOTE: The code written during the internship is well-documented on GitHub and lengthy so I chose to simply link to the repository under the relevant header. Nearly all code was written by me except for the boilerplate code used from the [Translation](https://huggingface.co/docs/transformers/en/tasks/translation) documentation, which went through heavy modification.**

## 2. Digital Divide Objective

### 2.1 Methodology

Low-resource languages could be defined as low-resource for many reasons, however we choose to follow the methodologies described in the _No Language Left Behind_ paper {%cite nllbteam2022languageleftbehindscaling %} and a threshold set by XRI Global. A language is defined as low-resource if less than 1 million sentences are available. Furthermore, languages within the range of 10k and 50k can achieve varying degrees of performance.

Historically, universities and government institutions were involved in the research for machine translation. Fast forward to today, and many of the recent advancements have come from Google Research and localized efforts who contribute heavily to the progress of machine translation for low-resource languages.

### 2.2 Data Sources & Collection

Successful machine translation systems often presuppose very large bitexts, which few low-resource languages have. Moreover, there exists no survey (to my knowledge) on the available datasets for machine translation. Data is arguably the most important step for translation systems and helps companies decide whether a dataset needs to be created or curated for future technologies.

Popular resources include:

1.  [Hugging Face](https://huggingface.co/)
2.  [OPUS](https://opus.nlpl.eu/)
3.  [StatMT](https://statmt.org/)
4.  [Wikipedia](https://www.wikipedia.org/) (excellent for monolingual data)

Hugging Face is a collaborative platform used for building, training, and deploying various artificial intelligence (AI) models, where many researchers and local efforts publish their datasets. For that reason we choose to use Hugging Face as the main source of data and include external datasets manually.

There are two types of data collections we are interested in: bitexts and language pairs. Bitexts contain generic information, such as where the data originates from and which languages are supported in each bitext. However, an additional collection is needed to describe the direction of a translation pair (e.g., Spanish-Asturian) and must contain the number of examples in the respective bitext. The specific direction, whether from Spanish-to-Asturian or from Asturian-to-Spanish is not delineated—the choice to ignore directionality was a practical one and would require heavy annotation pulls focus from the goal of the internship.

The primary collection of bitexts, is extracted semi-automatically from Hugging Face's API. It's important to note that Hugging Face defines the translation task more generally, including related tasks such as transliteration, translation for programming languages, and machine translation. That is, finding bitexts requires manual tagging to identify relevant datasets. Therefore we focus on extracting data from the `translation` task where the bulk of datasets are uploaded. Lastly Hugging Face offers support for finding the number of rows[^2] which makes this process much smoother.

The secondary collection includes datasets that are either missed during the extraction phrase or datasets uploaded to an external site. Information on language pairs are either inferred or manually tagged when a programmatic method cannot be found. Simple language pairs are defined to contain two languages and are automatically extracted.

### 2.4 Data Processing

The empirical challenges encountered during data collection are missing metadata, lack of language pair information (directionality), and no query option for bitexts.

For that reason quality procedures are carried out, namely:

1. Identifying relevant datasets (i.e., unsupported, parallel, multilingual, or reference).
2. Submit pull requests on Hugging Face to include missing metadata. Often low-resource languages are easily missed because they are not generated automatically. A full list of languages on Hugging Face can be found [here](https://huggingface.co/languages)

### 2.5 Data Pipeline

ℹ️ [GitHub - get_data.py](https://github.com/XRILLC/inclusiveai/blob/main/text/get_data.py)

The pipeline follows standard ETL (Extract, Transform, Load) practices:

- **Initialize:** extract MT data from hugging face.

- **Tag:** tag bitexts and other relevant datasets (monolingual, benchmarks, reference, etc) based on MT data. Also create/utilize custom tooling to make manual tagging easier to carry out.

- **Refresh:** update MT data periodically (biweekly).

- **Unit Testing:** conduct data quality tests to measure the uniqueness, completeness, and consistency of the extracted data.

- **Create:** transform MT data to create language pairs for all simple (languages = 2) or multilingual (languages > 2) datasets.

- **Monitor:** efficiently extract and transform language pairs from datasets not covered. This step also double-checks datasets that may be ignored during previous runs due to API connection errors.

### 2.6 Results

The data consists of two collections for the **bitexts** and **language pairs**, respectively. For each collection, there exists a split that contains semi-automated and manually tagged data.

##### Parallel Data

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/mt_hf.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Parallel datasets extracted from Hugging Face. The dataset type is the missing metadata that requires tagging only. The purpose of this variable defines datasets as either parallel, multilingual parallel, or unsupported.
</div>

One possible addition for the datasets extracted from Hugging Face is to include information on the domain. This is an attractive option because many systems may be biased towards the training data or may need to be fine-tuned to a certain domain. However that assumes more tagging will be needed for each refresh update.

##### Language Pairs

<div class="row justify-content-center">
    <div class="col-md-8 mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/hf_pairs.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Language pairs are deterministically inferred from each parallel dataset.
    Information for train/validation/test split is then extracted from the metadata.
</div>

Given the now available information on bitexts and pairs, we performed a preliminary analysis describing the state of machine translation. The purpose of the analysis surveyed the available data under the constraints proposed by XRI Global for optimal performance. This also allows for one to locate datasets that are not supported by Google Translate but contain enough data for training a machine translation system.

<div class="row justify-content-center">
    <div class="col-sm-7 mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/MT-HF_bg.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The performance of machine translation systems is largely dependent on the size of datasets being trained on. XRI Global has estimated varying degrees of performance based on the number of sentences in the data.
</div>

## 3. Modeling

### 3.1 Supported languages

ℹ️ [GitHub - utils.py](https://github.com/XRILLC/inclusiveai/blob/main/text/utils.py)

The preliminary analysis on the available corpora for machine translation served as a strong exploratory tool to expose low-resource languages unsupported by major translation engines. Google Translate was used as the main reference for commercially supported languages.

The list of supported languages can be extracted two ways: via the [Cloud Translation API](https://cloud.google.com/translate/docs/basic/discovering-supported-languages) or via a potentially unstable [public endpoint](https://github.com/ssut/py-googletrans/issues/268). The latter resource was also used because the translation API did not provide an exhaustive list of languages. In fact, both methods included languages fully supported by the translation engine that the other list did not.[^3]

### 3.2 Candidate languages

ℹ️ [GitHub - Workbook.ipynb](https://github.com/XRILLC/inclusiveai/blob/main/text/Workbook.ipynb)

Candidate languages with enough training data can now be identified for modeling. We chose to focus on Asturian, a low-resource language with ~ 100,00 native speakers.[^4] Additionally, the Workshop for Machine Translation (WMT) has released the first edition of the Shared Task on Translation into Low-Resource Languages of Spain 2024 {%cite sanchez-martinez-etal-2024-findings %}.

### 3.3 Benchmarking

The first step when evaluating progress on a task is defining a baseline, i.e., a simple system that you can put together quickly. The submission by the Helsinki Group to the WMT for translation into languages of Spain was used to understand the workflow in machine translation {%cite de-gibert-etal-2024-hybrid %}.

The OPUS-MT project is an effort to make neural machine translation (NMT) models accessible for multilingual NLP. Therefore we chose NMT models from Hugging Face that are suitable for fine-tuning, and appropriate for back-translation. Given this criteria, we decide to use the OPUS-MT for modeling: the model marked \* is for fine-tuning.[^5]

| Model                                          | Params (M) | BLEU | chrF2 |  Epochs   |
| :--------------------------------------------- | :--------: | :--: | ----: | :-------: |
| opus-mt-tc-bible-big-deu_eng_fra_por_spa-mul   |    241     | 8.93 | 39.95 | zero-shot |
| opus-mt-tc-bible-big-deu_eng_fra_por_spa-itc\* |    223     | 8.65 | 39.55 | zero-shot |
| Apertium                                       | rule-based | 17.0 |  50.8 |     —     |

_Table 1: Model performance comparison on Flores dev set._

While the performance for the opus-mt-...-mul model was superior, the italic (itc) model was chosen instead based on linguistic similarities between source-target pairs and fewer parameters. The zero-shot performance on the Flores dev set suggests fine-tuning could be successful if quality data is extracted.

Apertium is a rule-based MT system that offers support for various languages from Spain, and is the current state-of-the-art, notably for Asturian, Aranese, and Aragonese. The workshop uses this system as the baseline for each of the languages in the task.

### 3.4 Data

ℹ️ [GitHub - preprocessing.py](https://github.com/XRILLC/inclusiveai/blob/main/text/experiments/preprocessing.py)

The [Spanish-Asturian Parallel Corpus](https://huggingface.co/datasets/weezygeezer/Spanish-Asturian_Parallel-Corpus) is a dataset created to support the development of Machine Translation (MT) systems for translating from Spanish (es) into Asturian (ast). The text was extracted from [Opus](https://opus.nlpl.eu/results/es&ast/corpus-result-table) under the following resources: OpenSubtitles, Tatoeba, KDE4, wikimedia, GNOME {%cite TIEDEMANN12.463 lison-tiedemann-2016-opensubtitles2016 %}.

These datasets were chosen specifically because the source and target pairs are correct.[^6] Additionally, the data from [PILAR](https://github.com/transducens/PILAR) was used to create synthetic corpora {%cite PILAR %}. This dataset separates both synthetic and existing data for convenience and ablation studies. The synthetic data consist of Spanish translations generated from the Asturian monolingual corpus of the PILAR dataset. To create the synthetic Spanish translations we used the [OPUS-MT](https://huggingface.co/Helsinki-NLP/opus-mt-tc-bible-big-itc-fra_ita_por_spa) model with greedy decoding.

The data filtering process incorporated language identification using the [Idiomata Cognitor](https://github.com/transducens/idiomata_cognitor) tool {%cite idiomatacognitor %}. No other preprocessing steps (e.g. alignment, word dropout, swapping) were used, meaning any peculiarities exist solely from the data itself. However, this is not the case for the synthetic data—peculiarities certainly exist. Notably the synthetic data was filtered twice by the language identifier.

|            | Tatoeba | OpenSubtitles |  KDE4  | wikimedia | GNOME  |
| :--------: | :-----: | :-----------: | :----: | :-------: | :----: |
|  **ast**   |   159   |    17,486     | 26,023 |  45,506   | 68,668 |
| **langid** |   94    |     8,091     | 12,025 |  39,958   | 37,551 |

_Table 1: Parallel Data._

|                  | PILAR - literary | PILAR - crawled |
| :--------------: | :--------------: | :-------------: |
|     **ast**      |      14,776      |     24,094      |
| **langid (ast)** |      10,538      |     17,121      |
| **langid (es)**  |      9,329       |     15,409      |

_Table 2: Monolingual Data._

The number of rows for the entire dataset is 122,449.

### 3.5 Models

ℹ️ [GitHub - nmt_experiments.ipynb](https://github.com/XRILLC/inclusiveai/blob/main/text/experiments/nmt_experiments.ipynb)

The _opus-mt-tc-bible-big-deu_eng_fra_por_spa-itc_ model was fine-tuned on the parallel corpus we extracted above for 3 epochs; the performance was impressive, especially for a test run. We achieved approximately 20 bleu points above our baseline and and ~10 bleu points above Apertium's system. Table 3 reports the performance on the dev set (performance is similar on the devtest but we chose not to report it here).

| Model                                        | BLEU | chrF2 | Epochs |
| :------------------------------------------- | :--: | ----: | :----: |
| opus-mt-tc-bible-big-deu_eng_fra_por_spa-itc | 28.6 | 50.61 |   3    |

_Table 3: Model performance on Flores dev set._
<br>

Previous runs used a larger parallel corpus (~ 700,000 examples) but resulted in erratic performance. During these runs the bleu score would jump around 3, 8, 16, and 24 respectively. We attribute this behavior to the dataset containing noisy source-target pairs. The simple language identifier provided by the workshop was extremely useful in creating a quality dataset.

**NOTE: XRI has existing software for running NMT, ASR, and TTS experiments. I chose to follow the documentation from Hugging Face instead, which allowed for me to experiment with backtranslation/preprocessing and get comfortable with Hugging Face.**

### 3.6 Final Thoughts

Low-resource NLP is a difficult domain, that often requires building your own tools, using buggy tools, and largely working in a constrained environment. The extractor I wrote was a novel implementation that appears simple but required a lot of experimentation. It is currently pending additional unit tests and a better alternative than annotation.

Further work can be done for modeling also, namely during the preprocessing step. For example, alignment with [LaBSE Embeddings](https://www.kaggle.com/models/google/labse/tensorFlow2/labse), transfer learning on a similar language (perhaps Galician), and preprocessing for backtranslation {%cite feng2022languageagnosticbertsentenceembedding zoph2016transferlearninglowresourceneural edunov2018understandingbacktranslationscale %}.

A great deal of my time was spent reading research papers to understand the machine translation and its history; I enjoyed this part of the internship especially, it gives the field a dynamic and interesting experience. The environment also pushed myself to write clear and well-documented (for both my sake and others). I also worked with data manipulations constantly and quickly became comfortable with the Hugging Face platform.[^7] I find this domain very rewarding and would enjoy returning to it after graduation.

## Footnotes

[^1]: A bitext (also known as parallel corpora) is defined as a compiled document composed of source and target language translations of a given text.
[^2]: While most examples of parallel data are sentences, occasionally datasets contain words or various formats that need further preprocessing. Hence why we emphasize rows instead of sentences.
[^3]: Despite the extraction process being easy and can be remedied, this is unfortunate behavior by the major engine. Users interested in MT have resorted to the endpoint for further extraction.
[^4]: Asturian was chosen based on the large amount of training examples available.
[^5]: The translation models were also used in the Helinski Group's submission but we instead chose zero-shot performance to guide its usage.
[^6]: There exists a decent collection of bitexts on Opus, but the largest collections (~1,000,000) contain many sentences that are simply not translations but gibberish. This can be verified by spot-checking the text document and comparing the sentence length. That is why we chose to curate a dataset instead and prevent noise in the training data as much as possible.
[^7]: The course INFO 555: Applied Natural Language Processing covered a lot of relevant skills used in this internship, especially with pretrained language models. While I took this class many months after the start of the internship, I highly recommend students to consider taking the course.
