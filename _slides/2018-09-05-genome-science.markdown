---
layout: slide-deck
title:  "Genome Science"
date: 2018-09-05 12:00:00 -0000
author: Will Rowe
permalink: /genome-science/
group: presentation
theme: ember
transition: slide

---

<script type="text/template">

#### Genome Science 2018

***

# Sketching microbiomes

***

Will Rowe PhD

<br/>email: [will.rowe@stfc.ac.uk](will.rowe@stfc.ac.uk) | twitter: [wil_rowe](https://twitter.com/wil_rowe)

----

***

## Talk overview

***

* Background
 - what is data sketching?
 - how is it applied to microbial genomics?

* Sketching microbiomes
 - histosketching for fast indexing, clustering and ML
 - indexing variation graphs and resistome profiling

* Software and a case study
 - HULK, BANNER, GROOT & DRAX!
 - identifying neonatal dysbiosis

----

"[Analytics]() is the discovery, interpretation, and communication of meaningful patterns in data; focusing on what will happen next"

<br/>
<div align="right">
<i>wikipedia<i/>
<div/>

---

* Microbiome analytics has a [big data]() problem
  - certain questions don't scale
  - e.g. finding frequent items, counting distinct elements etc.
  - requires large time/compute resources

* Rather than bigger computers, what can we do?
 - reduce the data
 - approximate the data
 - streamline workflows

---

"[Data sketching]() produces an approximate answer based on a summary ([sketch]()) of the data stream in memory"

---

* [Data sketching]() can enable microbiome analytics
 - can process data streams in small, fixed memory
 - only need a single pass of the data
 - probabilistic but with error bounds

* Recent sketching tools using MinHash ( e.g. [mash](), [sourmash]()) are great!
 - find similar genomes
 - find what's in your microbiome sample
 - get distances to build quick trees

* There are some drawbacks, especially for microbiome analytics
 - MinHash doesn't include k-mer frequency information
 - MinHash doesn't account for impact of relative set size

----

"The [histogram sketch]() (or histosketch) data structure maintains a set of  fixed size sketches to approximate the overall histogram as it is received from a data stream"

<br/>
<div align="right">
<i>Yang et al. 2017<i/>
<div/>

---

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/figures/pngs/figure-1.png" width="60%">

> [Rowe, WPM et al. Streaming histogram sketching for rapid microbiome analytics. BioRxiv 2018](https://doi.org/10.1101/408070)
<br/>
> [twitter: @wil_rowe]()

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
  - classification of microbiome samples in near real-time.

---

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/figures/pngs/figure-2.png" width="50%">

* Microbiome samples from different body sites (CAMI project)

* Histosketched 48 samples in [1m30s]()

* Histosketches cluster by body site using Jaccard similarity

---

Histosketch collections can also be indexed and searched, or as features for ML classifiers. More on this later!

---

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/misc/hulk-logo-with-text.png" width="30%">

[H]()istosketching [U]()sing [L]()ittle [K]()mers

> $ conda install hulk || [github.com/will-rowe/hulk](https://github.com/will-rowe/hulk)

---

<img src="https://raw.githubusercontent.com/will-rowe/banner/master/misc/logo/banner-logo-with-text.png" width="40%">

> $ conda install hulk || [github.com/will-rowe/banner](https://github.com/will-rowe/banner)

---

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/figures/pngs/figure-4.png" width="50%">

* Index the sketches using LSH Forest scheme

* Indexes are updatable

* Index searches predominantly return samples from same body site

----

Once we have identified microbiomes of interest, how can we quickly check for genes of interest?

---

<img src="https://raw.githubusercontent.com/will-rowe/groot/master/paper/img/misc/groot-logo-with-text.png" width="30%">

[G]()raphing [R]()esistance [O]()ut [O]()f me[T]()agenomes

> $ conda install groot || [github.com/will-rowe/groot](https://github.com/will-rowe/groot)
<br/>
> [Rowe, WPM et al. Indexed variation graphs for efficient and accurate resistome profiling. Bioinformatics 2018](https://doi.org/10.1093/bioinformatics/bty387)

---

***

### Indexed variation graphs: [indexing]()

***

* A gene database is clustered, then converted to variation graphs

* Graph traversals are windowed and decomposed to k-mer sets

* A [MinHash signature]() is kept for each window of graph traversal

![groot-figure-1a]({{site.url}}/slides/slide-data/iror/figure-1a.png)

---

***

### Indexed variation graphs: [seeding]()

***

* Query reads are quality checked, trimmed and hashed

* The read signature is queried against the index using additional locality sensitive hashing

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

A case study: designing an analysis workflow for profiling the neonatal microbiome

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

---

![img]({{site.url}}/slides/slide-data/genome-science/tools-logos.png)

---

![img]({{site.url}}/slides/slide-data/genome-science/tools-workflow.png)

---

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

#### Genome Science 2018

***

# Thanks for listening

***

<br/>email: [will.rowe@stfc.ac.uk](will.rowe@stfc.ac.uk) | software: [github.com/will-rowe](https://github.com/will-rowe)

twitter: [wil_rowe](https://twitter.com/wil_rowe) | slides: [will-rowe.github.io/genome-science](https://will-rowe.github.io/genome-science)

</script>
<section>
    <pre><code data-trim data-noescape>
    </code></pre>
</section>
