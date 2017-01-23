---
layout: slide-deck
title:  "Host-Microbe theme talk"
author: Will Rowe
description: Host-Microbe theme talk
group: presentation
theme: ember
transition: slide
permalink: /host-microbe-talk-jan2017/
---


[//]: # (First group of slides - talk title, about me and overview of talk)
<section data-markdown data-separator="^\n----\n" data-separator-vertical="^\n---\n">
#### Host-microbe theme talk

***

# From 'poo' scientist to bioinformatician

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

</section>


[//]: # (Second group of slides - Cam pics)
<section data-background="/publications/slide-data/host-microbe-talk-jan2017/cam.jpg"></section>
<section data-background="http://exoplanets.phy.cam.ac.uk/Meetings/UKCambridgeRiverCamBanner.jpg"></section>
<section data-background-video="/publications/slide-data/host-microbe-talk-jan2017/sewer.mp4" data-background-video-loop data-background-video-muted data-background-color="#000"></section>


[//]: # (AMR and the environment - intro)
<section data-markdown data-separator="^\n----\n" data-separator-vertical="^\n---\n">
## Antibiotic resistance & the environment

***

Antibiotic resistance is a major public health issue

Acquisition of Antibiotic Resistance Genes (ARGs) by pathogens has serious implications for treatment and prevention of bacterial infections

---

What roles do the environment and human activities have in disseminating ARGs and propagating resistance?

</section>


[//]: # (AMR and the environment - approach)
<section data-background="/publications/slide-data/host-microbe-talk-jan2017/exap.svg"></section>


[//]: # (AMR and the environment - SEAR)
<section>
{% capture markdown-pt1 %}
## Search Engine for Antimicrobial Resistance (SEAR)

***

{% endcapture %}
{% capture markdown-pt2 %}
* SEAR annotates full-length ARGs from metagenomic data

* Command line, web and [BaseSpace](https://basespace.illumina.com/apps/2083081/SEAR-Antibiotic-Resistance) versions


>ROWE, W., BAKER, K. S., VERNER-JEFFREYS, D. et al. 2015. SEAR: A cloud compatible pipeline for... PLoS One
{% endcapture %}

{{ markdown-pt1 | markdownify }}
<iframe data-src="https://www.illumina.com/informatics/research/sequencing-data-analysis-management/basespace/basespace-apps/sear-antibiotic-resistance-2083081.html" width="700" height="350" frameborder="0" marginwidth="0" marginheight="0" scrolling="yes" style="border:2px solid #000; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
{{ markdown-pt2 | markdownify }}
</section>


[//]: # (AMR and the environment - SEAR 2)
<section data-background-video="/publications/slide-data/host-microbe-talk-jan2017/sear.mp4" data-background-video-loop data-background-video-muted data-background-color="#000"></section>


[//]: # (AMR and the environment - WST)
<section data-markdown data-separator="^\n----\n" data-separator-vertical="^\n---\n">
## Pilot study of Cam catchment

***

![pilot-study-fig-1](https://www.researchgate.net/profile/Will_Rowe/publication/287420147/figure/fig3/AS:453676109176832@1485176221310/Fig-1.png "Pilot study - fig1.")

 * ARGs are present in human and animal effluents

> ROWE, W., VERNER-JEFFREYS, D., BAKER-AUSTIN, C. et al. 2016. Comparative metagenomics reveals... WaterSciTech

---

Are ARGs being used in the environment?

Are we having an impact?

----

[//]: # (AMR and the environment - JAC)
## Over-expression of ARGs in effluents

***

![jac-study-fig-2](/publications/slide-data/host-microbe-talk-jan2017/jac-2.png)

 * Over-expression of *blaOXA* and *blaGES* in hospital effluents

> ROWE, W., VERNER-JEFFREYS, D., BAKER-AUSTIN, C. et al. In press. Over expression of... Journal Antimic. Chem.

---

![jac-study-fig-1](/publications/slide-data/host-microbe-talk-jan2017/jac-1.png)

---

![jac-study-fig-3](/publications/slide-data/host-microbe-talk-jan2017/jac-3.png)

---

Are pathogens using these ARGs?

(How) are they persisting?

----

[//]: # (AMR and the environment - endospores)
## Endospore-forming bacteria & ARGs

***

Sampling the river catchment, hospital and farm effluents and then enriching for clostridia spores

</section>

<section>
<img src="/publications/slide-data/host-microbe-talk-jan2017/tree-1.png" alt="Drawing" style="width: 500px;"/>
</section>


[//]: # (RNA-seq)
<section data-markdown>
## RNA-seq and Salmonella

***

Comparing global and African sequence types of Salmonella *typhimurium* using RNA-seq and infection relevant conditions

</section>
<section>
<iframe data-src="http://hinton-analysis.s3-website-eu-west-1.amazonaws.com/projects/rocio-07.10.2016/RNAseq-analysis.html" width="1200" height="600" frameborder="0" marginwidth="0" marginheight="0" scrolling="yes" style="border:2px solid #000; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
</section>


[//]: # (Acknowledgements)
<section data-markdown>
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

</section>


[//]: # (Announcements)
<section>
{% capture markdown %}
Troubleshooting / learn to code / discussion / have lunch

[will-rowe.github.io](https://will-rowe.github.io) / [MICROBIOINFO@liverpool.ac.uk](MICROBIOINFO@liverpool.ac.uk)
{% endcapture %}

<iframe data-src="https://will-rowe.github.io/bioinformatics/" width="1000" height="500" frameborder="0" marginwidth="0" marginheight="0" scrolling="yes" style="border:2px solid #000; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
{{ markdown | markdownify }}
</section>


<section data-markdown>
![website-image](https://camo.githubusercontent.com/a2a9a06c1f18cbb4834aa287834c5f85a4384c76/68747470733a2f2f73332d65752d776573742d312e616d617a6f6e6177732e636f6d2f68696e746f6e2d6c61622f7374617469632f696d672f6d6973632f68696e746f6e6c61622d73637265656e73686f742e706e67 "website shot")

New Hinton Lab website

[www.hintonlab.com](http://www.hintonlab.com)

</section>
