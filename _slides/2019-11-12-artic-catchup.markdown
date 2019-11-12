---
layout: slide-deck
title:  "ARTIC Catchup"
date: 2019-11-12 12:00:00 -0000
author: Will Rowe
permalink: /artic-catchup-2019/
group: presentation
theme: ember
transition: slide
---

<script type="text/template">

#### Nov 2019

***

# ARTIC Catchup

***

<br/>slides: [willrowe.net/artic-catchup-2019](https://willrowe.net/artic-catchup-2019)

----

## Assessing signal-level methods

***

* comparing signal vs. non signal methods
  * i.e. nanopolish vs. medaka

* variant calling and assembly polishing

* standardise the workflow and try different datasets
  * Ebola virus (**Mayinga**, Kikwit, Makona)
    * metagenomic protocol with rapid PCR kit
  * Nipah
  * both fast and high accuracy (HAC) basecalls

---

### Pipeline and notebooks

***

* Nextflow pipeline takes MinKnow output and reference genome multifasta
    * demux, trim, align, subsample
    * variant call and consensus build
    * reference guided assembly + correction
    * de novo assembly + correction
    * all the polishing combos (nanopolish + medaka)

* jupyter notebooks
  * fastani to refseq
  * consenus identity
  * nucdiff and msa

* plug and play Docker containers / conda envs
  * swap assemblers, trimmers etc.

* [github.com/will-rowe/signal-check](https://github.com/will-rowe/signal-check)

---

![](https://github.com/will-rowe/signal-check/raw/master/pipelines/long-read-assembly.png)

---

### Reference alignment + variant call

***

<img src="{{site.url}}/_slides/slide-data/artic-2019/nucdiff2.jpg" width="100%">

---

### Assemblies: consensus identity

***

<img src="{{site.url}}/_slides/slide-data/artic-2019/conident.jpg" width="100%">

---


### Assemblies: consensus identity

***

<img src="{{site.url}}/_slides/slide-data/artic-2019/nucdiff1.jpg" width="100%">

----

## Assessing signal-level methods

***

* high accuracy basecalling
  * no difference on variant calling
  * improves consensus identity of assemblies
  * not much improvement on medaka polished assemblies

* nanopolish vs medaka
  * no difference on variant calling
  * medaka polishing better than nanopolish
  * medaka then re-polishing with nanopolish gives best c. identity

* needs additional datasets for validation
  * known standard / synthetic dataset
  * pipeline takes 10 mins on laptop
  * try different subsampling thresholds

----

## Ditching signal for privacy-preserving apps

***

* given low utility of signal and small benefit of HAC (in some scenarios)

* white/black list reads during basecalling
  * discard all FAST5 and **bad** reads
  * e.g. remove human-derived data

* prevents recording/transfer of sensitive data

---

<img src="{{site.url}}/_slides/slide-data/artic-2019/antman-placeholder.png" width="30%">

[A]()utomated [N]()anopore [T]()racking [M]()odification [A]()nalysis daemo[N]()

> [github.com/will-rowe/antman](https://github.com/will-rowe/antman)

---

### Status:

***

* coded up proof of concept
  * C, basic unit tests, no CI

* basic system deamon
  * start/stop/restart
  * creates RAM disk
  * listens out for new FASTQ files
  * kmv sketch and containment search

* terrible backronym
  * small yet mighty?

---

### Next steps

***

* try out some different sketching approaches
  * containment good but do we want more?
  * approximate mapping, best hit etc.

* integration
  * daemon launch / status updates via WASM app
  * RAMPART / pipelines collect the filtered FASTQ
