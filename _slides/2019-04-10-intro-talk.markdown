---
layout: slide-deck
title:  "Introduction"
author: Will Rowe
permalink: /intro-talk-2019/
group: presentation
theme: ember
transition: slide

---

<script type="text/template">

#### April 2019

***

# An introduction!

***

<i>an overview of my career, research and skills</i>

<br/>email: [will.rowe@stfc.ac.uk](will.rowe@stfc.ac.uk) | twitter: [wil_rowe](https://twitter.com/wil_rowe) | github: [will-rowe](https://github.com/will-rowe)

----

***

## About me

***

* I work for UK Research and Innovation (UKRI)
 - computational biologist
 - research novel methods for microbiome research
 - funded by the Innovation Return On Research programme

* Based in the North West of England
 - moved for my Post Doc (University of Liverpool)
 - work at Daresbury Laboratory
 - live on the Wirral with my wife and 2 children

* Career so far
 - education / academic
    - biomedical science -> human genetics -> genomics
 - post doctoral
    - microbiology -> computational biology -> industry

---

### Education

***

* BSc Human Genetics
  - Newcastle University
  - 2007 - 2010
  - specialise after 1 year Biomedical science

* Research placement
  - Institute of Human Genetics
  - 2010
  - post-transcriptional exon shuffling events
  - wet lab work, with some computational validation
  - published research

---

### Education cont.

***

* PhD Microbial Genomics
  - University of Cambridge
  - 2011 - 2015

* Antimicrobial Resistance (AMR) research
  - wet and dry lab research
  - published 3 first-author papers
  - released my first software

* Industrial CASE sponsorship
 - GSK and CEFAS
 - research placements
 - awarded several small additional grants

---

### Post Doctoral Work

***

* NHS scientist training programme
  - Cambridge University Hospitals
  - 2015 - 2016
  - clinical scientist (genomics and bioinformatics)
  - left to pursue research and develop computational skills

* Postdoctoral Research Associate
  - University of Liverpool
  - 2016 - 2017
  - microbial genomics research (Hinton Lab)
  - invasive nontyphoidal Salmonella disease

* Computational biologist
  - UKRI
  - 2017 - present
  - develop methods for research (industry and academia)

----

## Research

***

* AMR and the environment

* Genome biology

* Computational methods for microbiome genomics

---

### AMR and the environment

***

How to detect AMR genes?

> Rowe et al. SEAR: A cloud compatible pipeline and Web Interface for Rapidly Detecting Antimicrobial Resistance Genes Directly from Sequence Data. PLoS One. 2015

Are AMR genes present in human and animal effluents 

> Rowe et al. Comparative metagenomics reveals a diverse range of antimicrobial resistance genes in effluents entering a river catchment. WaterSciTech. 2016

Are AMR genes being used in the environment and are we impacting this?

> Rowe et al. Over-expression of antibiotic resistance genes in hospital effluents over time. Journal Antimic. Chem. 2017

---

![jac-study-fig-2](/slides/slide-data/host-microbe-talk-jan2017/jac-2.png)

 * over-expression of beta-lactam resistance genes in hospital effluents
   * metagenomics + metatranscriptomics, 3 replicates per site
   * normalise transcript abundance to gene abundance

---

### Invasive nontyphoidal Salmonella disease

***

What differences between S. Typhimurium sequence types may cause the observed differences in pathogenicity?

> Canals, R et al. Adding function to the genome of African Salmonella Typhimurium ST313 strain D23580. PLoS biology. 2019

* genomics, transcriptomics and proteomics to characterise an invasive Salmonella and compare to a gastroenteritis-associated sequence type

---

![plosbio](https://journals.plos.org/plosbiology/article/figure/image?size=large&id=10.1371/journal.pbio.3000059.g008)

* transcriptional differences between S. Typhimurium D23580 and S. Typhimurium 4/74

---

### Computational methods for microbiome research

***

How can we determine allelic differences in composite samples?

> Rowe and Winn. Indexed variation graphs for efficient and accurate resistome profiling. Bioinformatics. 2018

Can we predict sample origin during sequencing / data retrieval?

> Rowe et al. Streaming histogram sketching for rapid microbiome analytics. Microbiome. 2019

> Carrieri, Rowe et al. A Fast Machine Learning Workflow for Rapid Phenotype Prediction from Whole Shotgun Metagenomes. IAAI. 2019

Are there associations between clinical metadata and microbiome composition?

> IBM, Unilever and UKRI. Characterising the microbiome of dry skin. In preparation. 2019

----

## Skills

***

* Experimental design
  * successsful academic track record
  * grant awardee

* Computational Biology
  * data sketching algorithms
  * graph encoding
  * machine learning (RF, SVM, regression)

* Coding
  * Go, Python, C
  * Version control & reproducibility (conda, Nextflow, Jupyter etc.)

* Web
  * HTML, CSS, JS
  * Django, AWS, Docker

---

![github](/slides/slide-data/intro/github.png)

* visit [GitHub](https://github.com/will-rowe) to view my Open Source projects and contributions

---

![sourcerer1](/slides/slide-data/intro/sourcerer1.png)

* breakdown of my current GitHub projects

----

#### Introduction 2019

***

# Thanks for listening

***

<br/>email: [will.rowe@stfc.ac.uk](will.rowe@stfc.ac.uk) | software: [github.com/will-rowe](https://github.com/will-rowe)

twitter: [wil_rowe](https://twitter.com/wil_rowe) | slides: [will-rowe.github.io](https://will-rowe.github.io/intro-talk-2019)

----

Extra content...

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

<img src="https://media.springernature.com/full/springer-static/image/art%3A10.1186%2Fs40168-019-0653-2/MediaObjects/40168_2019_653_Fig1_HTML.png" width="60%">

> Rowe, WPM et al. [Streaming histogram sketching for rapid microbiome analytics. Microbiome 2019](https://doi.org/10.1101/408070)

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

> Carrieri , AP., Rowe, WPM et al. [A Fast Machine Learning Workflow for Rapid Phenotype Prediction from Whole Shotgun Metagenomes. IAAI 2019]()

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


</script>
<section>
    <pre><code data-trim data-noescape>
    </code></pre>
</section>
