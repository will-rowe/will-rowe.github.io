---
layout: slide-deck
title:  "Leverhulme Fellowship Proposal"
author: Will Rowe
permalink: /leverhulme-fellowship-proposal/
group: presentation
theme: ember
transition: slide

---

<script type="text/template">

#### Leverhulme Fellowship 2019

***

# Novel methods for analysing long read sequencing data in real-time

***

<i>IIB grant review meeting</i>

----

***

## Fellowship proposal

***

* Leverhulme ECF (remit)
 - for early career researchers
 - must have a research record but not yet held a full-time permanent academic post
 - ECF is funding to undertake a significant piece of publishable work

* Dates
 - closing date of 28 February 2019 (4pm)
 - applications must be approved and submitted by the host institution

* Scheme
 - applications will be considered in all subject areas (except clinical/medical applications)
 - ECF contributes 50% of Fellow's total salary costs (max. £25,000p.a)
 - annual research expenses of up to £6,000p.a

----

### Background: [big data in genomics]()

***

* Genomic research has a 'big data' problem
  - collections of genomic data continue to grow
  - certain questions don't scale
    - finding frequent items
    - counting distinct elements
  - requires large time/compute resources

* Rather than bigger computers, what can we do?
 - reduce the data
 - approximate the data
 - streamline data processing

----

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

* [Data sketching]() enables genome analytics
 - can process data streams in real time with small, fixed memory use
 - only needs a single pass of the data
 - probabilistic but with error bounds

* Recent MinHash sketching tools ( e.g. [mash]()) are great!
 - find similar genomes
 - find what's in your microbiome sample
 - get distances to build quick trees

* There are some drawbacks to MinHash
 - MinHash doesn't include k-mer frequency information during sketching
 - MinHash doesn't account for impact of relative set size

> Rowe, WPM. [When The Levee Breaks: a practical guide to data sketching algorithms... 2019. In preparation]()

---

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/figures/pngs/figure-1.png" width="60%">

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

* Histosketched 48 microbiome samples in [1m30s]()

* Histosketches cluster by body site using Jaccard similarity

---

![]({{site.url}}/slides/slide-data/melb-2018/iaai19.png)

* Multi-class and binary classification of microbiome samples

* Relevance Vector Machine (RVM), Support Vector Machines (SVM), Random Forests (RF), Naive Bayes (NB)

> Carrieri , AP., Rowe, WPM et al. [A Fast Machine Learning Workflow for Rapid Phenotype Prediction from Whole Shotgun Metagenomes. IAAI 2019]()

---

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

* [Gene profiling]() can be difficult
 - high similarity between reference genes
 - genomic data can be massive in size
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

---

<img src="https://raw.githubusercontent.com/will-rowe/groot/master/paper/img/misc/groot-logo-with-text.png" width="30%">

[G]()raphing [R]()esistance [O]()ut [O]()f me[T]()agenomes

> $ conda install groot || [github.com/will-rowe/groot](https://github.com/will-rowe/groot)

---

![]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/fig5-groot.png)

----

### Background: [long read sequencing]()

***

* Increasing uptake of long read sequencing technologies
  - relatively new technology
  - generates large amounts of data
  - several unsolved big data challenges 
    * how to efficiently process, analyse and store the data

* Need solutions to big data problems in the long read space
  - aim to lower sequencing costs and reduce runtimes
  - facilitate scientific discoveries via long read technologies
  - focus on Oxford Nanopore Technologies (ONT)

----

Design and implement novel algorithms for real-time long read sequencing analytics

----

### Aims

***

* [Aim 1](): selective sequencing using squiggle space sketches

* [Aim 2](): aligning reads to variation graphs in squiggle space

* [Aim 3](): squiggle compression and indexing

* [Aim 4](): quantum algorithms for read classification

---

### Aim 1: [selective sequencing using squiggle space sketches]()

***

* Selective sequencing enables target enrichment or host depletion
 - bypasses lab assays
 - reduces sequencing costs and runtimes
 - barriers had been due to hardware and software limitations
 - Nanopore long read, native DNA sequencing overcomes hardware limits

* Nanopore '[Read Until]()' API
 - all nanpore channels are addressable during sequencing
 - they can be instructed to reverse voltage, ejecting a DNA fragment
 - new fragment can enter channel for sequencing
 - address channels using the Read Until API

---

### Aim 1: [selective sequencing using squiggle space sketches]()

***

* Electrical current signal generated as long reads transit nanopores
 - termed '[squiggle space]()'
 - operating in squiggle space bypasses basecalling and polishing
 - squiggle space has not undergone any dimensionality reduction
 - faster runtimes, lower resource requirements, greater accuracy

* Squiggle space and Read Until API for selective sequencing
 - one publication to date (Loose et al. 2016)
 - relied on significant compute (64 cores for 512 pore flowcell)
 - restricted to amplicon sequencing
 - ONT throughtput was much lower than it is now

---

### Aim 1: [approach]()

***

* Sketch squiggle space in real time using the histosketch algorithm
 - squiggles are determined by the ONT event detection software
 - bypass the need to enter nucleotide space
 - account for changes in the underlying distribution of the data (concept drift)

* Try several other sketching algorithms, e.g. Order MinHash
 - utilise event order during sketching
 - sampling consecutive event tuples increases accuracy
 - would require windowing strategy

* Apply Machine Learning to sketches
 - as in previous work, classify sketches in real time
 - use classifications to inform Read Until decision

---

### Aim 1: [approach]()

***

* Create sketching toolkit
 - operates in squiggle space
 - broad scope C library
 - build python/Go tools on top, utilising Read Until API

* Validation
 - ONT read simulator
 - train using RefSeq
 - ZymoBIOMICS log community standard



---

### Aim 2: [aligning reads to variation graphs in squiggle space]()

***

* Build upon existing squiggle mappers

* Combine with variation graph representation
 - more efficient than linear representation
 - add in all known variation
 - variant calling

* Use sketching algorithm to seed reads
 - e.g. MinHash + LSH Forest index

---

### Aim 3: [squiggle compression and indexing]()

***

* Rather than selective sequencing, index sequencing data
  - selective basecalling
  - sequence entire sample, analyse what you are interested in

* Improve storage efficiency
 - current bottlenecks involve I/O operations on raw data
  
---

### Aim 4: [quantum algorithms for read classification]()

***

* The best cases show exponential and super-exponential reductions in query complexity

* Supervised Machine Learning and classification

* Technology not ready for mass use yet
  - several intermediate-scale quantum (NISQ) devices exist
  - can use these machines (~4 qubits)
  - can show utility of these algorithms in real-time long read analytics

----

### Costs

***

| YEAR | ITEM | £GBP |
|------|------|------|
|   1  |   Nanopore Startup Package + extra flowcell   |    1,470   |
|   1  |   ZymoBIOMICS Microbial Community Standard II (Log Distribution)   |    351   |
|   1  |   Travel (conferences)    |    1,000   |
|   2   |   Nanopore consumables + flowcells    |   4,000    |
|   2   |   Travel (conferences)    |   1,000    |
|   2  |   Publication costs   |   1,000   |
|   3   |   Travel (conferences)    |   2,000    |
|   3  |   Publication costs   |   3,000   |


----

### Summary

***

* Genomics has a big data problem
 - lots of development in this area
 - data sketching is a family of algorithms used to address this
 - I have a lot of experience of applying sketching in this area

* Long read sequencing data is one emerging problem
 - how to analyse, index and store?
 - can these operations be performed in real time?
 - consequently, can this feedback to inform sequencing?

* Design and implement novel algorithms for sequencing analytics
 - data sketching for real-time signal processing
 - quantum machine learning algorithms for read classification

</script>
<section>
    <pre><code data-trim data-noescape>
    </code></pre>
</section>
