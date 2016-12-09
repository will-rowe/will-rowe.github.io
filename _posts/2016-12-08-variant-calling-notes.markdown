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

[Variant calling][olsonPaper] is the term used when identifying a nucleotide difference at a given position in a genome, relative to a reference sequence. The programs we use to call variants provide an estimate of frequency and a measure of confidence for each variant call.

When reporting variants, we must assess the quality of each variant called in our sample. We can apply fixed-threshold filters to increase our confidence in any variants that we are reporting, however *a filtering approach that worked for one sample may not work for another*.

[Filtering][carsonPaper] is needed in part due to the sheer size of the variant call files, which can't all be validated. The aim is to exclude false-positives (arising from inappropriate or erroneous reference sequences), without masking true-positives in the process.

Recently, our group sequenced a bunch of Salmonella mutants that weren't behaving as expected in some RNA-seq experiments (variant calling from RNA-seq data is a topic for another time). The idea was to see if any genomic variation could be identified in the mutants (relative to the original *S. typhimurium* strain from which the mutants were derived) that could account for the phenotypes seen in the lab. This post will run through the workflow we used in this example.


---

# Workflow

Once we have sequenced a sample and quality checked the sequencing data, a basic workflow for variant calling a microbial genome follows these steps:

1.	Align reads to reference genome
2.	QC and visualise alignment
3.	Variant call the alignment (SNPs, InDels, MNPs)
4.	Filter and interpret the variant calls


## 1. Align your reads to the reference genome

The main short read aligners used by the group are [Bowtie2][bowtie2] and [SMALT][smalt]. These aligners generate alignments in Sequence Alignment Map (SAM) format and [SAMtools][samtools] is a set of tools to process these files. SAMtools can work with binary SAM files (BAM files) without uncompressing the whole file. Useful information for understanding SAM files and flags is found [here][samspecs] and [here][picard_sam_flags]. The following code snippets are tested with Samtools v1.3.1 and Bowtie2 v2.2.5.

* Align reads, apply a MAPQ filter and then sort and index the bam file

{% highlight ruby %}
$ bowtie2 -x REFERENCE -1 r1.fastq -2 r2.fastq -U r0.fastq -p 40 | samtools view -q 10 -bS - | samtools sort - -o isolate.outfile.sorted.bam && samtools index isolate.outfile.sorted.bam
{% endhighlight %}

> Side Note:	commands are piped to reduce disk I/O

> Side Note:	the q option applies a MAPQ filter. The MAPQ score is a Phred-scaled probability value that indicates the probability that the alignment to the genome is incorrect. I.E. the higher the value, the higher the confidence in the alignment. One reason to add this filter is to pick the best alignment for multi-mapped reads.

> Side Note:	the higher the MAPQ filter, the higher the reference bias. Too high a MAPQ filter could mask true positive variants.


## 2. QC / visualise the alignment

We already applied a quality filter in the previous step (the MAPQ score) but there are a few more things we can do to check and improve the quality of the alignment.

* If duplicates are present (e.g. if PCR was used in library prep), remove them using Samtools. Duplicates will bias variant calls or propogate PCR errors (false positives).

{% highlight ruby %}
$ samtools rmdup isolate.outfile.sorted.bam isolate.rmdup.bam
$ samtools index isolate.rmdup.bam
{% endhighlight %}

* Check the number of aligned reads and the genome coverage of the alignment using bedtools/R or visualise with Artemis etc. to see if we have any suspect regions in our sample (was an appropriate reference used?)

{% highlight ruby %}
$ samtools flagstat isolate.outfile.sorted.bam
$ bedtools genomecov -ibam isolate.rmdup.bam
$ bedtools genomecov -bg -ibam isolate.rmdup.bam | gzip > isolate.bedGraph.gz
{% endhighlight %}

<details>
<summary>Click to see example R code for coverage plot</summary>
{% highlight ruby %}
example from: https://blog.liang2.tw/posts/2016/01/plot-seq-depth-gviz/
bedgraph_dt <- fread('./84.bedGraph',col.names = c('chromosome', 'start', 'end', 'value'))
library(data.table)
library(Gviz)

bedgraph_dt <- fread(
    './coverage.bedGraph',
    col.names = c('chromosome', 'start', 'end', 'value')
)

# Specifiy the range to plot
thechr <- "chr17"
st <- 41176e3
en <- 41324e3

bedgraph_dt_one_chr <- bedgraph_dt[chromosome == thechr]
dtrack <- DataTrack(
    range = bedgraph_dt_one_chr,
    type = "a",
    genome = 'hg19',
    name = "Seq. Depth"
)
plotTracks(
    list(dtrack),
    from = st, to = en
)

{% endhighlight %}
</details>

## 3. Variant call the alignment

Next we use the aligned reads to calculate the genotype likelihoods. Here we will use [Freebayes][freebayes] - a program that finds SNPs/indels/MNPs **smaller** than the length of a short-read sequencing alignment.

Variant calling programs, such as Freebayes, report positions that are putatively polymorphic in a variant call file (VCF) format.

Freebayes uses the literal sequences of reads aligned to a particular target, not their precise alignment (like Samtools/PolyBayes) to determine the most-likely combination of genotypes for the population at each position in the reference. Freebayes can use an input set of variants as a source of prior information. A major advantage of Freebayes is the ability to supply multiple samples from the same species/population to inform the variant call.

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


## 4. Filtering the variant calls

VCF files can be fixed-threshold filtered using the information generated by the variant caller, such as the reported quality (QUAL), depth (DP), observation count (AO), proximity to InDels (g), high-qual non-reference reads (DV)

The QUAL field of the VCF file is an estimate of the probability that there is a polymorphism at the given loci. In VCF files that have been generated by programs like Freebayes, this value "can be understood as 1 - P(locus is homozygous given the data)".

The DP of the VCF file is the sequencing depth for that position. A DP greater than the average could mean an artefact is present in the region. A low DP may not provide enough evidence for a variant.

* filter using report quality, proximity to indels, clusters of indels and depth

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


### Interpret filtered variants

Now that we have applied a filter to the VCF file, we need to assess the variants and reapply filters if necessary.

* check the transition/transversion ratio (a value ~0.5 could indicate a flase positive)

{% highlight ruby %}
$ bcftools stats isolate.SNPs.filtered.vcf > isolate.SNPs.filtered.stats
$ bcftools stats isolate.SNPs.filtered.vcf | grep TSTV
{% endhighlight %}

> Side Note: transitions are more likely than transversions, so a ts:tv ratio is not usually expected to equal 0.5. TS:TV ratios are species specific so some consideration should be given to the value used here.

* visualise the variants


---

# Closing

This post has covered some of the basics in variant calling microbial genomes. We've outlined a workflow that we use to call variants in our lab, giving consideration at each stage to our experimental design by taking into account things such as sample type, the quality and depth of sequencing, the population being sampled and the overall aims of the variant call experiment (e.g. are we looking for known variants or exploring for novel SNPs etc.?).

It can be difficult to get started calling variants, especially if you are new to the microbial genomics, so hopefully this post will assist people who want to identify variation in microbial genomes. There is a large amount of literature, blog posts and general discussion over the best practices for variant calling, these are definitely worth looking in to when planning a variant call experiment. The take home message form this post is that fixed-threshold filtering is a good approach to improve confidence in variant calls by removing false positives from datasets **but** fixed-threshold filtering can only go so far and further validation is always needed in order to be sure of the variants we are reporting.

---

# Acronyms

SNP	-	Single Nucleotide Polymorphism

InDel	-	Insertion/Deletion

MNP	-	Multiple Nucleotide Polymorphism

QC	-	Quality Check

[olsonPaper]: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4493402/
[carsonPaper]: http://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-15-125
[bowtie2]: http://bowtie-bio.sourceforge.net/bowtie2/manual.shtml
[smalt]: http://www.sanger.ac.uk/science/tools/smalt-0
[samtools]: http://www.htslib.org/doc/samtools.html
[samspecs]: https://samtools.github.io/hts-specs/SAMv1.pdf
[picard_sam_flags]: https://broadinstitute.github.io/picard/explain-flags.html
[freebayes]: https://github.com/ekg/freebayes
[vcf]: https://github.com/samtools/bcftools/wiki/HOWTOs
[vcf2]: https://github.com/ekg/alignment-and-variant-calling-tutorial
