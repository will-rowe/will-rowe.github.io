---
layout: slide-deck
title:  "Doherty Institute Seminar"
author: Will Rowe
permalink: /doherty-seminar-2018/
group: presentation
theme: ember
transition: slide

---

<script type="text/template">

#### Doherty Institute 2018

***

# Microbiome, Assemble!

***

<i>bringing together microbiome analytics to combat the big data challenges facing genomics</i>

***

<br/>email: [will.rowe@stfc.ac.uk](will.rowe@stfc.ac.uk) | twitter: [wil_rowe](https://twitter.com/wil_rowe)

----

***

## Talk overview

***

* Background
 - what are the big data challenges facing genomics?
 - what is microbiome analytics?

* Sketching microbiomes
 - histosketching for fast indexing, clustering and ML
 - indexing variation graphs and resistome profiling

* Software and a case study
 - **M**icrobiome **C**haracterisation **U**tilities
    - HULK, BANNER, THOR, GROOT & DRAX!
 - identifying neonatal dysbiosis

----

* Genomic research has a '[big data]()' problem
  - collections of genomic data continue to grow
  - certain questions don't scale
    - finding frequent items
    - counting distinct elements
  - requires large time/compute resources

* Rather than bigger computers, what can we do?
 - reduce the data
 - approximate the data
 - streamline data processing

---

"[Analytics]() is the discovery, interpretation, and communication of meaningful patterns in data; focusing on what will happen next"

<br/>
<div align="right">
<i>Wikipedia<i/>
<div/>

----

<section data-background-image="{{site.url}}/slides/slide-data/melb-2018/comic-strip-1.png" data-background-size="contain" background-repeat="no-repeat"><h2></h2></section>

----

"[Data sketching]() produces an approximate answer based on a summary ([sketch]()) of the data. Following its processing, data is dropped and is no longer accessible"

<br/>
<div align="right">
<i>Cormode, G. ACM 2017<i/>
<div/>

---

* [Data sketching]() can enable microbiome analytics
 - can process data streams in small, fixed memory
 - only needs a single pass of the data
 - probabilistic but with error bounds

* Recent sketching tools using MinHash ( e.g. [mash](), [sourmash]()) are great!
 - find similar genomes
 - find what's in your microbiome sample
 - get distances to build quick trees

* There are some drawbacks, especially for microbiome analytics
 - MinHash doesn't include k-mer frequency information during sketching
 - MinHash doesn't account for impact of relative set size

---

"The [histogram sketch]() (or histosketch) data structure maintains a set of fixed size sketches to approximate the overall histogram as it is received from a data stream"

<br/>
<div align="right">
<i>Yang et al. ICDM 2017<i/>
<div/>

---

![](http://image.slidesharecdn.com/trimble-msu-sequences-140505091324-phpapp02/95/all-kmers-are-not-created-equal-recognizing-the-signal-from-the-noise-in-largescale-metagenomes-23-638.jpg?cb=1399281754)

> Trimble, W. - [tinyurl.com/y8823y6v](https://tinyurl.com/y8823y6v)

---

<img src="https://github.com/will-rowe/hulk/raw/master/paper/img/figures/pngs/figure-1.jpg" width="60%">

> Rowe, WPM et al. [Streaming histogram sketching for rapid microbiome analytics. BioRxiv 2018](https://doi.org/10.1101/408070)

---

* [Histosketch]() algorithm
  - designed for similarity comparisons of customer activity information
  - implemented here to process streaming k-mer spectra

* Uses [consistent weighted sampling]()
 - keeps track of k-mer frequency information
 - accounts for differences in relative set size

* Several applications
  - sample dissimilarity estimation
  - rapid microbiome catalogue searching
  - classification of microbiome samples in near real-time

---

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/figures/pngs/figure-2.png" width="50%">

* Microbiome samples from different body sites (CAMI project)

* Histosketched 48 samples in [1m30s]()

* Histosketches cluster by body site using Jaccard similarity

---

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/figures/pngs/figure-4.png" width="50%">

* Index the sketches using LSH Forest scheme

* Indexes are updatable

* Index searches predominantly return samples from same body site

---

![]({{site.url}}/slides/slide-data/melb-2018/iaai19.png)

* Multi-class and binary classification of microbiome samples

* Relevance Vector Machine (RVM), Support Vector Machines (SVM), Random Forests (RF), Naive Bayes (NB)

> Carrieri , AP., Rowe, WPM et al. [A Fast Machine Learning Workflow for Rapid Phenotype Prediction from Whole Shotgun Metagenomes. IAAI 2018]()

----

<section data-background-image="{{site.url}}/slides/slide-data/melb-2018/comic-strip-1.png" data-background-size="contain" background-repeat="no-repeat"><h2></h2></section>

----

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/misc/hulk-logo-with-text.png" width="30%">

[H]()istosketching [U]()sing [L]()ittle [K]()mers

> $ conda install hulk || [github.com/will-rowe/hulk](https://github.com/will-rowe/hulk)

---

<img src="https://raw.githubusercontent.com/will-rowe/banner/master/misc/logo/banner-logo-with-text.png" width="40%">

> $ conda install banner || [github.com/will-rowe/banner](https://github.com/will-rowe/banner)

---

<img src="https://raw.githubusercontent.com/will-rowe/thor/master/paper/img/misc/thor-logo-with-text.png" width="30%">

[T]()ransforming [H]()ashed [O]()TUs to [R]()GB

> [github.com/will-rowe/thor](https://github.com/will-rowe/thor)

----

<section data-background-image="{{site.url}}/slides/slide-data/melb-2018/comic-strip-2.png" data-background-size="contain" background-repeat="no-repeat"><h2></h2></section>

----

* [Resistome profiling]() can be difficult
 - high similarity between reference genes
 - microbiome data can be massive in size
 - existing workflows are complicated

* Existing tools aren't ideal
 - few tools are designed for microbiome samples
 - reference dependent vs. independent
 - slow and/or inaccurate

* Variation graph encoding of sequences
 - use graph traversals to identify variants
 - collapses similar sequences
 - improves speed and accuracy

---

![]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/arg-tool-timeline.jpg)

---

***

### Indexed variation graphs: [indexing]()

***

* A gene database is clustered, then converted to variation graphs

* Graph traversals are windowed and decomposed to k-mer sets

* A [MinHash sketch]() is kept for each window of a graph traversal

![groot-figure-1a]({{site.url}}/slides/slide-data/iror/figure-1a.png)

> [Rowe, WPM et al. Indexed variation graphs for efficient and accurate resistome profiling. Bioinformatics 2018](https://doi.org/10.1093/bioinformatics/bty387)

---

***

### Indexed variation graphs: [seeding]()

***

* Query reads are quality checked, trimmed and MinHashed

* The read sketch is queried against the index using additional [Locality Sensitive Hashing]()

* Seeds are determined using ranked [Jaccard Similarity]() estimates

![groot-figure-1b]({{site.url}}/slides/slide-data/iror/figure-1b.png)

---

***

### Indexed variation graphs: [aligning]()

***

* Assumption: majority of reads do not contain novel SNPs or errors

* Hierarchical local alignment
 - exact match > shuffled seed > gapped-end alignment

* Score traversal to classify an alignment (unique, perfect etc.)

![groot-figure-1c]({{site.url}}/slides/slide-data/iror/figure-1c.png)

----

<section data-background-image="{{site.url}}/slides/slide-data/melb-2018/comic-strip-2.png" data-background-size="contain" background-repeat="no-repeat"><h2></h2></section>

----

<img src="https://raw.githubusercontent.com/will-rowe/groot/master/paper/img/misc/groot-logo-with-text.png" width="30%">

[G]()raphing [R]()esistance [O]()ut [O]()f me[T]()agenomes

> $ conda install groot || [github.com/will-rowe/groot](https://github.com/will-rowe/groot)

---

![]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/fig5-groot.png)

----

[Case study](): designing an analysis workflow for profiling the neonatal microbiome

<br/>
<div align="right">
<i>collaboration with Lindsay Hall (Quadram)<i/>
<div/>

---

* A clinically relevant dataset
  - gut microbiome profiles from a cohort of healthy pre-term neonates
  - from a single hospital

* Profiling the gut microbiota of preterm infants
  - correlating this to health data
  - investigate impact of antibiotics

* Workflow aims
  - quickly identify microbiomes exhibiting dysbiosis
  - identify Antibiotic Resistance Genes (ARGs)
  - determine ARG carriage
  - detect changes in longitudinal samples
  - work on a laptop

----

<section data-background-image="{{site.url}}/slides/slide-data/genome-science/tools-logos.png" data-background-size="contain" background-repeat="no-repeat"><h2></h2></section>

----

<section data-background-image="{{site.url}}/slides/slide-data/genome-science/tools-workflow.png" data-background-size="contain" background-repeat="no-repeat"><h2></h2></section>

----

* Histosketching with [HULK]()
  - clusters the samples

* Classification with [BANNER]()
  - predicts dysbiosis in ~10 seconds / sample

* Gene detection with [GROOT]()
 - ARGs identified in ~30 seconds / sample

* Automate with [DRAX]() (in development)
 - reproducible pipeline
 - adds Metacherchant for identifying gene carriage

---

![img]({{site.url}}/slides/slide-data/iror/tmp.jpg)

* Single blaSHV (blaSHV-11) gene present at day 7 post initial antibiotic treatment

*  Multiple blaSHV variants present at day 18 post initial antibiotic treatment

* Graph bubbles correspond to variant nodes that bring additional extended-spectrum beta-lactamase activity to blaSHV (e.g. blaSHV-40)

----

### [Acknowledgements]()

***

| STFC | IBM Research |
| :--: | :----------: |
| Martyn Winn | Anna Carrieri |
| | Edward Pyzer-Knapp |
| | |

| Quadram Institute | Imperial College London |
| :----------------:| :---------------------: |
| Lindsay Hall | Simon Kroll |
| Cristina Alcon-Giner | Alex Shaw |
| Shabhonam Caim | Kathleen Sim |
| | |

----

#### Doherty Institute 2018

***

# Thanks for listening

***

<br/>email: [will.rowe@stfc.ac.uk](will.rowe@stfc.ac.uk) | software: [github.com/will-rowe](https://github.com/will-rowe)

twitter: [wil_rowe](https://twitter.com/wil_rowe) | slides: [will-rowe.github.io/doherty-seminar-2018](https://will-rowe.github.io/doherty-seminar-2018)

</script>
<section>
    <pre><code data-trim data-noescape>
    </code></pre>
</section>
