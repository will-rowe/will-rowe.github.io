---
layout: post
title:  "Notes on variant calling"
date:   2016-12-08 09:00:00
author: will
categories: main
group: workshop
---

---

This post was written for the weekly IIB microbial bioinformatics lunch and discusses how to *identify reference-relative variants in a microbial genome*.

---

<h1>Contents</h1>

* TOC
{:toc}

---

# Background

Variant calling is the term used when identifying a nucleotide difference at a given position in a genome vs. a reference sequence.  Variant calling programs provide an estimate of variant frequency and a measure of confidence in the variant call - *consideration must be given to the quality of the variant calls and a filtering approach that worked for one sample may not work for another*.

Variant calling can yield big datasets, even for just one sample, and variants can not always be validated. These datasets can contain false-positives (which can arise from inappropriate or erroneous reference sequences), which we attempt to filter out without masking true-positives in the process.

Recently, our group sequenced a bunch of Salmonella mutants that weren't behaving as expected in some RNA-seq experiments (variant calling from RNA-seq data is a topic for another time). The idea was to see if any genomic variation could be identified in the mutants, relative to the original *S. typhimurium* strain from which the mutants were derived, that could account for the phenotypes seen in the lab. The rest of this post will run through the workflow used in this example.

---

# Workflow

Once we have sequenced a sample and quality checked the sequencing data, a basic workflow for variant calling a microbial genome follows these steps:

1.	align reads to reference genome
2.	QC / visualise alignment
3.	identify variants (SNPs, indels, MNPs, )


## Align your reads to the reference genome

The main short read aligners used by the group are [Bowtie2][bowtie2] and [SMALT][smalt]. These aligners generate alignments in Sequence Alignment Map (SAM) format and [SAMtools][samtools] is a set of tools to process these files. SAMtools can work with binary SAM files (BAM files) without uncompressing the whole file. A really useful tool for understanding SAM flags is found [here][picard_sam_flags]. The following code snippets are tested with Samtools v1.3.1 and Bowtie2 v2.2.5.

* Align reads, apply a MAPQ filter and then sort and index the bam file

{% highlight ruby %}
$ bowtie2 -x REFERENCE -1 r1.fastq -2 r2.fastq -U r0.fastq -p 40 | samtools view -q 10 -bS - | samtools sort - -o isolate.outfile.sorted.bam && samtools index isolate.outfile.sorted.bam
{% endhighlight %}

> Side Note:	commands are piped to reduce disk I/O

> Side Note:	the q option applies a MAPQ filter. The MAPQ score is a Phred-scaled probability value that indicates the probability that the alignment to the genome is incorrect. I.E. the higher the value, the higher the confidence in the alignment. One reason to add this filter is to pick the best alignment for multi-mapped reads.

> Side Note:	the higher the MAPQ filter, the higher the reference bias. Too high a MAPQ filter could mask true positive variants.


## QC / visualise the alignment

We already applied a quality filter in the previous step (the MAPQ score) but there are a few more things we can do to check and improve the quality of the alignment.

* If duplicates are present (e.g. if PCR was used in library prep), remove them using Samtools.

{% highlight ruby %}
$ samtools rmdup isolate.outfile.sorted.bam isolate.rmdup.bam
$ samtools index isolate.rmdup.bam
{% endhighlight %}

* Check the number of aligned reads and the genome coverage of the alignment using bedtools/R or visualise with Artemis etc. to see if we have any suspect regions in our sample (was an appropriate reference used?)

{% highlight ruby %}
$ samtools flagstat isolate.outfile.sorted.bam
$ bedtools genomecov -ibam isolate.rmdup.bam
{% endhighlight %}


## Variant call the alignment

Next we use the aligned reads to calculate the genotype likelihoods. Here we will use [Freebayes][freebayes] - a program that finds SNPs/indels/MNPs **smaller** than the length of a short-read sequencing alignment.

Freebayes uses the literal sequences of reads aligned to a particular target, not their precise alignment (like Samtools/PolyBayes) to determine the most-likely combination of genotypes for the population at each position in the reference. Freebayes can use an input set of variants as a source of prior information.

Variant calling programs, such as Freebayes, report positions that are putatively polymorphic in a variant call file (VCF) format.

* Call putative variants with Freebayes

{% highlight ruby %}
$ freebayes -f ../D23580_liv_2016_chrom_4plasmids.fasta --ploidy 1 -F 0.1 isolate.rmdup.bam > isolate.vcf
{% endhighlight %}

<details>
<summary>Click to see command explanation</summary>
{% highlight ruby %}
ploidy = 1	Indicates that the sample should be genotyped as haploid
F = 0.1 	Require at least this fraction of observations supporting an alternate allele within a single individual in the in order to evaluate the position.
{% endhighlight %}
</details>

> Side Note:	Bayesian inference is a method of statistical inference in which Bayes' theorem is used to update the probability for a hypothesis as more evidence or information becomes available

* compress and index the VCF file

{% highlight ruby %}
$ bgzip isolate.vcf
$ tabix -p vcf isolate.vcf.gz
{% endhighlight %}

* alternatively, we could have used samtools / bcftools to generate VCF file

{% highlight ruby %}
$ samtools mpileup -uf ref.fa isolate.rmdup.bam | bcftools call -mv -Oz > isolate.vcf.gz
{% endhighlight %}


## Filtering the variant calls

* using reported QUAL and/or depth (DP) or observation count (AO).

The QUAL field of the VCF file is an estimate of the probability that there is a polymorphism at the given loci. In VCF files that have been generated by programs like freebayes, this value "can be understood as 1 - P(locus is homozygous given the data)"


{% highlight ruby %}
$ bcftools filter -sLowQual -g3 -G10 -e '%QUAL<20 | MIN(DP)<10' isolate.vcf.gz > isolate.SNPs.filtered.vcf
{% endhighlight %}

<details>
<summary>Click to see command explanation</summary>
{% highlight ruby %}
g = 3		filter SNPs within 3 base pairs of an indel
G = 10		filter clusters of indels separated by 10 or fewer base pairs allowing only one to pass
e		exclude the expression %QUAL<20 | MIN(DP)<10 (exclude SNPs with quality score <10 and site coverage depth <10)
{% endhighlight %}
</details>


## Interpret filtered variants

* assess filtered SNPs and reapply filters if necessary

E.G. check the transition transversion ratio, depth of sequencing at SNP, allele frequency ...

{% highlight ruby %}
$ bcftools stats isolate.SNPs.filtered.vcf > isolate.SNPs.filtered.stats
$ bcftools stats isolate.SNPs.filtered.vcf | grep TSTV
{% endhighlight %}


> Side Note: "In most biological systems, transitions are far more likely than transversions, so we expect the ts/tv ratio to be pretty far from 0.5, which is what it would be if all mutations between DNA bases were random. In practice, we tend to see something that's at least 1 in most organisms, and ~2 in some, such as human. In some biological contexts, such as in mitochondria, we see an even higher ratio, perhaps as much as 20.
As we don't have validation information for our sample, we can use this as a simple guide for our first filtering attempts."

* visualise the variants


---

# Closing



## Acronyms

SNP	-	Single Nucleotide Polymorphism
InDel	-	Insertion/Deletion
MNP	-	Multiple Nucleotide Polymorphism
QC	-	Quality Check

[bowtie2]: http://bowtie-bio.sourceforge.net/bowtie2/manual.shtml
[smalt]: http://www.sanger.ac.uk/science/tools/smalt-0
[samtools]: http://www.htslib.org/doc/samtools.html
[picard_sam_flags]: https://broadinstitute.github.io/picard/explain-flags.html
[freebayes]: https://github.com/ekg/freebayes
[vcf]: https://github.com/samtools/bcftools/wiki/HOWTOs
[vcf2]: https://github.com/ekg/alignment-and-variant-calling-tutorial
