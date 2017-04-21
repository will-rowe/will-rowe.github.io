---
layout: slide-deck
title:  "Host-Microbe Theme Talk"
author: Will Rowe
permalink: /host-microbe-talk-jan2017/
group: presentation
theme: ember
transition: slide

---

<script type="text/template">


#### Internal seminar series

***

# Host-microbe theme talk 2017

***

Will Rowe

---

## About me

***

 * Care assistant

 * B.Sc. Human Genetics

 * Ph.D. Microbial Genomics

 * Currently on first PostDoc

----

### Today's talk

 * Antibiotic resistance and the environment
  - tool development

  - pilot study

  - main research findings

 * RNA-seq and Salmonella
  - pipelines

  - websites

----

<section data-background="{{site.url}}/slides/slide-data/host-microbe-talk-jan2017/cam.jpg"></section>

<section data-background="http://exoplanets.phy.cam.ac.uk/Meetings/UKCambridgeRiverCamBanner.jpg"></section>

<section data-background-video="/slides/slide-data/host-microbe-talk-jan2017/sewer.mp4" data-background-video-loop data-background-video-muted data-background-color="#000"></section>

----

## Antibiotic resistance & the environment

***

 Antibiotic resistance is a major public health issue

 Acquisition of Antibiotic Resistance Genes (ARGs) by pathogens has serious implications for treatment and prevention of bacterial infections

---

What roles do the environment and human activities have in disseminating ARGs and propagating resistance?

----


<section data-background="/slides/slide-data/host-microbe-talk-jan2017/exap.svg"></section>

----

{% capture markdown-pt1 %}
## Search Engine for Antimicrobial Resistance (SEAR)

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


----


<img data-src="/slides/slide-data/host-microbe-talk-jan2017/sear.jpg">


----

## Pilot study of Cam catchment

***

![pilot-study-fig-1](https://www.researchgate.net/profile/Will_Rowe/publication/287420147/figure/fig3/AS:453676109176832@1485176221310/Fig-1.png "Pilot study - fig1.")

 * ARGs are present in human and animal effluents

> ROWE, W. et al. 2016. Comparative metagenomics reveals a diverse range of antimicrobial resistance genes in effluents entering a river catchment. WaterSciTech

---

Are ARGs being used in the environment?

Are we having an impact?

----

## Over-expression of ARGs in effluents

***

![jac-study-fig-2](/slides/slide-data/host-microbe-talk-jan2017/jac-2.png)

 * Over-expression of *blaOXA* and *blaGES* in hospital effluents

> ROWE, W. et al. 2017 In press. Over-expression of antibiotic resistance genes in hospital effluents over time. Journal Antimic. Chem.

---

![jac-study-fig-3](/slides/slide-data/host-microbe-talk-jan2017/jac-3.png)

---

Are pathogens using these ARGs?

(How) are they persisting?

---

![jac-study-fig-1](/slides/slide-data/host-microbe-talk-jan2017/jac-1.png)

----

## Endospore-forming bacteria & ARGs

***

Sampling the river catchment, hospital and farm effluents and then enriching for clostridia spores

----


![spores]("{{site.url}}/slides/slide-data/host-microbe-talk-jan2017/tree-1.png")

ARGs are abundant in effluents, they can be over-expressed and also contained within stable endospores

What impact are these effluents having on the wider environment?

----


## RNA-seq and Salmonella

***

Comparing global and African sequence types of Salmonella *typhimurium* using RNA-seq and infection-relevant conditions

----

slides removed

----

## Acknowledgements

***

| University of Cambridge | CEFAS | GSK | CU Hospital |
| :---------------------: | :---: | :--: | :--------: |
| Gareth Pearce | David Verner-Jeffreys | Jim Ryan | Christianne Micallef |
| Duncan Maskell | Craig Baker-Austin | | |
| | | |
| | | |
| | | |
| | | |

| Sanger Institute | Hinton Lab |
| :--------------: | :--------: |
| Trevor Lawley | Jay Hinton |
| Mark Stares | Rocio Canals Alvarez |
| Kate Baker | Nico Wenner |
| | Lizeth Lacharme Lora |
| | Blanca Perez Sepulveda |
| | Siân Owen |
| | Caisey Pulford |
| | Wai Yee Fong |

----

{% capture markdown %}
Troubleshooting / learn to code / discussion / have lunch

site: [will-rowe.github.io](https://will-rowe.github.io) || mailing list: [MICROBIOINFO@liverpool.ac.uk](MICROBIOINFO@liverpool.ac.uk)
{% endcapture %}

<iframe data-src="https://will-rowe.github.io/bioinformatics/" width="1000" height="500" frameborder="0" marginwidth="0" marginheight="0" scrolling="yes" style="border:2px solid #000; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
{{ markdown | markdownify }}



----

</script>
