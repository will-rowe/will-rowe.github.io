---
layout: default
title:  "A general RNA-seq analysis guide"
date:   2016-10-21 17:50:00
categories: main
group: RNA-seq
---

<h1>A general RNA-seq analysis guide</h1>

This page covers general notes on RNA-seq. It includes links to relevant literature, how-to guides and an overview of the analyses we are currently performing at the Hinton Lab. The content can be divided into:

* TOC
{:toc}


**Caveat** - this guide is written for the Hinton Lab and based on single end, strand-specific, Illumina sequencing data.


---

# The basics

A few notes here on the principles of RNA-seq...

---

# Quality assessment of RNA-seq experiments

## 1. sequence read quality

The first analysis stage for an RNA-seq experiment is to quality check the raw reads obtained from the sequencing run (i.e. the data we receive from Vertis/CGR). We want to make sure that we have an appropriate number of reads for the sample from the sequencing lane and we want to identify artefacts, contamination or other sources of bias that will impact the downstream analysis.

The set of quality checks we use can be divided into:

* Sequence adapter removal

	Sequencing adapters are an important part of generating the sequencing library but they must be removed prior to downstream analysis. Tools such as Trimmomatic and Cutadapt can remove adapter sequences from the data (however the sequencing center often removes them for you).

	> Side note: Adapters are added to the (c)DNA fragments during library preparation to: 1. bind fragments to the flowcell, 2. allow for PCR enrichment of bound fragments and 3. allow for barcoding of multiple libraries

* Assessing sequence read quality

	The quality of each sequence read is assessed using a base calling accuracy metric (Phred quality or Q score). **The Q score indicates the probability that a given base is called incorrectly by the sequencer**. The Q score is defined as: Q = -10log10(e) where e is the estimated probability of the base call being wrong. Therefore, a score of Q20 means that the probability of an incorrect base call is 1 in 100, and the overall accuracy of the base call is 99%. Typically read quality gradually declines toward the 3' end of reads (e.g. because of the read cluster becoming out of phase), however a sudden drop in quality can indicate an error during sequencing (e.g. flooding of the flow cell).

	It is important to remove low quality bases as low Q scores can reduce the sensitivity and/or speed of read aligners, increase false positive variant calls, reduce the quality of assemblies etc.

	Tools such as Trimmomatic and the FastX-toolkit can provide quality based read trimming to remove poor quality bases (e.g. sliding window trimming) or exclude reads completely from subsequent analysis.

* Check sample composition (GC content)

	The GC content of reads from a random sequence library follow a normal distribution (mean equals overall GC content of transcriptome), therefore per sequence GC content is a rough measure of the randomness of sequencing. I.E. A poorly prepared or contaminated library will exhibit a skewed distribution ([1][1]).

	GC-rich and GC-poor fragments are often under-represented in RNA-Seq and biases related to GC-content confound differential expression analysis ([2][2]).

	> Side note: GC content is the % of bases in a sequence that are either guanine or cytosine. GC pairs are more stable than AT, a high GC content is typically associated with biological function.

* Detection of artefacts and contamination (duplicated reads / over-represented k-mers)

	Checking read duplication levels is standard in NGS QC as it can indicate if there has been problems in the library prep (e.g. low library complexity or over-amplification, issues with reagents or overloading of flow cells). **However**, in RNA-seq libraries it is difficult to differentiate between read duplication that arises from high gene expression vs. the fraction induced by artefacts (e.g. PCR bias based on GC content or length due to non-linear amplification). One approach to circumvent this issue is to relate the read duplication rate to the length-normalised read counts of each gene, modeling the dependency of these two variables ([3][3]).

	Over-represented k-mers can be used to identify different issues with the library. For example, over-represented k-mers at the end of the read are a good indicator of adapter contamination (which should be removed, see above). Another example of the use of over-represented k-mers is to identify biases in libraries caused by random hexamer priming (not that random in some cases) ([4][4]).

	> Side note: A kmer is a nucleotide sequence of fixed k length.

	Taxanomic binning of sequencing reads can be used to identify possible biological contamination in the library. For example, Kraken aligns k-mers to a classification database and bins reads according to taxa ([5][5]).


## 2. sequence read alignment

Once sequence read quality has been assessed, the reads are aligned to the reference genome (or transcriptome) to allow for quantification of transcripts. The proportion of aligned reads is a good indicator of the sequence library quality - a proportion lower than expected can indicate low sequencing quality, contamination or an inappropriate choice of reference genome.

The expected proportion of aligned reads is specific to the experiment, factors influencing this value range from the aligner used through to the organism / choice reference genome. For instance, we usually see 90-99% reads from an in vitro *Salmonella typhimurium* RNA-seq library align to the D23580 reference genome when using the Hinton Lab RNA-seq pipeline. Anything below 90% would raise a flag and encourage us to check the library for contamination / variation from our reference sequence.


## 3. reproducibility

Reproducibility is a critical factor when designing and assessing the quality of RNA-seq experiments. Sufficient replicates are required to distinguish between biological variation vs. experimental noise. In other words, increasing the number of measurements increases the certainty of the expression levels (allowing for detection of statistically significant gene expression).

**Experiments should be designed to obtain enough data to assess a gene's mean expression level AND the amount of variance expected in the measurement.**

In terms of reproducibility and assessing the quality of an RNA-seq experiment, we first check to see that technical replicates (if present) are highly correlated (RHO > 0.9). This is important to know as high technical variability has been shown to result in quantification inconsistencies for genes of low coverage ([6][6]). Secondly, if we are expecting differences in gene expression between experimental conditions, we check to see that biological replicates cluster according to experimental condition (but we come on to this in more detail later).

---

# Quantifying transcripts


In various stages of the processing of an NGS dataset it can be useful to filter the data to remove poor quality reads.  At an early stage this could be reads with poor quality base calls, but after aligning to a reference genome you may want to filter out alignments which show a poor match to the reference, or which could have aligned to a number of different places in the genome. These ambiguously aligned reads can add a lot of noise to an analysis and will tend to accumulate over repetitive regions.  In the example below you can see the comparison of the reads from a standard bowtie2 aligning of a genomic dataset, and the result of applying a MAPQ filter to the data.  The peaks over repetitive elements are largely suppressed in the filtered data.


## 1. raw counts


## 2. within-sample normalisation

### RPKM / FPKM

### TPM


## 3. k-mer counting



---

# Differential Gene Expression


If gene expression differences exist among experimental conditions, it should be expected that biological replicates of the same condition will cluster together in a principal component analysis (PCA).


---

# Additional Analyses

## 1. identifying novel transcripts / de-novo transcript assembly

## 2. SNP calling from RNA-seq data

SNP calling from RNA-seq data without a reference genome: identification, quantification, differential analysis and impact on the protein sequence -
http://nar.oxfordjournals.org/content/44/19/e148.full

## 3. assembly


---

# Acronyms

NGS	-	next generation sequencing
QC	-	quality check


---

# References











really good [paper][paper]:
another 1: https://www.labome.com/method/RNA-seq-Using-Next-Generation-Sequencing.html#ref7



transcipt quantification vs. differential gene expression analysis

good blog post : http://michelebusby.tumblr.com/post/26913184737/thinking-about-designing-rna-seq-experiments-to

Seqlopedia article - http://rnaseq.uoregon.edu/#analysis
RNA-seq news - http://www.rna-seqblog.com/


[paper]: https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0881-8



[3]: http://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-016-1276-2
[4]: https://www.ncbi.nlm.nih.gov/pubmed/20395217
[1]: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3378858/
[2]: http://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-12-480
[5]: http://genomebiology.biomedcentral.com/articles/10.1186/gb-2014-15-3-r46
[6]: http://bmcgenomics.biomedcentral.com/articles/10.1186/1471-2164-12-293
