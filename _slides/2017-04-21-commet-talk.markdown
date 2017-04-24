---
layout: slide-deck
title:  "ComMet Talk"
author: Will Rowe
permalink: /ComMet-talk-jan2017/
group: presentation
theme: ember
transition: slide

---


<script type="text/template">

ComMet 2017

***

# Finding antibiotic resistance genes in metagenomes

***

Will Rowe PhD


<br/>web: [will-rowe.github.io](https://will-rowe.github.io) | twitter: [wil_rowe](https://twitter.com/wil_rowe)

----

## Today's talk

***

* Mining metagenomes for Antibiotic Resistance Genes (ARGs)

  - a resource overview

  - current approaches

  - development & demonstration of SEAR

* Antibiotic resistance and the environment

  - pilot study of the River Cam catchment

  - going beyond detection

  - current work

* Closing

  - considerations for mining large-datasets

----

## Mining metagenomes for ARGs

***

* Antibiotic resistance is a major public health issue

  - acquisition of ARGs by pathogens has serious implications for treatment and prevention of bacterial infections

* Metagenomics is well suited to ARG detection

  - circumvents need to culture / design detection panel

* Several practical considerations

  - resource availability (e.g. computational infrastructure)

  - metagenomic design (functional / shotgun / amplicon)

  - optimal DNA / library preparation

* The field is continually advancing

  - sequencing and algorithmic advances & integrative approaches

---

In designing a metagenomic experiment to identify ARGs, what analysis methods and resources are available?

---

![arg-tool-timeline]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/arg-tool-timeline.svg)

---

So there are a lot of ARG databases and analysis tools/methods available - what are being used and what should we use?

---

![arg-tool-timeline]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/papers-over-time.png)

---

### Screening Currency Notes for Microbial Pathogens and Antibiotic Resistance Genes Using a Shotgun Metagenomic Approach

***

![currency-fig](http://journals.plos.org/plosone/article/figure/image?download&size=large&id=info:doi/10.1371/journal.pone.0128711.g005)

> Jalali S, Kohli S, Latka C, Bhatia S, Vellarikal SK, et al. (2015) Screening Currency Notes for Microbial Pathogens and Antibiotic Resistance Genes Using a Shotgun Metagenomic Approach. PLOS ONE 10(6): e0128711. [https://doi.org/10.1371/journal.pone.0128711](https://doi.org/10.1371/journal.pone.0128711)

* metagenome assembly > blast contigs to ARG database (ARDB)

---

### Elucidating selection processes for antibiotic resistance in sewage treatment plants using metagenomics

***

![BP-paper-fig](http://ars.els-cdn.com/content/image/1-s2.0-S0048969716314176-fx1.jpg)

> Johan Bengtsson-Palme et al. Elucidating selection processes for antibiotic resistance in sewage treatment plants using metagenomics, Science of The Total Environment, Volume 572, 1 December 2016, Pages 697-712, ISSN 0048-9697, [http://doi.org/10.1016/j.scitotenv.2016.06.228](http://doi.org/10.1016/j.scitotenv.2016.06.228).

* metagenome assembly > blast contigs to ARG database (Resqu)

---

### ...monitoring antimicrobial resistance in swine herds

***

![](https://www.researchgate.net/profile/Berith_Knudsen/publication/309846485/figure/fig1/AS:427245778018307@1478874739241/Figure2-Metagenomic-associations-between-pig-and-manure-floor-samples-The-resistance.png)

> Patrick Munk et al. A sampling and metagenomic sequencing-based methodology for monitoring antimicrobial resistance in swine herds. J Antimicrob Chemother 2017; 72 (2): 385-392. doi: [10.1093/jac/dkw415](10.1093/jac/dkw415)

* metagenomic reads > map reads to ARG database (ResFinder)

---

### A few more...

***

> Baowei Chen, Ke Yuan, Xin Chen, Ying Yang, Tong Zhang, Yawei Wang, Tiangang Luan, Shichun Zou, and Xiangdong Li, Metagenomic Analysis Revealing Antibiotic Resistance Genes (ARGs) and Their Genetic Compartments in the Tibetan Environment, Environmental Science & Technology 2016 50 (13), 6670-6679
DOI: [10.1021/acs.est.6b00619](10.1021/acs.est.6b00619)

> Tao W, Zhang XX, Zhao F, Huang K, Ma H, et al. (2016) High Levels of Antibiotic Resistance Genes and Their Correlations with Bacterial Community and Mobile Genetic Elements in Pharmaceutical Wastewater Treatment Bioreactors. PLOS ONE 11(6): e0156854. [https://doi.org/10.1371/journal.pone.0156854](https://doi.org/10.1371/journal.pone.0156854)

> Feng Ju, Bing Li, Liping Ma, Yubo Wang, Danping Huang, Tong Zhang, Antibiotic resistance genes and human bacterial pathogens: Co-occurrence, removal, and enrichment in municipal sewage sludge digesters, Water Research, Volume 91, 15 March 2016, Pages 1-10, ISSN 0043-1354, [http://doi.org/10.1016/j.watres.2015.11.071](http://doi.org/10.1016/j.watres.2015.11.071)

> David Fitzpatrick, Fiona Walsh; Antibiotic resistance genes across a wide variety of metagenomes. FEMS Microbiol Ecol 2016; 92 (2): fiv168. doi: [10.1093/femsec/fiv168](10.1093/femsec/fiv168)

> Li An-Dong, Li Li-Guan, Zhang Tong, Exploring antibiotic resistance genes and metal resistance genes in plasmid metagenomes from wastewater treatment plants, Frontiers in Microbiology [http://journal.frontiersin.org/article/10.3389/fmicb.2015.01025](http://journal.frontiersin.org/article/10.3389/fmicb.2015.01025)    

* metagenomic reads > blast reads to ARG database (ARDB)

---

### Rapid resistome mapping using nanopore sequencing

***

![nanopore-fig](https://pbs.twimg.com/media/CpAds8LVYAAPbyW.jpg)

> Eric van der Helm, Lejla Imamovic, Mostafa M. Hashim Ellabaan, Willem van Schaik, Anna Koza, Morten O.A. Sommer; Rapid resistome mapping using nanopore sequencing. Nucleic Acids Res 2017 gkw1328. doi: [10.1093/nar/gkw1328](10.1093/nar/gkw1328)

* functional screen > nanopore sequencing > blast reads to ARG database (CARD)

---

### Pathomap

***

![pathomap](http://here360-content.s3.amazonaws.com/uploads/2015/05/PathoMap-6.jpg)

> Afshinnekoo et al., Geospatial Resolution of Human and Bacterial Diversity with City-Scale Metagenomics, CELS
(2015), [http://dx.doi.org/10.1016/j.cels.2015.01.001](http://dx.doi.org/10.1016/j.cels.2015.01.001)

* metagenomic reads > map reads to specific ARG targets

---

### Underworlds

***

![underworlds](https://spectrum.mit.edu/wp-content/uploads/underworlds_emblem_05-400x300.jpg)

> project home: [http://underworlds.mit.edu/](http://underworlds.mit.edu/)

* monitoring microbial life in "smart sewers" (via Luigi robot)

* no current paper / ARG data

---

![underworlds_2](https://spectrum.mit.edu/wp-content/uploads/underworlds_2.png)

---

| Algorithm | Advantages | Disadvantages |
| --------- | --------- | --------- |
| BLAST (reads) | easy to run(?) | no full-length annotations, <br/>high false positive rate, <br/>limited downstream use |
||||
| BLAST (contigs) | accessible (GUI and db-based) | relies on high quality assembly |
||||
| k-mer | good sensitivity with low sequencing coverage | very database dependent, <br/>hard to resolve multiple closely-related ARGs |
||||
| mapping | high-throughput, <br/>infer relative ARG abundance | consideration needed for mapping ambiguities |
||||
| HMM | detect highly divergent ARGs, <br/>circumvents some db bias | high computational demand, <br/>hard to implement |

---

### Mining metagenomes for Antibiotic Resistance Genes (ARGs)

***

* that was a non-exhaustive methods comparison for short read shotgun metagenomics

* no definitive method but there are some common themes

  - contig blast and read mapping are common, newer dbs not trickling through yet though

  - a lot of publications are still just blasting reads to ARDB

  - its good to have a catchy project name

* experimental design dictates most appropriate analysis method

  - sequencing platform (read length, error, coverage)

  - amount of isolated DNA (and library prep type)

* try several methods and/or implement a new one

---

### Search Engine for Antimicrobial Resistance (SEAR)

***

* developed at start of PhD (~2012)

* needed to annotate full-length ARGs from metagenomic data

  - circumvents need to assemble

  - blasting reads took too long & not informative

  - wanted to quantify abundance

* first foray into programming

----

<section data-background-video="/slides/slide-data/host-microbe-talk-jan2017/sear_lowres2.mp4" data-background-video-loop data-background-video-muted data-background-color="#000"></section>

----

### SEAR

***

* clusters the database to collapse closely related references

* optional host subtraction

* clusters reads to representative sequences

* maps reads within each cluster to generate consensus sequence

* calculates relative abundance using Reads Per Kilobase / Million (RPMK)

* blasts consensus sequence to ARG databases

---

{% capture markdown-pt1 %}
### SEAR

***

{% endcapture %}
{% capture markdown-pt2 %}

* SEAR annotates full-length ARGs from metagenomic data

* Command line, web and [BaseSpace](https://basespace.illumina.com/apps/2083081/SEAR-Antibiotic-Resistance) versions

>ROWE, W. et al. 2015. SEAR: A cloud compatible pipeline and Web Interface for Rapidly Detecting Antimicrobial Resistance Genes Directly from Sequence Data PLoS One
{% endcapture %}

{{ markdown-pt1 | markdownify }}
<iframe data-src="https://www.illumina.com/informatics/research/sequencing-data-analysis-management/basespace/basespace-apps/sear-antibiotic-resistance-2083081.html" width="700" height="350" frameborder="0" marginwidth="0" marginheight="0" scrolling="yes" style="border:2px solid #000; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
{{ markdown-pt2 | markdownify }}

---

![basespace1]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/basespace2.png)

---

![basespace1]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/basespace3.png)

---
![basespace1]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/basespace4.png)

----

## Antibiotic resistance & the environment

***

* What roles do the environment & human activities have in disseminating ARGs?

* Focus on a single river catchment and identify sources of ARGs

  - repeat sampling

  - background sampling

* Apply SEAR to estimate ARG abundance

----

<section data-background="{{site.url}}/slides/slide-data/host-microbe-talk-jan2017/cam.jpg"></section>

<section data-background="http://exoplanets.phy.cam.ac.uk/Meetings/UKCambridgeRiverCamBanner.jpg"></section>

<section data-background-video="/slides/slide-data/host-microbe-talk-jan2017/sewer.mp4" data-background-video-loop data-background-video-muted data-background-color="#000"></section>

----

### Pilot study of Cam catchment

***

![pilot-study-fig-1](https://www.researchgate.net/profile/Will_Rowe/publication/287420147/figure/fig3/AS:453676109176832@1485176221310/Fig-1.png)

 * ARGs are present in human and animal effluents

> ROWE, W. et al. 2016. Comparative metagenomics reveals a diverse range of antimicrobial resistance genes in effluents entering a river catchment. WaterSciTech

----

As with the previously mentioned papers - ARGs are found everywhere! But what is their significance / should we refine our search?

----

<section data-background="/slides/slide-data/host-microbe-talk-jan2017/exap.svg"></section>

----

### Over-expression of ARGs in effluents

***

![jac-study-fig-2]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/jac-2.png)

 * Over-expression of *blaOXA* and *blaGES* in hospital effluents

> ROWE, W. et al. 2017. Overexpression of antibiotic resistance genes in hospital effluents over time. J Antimicrob Chemother dkx017. doi: [10.1093/jac/dkx017](https://doi.org/10.1093/jac/dkx017)

---

![jac-study-fig-3]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/jac-3.png)

---

![jac-study-fig-1]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/jac-1.png)

---

### Over-expression of ARGs in effluents

***

* ARG transcripts are in the environment

* Observed differentially expressed ARG transcripts

  - one-sample t-tests were used to determine significant deviation of abundance log DNA/RNA ratios from zero (+FDR)

  - should we consider a more sophisticated approach utilising metatranscriptome count data and modelling? (how many replicates?)

* Associated ARG data with drug usage & LCMS data

  - found possible links to anthropogenic activity

----

So we can filter a gene list & find ARGs that appear to being utilised . . . Are they persisting and are pathogens using them?

----

### Endospore-forming bacteria & ARG distribution

***

* Extensive sampling of the river catchment for bacterial communities & endospores

  - evaluating human impact & ARG persistance / metagenome limitations

* Sample the river catchment, hospital and farm effluents and then enriching for clostridia spores

* Combine data with existing catchment information

---

![cam-catchment]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/cam-catchment.png)

---

![spores]({{site.url}}/slides/slide-data/host-microbe-talk-jan2017/tree-1.png)

---

### Endospore-forming bacteria & ARG distribution

***

* Some conclusions so far:

  - ARGs are abundant in effluents

  - they can be over-expressed

  - dormant endospores harbor ARGs

* What impact are these effluents having on the wider environment?

  - waiting on sequencing river sample sequencing . . .

  - several good papers out there that address this question

----

## Closing

***

What considerations should we make for mining ARGs from large metagenomic datasets?

---

### Considerations

***

* If we are wanting to quantify ARGs, how to normalise?

  - within-sample normalisation? e.g. RPKM / FPKM / TPM - what's the bias?

  - implement additional normalisation for between samples - 16S normalisation of RPKM values?

  - replicates and RNA-seq style differential expression? apply statistical models to counts?

* False positives and negatives?

  - what does metagenomics miss & should we be confirming findings?

* What are the implications?

  - should we look beyond ARG presence/absence? transcriptomics / proteomics etc.

---

### Considerations cont.

***

* Where should we be looking to?

  - lots of new databases which aren't being utilised in the literature yet

  - technology - long read sequencing for metagenomics? single-cell? a few papers emerging

* New algorithms for metagenomics

  - pseudoalignment & expectation maximization

  - singular value decomposition (latent strain analysis)

  - random forest and neural nets (machine learning)

* Best practice for ARG identification comes down to question being asked!

---

### Acknowledgements

***

| University of Cambridge | CEFAS | GSK |
| :---------------------: | :---: | :--: |
| Gareth Pearce | David Verner-Jeffreys | Jim Ryan |
| Duncan Maskell | Craig Baker-Austin | |
| | | |


| Sanger Institute | CU Hospital |
| :--------------: | :--------: |
| Trevor Lawley | Christianne Micallef |
| Mark Stares | |
| Kate Baker | |

***

slides available: [will-rowe.github.io/ComMet-talk-jan2017](https://will-rowe.github.io/ComMet-talk-jan2017)

</script>
