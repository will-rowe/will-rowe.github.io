---
layout: publication
title:  "<strong>ROWE, W. P. M.</strong>, WINN, M. D. 2018. Indexed variation graphs for efficient and accurate resistome profiling. Bioinformatics bty387"
author: Will Rowe
group: publication
permalink: /groot-paper/
---

# Indexed variation graphs for efficient and accurate resistome profiling

Will P. M. Rowe, Martyn D. Winn

## Abstract

### Motivation
Antimicrobial resistance (AMR) remains a major threat to global health. Profiling the collective AMR genes within a metagenome (the ‘resistome’) facilitates greater understanding of AMR gene diversity and dynamics. In turn, this can allow for gene surveillance, individualized treatment of bacterial infections and more sustainable use of antimicrobials. However, resistome profiling can be complicated by high similarity between reference genes, as well as the sheer volume of sequencing data and the complexity of analysis workflows. We have developed an efficient and accurate method for resistome profiling that addresses these complications and improves upon currently available tools.

### Results
Our method combines a variation graph representation of gene sets with a locality-sensitive hashing Forest indexing scheme to allow for fast classification of metagenomic sequence reads using similarity-search queries. Subsequent hierarchical local alignment of classified reads against graph traversals enables accurate reconstruction of full-length gene sequences using a scoring scheme. We provide our implementation, graphing Resistance Out Of meTagenomes (GROOT), and show it to be both faster and more accurate than a current reference-dependent tool for resistome profiling. GROOT runs on a laptop and can process a typical 2 gigabyte metagenome in 2 min using a single CPU. Our method is not restricted to resistome profiling and has the potential to improve current metagenomic workflows.

### Availability and implementation
GROOT is written in Go and is available at [github.com/will-rowe/groot](https://github.com/will-rowe/groot) (MIT license).
