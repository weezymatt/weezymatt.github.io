---
layout: page
title: "Bridging the digital divide: Where do we stand?"
description: XRI Global Internship
img: assets/img/test400.jpg
# 12.jpg
importance: 1
category: internship
related_publications: true
---

## 1. Introduction

The purpose of my internship at [XRI Global](https://www.xriglobal.ai/) was to extract the available training data (i.e., parallel corpora) used in machine translation systems. The first step in bridging the digital divide starts by cataloging all the data and models from popular hubs (e.g., Hugging Face, GitHub, Common Voice) to assess the amount of support needed to develop these language technologies, in particular for those classified as low-resource.

The work was also presented during a poster session at the International Conference on Language Technologies for All (LT4All 2025) held at the UNESCO Headquarters. The abstract is shown below:

<div class="row justify-content-center">
    <div class="col-md-8 mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/image.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

> :rocket: GitHub repository is available [here](https://github.com/XRILLC/inclusiveai) under the `text` folder <br>
> :page_facing_up: Poster abstract is available [here](https://www.lt4all2025.eu/2025/02/24/lt4all-2025-book-of-abstracts-now-available/) under `Bridging the digital divide: Where do we stand?` <br>
> :computer: XRI Global and students at UA produced a map that is live [here](https://inclusiveai-app.vercel.app/)

## 2. Digital Divide Objective

### 2.1 Methodology

Low-resource langauges could be defined as low-resource for many reasons, however we choose to follow the methodologies described in the _No Language Left Behind_ paper {%cite nllbteam2022languageleftbehindscaling %} and a threshold set by XRI Global. A language is defined as low-resource if less than 1 million sentences are available. Furthermore, languages within the range of 10k and 50k can achieve varying degrees of performance.

Historically, universities and government institutions were involved in the research for machine translation. Fast forward to today, and many of the recent advancements have come from Google Research and localized efforts who contribute heavily to the progress of machine translation for low-resource languages.

### 2.2 Data Sources & Collection

Successful machine translation systems often presuppose very large bitexts, which few low-resource languages have. Moreover, there exists no survey on the available datasets needed for machine translation. Data is arguably the most important step for translation systems and helps companies decide whether a dataset needs to be created or currated for future technologies.

Popular resources include:

1.  [Hugging Face](https://huggingface.co/)
2.  [OPUS](https://opus.nlpl.eu/)
3.  [StatMT](https://statmt.org/)
4.  [Wikipedia](https://www.wikipedia.org/) (excellent for monolingual data)

Hugging Face is a collaborative platform used for building, training, and deploy various artificial intelligence (AI) models, and many researchers and local efforts publish their datasets there. Therefore we choose to use Hugging Face as the main source of parallel data and manually add external datasets periodically.

There are two data collections we are interested in: bitexts and their available language pairs.

The primary collection is extracted semi-automatically from Hugging Face's API. It's important to note that Hugging Face defines the translation task more generally, including related tasks such as transliteration, translation for programming languages, and machine translation. That is, finding bitexts requires additional work (i.e., manual tagging) to identify relevant datasets. Therefore we focus on extracting data from the `translation` task where the bulk of datasets are uploaded.

The secondary collection includes datasets that are either missed during the extraction phrase or datasets uploaded to an external site. Information on language pairs are either inferred or manually tagged when a programmatic method cannot be found. Simple language pairs are defined to contain two languages and can be automatically extracted. The number of rows can easily be found[^1] and the number of rows can be found.

### 2.4 Data Processing

The empirical challenges encountered during data collection are missing metadata, lack of directionality for language pairs, and no query option for bitexts.

For that reason quality procedures are carried out, namely:

1. Identifying relevant datasets (i.e., unsupported, parallel, multilingual, or reference).
2. Submit pull requests on Hugging Face to include missing metadata. Often low-resource languages are easily missed because they are not generated automatically. A full list of languages on Hugging Face can be found [here](https://huggingface.co/languages)

### 2.5 Data Pipeline

The pipeline follows standard ETL (Extract, Transform, Load) practices:

- **Initialize:** extract MT data from hugging face.

- **Tag:** tag parallel corpora and other relevant datasets (monolingual, benchmarks, reference, etc) based on MT data. Also create/utilize custom tooling to make manual tagging easier to carry out.

- **Refresh:** update MT data periodically (biweekly).

- **Unit Testing:** conduct data quality tests to measure the uniqueness, completenesss, and consistency of the extracted data.

- **Create:** transform MT data to create language pairs for all simple (languages = 2) or multilingual (languages > 2) datasets.

- **Monitor:** efficiently extract and transform language pairs from datasets not covered. This step also double-checks datasets that may be ignored during previous runs due to API connection errors.

### 2.6 Results

The data consists of two collection for the **parallel data** and **language pairs**, respectively. For each collection, there exists a split that contains semi-automated and manually tagged data.

##### Parallel Data

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/mt_hf.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Parallel datasets extracted from Hugging Face. The dataset type is the missing metadata that requires tagging only. The purpose of this variable defines datasets as either parallel, multilingual, parallel, or unsupported.
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

The preliminary analysis on the available corpora for machine translation served as a strong exploratory tool to expose low-resource languages unsupported by major translation engines. Google Translate was used to the main reference for commerically supported languages.

The list of supported languages can be extracted two ways: via the [Cloud Translation API](https://cloud.google.com/translate/docs/basic/discovering-supported-languages) or via a potentially unstable [public endpoint](https://github.com/ssut/py-googletrans/issues/268). The latter resource was used because the translation API did not provide an exhaustive list of languages. In fact, both lists included languages supported by the translation engine but that the either list did not[^2].

### 3.2 Candidate languages

Candidate languages with enough training data were now be identified for modeling. We chose to focus on Asturian, a low-resource language with ~ 100,00 native speakers. Additionally, the Workshop for Machine Translation (WMT) has released the first edition of the Shared Task on Translation into Low-Resource Languages of Spain 2024.

### 3.3 Benchmarking

The first step when evaluating progress on a task is defining a baseline, i.e., a simple system that you can put together quickly. The OPUS-MT project is an effort to make neural machine translation (NMT) models accessible for multilingual natural language processing (NLP). Therefore we chose NMT models from Hugging Face that are suitable for fine-tuning, and appropriate for back-translation. Given this criteria, we decide to use the OPUS-MT for modeling: the model marked \* is for fine-tuning.

| Model                                          | Params (M) | BLEU | chrF2 |  Epochs   |
| :--------------------------------------------- | :--------: | :--: | ----: | :-------: |
| opus-mt-tc-bible-big-deu_eng_fra_por_spa-mul   |    241     | 8.93 | 39.95 | zero-shot |
| opus-mt-tc-bible-big-deu_eng_fra_por_spa-itc\* |    223     | 8.65 | 39.55 | zero-shot |
| Apertium                                       | rule-based | 17.0 |  50.8 |     —     |

_Table 1: Model performance comparison on Flores dev set._

While the performance for the opus-mt-...-mul model was superior, the italic (itc) model was chosen instead based on linguistic similarities between source/target pairs and fewer parameters. The zero-shot performance on the Flores dev set suggests fine-tuning could be successful is quality data is extracted. Apertium is a rule-based MT system that offers support for various languages from Spain, and is the current state-of-the-art, notably for Asturian, Aranese, and Aragonese.

### 3.4 Data

### 3.5 Models

Every project has a beautiful feature showcase page.
It's easy to include images in a flexible 3-column grid format.
Make your photos 1/3, 2/3, or full width.

To give your project a background in the portfolio page, just add the img tag to the front matter like so:

    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    ---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images, even citations {% cite einstein1950meaning %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}

## Footnotes

[^1]: While most examples of parallel data are sentences, occasionally datasets contain words or various formats that need further preprocessing. Hence why we emphasize rows instead of sentences.
[^2]: While extraction is relatively easy, this is unfortunate behavior by the major engine. Google provides clear documentation for discovering supported languages by their engine but the list is incomplete. Users interesting in MT have resorted to the endpoint for further extraction.
