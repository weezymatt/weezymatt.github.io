---
layout: page
title: "Bridging the digital divide: Where do we stand?"
description: Details for internship with XRI Global TODO
img: assets/img/12.jpg
importance: 1
category: internship
related_publications: true
---

## Abstract

As we enter the decade of indigenous languages, it is important to track how much progress is being made in bridging the digital divide. To this end XRI Global and students from the University of Arizona have joined efforts to take inventory of the current data and models that exist for the world's low-resource languages. By cataloging all the data and models on huggingface, GitHub, Common Voice, and other well-known hubs for training data and AI models, our team has produced a map which shows the current level of support for a large number of low-resource languages

> :computer: GitHub repository is available [here](https://github.com/XRILLC/inclusiveai) under the `text` folder <br>
> :page_facing_up: Poster abstract is available [here](https://www.lt4all2025.eu/2025/02/24/lt4all-2025-book-of-abstracts-now-available/) under the title `Bridging the digital divide: Where do we stand?`

<!-- ##


In light of recent advancements, the development of language technologies, in particular for those classified as low-resource,  is the first step to bridge the digital divide. To this end XRI Global and students from the University of Arizona have joined efforts to unveil the current landscape of information that exists for low-resource languages. By cataloging all the data and models from well-known hubs (e.g., Hugging Face, GitHub, Common Voice) for training data and AI models, our team has produced a map which shows the current level of support for a large number of low-resource languages.
 -->

## Introduction

<br>
&nbsp; &nbsp; **Mission statement:** All training data and language technology for low-resource languages for a world without language barriers.

The data collection process, filtering, and other stuff were chosed based on the methodologies described in detail in NLLB.

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
