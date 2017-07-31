---
layout: slide-deck
title:  "PostDoc Handover Notes"
author: Will Rowe
permalink: /postdoc-handover-notes/
group: presentation
theme: ember
transition: slide
---
<section>
<pre><code data-trim data-noescape>

### July 2017

***

# PostDoc Handover Notes

***

Slides online at:

[will-rowe.github.io/postdoc-handover-notes](https://will-rowe.github.io/postdoc-handover-notes)

----

## Overview

***

* Data management

 - sequence data
 - NAS storage
 - websites

* RNA-seq analysis

 - then and now
 - pipeline

* Some other projects

 - phylogenetic workflow
 - large-scale data analysis

----

## Data management: sequence data

***

We have **~435** RNA-seq libraries in the collection.

What are they, where are they located and what other data do we have?

---

### Data overview

* RNA-seq data from [Vertis](http://vertis-biotech.com/) (x8 batches) and [CGR](https://www.liverpool.ac.uk/genomic-research/) (x2 batches) sequencing centres (Illumina SE + IonTorrent)

* Genomic data from [MicrobesNG](https://microbesng.uk/) (Illumina PE) and [CGR](https://www.liverpool.ac.uk/genomic-research/) (Illumina PE + PacBio)

* Data not consolidated/backed-up pre-Will, we still have missing historic libraries and some unidentified libraries

* We also store raw data and analysis for ~9 collaborator projects

---

### Data storage

* All sequencing data (RNA-seq + genomic) is stored on the CGR servers

```html
/pub46/willr/000_HOME/0001_HINTON_DATA_BACKUP
```

* CGR backs up the *pub\** disks, rolling backups are kept for 1 month

* Sequencing data is also backed up to the Hinton NAS and a personal networked drive

* The NAS is accessible to registered users
 - access by IP address (if on Liverpool network)
 - access by [WD MyCloud](http://www.mycloud.com/) (remotely)
 - FTP access available for collaborators etc.

---

### General notes on sequencing data

* Vertis assigns VB identifiers to batches, we then reassign to a '*Vertis Number*' that can combine multiple VB identifiers / batches

* Data compression varies between centres / batches - most libraries re-compressed with gzip
 - filenames used by lab group may not reflect current compression

* Only some data was delivered with MD5 checksums

---

## Data management: websites

***

What websites and web resources do we maintain?

---

* Hinton Lab ([hintonlab.com](http://hintonlab.com))
 - dynamic site (written in Python - code [here](http://github.com/will-rowe/hintonlab.com))
 - run in Docker containers and deployed on [Digital Ocean](https://cloud.digitalocean.com) server
 - static content stored on [AWS S3](https://aws.amazon.com/s3/)
 - content management system for lab members to update content

* 10K *Salmonella* Genomes Project ([10k-salmonella-genomes.com](http://10k-salmonella-genomes.com))
 - static site (using [Jekyll](http://jekyllrb.com/) and [GitHub Pages](https://pages.github.com/))
 - commit markdown entries to [repo](https://github.com/10k-salmonella-genomes) to update site

* Other information
 - domains registered with [123-reg](https://123-reg.co.uk)
 - AWS S3 also used to host [JBrowse instances](http://hinton-jbrowse.s3-website-eu-west-1.amazonaws.com/JBrowse/index.html?data=00_SAMPLES/ben-2017/data) and [RNA-seq analyses](http://hinton-analysis.s3-website-eu-west-1.amazonaws.com/projects/rocio-07.10.2016/RNAseq-analysis.html) (DGE reports etc.)
 - Will's website ([will-rowe.github.io](http://will-rowe.github.io)) for LIFE708 teaching / bioinformatics notes

----

## RNA-seq: then and now

***

How did the lab used to analyse RNA-seq data, what do we now do and what are the improvements?

---

### Old RNA-seq analysis

* No replicates used
 - expression differences between conditions represent either biological differences or experimental noise
 - to identify statistically significant expression, difference between conditions must be > uncertainty within a condition
 - replicates needed to asses variability - increases certainty of recorded expression levels

* Old (Dublin?) pipeline
 - no library QC (at least none is documented)
 - used iterative shortening of reads until they align - increases missmaps, multimaps etc.
 - used outdated unique alignment concept - should instead consider probability of incorrect alignments ([MAPQ](http://lh3lh3.users.sourceforge.net/mapuniq.shtml))

* Use of Transcripts Per Million (TPM) for DGE
 - TPMs are for *within-sample* normalisation (removes gene length + library size effects)
 - don't need to account for gene length between samples + we don't want to remove depth info
 - normalisation must account for skewed count distributions (from DE genes)

---

### Current RNA-seq analysis

* Replicates!
 - we are now using at least 3 replicates
 - ideally, these are prepared by the same person and sequenced in the same batch...

* New pipeline
 - extensive QC
 - uses MAPQ score to filter alignments
 - much quicker runtimes
 - outputs files needed for JBrowse / DGE analysis
 - also outputs TPMs and QC report

* Limma/Voom used for DGE
 - linear modelling with mean-variance weighting
 - [Degust](http://degust.erc.monash.edu/) used for visulation and draft DGE analysis
 - in-house R scripts / markdown reports used for full [DGE analysis](http://hinton-analysis.s3-website-eu-west-1.amazonaws.com/projects/rocio-07.10.2016/RNAseq-analysis.html)

---

### Pipeline software

* QC
  - **FastQC**: assess read quality/length, adapters etc.
  - **Kraken**: do we have our typical taxonomic signature?
  - **Trimmomatic**: remove adapters and improve read quality
  - **DupRadar**: do we have any PCR artifacts?

* Alignment
  - **Bowtie2**: align reads to a reference genome
  - **Samtools**: alignment post-processing
  - **Bedtools**: are the usual transcripts covered? (QC)
  - **FastQC**: repeat initial QC and get alignment stats (QC)

* Quantify
  - **FeatureCounts**: count reads per feature

---

* The pipeline knits all these pieces of software together
  - checks input files
  - runs software
  - parses information
  - computes TPM values, outliers, signatures
  - writes sample reports

* Once complete, downstream analysis begins
  - R pipelines for differential expression testing (limma-voom / edgeR)
  - Bash pipeline for JBrowse visualisation
  - Degust for DGE visualisation

* Examples
  - [installing](https://github.com/will-rowe/rnaseq)
  - [multiQC report]({{site.url}}/_slides/slide-data/handover-data/00_multiqc_report.html)
  - [error-log]({{site.url}}/_slides/slide-data/handover-data/log-fail-eg.txt)
  - [full-log]({{site.url}}/_slides/slide-data/handover-data/log-eg.txt)

---

### Current pipeline to-do list

* Inclusion of heatmap script to visualise sample similarity
 - pairwise euclidean distance for batch effect identification

* Estimate library complexity with [Preseq](http://smithlabresearch.org/software/preseq/)
 - have we sequenced deeply enough?

* Fully automate downstream tasks
 - e.g. create JBrowse instances

----

## Other projects

***

* Phylogenetics workflow

* Variant calling

* Short read aligner

* RNA-seq collaborations

* Dragons den

* [Bioinformatics lunch and teaching](https://will-rowe.github.io)

* [HiPy](http://www.hipy.uk/)

---

### Phylogenetic workflow: [pipeline 2]((https://github.com/will-rowe/gopherseq)

***

* QC the data
  - similar workflow to the RNA-seq pipeline

* Align data to suitable reference
  - align with BWA-MEM
  - post-process & QC the alignment
  - InDel correction, base recalibration, mark duplicates etc.

* Variant call
  - call & filter SNPs
  - create pseudogenome
  - MSA, mask, snp-sites & make tree (manual atm.)

* Feature coverage
  - calculate feature coverage for targets
  - R for heatmaps ([example-output]({{site.url}}/_slides/slide-data/handover-data/plot.png))

---

### Short read aligner: [MinHash + variant graph alignment]()

***

Identifying highly similar genes in composite samples?

---

![figure]({{site.url}}/_slides/slide-data/handover-data/figure.png))

----

## Final notes

***

* If you want even quicker RNA-seq runtimes, try older version of the pipeline which sends jobs from the head node, writes to scratch disks and then collates all data when done (it's not very friendly)

* We are yet to generate enough data to justify automated distributed backups, but it is something to consider in the future

* Code / documentation for all projects is available on [GitHub](https://github.com/will-rowe)

----

## Thank you

***

* Thank you all for your help and support over the last 1.5 years, and the interesting problems to work on

* I'm starting at the Science and Technologies Facilities Council next week
 - Europe's largest multidisciplinary research organisation
 - government agency that carries out and funds research in biology, physics and engineering
 - I'll be a computational biologist at the Hartree Centre (Daresbury)

* I'm happy to continue helping with your projects and future collaborations

* w.p.m.rowe@gmail.com ([@wil_rowe](https://twitter.com/wil_rowe))

</code></pre>
</section>
