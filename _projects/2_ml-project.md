---
layout: page
title: Named Entity Recognition with Classical ML
description: Course Project — INFO 521 "Introduction to Machine Learning"
img: assets/img/ner-project.png
importance: 2
category: coursework
giscus_comments: false
related_publications: true
---

## Highlights

This blog post present a brief survey on classical machine learning algorithms in the context of the CoNNL-2003 Shared Task. A named entity recognition (NER) system is built to recognize and classify objects in a body of text into predefined categories. While the task is certainly not new, I approach the task naively[^1] by creating a baseline to get some inspiration and determine the complexity of the problem which motivates the use of machine learning. Finally, an analysis is included between generative and discriminative machine learning algorithms.

[^1]: Naive in the sense that one should attempt to solve the task with appropriate tools. This also exposes details in the data when one uses relevant baselines.

> **NOTE** Access to GitHub repository [here](https://github.com/weezymatt/NER-with-Classical-Machine-Learning) and paper [here](https://github.com/weezymatt/NER-with-Classical-Machine-Learning/blob/main/reports/INFO_521_report.pdf).

A secondary objective for the project required each algorithm to be written in python. While not required, the `scikit-learn` library was used as comparision for a sanity-check during evaluation.[^2]

- Since the writing of this blog/paper, I've implemented various architectures from papers (e.g., deep averaging networks, lstms with attention) and can confidently cite the difference in runtime between the written algorithm and the imported algorithm due to inefficent matrix multiplication. That is, my inexperience was at fault. While the performance was nearly identical (suggesting a correctly implemented algorithm) that did not take advantage of the hardware, resulting is slower performance. I highly suggest newcomers to try implementing an architecture from scratch as it really teaches you the dirty details in machine learning.

[^2]: Debugging machine learning is a difficult but useful skill. Writing algorithms from scratch (in some capacity) is an excellent exercise.

## Introduction

The Named Entity Recognition and Classification (NERC) task refers to the process that classifies phrases in text as various categories. Common entity types include ORGANIZATION (ORG), PERSON (PER), LOCATION (LOC), and MISCELLANEOUS (MISC). Tagging is the process of labeling each word in a sentence with its respective named entity (NE). As an example:

<div class="entities" style="line-height: 2.5; direction: ltr">
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    The University of Arizona
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
 is a public land-grant university located in 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Tucson
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
, 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Arizona
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
.</div> <br>
The remaining tokens are classified as the label outside (O), meaning the token is simply outside the named entity phrase. Note the categorization is subjective as one can argue Arizona is also a LOCATION. Approaches to NERC generally use BIO notation which separates the beginning (B-) and inside (I-) of entities. The example above is now:

<div class="entities" style="line-height: 2.5; direction: ltr">
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    The
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">B-ORG</span>
</mark>
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    University
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">I-ORG</span>
</mark>
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    of
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">I-ORG</span>
</mark>
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Arizona
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">I-ORG</span>
</mark>
 is a public land-grant university located in 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Tucson
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">B-GPE</span>
</mark>
, 
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
    Arizona
    <span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">B-GPE</span>
</mark>
.</div> <br>
NER remains a relevant natural language processing task that benefits several extrinsic tasks
such as question answering and information extraction. The traditional schema largely
ignores nested entities in favor of flat structures that are easier to identity but frames the
problem with missing information. State-of-the-art (SOTA) methods make use of bidirectional Long Short-Term Memory (LSTM) networks with a Conditional Random Field (CRF) layer alongside contextualized embeddings {% cite ju-etal-2018-neural %}.

Contributions:

<ul>
    <li> The NER task is approached with a simple rule-based baseline that is further developed into a more complex model, i.e., a hidden markov model. The purpose of this methodology was to motivate the use of machine learning for this particular task, and to motivate the use of Logistic Regression applied to a sequence-labeling problem.</li>
    <li> An analysis is provided between generative and discriminative machine learning algorithms.</li> 
</ul>

## CoNNL-2002/-03

The project describes a baseline & benchmark methodology for developing a NER system in the context of the CoNNL-2003 Shared Task {%cite tjong-kim-sang-de-meulder-2003-introduction %}. We will focus on the English dataset and compare[^3] the Spanish corpus from the CoNNL-2002 Shared {%cite tjong-kim-sang-2002-introduction %}.

[^3]: The transformation-based model is inspired by Brill's Tagger: an inductive method for part-of-speech tagging. We used the model for comparision because it's intended to use linguistic intuition.

The English dataset was preprocessed into training, development, and test sets. Additionally, the number of tokens per entity type highlight the imbalanced nature of the CoNNL-2003 dataset and of NER in general.

<div class="row justify-content-center">
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/eng-tokens.png" title="????example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/eng-dataset-split.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Comparison of the English split and number of tokens per entity type.
</div>

## Methods

Named entity recognition is treated as a sequence labeling task, that is, given a sequence of words $$ W = (w_1, ..., w_n) $$, we are interested in maximizing the best sequence of tags:

$$ \hat{t}_{1:n} \approx \arg\max_{\hat{t}_{1:n}} P(tag_{1:n}|word\_{1:n}) \tag{1} $$

We'll introduce two class sequence labeling algorithms, the generative—Hidden Markov Model (HMM)—and the discriminative—Maximum Entropy Markov Model (MEMM). We encourage the reader who is interested in the mathematical derivation of each algorithm to read our paper. Otherwise the equation above is sufficient for intuition.

Key differences:

<ul>
    <li> Generative: probabilistic modeling may lead to greater interpretability</li>
    <li> Discriminative: easy to implement feature functions </li>
</ul>

## Baselines

Two rule-based systems were computed for the English and Spanish corpora with the seqeval package {%cite seqeval %}. The **AlwaysNonEntity** is a naive baseline that labels all tokens as non-entities (O), see Table 1. The less naive system **SingleEntity** is based on a lookup table where only the beginning (B-) of entities is added, see Table 2.

<div class="row justify-content-center">
    <div class="col-md-9 mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/baselines.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

As evidenced with the **AlwaysNonEntity** baseline, token-level accuracy has a modest 80% but is meaninguless for NEs. The improved baseline **SingleEntity** labels entities only if they appear in the training data and is the default baseline for our system to improve upon.

Earlier I stated the CoNNL-2003 dataset is imbalanced. Largely the distribution of named entities is imbalanced where the majority class is outside (O). Therefore the micro-averaged F1 score will be used for evaluation because this doesn't prefer the majority class. However other evaluation methods may be appropriate depending on the domain, but are not considered to focus on the general design process for machine learning.

## Experiments

We compare the MEMM and HMM on the NERC task. The Spanish dataset was evaluated exclusively by an HMM, and the results in Table 4 suggest a first-order model an appropriate benchmark for the task. We obtained an 𝐹1 score of 67.5% which is an improvement over baselines and the transformation-based model proposed by Villalba and Guzmán {%cite villalba2020namedentityextractionfinite %}.

<div class="row justify-content-center">
    <div class="col-md-8 mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/esp-results.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

For the English dataset, several variants were tested by using various add-𝛼 smoothing (𝛼 = 1 or
\> 1) values and used pairwise grid search to find optimal values for regularization, epochs, and learning rate. We compared the results of the MEMM imported from scikit-learn and
our implementation in Table 4.

<div class="row justify-content-center">
    <div class="col-md-8 mt-3 mt-md-0 text-center"> <!-- Adjust col-md-4 to col-md-3 or smaller -->
        {% include figure.liquid loading="eager" path="assets/img/eng-results.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

The MEMM clearly shows discriminative modeling gives good results from feature functions and word embeddings. It easily outperforms the baseline and shows a 4% improvement over the HMM on the CoNNL-2003 dataset. We are satisfied that the performance of these two models is almost identical suggesting
our implementation is on par with scikit-learn’s version except for runtime. The MEMM
performed and generalized the best with greedy search as the validation score did not drop
drastically for the test data.

## Ablation Study

The motivation for maximum-entropy modeling is clear: global and local features are useful for prediction. The inclusion of feature functions and static word embeddings allowed the MEMM to recognize complex entities. This increase in features, and in effect parameters, improved the overall performance of the system. The ablation study below highlights how each category of features improved our system.

<div class="row justify-content-center">
    <div class="col-md-6 mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/model-size.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Conclusion

We described the performance of our algorithms and their variants in Tables 3 & 4. The ME approach clearly shows discriminative modeling gives good results; the approach easily outperforms the stated baselines, **AlwaysNonEntity** and **SingleEntity**. It also achieves a ~10 point performance increase when compared to the first-order HMM on the dev set. Importantly, the generalization error is reduced with the ME approach, which is highly desirable.

The discussion of generative and discriminative modeling by Ng and Jordan {%cite NIPS2001_7b7a53e2 %} also suggests logistic regression will eventually catch up and overtake the performance of a generative classifier given enough data. This implication may be extended to deep neural networks that already hold SOTA performance.

<div class="row justify-content-center">
    <div class="col-md-8 mt-3 mt-md-0 text-center">
        {% include figure.liquid loading="eager" path="assets/img/model-size2.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Therefore recent advancements (approx. 10 years ago) in NLP (i.e., embeddings) have improved our
model (see Table 7) with little feature engineering and can reach competitive performance
with tuning or more careful modeling. Ultimately, the performance is a secondary interest,and propose that the CoNNL-2003 dataset may be dated and we should instead access the generalization of these models by iteratively included recent entities or perhaps tackle nested entities.
