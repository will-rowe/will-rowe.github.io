---
layout: post
title:  "LIFE708 workshop 2 - genome assembly"
author: Will Rowe
group: teaching-life708
permalink: /LIFE708-workshop-2/
---

***

<h1>Contents</h1>

* TOC
{:toc}

***

# Introduction

This is the genome assembly practical for the LIFE708 genomics block. The objective is to assemble a draft genome from a sequencing experiment and to critically assess the quality of the assembly.


## Expected learning outcomes

* to be able to download and quality check publicly-available sequencing data

* to run a genome assembly program

* to observe the effect of varying k-mer size on assembly quality

* to compare a draft assembly to a reference (finished) genome


## Background

In this practical we will once again be looking at *Shigella flexneri*. We will be assembling a draft genome for *Shigella flexneri* 2a strain 2457T, that was sequenced as part of the STOPENTERICS consortium project to characterise Shigella strains and identify possible vaccine targets ([paper here](https://www.ncbi.nlm.nih.gov/pubmed/26042182)).


## Getting started

We will be working inside the LIFE708 Docker container again. Please follow the setup instructions [here](https://will-rowe.github.io/running-Docker/). There have been some updates since your last practical so please make sure you update the container by following the instructions.

> Note: Unless told to use the Graphical User Interface (GUI), type all commands into the running Docker container.

 * Once you are inside the running container, let's make all the directories we will need for the practical:

```
mkdir /MOUNTED-VOLUME-LIFE708/RefSeq
mkdir /MOUNTED-VOLUME-LIFE708/SEQUENCE_READS
mkdir /MOUNTED-VOLUME-LIFE708/ABySS_assembly
mkdir /MOUNTED-VOLUME-LIFE708/abacas
```


***

# Preparing the data

## Reference assembly

We will use the RefSeq database to download the reference sequence that we will be using to validate our assembly. The accession of the reference sequence is **NC_004741**.

* download the reference sequence

```
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/007/405/GCF_000007405.1_ASM740v1/GCF_000007405.1_ASM740v1_genomic.fna.gz -O /MOUNTED-VOLUME-LIFE708/RefSeq/NC_004741.fasta.gz
```

* download the annotation for the reference sequence

```
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/007/405/GCF_000007405.1_ASM740v1/GCF_000007405.1_ASM740v1_genomic.gff.gz -O /MOUNTED-VOLUME-LIFE708/RefSeq/NC_004741.gff.gz
```

* unzip the files

```
gunzip /MOUNTED-VOLUME-LIFE708/RefSeq/*.gz
```


## Sequence data

The Short Read Archive (SRA) stores sequencing data from high-throughput sequencing platforms. The data for our experiment is stored under the sequencing run accession number **ERR1107833**.

* first check that we have the right sample

Navigate to the sequencing run on the SRA website [here](https://trace.ncbi.nlm.nih.gov/Traces/sra/?run=ERR1107833) and check that the accession number is for the data we want (*Shigella flexneri*).

* download the sequence reads from the **ERR1107833** sequencing run

We will use **fastq-dump** from the SRA toolkit to download the data via the command line. This will save the sequencing data to our machines (in FastQ file format)

```
fastq-dump -origfmt -I --outdir SEQUENCE_READS -read-filter pass --skip-technical --split-files --gzip ERR1107833
```

This data may take a while to download (~3 minutes), so in the mean time please find out some basic information on this dataset from the SRA (using the link above):

> What sequencing platform was used?

> What's the coverage for this sequencing run?

> What's the read length?

> What's the GC content of this data - how does this compare to a typical *Shigella flexneri* genome?


## Quality checking

Now that we have the sequencing data, we need to check the quality before we assemble. We will use **FastQC** - a program which runs several analyses and gives an overview of whether the sequencing data has any problems that may impact downstream analysis.

* run FastQC

```
fastqc SEQUENCE_READS/*.fastq.gz
```

* check the FastQC output

```
head SEQUENCE_READS/ERR1107833_pass_*_fastqc/summary.txt
```

There are no QC fails in the FastQC report but we do have a couple of warnings. Let's open up the full report and check the plots. Navigate to the following directory **using the GUI** and open up the report:

```
LIFE708-WORKSHOP > SEQUENCE_READS > ERR1107833_pass_1_fastqc > fastqc_report.html
```

Discuss with each other whether or not you think we can proceed given these warnings - what might have caused them and will they affect the assembly?


## Quality trimming

Quality trimming is a process by which we remove sequencing adapters and low quality bases from sequencing data. Our data actually looks very good - there are no sequencing adapters detected (they are usually removed by sequencing centres but we should always check) and the per base sequence quality is good. There is a bit of a quality drop off in the last few bases of our reads, let's remove them using **fastq_quality_trimmer**.

* we'll use a small bash loop to trim each of our files (forward and reverse reads) and a bash sub-shell to unzip the files on the fly

```
for file in SEQUENCE_READS/*.fastq.gz
do
fastq_quality_trimmer -t 30 -l 100 -z -i <(zcat $file) -o ${file%.fastq.gz}.trimmed.fastq.gz
done
```

> the fastq_quality_trimmer command takes the input file, trims bases that have a QUAL < 30 and also removes the read if it is less than 100 bases long (after it has been trimmed)

Now that we have downloaded, quality checked and trimmed our data, it is time to try assembling it!

***

# Assembly

## ABySS

ABySS is the first assembly program we will use to assemble our trimmed reads. We use the **abyss-pe** command as we have paired-end reads. The k parameter sets the size of the k-mer to use in the graph. We also use the name parameter to set a prefix for the output files.

* To investigate the impact of varying k-mer size, we'll each get a random k-mer size and then compare our assembly results with each other

```
k_SIZE=$(shuf -i 29-49 -n 1)
```

* Now let's run ABySS!

```
cd ABySS_assembly

abyss-pe k=$k_SIZE name=shigella-$k_SIZE in='../SEQUENCE_READS/ERR1107833_pass_1.trimmed.fastq.gz ../SEQUENCE_READS/ERR1107833_pass_2.trimmed.fastq.gz'
```

This should take around 10 minutes to run on our machines - in the mean time, let's do another fun activity....


## Another activity here - ?????






## ABySS revisited

Our ABySS assembly should have finished by now. Let's check the assembly statistics and see how good our assembly is.

* ABySS gives us assembly statistics in an output file - take a look in shigella-k25-stats.md:

```
more shigella-$k_SIZE-stats.md
```

> How do your N50 values compare to neighbour's?

> Do you all have the same number of contigs?

* To compare our assembly to the reference sequence we downloaded earlier we first need to use a program called **abacas** to order the contigs from our assembly against the reference sequence.

```
cd ../abacas

abacas.pl -r ../RefSeq/NC_004741.fasta -q ../ABySS_assembly/shigella-$k_SIZE-contigs.fa -p nucmer -m
```

* **Use the GUI** to open up **ACT** on your computers and load in the files as instructed by the output from the abacas command. Your screen should look something like this:

!!!SCREEN SHOT HERE!!!

> Add questions here. e.g. contig breaks....

> Compare your assemblies with each other again.

**We should notice that some k-mer sizes have performed better than others....**


## SPAdes

SPAdes is another de Bruijn graph-based assembler but is different from ABySS because it generates a final assembly from multiple k-mers. A list of kmers is automatically selected by SPAdes using the maximum read length of the input data, and each individual kmer contributes to the final assembly.

SPAdes will take a while on our machines (as we are limited by RAM and time) so we have provided a SPAdes assembly for our data that was run using the following command:

```
spades.py -1 ../SEQUENCE_READS/ERR1107833_pass_1.fastq.gz -2 ../SEQUENCE_READS/ERR1107833_pass_2.fastq.gz --careful -t 40 -o SPAdes_assembly
```

* Let's get the SPAdes assembly and find out the assembly statistics using **assembly-stats**

```
mv /opt/LIFE708/solutions/workshop2/SPAdes_assembly /MOUNTED-VOLUME-LIFE708/

cd SPAdes_assembly

assembly-stats contigs.fasta
```

* Remembering **contiguity**, **completeness** and **correctness** from the lecture and using the techniques we have covered so far in this practical, is this a better assembly than the ABySS one?

> Discuss with your partners - make sure to use the reference assembly (**NC_004741**), are we missing contigs in any particular areas?


***

# Extra step - genome annotation

If you have finished the practical and want to try the next step in a typical genome assembly workflow, you can try annotating the SPAdes assembly.

 * to quickly annotate a genome, use **Prokka** (a tool for the rapid annotation of prokaryotic genomes)

```
prokka --cpus 1 --genus Shigella --species sonei /MOUNTED-VOLUME-LIFE708/SPAdes_assembly/scaffolds.fasta
```

As a final step, you can compare our assembled *Shigella flexneri* 2a strain 2457T against the NCTC1 (NZ_LM651928) genome that we looked at in workshop 1. Use **abacas** and **ACT** to perfrom a comparative genomic analysis and see if you can find any significant differences / additional gene content that show how Shigella has been changing over the years to remain a significant human pathogen.
