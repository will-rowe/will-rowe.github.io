---
layout: publication
title:  "<strong>ROWE, W. P. M.</strong>, CARRIERI, A., ALCON-GINER, C., CAIM, S., SHAW, A., SIM, K., KROLL, J.S., HALL, L. J., PYZER-KNAPP, E. O., WINN, M. D. 2018. Streaming histogram sketching for rapid microbiome analytics. bioRxiv"
author: Will Rowe
group: publication
permalink: /hulk-paper/
doi: https://doi.org/10.1101/408070
---

# Streaming histogram sketching for rapid microbiome analytics

Will P. M. Rowe, Anna Paola Carrieri, Cristina Alcon-Giner, Shabhonam Caim, Alex Shaw, Kathleen Sim, J Simon Kroll, Lindsay J. Hall, Edward O. Pyzer-Knapp, Martyn D. Winn

***

## Abstract

### Motivation

The growth in publically available microbiome data in recent years has yielded an invaluable resource for genomic research; allowing for the design of new studies, augmentation of novel datasets and reanalysis of published works. This vast amount of microbiome data,  as well as the widespread proliferation of microbiome research and the looming era of clinical metagenomics, means there is an urgent need to develop analytics that can process huge amounts of data in a short amount of time.
To address this need, we propose a new method for the compact representation of microbiome sequencing data using similarity-preserving sketches of streaming k-mer spectra. These sketches allow for dissimilarity estimation, rapid microbiome catalogue searching, and classification of microbiome samples in near real-time.

### Results

We apply streaming histogram sketching to microbiome samples as a form of dimensionality reduction, creating a compressed ‘histosketch’ that can be used to efficiently represent microbiome k-mer spectra. Using public microbiome datasets, we show that histosketches can be clustered by sample type using pairwise Jaccard similarity estimation, consequently allowing for rapid microbiome similarity searches via a locality sensitive hashing indexing scheme.
Furthermore, we show that histosketches can be used to train machine learning classifiers to accurately label microbiome samples. Specifically, using a collection of 108 novel microbiome samples from a cohort of premature neonates, we trained and tested a Random Forest Classifier that could accurately predict whether the neonate had received antibiotic treatment (95% accuracy, precision 97%) and could subsequently be used to classify microbiome data streams in less than 12 seconds.
We provide our implementation, Histosketching Using Little K-mers (HULK), which can histosketch a typical 2GB microbiome in 50 seconds on a standard laptop using 4 cores, with the sketch occupying 3000 bytes of disk space.

### Availability

Our implementation (HULK) is written in Go and is available at:  [github.com/will-rowe/hulk](https://github.com/will-rowe/hulk) (MIT license).
