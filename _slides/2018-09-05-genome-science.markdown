---
layout: slide-deck
title:  "Genome Science"
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
 - indexing variation graphs
 - resistome profiling

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
 - refine our questions

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
 - build quick trees

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

***

### Histosketching: [an overview]()

***

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/figures/pngs/figure-1.png" width="50%">

> [Rowe, WPM et al. Streaming histogram sketching for rapid microbiome analytics. BioRxiv 2018](https://doi.org/10.1101/408070)

---

* [histosketch]() algorithm
  - designed for similarity comparisons of customer activity information
  - implemented here to process streaming k-mer spectra

* uses [consistent weighted sampling]()
 - keeps track of k-mer frequency information
 - accounts for differences in relative set size

* several applications
  - sample dissimilarity estimation
  - rapid microbiome catalogue searching
  - classification of microbiome samples in near real-time.

---

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/figures/pngs/figure-2.png" width="50%">

* microbiome samples from the CAMI project

* histosketched in 1m30s

* histosketches cluster by body site using Jaccard similarity

---

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/figures/pngs/figure-4.png" width="50%">

* indexed the sketches using LSH Forest scheme

* searches predominantly return samples from same body site

---

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/misc/hulk-logo-with-text.png" width="30%">

[H]()istosketching [U]()sing [L]()ittle [K]()mers

> $ conda install hulk || [github.com/will-rowe/hulk](https://github.com/will-rowe/hulk)
<br/>
> [Rowe, WPM et al. Streaming histogram sketching for rapid microbiome analytics. BioRxiv 2018](https://doi.org/10.1101/408070)

---

<img src="https://raw.githubusercontent.com/will-rowe/banner/master/misc/logo/banner-logo-with-text.png" width="40%">

> $ conda install hulk || [github.com/will-rowe/banner](https://github.com/will-rowe/banner)

----

Once we have identified microbiomes of interest, how can we quickly check for genes of interest?

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

---

<img src="https://raw.githubusercontent.com/will-rowe/groot/master/paper/img/misc/groot-logo-with-text.png" width="30%">

[G]()raphing [R]()esistance [O]()ut [O]()f me[T]()agenomes

> $ conda install groot || [github.com/will-rowe/groot](https://github.com/will-rowe/groot)
<br/>
> [Rowe, WPM et al. Indexed variation graphs for efficient and accurate resistome profiling. Bioinformatics 2018](https://doi.org/10.1093/bioinformatics/bty387)

----

Here comes an example....

---

Change this:

* some babies are given antibiotics from birth (e.g. to treat sepsis)

* do the babies have bacteria that are resistant to these drugs?

* if so, when does this happen?

* we have timecourse samples from these babies guts
 - we can extract the collective DNA
 - from this, we can get composite samples of the bacteria genomes

---

![img]({{site.url}}/slides/slide-data/iror/tmp.jpg)

Fig 1A: Sankey visualisation of the taxonomic composition of a pre-term infant gut microbiome, sampled 18 days post initial antibiotic treatment. 1B. Subgraph single blaSHV (blaSHV-11) gene present at day 7 post initial antibiotic treatment. 1C. Subgraph of multiple blaSHV variants present at day 18 post initial antibiotic treatment. Bubbles correspond to variant nodes that bring additional extended-spectrum beta-lactamase activity to blaSHV (e.g. blaSHV-40). Node width corresponds to read coverage.

----

![img]({{site.url}}/slides/slide-data/genome-science/tool-logos.png)

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
