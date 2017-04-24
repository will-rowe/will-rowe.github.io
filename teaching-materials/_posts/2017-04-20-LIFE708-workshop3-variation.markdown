---
layout: post
title:  "Workshop 3 - identifying variation"
author: Will Rowe
group: life708-workshop
permalink: /LIFE708-workshop-3/
---


***

<h1>Contents</h1>

* TOC
{:toc}

***

# Introduction

This is the genome variation workshop for the LIFE708 genomics block. The objective is to identify a gene variant in a bacterial pathogen (*Shigella sonnei*) that may be responsible for an observed antibiotic resistance phenotype.


## Expected learning outcomes

* to be able to download and quality check publicly-available sequencing data

* run a short read alignment program

* to perform a variant call

* to interpret called variants and identify potential causes for observed phenotype


## Background

In this workshop we will be looking at the bacterial pathogen, *Shigella sonnei*. The scenario is that we have isolated *S. sonnei* from a patient and the isolate appears to be resistant to **nalidixic acid** (a quinolone antibiotic). As wild type *S. sonnei* is not resistant to quinolones, we would like to find out if our isolate has a genotype that would confirm the observed phenotype. To do this, we have **whole genome sequenced** our isolate and this workshop will involve us aligning the sequencing reads to a reference, calling variants and then interpreting the results to see if any variants account for the observed quinolone resistance.


## Getting started

We will be working inside the LIFE708 Docker container again - if you need to refresh your memory, please follow the instructions [here](https://will-rowe.github.io/running-Docker/).

* To start with - let's clean up all unused containers

```
docker rm $(docker ps -aq)
```

* Pull the newest version of the LIFE708 container image

```
docker pull wpmr/life708:latest
```

* Now start up the LIFE708 Docker container

```
docker run -itP --rm --name life708-$USER -v ~/Desktop/LIFE708-WORKSHOP/:/MOUNTED-VOLUME-LIFE708 wpmr/life708:latest
```

> Note: Unless told to use the Graphical User Interface (GUI), type all commands into the running Docker container.

* Once you are inside the running container, let's make all the directories we will need for the workshop:

```
cd /MOUNTED-VOLUME-LIFE708
mkdir RefSeq
mkdir SEQUENCE_READS
```

***

# Preparing the data

## Reference genome

For this workshop we need a reference genome to align our sequencing reads to. Like last time, we will use the RefSeq database to download an appropriate reference genome. The accession of the *Shigella sonnei* reference genome is **NC_007384**.

* download the reference genome sequence

```
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/refseq/bacteria/Shigella_sonnei/latest_assembly_versions/GCF_000092525.1_ASM9252v1/GCF_000092525.1_ASM9252v1_genomic.fna.gz -O /MOUNTED-VOLUME-LIFE708/RefSeq/NC_007384.fasta.gz
```

> **wget** is a program that retrieves content from web servers, the -O option tells it where to save the file

* download the reference genome annotation

```
wget ftp://ftp.ncbi.nlm.nih.gov/genomes/refseq/bacteria/Shigella_sonnei/latest_assembly_versions/GCF_000092525.1_ASM9252v1/GCF_000092525.1_ASM9252v1_genomic.gff.gz -O /MOUNTED-VOLUME-LIFE708/RefSeq/NC_007384.gff.gz
```

* uncompress the downloaded files

```
gunzip /MOUNTED-VOLUME-LIFE708/RefSeq/*.gz
```

> **gunzip** is part of the **gzip** compression utility - we compress files to make them easier to manage / transfer


## Sequence data

We next need to download the sequencing data for our experiment. The European Nucleotide Archive (ENA) stores sequencing data from high-throughput sequencing platforms. The data for our experiment is stored under the sequencing run accession number **ERR200544**.

* first check that we have the right sample

Navigate to the sequencing run on the ENA website [here](http://www.ebi.ac.uk/ena/data/view/ERR200544) and check that the accession number is for the data we want (*Shigella sonnei*).

* download the sequence reads from the **ERR200544** sequencing run

```
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR200/ERR200544/ERR200544_1.fastq.gz -O /MOUNTED-VOLUME-LIFE708/SEQUENCE_READS/ERR200544_1.fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR200/ERR200544/ERR200544_2.fastq.gz -O /MOUNTED-VOLUME-LIFE708/SEQUENCE_READS/ERR200544_2.fastq.gz
```

This data may take a while to download (~2 minutes/file), so in the mean time please find out some basic information on this dataset from the Short Read Archive (remember how we did this from the last workshop):

> What sequencing platform was used?

> What's the coverage for this sequencing run?

> What's the read length?

> What's the GC content of this data - how does this compare to a typical *Shigella sonnei* genome?


## Quality checking

Now that we have the sequencing data, we need to check the quality and ensure sequencing adapters have been removed. As we covered this in the last workshop, see if you can remember how to run FastQC and run it again on our new sequencing data (optional).


## Acquired antibiotic resistance genes

Antibiotic resistance can be conferred by either **horizontally-acquired genes** or by **de novo** mutations in wild-type genes. In a real-world scenario, the isolate we are investigating may have a horizontally-acquired **Antiobiotic Resistance Gene** (ARG) and this would not be detected in this analysis (as we have no reference comparison to aid detection). Therefore, to check for horizontally-acquired ARGs we would use a tool like **ResFinder**, **SEAR** or **ARIBA** to check our isolate prior to considering de novo mutations. You can assume that the isolate we are using has no horizontally-acquired ARGs, but if you would like to check for yourself:

* upload the sample reads to [ResFinder](https://cge.cbs.dtu.dk/services/ResFinder/) and check for quinolone ARGs (optional)


***

# Read alignment

We have downloaded and quality checked the sequencing data for our *S. sonnei* isolate. We know that there are no horizontally-acquired ARGs present that could account for nalidixic acid resistance. We now want to look for gene variants in our isolate that could confer resistance to nalidixic acid. To do this, we need to compare our isolate to the reference sequence.

## BWA

We are going to use BWA short read aligner to align the sequencing reads from our experiment to the reference genome (see the lecture notes for an explanation of Burrows-Wheeler transform and alignment). Within the BWA software there are several related algorithms. We are going to use the most recent algorithm, BWA MEM, as it is generally faster and more accurate (it is also the most suited to longer reads).

* Construct the FM-Index

```
bwa index RefSeq/NC_007384.fasta
```

* Run the alignment algorithm and perform basic post-processing

```
bwa mem -t 2 -R '@RG\tID:foo\tSM:bar\tLB:library1' RefSeq/NC_007384.fasta SEQUENCE_READS/ERR200544_1.fastq.gz SEQUENCE_READS/ERR200544_2.fastq.gz | samtools view -@ 2 -q 10 -bh - | samtools fixmate -O bam - - | samtools sort -@ 2 - -o alignment.bam
```

> The above command is actually several commands piped into one another - we have done this to reduce unnecessary disk I/O and also to keep the data in binary format as it is being processed (rather than performing BAM/SAM conversions)

> The above commands basically follow this workflow: Align Reads | Keep high-quality reads | Ensure paired-end information intact | sort the alignment and save

Here is a breakdown of the above commands:

| COMMAND | OPTION | EXPLANATION |
| ------- | ------- | ------- |
| bwa mem | -t | the number of CPUs to use
| bwa mem | -R | read group ID (useful for analysing multiple samples)
| samtools view | -@ | the number of CPUs to use
| samtools view | -q | mapping quality (MAPQ) quality threshold
| samtools fixmate | -O | output format
| samtools sort | -o | output filename

As this takes a while to run (~10mins), let's look into the SAM file format - check the specification document [here](https://samtools.github.io/hts-specs/SAMv1.pdf)

If you want some extra information on the SAM format and samtools, [this](https://medium.com/@shilparaopradeep/samtools-guide-learning-how-to-filter-and-manipulate-with-sam-bam-files-2c28b25d29e8) blog and [this](http://biobits.org/samtools_primer.html) practical are recommended.


## Check alignment

* Now that the alignment has run, we can get some basic stats using samtools

```
samtools flagstat alignment.bam
```

> Using what you have learned from the SAM format specification, can you interpret the output of flagstat?

* Index the alignment file

```
samtools index alignment.bam
```

* **Use the GUI** to visualise the alignment - open up Artemis, load the reference genome and annotation and then read in the BAM alignment file

> Hint: Turn off stop codons to get some performance gain (right click on the sequence track)

> Load in the bam using File > Read BAM / VCF ...

![artemis-bam]({{ site.url }}/_slides/slide-data/workshop3-variation/artemis-bam.png){:width="700px"}

> Zoom out and you should see something like this:

![artemis-bam-2]({{ site.url }}/_slides/slide-data/workshop3-variation/artemis-bam-2.png){:width="700px"}

* Check for any suspect regions in the alignment - have we fully covered the reference genome?

> Are there any gaps of coverage? What could have caused this? What considerations should we make for downstream analysis / future experiments?

> Have a think how you could check for coverage programmatically

As discussed in the lecture, a best practice variant call workflow would now involve alignment post-processing (duplicate marking, InDel realignment etc.). However, we unfortunately don't have time to include this in our workshop. If you want to revisit this, check out the tools offered by GATK for post-processing.

> Keep the Artemis window open for use later


***

# Variant calling

## Call variants

Now that we have an alignment of our sequence data against a reference genome, let's use Freebays to generate genotype likelihoods at each genomic position with alignment coverage.

* Freebayes requires an indexed reference sequence

```
samtools faidx RefSeq/NC_007384.fasta
```

> an index allows quick access to specific regions in the reference. A samtools .fai index is a record of the lengths of all the sequences in the reference and their offsets from the beginning of the file.

* Call variants with Freebayes

```
freebayes -f RefSeq/NC_007384.fasta --ploidy 1 alignment.bam > alignment.vcf
```

> the option --ploidy 1 indicates that the sample should be genotyped as haploid

> freebayes should take around 3 minutes to run on our alignments

* Have a quick look at the `alignment.vcf` file

```
more alignment.vcf
```

> Take a look at the VCF format specification for help understanding these files


## Filter variants

To increase our confidence in the putative variants we perform a series of fixed-threshold filters. As we saw with the last command, a vcf file contains a lot of information and we need a way to make these files manageable. The aim of filtering is ultimately to reduce the number of false positives in a vcf file by removing variants that have little support.

* Let's restrict our variant calls to the chromosome for now:

```
grep '#' alignment.vcf >> chromosomeVariants.vcf && grep -v '#' alignment.vcf | awk '{if($1=="NC_007384.1") print}' >> chromosomeVariants.vcf
```

> grep and awk are command line utilities that are great for searching and parsing files

* Let's apply some filter thresholds

```
bcftools filter -g 5 -G 15 -e '%QUAL<30 | MIN(DP)<50' chromosomeVariants.vcf > chromosomeVariants.filtered.vcf
```

| OPTION | EXPLANATION |
| ------- | ------- |
| -g | filter SNPs within 5 base pairs of an InDel |
| -G | filter clusters of InDels separated by 15 or fewer base pairs allowing only one to pass |
| -e | exclude variants with quality score <30 and site coverage depth <50 |

> NOTE: ordinarily we would perform the variant call and an initial filter in one go by piping commands (or we would keep files in binary format until writing them), this would speed the process up. However, it is important for us to understand how filtering will impact what variants we are seeing.


* Use grep and wc to quickly see how many variants we now have

```
grep -v '#' chromosomeVariants.vcf | grep 'NC_007384.1' | wc -l
grep -v '#' chromosomeVariants.filtered.vcf | grep 'NC_007384.1' | wc -l
```

***

# Interpretation

We've now managed to reduce the number of variants using fixed-threshold filters. Fixed-threshold filtering isn't perfect - not all false positives will have been removed, we may have masked real variants and also, different samples will often require different filtering thresholds.

We have ended up with a large number of variants (>1000). Reasons for this include:
* the reference genome used varies from our sequenced isolate
   - did we use the most appropriate reference?
* we haven't filtered aggressively/well enough
   - should we re-filter?
* we have only used one sample
   - how would this make a difference?

The reason for the high number variants called here is likely to be a combination of all of these reasons. We could improve confidence in our variant calls through using a more closely related reference genome, or by providing the variant caller with a set of known variants (not applicable here). We could also provide additional samples from the same bacterial population we are studying to the variant caller, this will help improve the genotype likelihoods that the caller is calculating. Inspecting statistics such as the **transition:transversion ratio** would also be a good additional step. Finally, we could also try a different variant calling approach - machine learning is beginning to be utilised in variant calling and may offer an alternative approach to what we have covered here.


## Explaining the observed phenotype

The aim of this workshop was to identify a genotypic basis for the observed resistance to nalidixic acid in our *S.sonnei* isolate. We have already checked for **horizontally-acquired ARGS** using **Resfinder** and have not found any that could be responsible for the observed phenotype. Therefore, we have created a list of variants (SNPs and InDels) and now we would like to check if any of these could explain the resistance phenotype. As we have a large number of variants in our list, a good place to start would be to identify candidate genes that may be responsible for the resistance.

* Use a resistance database to identify candidate genes

Try looking at an antibiotic resistance gene (ARG) database for candidate genes that can confer resistance to nalidixic acid if mutated. Some suggested databases are: [CARD](https://card.mcmaster.ca/home), [MEGARes](https://megares.meglab.org/) and [ARDB](http://ardb.cbcb.umd.edu/).

You should hopefully find several gene variants that are known to confer resistance to nalidixic acid, let's look to see if our sample have mutations in any of these genes.

* Compress and index our filtered variant call file (for use in Artemis)

```
bgzip chromosomeVariants.filtered.vcf
tabix -p vcf chromosomeVariants.filtered.vcf.gz
```

* Overlay the variant calls against the reference genome in Artemis

> Use the same menu as for loading the BAM file earlier, you should see a new track with small vertical lines (the variant sites):

![artemis-vcf-1]({{ site.url }}/_slides/slide-data/workshop3-variation/artemis-vcf-1.png){:width="700px"}

* Use the navigator to find and highlight the gene of interest (use the Qualifier Value search box)

> You should have found from the database search that variants of "DNA gyrase subunit A" can confer resistance to nalidix acid

![artemis-vcf-2]({{ site.url }}/_slides/slide-data/workshop3-variation/artemis-vcf-2.png){:width="700px"}



## Mutation effect

You should have found a variant in the DNA gyrase subunit A! You can find out more information on this variant by looking at the variant call file:

```
NC_007384.1	2410773	.	C	T	225
```

So we have found a variant in a gene that is known to confer nalidixic acid resistance if mutated. We are fairly sure this is responsible for the observed phenotype in our isolate of *S. sonnei* but we need to perform some more checks before reporting this variant. Write the bases of our suspect gyrA gene to a fasta file (we did this last workshop) and let's do some checks.

* Use the CARD [resistance gene identifier](https://card.mcmaster.ca/analyze/rgi) (RGI) to see if our *gyrA* variant is known to cause resistance

> What else could you do to check the effect of the variant we have found?


***

# Wrap up

This workshop has some of the basics for variant calling microbial genomes. The workflow has taken us from raw data to a variant that explains an observed phenotype.

The main caveats to this workflow are that:

* it is hard to be 100% certain of variant calls - we are aiming to increase confidence in putative variants.
* we have dealt with reference-relative variants - we mentioned the fact that we could use additional samples
* it is difficult to identify large deletions/insertions using this approach - comparative genomics would be a good place to start for this (see earlier workshops)
* we've not discussed what happens if we can't identify a candidate gene (i.e. no ARG database hits?)

There is a large amount of literature, blog posts and general discussion over the best practices for variant calling, these are definitely worth looking in to when planning a variant call experiment. The take home message form this post is that fixed-threshold filtering is a good approach to improve confidence in variant calls by removing false positives from datasets but fixed-threshold filtering can only go so far and further validation is always needed in order to be sure of the variants we are reporting.
