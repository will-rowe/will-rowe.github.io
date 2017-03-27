---
layout: post
title:  "Workshop 2 - genome assembly"
author: Will Rowe
group: life708-workshop
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

* to observe the effect of assembler and parameter choice on assembly quality

* to compare a draft assembly to a reference (complete) genome


## Background

In this practical we will once again be looking at *Shigella flexneri*. We will be assembling a draft genome for *Shigella flexneri* 2a strain 2457T that was sequenced as part of the STOPENTERICS consortium project to characterise Shigella strains and identify possible vaccine targets ([paper here](https://www.ncbi.nlm.nih.gov/pubmed/26042182)).


## Getting started

We will be working inside the LIFE708 Docker container. **IT HAS BEEN UPDATED SINCE THE LAST PRACTICAL** - please follow the setup instructions again [here](https://will-rowe.github.io/running-Docker/).

> Note: Unless told to use the Graphical User Interface (GUI), type all commands into the running Docker container.

 * Once you are inside the running container, let's make all the directories we will need for the practical:

```
mkdir /MOUNTED-VOLUME-LIFE708/RefSeq
mkdir /MOUNTED-VOLUME-LIFE708/SEQUENCE_READS
mkdir /MOUNTED-VOLUME-LIFE708/Velvet_assembly
mkdir /MOUNTED-VOLUME-LIFE708/abacas
```


***

# Preparing the data

## Reference genome

For this practical we need a reference genome in order to validate the assemblies we make. We will use the RefSeq database to download an appropriate reference genome. The accession of the *Shigella flexneri* 2a reference genome is **NC_004741** - this genome assembly is complete (i.e. there is a single scaffold with no gap / unknown bases)

* download the reference genome sequence

```
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/007/405/GCF_000007405.1_ASM740v1/GCF_000007405.1_ASM740v1_genomic.fna.gz -O /MOUNTED-VOLUME-LIFE708/RefSeq/NC_004741.fasta.gz
```

> **wget** is a program that retrieves content from web servers, the -O option tells it where to save the file

* download the reference genome annotation

```
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/007/405/GCF_000007405.1_ASM740v1/GCF_000007405.1_ASM740v1_genomic.gff.gz -O /MOUNTED-VOLUME-LIFE708/RefSeq/NC_004741.gff.gz
```

* uncompress the downloaded files

```
gunzip /MOUNTED-VOLUME-LIFE708/RefSeq/*.gz
```

> **gunzip** is part of the **gzip** compression utility - we compress files to make them easier to manage / transfer


## Sequence data

We next need to download the sequencing data for our experiment. The Short Read Archive (SRA) stores sequencing data from high-throughput sequencing platforms. The data for our experiment is stored under the sequencing run accession number **ERR1107833**.

* first check that we have the right sample

Navigate to the sequencing run on the SRA website [here](https://trace.ncbi.nlm.nih.gov/Traces/sra/?run=ERR1107833) and check that the accession number is for the data we want (*Shigella flexneri*).

* download the sequence reads from the **ERR1107833** sequencing run

```
fastq-dump --outdir SEQUENCE_READS -read-filter pass --readids --skip-technical --split-files --gzip ERR1107833
```

> **fastq-dump** is part of the SRA toolkit and is used to download sequence data from the SRA, saving it in **FASTQ** format on our machines. We supply several options to filter what data we receive - check the toolkit manual to find out more!


This data may take a while to download (~3 minutes), so in the mean time please find out some basic information on this dataset from the SRA (using the link above):

> What sequencing platform was used?

> What's the coverage for this sequencing run?

> What's the read length?

> What's the GC content of this data - how does this compare to a typical *Shigella flexneri* genome?

## Quality checking

### initial QC

Now that we have the sequencing data, we need to check the quality before we assemble. We will use **FastQC** - a program which runs several analyses and gives an overview of whether the sequencing data has any problems that may impact downstream analysis.

* run FastQC

```
fastqc SEQUENCE_READS/*.fastq.gz
```

> Whilst this is running, check the **FastQC** [documentation](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/) to find out more about the program and how we might interpret the results

* check the FastQC output

```
head SEQUENCE_READS/ERR1107833_pass_*_fastqc/summary.txt
```

There are no QC fails in the FastQC report but we do have a couple of warnings. Let's open up the full report and check the plots. Navigate to the following directory **using the GUI** and open up the report:

```
LIFE708-WORKSHOP > SEQUENCE_READS > ERR1107833_pass_1_fastqc > fastqc_report.html
```

> Discuss with each other whether or not you think we can proceed given these warnings - what might have caused them and will they affect the assembly?


### quality-based trimming

Quality trimming is a process by which we remove sequencing adapters and low quality bases from sequencing data. Our data actually looks very good - there are no sequencing adapters detected (they are usually removed by sequencing centres but we should always check) and the per base sequence quality is good. There is a bit of a quality drop off in the last few bases of our reads, let's remove them using **fastq_quality_trimmer**.

* we'll use a small bash loop to trim each of our files (forward and reverse reads) and a bash sub-shell to unzip the files on the fly

```
for file in SEQUENCE_READS/*.fastq.gz
do
fastq_quality_trimmer -t 30 -l 120 -i <(zcat $file) -o ${file%.fastq.gz}.all-data.trimmed.fastq
seqtk sample -s100 ${file%.fastq.gz}.all-data.trimmed.fastq 0.7 > ${file%.fastq.gz}.trimmed.fastq
done
```

> the fastq_quality_trimmer command takes the input file, trims bases that have a QUAL < 30 and also removes the read if it is less than 120 bases long (after it has been trimmed)

> UPDATE 27/03/2017: I have just added a step to this loop in order to down-sample the sequence data as your computers are running very slow today - the assembly would take all day! We are using **seqtk** to use randomly select 70% of the sequence data. You don't need to include this step if you are running this tutorial yourselves!

Now that we have downloaded, quality checked and trimmed our data, it is time to try assembling it!


***

# Assembly

## Velvet

**Velvet** is the first genome assembler we will look at today. Velvet was one of the original De Bruijn graph-based assemblers and was released by Daniel Zerbino in 2008. See the paper [here](http://genome.cshlp.org/content/18/5/821.full) for more information on the algorithm.

* Velvet requires our input paired-end data to be interleaved into a single file

```
shuffleSequences_fasta.pl SEQUENCE_READS/ERR1107833_pass_1.trimmed.fastq SEQUENCE_READS/ERR1107833_pass_2.trimmed.fastq Velvet_assembly/input_reads.fq
```

* To investigate the impact of varying k-mer size, we'll each get a random k-mer size and then compare our assembly results with each other

```
KMER_SIZE=$(shuf -i 17-29 -n 1)
```

* Change directory to the `Velvet_assembly` directory

```
cd Velvet_assembly
```

* Velvet has two core algorithms, **velveth** and **velvetg** - first run **velveth** to generate a hash-table of all k-mer subsequences from our input data

```
velveth auto_$KMER_SIZE $KMER_SIZE -fastq -shortPaired1 input_reads.fq
```

> Did Velvet give you any warnings about your k-mer size? If so, why do you think that was?

* run **velvetg** to build the De Bruijn graph and resolve contigs

```
velvetg auto_$KMER_SIZE -exp_cov auto
```

> this command estimates the insert size, expected k-mer coverage and coverage cutoff - we'll discuss how to fine-tune these parameters and how to improve our assemblies later. We are also telling Velvet not to scaffold the contigs.

* let's get a some information on how good our assemblies are

```
assembly-stats auto_$KMER_SIZE/contigs.fa
```

> **assembly-stats** is a program from the Sanger Institute that gathers sequence length statistics from genome assemblies

* write your k-mer size, number of contigs and N50 on the whiteboard

> Do you all have the same number of contigs?

> How do your N50 values compare?


## SPAdes

We will now have a look at an assembly created using the SPAdes assembler. SPAdes is another De Bruijn graph-based assembler but is different from Velvet because it generates a final assembly based on multiple k-mers. A list of k-mers is automatically selected by SPAdes using the maximum read length of the input data, and each individual k-mer contributes to the final assembly.

SPAdes will take a while on our machines (as we are limited by RAM and time) so we have provided a SPAdes assembly for our data that was run using the following command (**you don't have to / can't run this**):

```
spades.py -1 SEQUENCE_READS/ERR1107833_pass_1.fastq.gz -2 SEQUENCE_READS/ERR1107833_pass_2.fastq.gz --careful -t 40 -o SPAdes_assembly
```

* Let's get the SPAdes assembly and find out the assembly statistics using **assembly-stats** (we'll look at the assembly scaffolds file as this is comparable to our Velvet assembly)

```
cp -r /opt/LIFE708/solutions/workshop2/SPAdes_assembly /MOUNTED-VOLUME-LIFE708/

assembly-stats /MOUNTED-VOLUME-LIFE708/SPAdes_assembly/scaffolds.fasta
```

> How do your Velvet assemblies compare to this SPAdes assembly?


## Group discussion

### recap & questions

Let's take a moment to talk about some of the things we have done so far and work through any issues we have noticed.

1. Did anyone receive a warning during velveth about their k-mer size? Think back to the lecture about why even-numbered k-mers are not desirable for De Bruijn graphs...

2. Did you notice the **N_count** and **Gaps** in the assembly stats? Why has Velvet introduced N's and Gaps during the assembly? Think about the filename of the SPAdes assembly that you looked at...

3. You should see from the whiteboard that as k-mer size increases, we are getting longer (& fewer) contigs in our Velvet assemblies. The general trend we should see is that the longer k-mers are resulting in better assemblies.

4. We have used short k-mer sizes for building our De Bruijn graphs with the Velvet assembler. This is for two reasons:

* we are using desktop computers with only 2GB RAM - the longer the k-mer, the more RAM needed. These machines can only manage an assembly with k-mers < 29

* we want to run the assemblies quickly - the main point of this practical is to learn how to run an assembly, observe the impact of k-mer size and to assess assembly quality


### k-mer size

In the real-world, a general rule of thumb for selecting a good k-mer size is to have k-mer size ~= 70% read length, which would mean our k-mer size should be around 78. A more accurate way of deciding k-mer size is to determine the **k-mer depth** of our samples.

* k-mer depth (KD) takes into account sequencing coverage (C) and read length (L)

```
KD = C*(L-K+1)/L
```

* generally, a longer k-mer means a better assembly **until** the k-mer depth becomes too low

* a k-mer depth of around 40x results in good assemblies, although k-mer depth will be impacted by sequencing quality and coverage variability

* in practice, we often use multiple k-mers and pick the best assembly - tools like **Velvet Optimiser** and assemblers like **SPAdes** will run assemblies using different k-mer sizes and use the best k-mer for a given assembly

* a good visual exploration of the effect of k-mer size can be found [here](https://github.com/rrwick/Bandage/wiki/Effect-of-kmer-size)


***

# Evaluating assemblies

Now that we have two assemblies for our sequencing data (Velvet & SPAdes), let's assess them both and determine which is the better assembly and how close they are to the reference genome. Remember from the lecture that we use the **contiguity**, **completeness** and **correctness** of an assembly to determine how good it is. We've already learned from the assembly-stats program that the SPAdes assembly has a lower N50 and more contigs than some of our Velvet assemblies (check the whiteboard). This means that in terms of contiguity, the Velvet assemblies are better than the SPAdes one...


## QUAST

Now let's focus on the **completeness** and **correctness** of our assemblies - we need our reference genome from the start of this practical in order to evaluate these two assembly characteristics. We'll use a program called **QUAST** - a Quality Assessment Tool for Genome Assemblies that has a web application version that we can use [here](http://quast.bioinf.spbau.ru/).


* First, let's rename our assemblies so that we can compare them more easily

```
cp /MOUNTED-VOLUME-LIFE708/Velvet_assembly/auto_$KMER_SIZE/contigs.fa /MOUNTED-VOLUME-LIFE708/velvet_assembly.fa

cp /MOUNTED-VOLUME-LIFE708/SPAdes_assembly/scaffolds.fasta /MOUNTED-VOLUME-LIFE708/spades_assembly.fa
```

* Now enter the two assemblies, the reference genome and the reference annotation (which we downloaded at the start) into [QUAST](http://quast.bioinf.spbau.ru/)

> The input form looks like this:

![quast-input]({{ site.url }}/_slides/slide-data/workshop2-assembly/QUAST-input.png){:width="500px"}

> The QUAST job should take a few minutes - you can type in your email address for a notification or just click on the report link to the right of the screen and wait for it to refresh.

* Once the report is complete, take a look and compare the two assemblies!

> The report looks like this:

![quast-report]({{ site.url }}/_slides/slide-data/workshop2-assembly/QUAST-report.png){:width="700px"}

> How complete are our assemblies?

> How correct are they? How many mismatches / misassemblies are there?


## Visualising with ACT

Based on the QUAST analysis of the assemblies, the SPAdes assembly is better than our Velvet ones. Although both the Velvet and SPAdes assemblies have similar contiguity (number / length of contigs) and completeness (reference genome fraction covered), the SPAdes one is more correct as it has far fewer mismatches and and misassemblies.

However, the SPAdes assembly is still only a draft genome and hasn't completely resolved our reference genome. Let's compare the SPAdes assembly to the reference and have a look at where we are currently unable to assemble

* To compare the assembly to the reference sequence we first need to use a program called **abacas** to order the contigs from our assembly against the reference genome

```
cd /MOUNTED-VOLUME-LIFE708/abacas

abacas.pl -r ../RefSeq/NC_004741.fasta -q ../spades_assembly.fa -p nucmer -m
```

* **Use the GUI** to open up **ACT** on your computers and load in the files as instructed by the output from the abacas command

> Your screen should look something like this:

![act-input]({{ site.url }}/_slides/slide-data/workshop2-assembly/ACT-input.png){:width="700px"}

* So we know what we're looking at, load the annotation onto the reference genome (NC_004741.gff)

![act-ann]({{ site.url }}/_slides/slide-data/workshop2-assembly/ACT-ann.png){:width="700px"}

> Using the same method, load the SPAdes assembly gaps and contigs (gaps.tab and fasta.tab files from our abacas directory) onto the SPAdes assembly track.

> Refer back to the last workshop for a reminder on using ACT. Ask us if you need a hand!


Now that we have the SPAdes assembly and the reference genome aligned to one another, you have probably noticed the gaps where the SPAdes assembly hasn't covered the reference genome (remember the QUAST report - the SPAdes assembly had 92% Genome fraction).

> What genes / features are in these regions that mean we haven't been able to assemble using our sequencing data?

* Use ACT to highlight and zoom in on the largest gap in the SPAdes assembly (region = 2471746..2513880)

![act-region]({{ site.url }}/_slides/slide-data/workshop2-assembly/ACT-region.png){:width="700px"}

* Create a feature in the reference genome for this range and then look at the genes within it

> Do you notice any common gene types or features - if so, do you think they might be responsible for the assembly gap we are investigating?


## Final thoughts

You hopefully have now found tRNA genes, transposon genes and repeat regions within the region we were investigating. As we discussed in the lecture, repetitive regions such as these severely impact our ability to fully assemble genomes (useful [review paper](http://www.sciencedirect.com/science/article/pii/S0888754312001279)). This is the main reason we have to perform additional work (through PCR / long read sequencing) to *complete* our draft genome assemblies.


***

# Extra step - genome annotation

You have now finished the practical - you have performed a genome assembly experiment, going from raw data to evaluating the draft assembly. If want to try the next step in a typical genome assembly workflow, you can try annotating the SPAdes assembly.

 * to quickly annotate a genome, use **Prokka** (a tool for the rapid annotation of prokaryotic genomes)

```
prokka --cpus 1 --genus Shigella --species flexneri --centre X --compliant /MOUNTED-VOLUME-LIFE708/abacas/spades_assembly.fa_NC_004741.fasta.fasta
```

As a final step, you can compare our assembled *Shigella flexneri* 2a strain 2457T against the NCTC1 (NZ_LM651928) genome that we looked at in workshop 1. Use **abacas** and **ACT** to perfrom a comparative genomic analysis and see if you can find any significant differences / additional gene content that show how Shigella has been changing over the years to remain a significant human pathogen.
