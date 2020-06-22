---
layout: slide-deck
title: 'Democratising Genomics'
date: 2020-06-20 12:00:00 -0000
author: Will Rowe
permalink: /dg-2020/
group: presentation
theme: ember
transition: slide
---

<script type="text/template">

#### June 2020

***

# Democratising Genomics
### using the decentralised web for open and collaborative science

***

slides: [willrowe.net/dg-2020](https://willrowe.net/dg-2020) | twitter: [wil_rowe](https://twitter.com/wil_rowe)

----

<iframe src="https://artic.network/" width="100%" height="500" frameBorder="0" allowFullScreen></iframe>

---

* Real-time molecular epidemiology for outbreak response
  * process samples from viral outbreaks
  * generate real-time epidemiological information
  * interpretable and actionable by public health bodies

* Targets a wide-range of emerging viral diseases
  * E.g. Ebola, SARS-CoV-2, influenza
  * portable sequencing (lab-in-a-suitcase) and analysis (lab-on-an-ssd)
  * deploy to resource-limited locations
  * determine transmission, virus evolution and epidemiological linkage

* Globally collaborative work
  * produce and analyse sequence data in the field
  * integrate data from multiple sources
  * provide resources and training

----

What is data democratisation and how does it fit in with the ARTIC Network?

---

* Data democratisation
  * open and accessible data
  * remove gate keepers
  * enable others to derive insight

* Data democratisation is part of [Open Science](https://www.cos.io/)
  * reproducibility and transparency
  * impactfulness and value
  * collaboration, dissemination and empowerment

* ARTIC network relies on Open Science
  * real-time response
  * outbreak preparedness
  * resource provision

---

* Considerations, bottlenecks and barriers
  * data management and validity
  * technical
    * E.g. training, compute, bandwidth
  * logistical
    * E.g. curation, timezones

* Example 
  * protocol validation at PHE (Porton Down)
  * secure site, no connectivity, lab-on-an-SSD
  * sequenced Ebola and Nipah virus
  * multiple libraries and flowcells

---

| run name | date | barcodes | protocol(s) | kit | flow cell ID | run folder download |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| EBOV Metagenomics | 30.08.2019 | 9=mayinga, <br/> 10=kikwit, <br/> 11=makona | 9n | rapid PCR | FAK25288 | [download](http://artic.s3.climb.ac.uk/run-folders/EBOV_Metagenomics.tar.gz) |
| EBOV Metagenomics Smartplex | 30.08.2019 | 9=mayinga, <br/> 10=kikwit, <br/> 11=makona | smartplex | rapid PCR | FAK25413 | [download](http://artic.s3.climb.ac.uk/run-folders/EBOV_Metagenomics_smartplex.tar.gz) |
| EBOV Amplicons | 30.08.2019 | 3=mayinga, <br/> 4=kikwit, <br/> 5=makona, <br/> 6=negative | multiplex PCR | native ligation PCR | FAL30025 | [download](http://artic.s3.climb.ac.uk/run-folders/EBOV_Amplicons.tar.gz) |
| EBOV Amplicons Flongle | 30.08.2019 | 3=mayinga, <br/> 4=kikwit, <br/> 5=makona, <br/> 6=negative | multiplex PCR | native ligation PCR | AAQ411 | [download](http://artic.s3.climb.ac.uk/run-folders/EBOV_Amplicons_flongle.tar.gz) |
| NiV Metagenomics | 29.08.2019 | 7=NiV, <br/> 8=NiV | 7=9n, <br/> 8=smartplex | rapid PCR | FAK25288 | [download](http://artic.s3.climb.ac.uk/run-folders/NiV_Metagenomics.tar.gz) |
| NiV Amplicons | 30.08.2019 | - | smartplex | native ligation PCR | FAK25288 | [download](http://artic.s3.climb.ac.uk/run-folders/NiV_Amplicons.tar.gz) |

----

What is the decentralised web and how does it enable data democratisation?

---

> ![dweb](https://2r4s9p1yi1fa2jd7j43zph8r-wpengine.netdna-ssl.com/files/2018/07/network-node-distributed.png)
> image:[Openclipart.org](https://openclipart.org/)

The decentralised web (*dWeb* or *Web 3.0*) moves the power to control participation from one entity and shares it between many

---

* No central point of control
  * this makes the dWeb resilient
  * if a server goes down, content can be got from elsewhere

* Data ownership
  * you decide to share or encrypt your content
  * content can come from anywhere; so it's harder for others to censor

* Security and speed
  * hacks and breaches could no longer target single servers
  * content can be got from the nearest provider, rather than a server which is far away

* Further reading on the dWeb
  * [the decentralised web](https://dci.mit.edu/decentralizedweb) - a report by the MIT Digital Currency Initiative
  * [why the web 3.0 matters](https://medium.com/@matteozago/why-the-web-3-0-matters-and-you-should-know-about-it-a5851d63c949) - a blog post by Mat Zago

---

* Achieving the dWeb
  * transition to dWeb shouldn't impact typical users
  * utilise existing technologies (e.g. peer-to-peer (**P2P**) networking)

* Architectures for the dWeb
  * blockchain
  * [Inter Planetary File System (IPFS)](https://ipfs.io/)
  * [Dat](https://dat.foundation/)

* Building decentralised apps and services (dApps)
  * [OpenBazaar](https://openbazaar.org/) (a decentralised marketplace)
  * [Matrix](https://matrix.org/) (a messaging platform)
  * [Diaspora](https://diasporafoundation.org/) (a social network)
  * [Graphite Docs](https://www.graphitedocs.com/) (a Google Docs alternative)

----

What is the IFPS?

---

* A distributed system for storing and accessing data
  * free and open-source
  * as well as getting data, you help distribute it
  * based on technology from Git, BitTorrent and Bitcoin

* Replacement for HTTP
  * distribute high volume of data with high efficiency

* Combines several ideas
  * content addressing for linking and identifying content
  * [Merkle DAGs](https://docs.ipfs.io/concepts/merkle-dag/) for verifying content
  * [Distributed Hash Tables](https://docs.ipfs.io/concepts/dht/) (DHT) for locating content
  * [BitSwap](https://docs.ipfs.io/concepts/bitswap/) for exchanging content

---

* Direct Acyclic Graph (DAG) is a directed graph which disallows cycles

* In a Merkle DAG, each node is hashed by its contents
  * the hash is used to identify the node
  * makes the node immutable

* Merkle-DAGs are constructed from the leaves, so parent nodes are added after children
  * every node in a graph is a root of a subgraph
  * self verified structure 

---

> ![mDAG](https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Hash_Tree.svg/2880px-Hash_Tree.svg.png)
> image:[wikipedia](https://en.wikipedia.org/wiki/Merkle_tree)

IPFS uses the Merkle DAG to represent and link content - which IPFS often first splits it into blocks

---

> ![BitSwap](https://docs.ipfs.io/assets/img/diagram-of-the-want-have-want-block-process.6ef862a2.png)
> image:[IPFS docs](https://docs.ipfs.io/concepts/bitswap/)

BitSwap orchestrates content discovery & transfer between nodes, searching the DHT if connected nodes don't have the content

---

> ![venn](https://hackernoon.com/hn-images/0*erhuY4fEfd3PA4Im.)
> image:[@kojoaddaquay](https://hackernoon.com/a-beginners-guide-to-ipfs-20673fedd3f)

---

> ![bypass-node-discovery](https://miro.medium.com/max/1400/1*bUqhjNl_OOVpm-SzvyoI7Q.png)
> image:[Matt Ober / Pinata](https://medium.com/pinata/speeding-up-ipfs-pinning-through-swarm-connections-b509b1471986)

Pinning services and manual swarms speed up sharing by bypassing the IPFS DHT search

----

Who's already using the dWeb for genomics?

---

## wort

a decentralised database and API for sourmash signatures

> <iframe src="https://wort.oxli.org" height="500" frameBorder="0" allowFullScreen></iframe>

> L Irber. [Building decentralized indexes for public genomic data](https://github.com/luizirber/2018-biods). Biological Data Science 2018. | [github.com/dib-lab/wort](https://github.com/dib-lab/wort)

---

## darkQ

a messaging queue for microbial genomes

> ![](https://github.com/phiweger/darkq/raw/master/img/flow.png)

> A Viehweger | [github.com/phiweger/darkq](https://github.com/phiweger/darkq)

---

## Cancer Gene Trust

a decentralised repository of genomic, clinical, and imaging data

> ![](https://www.cancergenetrust.org/docs/architecture.png)

> [www.cancergenetrust.org](https://www.cancergenetrust.org/) | [github.com/cancergenetrust](https://github.com/cancergenetrust)

----

Democratising genomics with the dWeb

---

<img src="https://raw.githubusercontent.com/will-rowe/stark/master/docs/img/stark-logo-with-text.png" width="30%">

[S]()equence [T]()ransmission [A]()nd [R]()ecord [K]()eeping

> [github.com/will-rowe/stark](https://github.com/will-rowe/stark)

---

* An app for sharing and tracking sequencing experiments
  * platform and language agnostic (gRPC and protobuf)
  * Go API provided

* Decentralised database
  * backed by the IPFS
  * persistant via [pinning services](https://docs.ipfs.io/concepts/persistence/#pinning-services)
  * custom swarms

* Update in real time
  * use [PubSub](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) messaging system to broadcast database changes

* Retain control
  * version database entries
  * encrypt data

---

> ![](https://github.com/will-rowe/stark/raw/master/docs/img/dag/mdag-eg.gif)

---

* Sequencing experiments stored as records
  * encode samples, libraries, runs etc.
  * store in IPFS
  * versioned and linked

* Organise records in database projects
  * projects are filesytem nodes in the IPFS
  * versioned and easily shareable

* Collaborative
  * pin snapshots
  * broadcast database changes
  * swarm peers

---

> ![](https://github.com/will-rowe/stark/raw/master/docs/img/dag/mdag-3.png)

----

How to make this accessible and integrate with existing genome democratisation efforts?

---

* Existing dWeb services for genomics
  * [wort](https://github.com/dib-lab/wort) for genome pinning and query
  * [darkq](https://github.com/phiweger/darkq) for genomic surveillance
  * these facilitate data openness, persistence and collaboration

* Data democratisation needs accessibility
  * commitment to the platform
  * enable others to use the technology
  * keep it working and manageable

* Encourage sharing
  * make it easy to understand
  * make it easy to search - more indexing (+sketching!)
  * make it easy to contribute - GUIs and workflows

---

<img src="https://raw.githubusercontent.com/will-rowe/herald/master/misc/logo-with-text.png" width="30%">

announce your experiments

> [github.com/will-rowe/herald](https://github.com/will-rowe/herald)

---

* HTML5 desktop app
  * [lorca](https://github.com/zserge/lorca)
  * like electron

* Messages for controlling experiment flow
  * gRPC and protobuf
  * use existing services (MinKNOW, stark etc.)

---

![](https://github.com/will-rowe/herald/raw/master/docs/img/screenshots/herald-screenshot.png)

---

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/misc/hulk-logo-with-text.png" width="30%">

[H]()istosketching [U]()sing [L]()ittle [K]()mers

> $ conda install hulk || [github.com/will-rowe/hulk](https://github.com/will-rowe/hulk)

---

"The [histogram sketch]() (or histosketch) data structure maintains a set of  fixed size sketches to approximate the overall histogram as it is received from a data stream"

<br/>
<div align="right">
<i>Yang et al. 2017<i/>
<div/>

---

<img src="https://github.com/will-rowe/hulk/raw/master/paper/img/figures/pngs/figure-1.jpg" width="60%">

> [Rowe, WPM et al. Streaming histogram sketching for rapid microbiome analytics. Microbiome 2019](https://microbiomejournal.biomedcentral.com/articles/10.1186/s40168-019-0653-2)
<br/>
> [twitter: @wil_rowe]()

---

* [Histosketch]() algorithm
  - designed for similarity comparisons of customer activity information
  - implemented here to process streaming k-mer spectra

* Uses [consistent weighted sampling]()
 - keeps track of k-mer frequency information
 - accounts for differences in relative set size

* Several applications
  - sample dissimilarity estimation
  - rapid microbiome catalogue searching
  - classification of microbiome samples in near real-time.

---

<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/figures/pngs/figure-2.png" width="50%">

* Microbiome samples from different body sites (CAMI project)

* Histosketched 48 samples in [1m30s]()

* Histosketches cluster by body site using Jaccard similarity

---

Histosketch collections can also be indexed and searched, or as features for ML classifiers

---

<img src="https://raw.githubusercontent.com/will-rowe/banner/master/misc/logo/banner-logo-with-text.png" width="40%">

> $ conda install hulk || [github.com/will-rowe/banner](https://github.com/will-rowe/banner)

---


<img src="https://raw.githubusercontent.com/will-rowe/hulk/master/paper/img/figures/pngs/figure-4.png" width="50%">

* Index the sketches using LSH Forest scheme

* Indexes are updatable

* Index searches predominantly return samples from same body site

---

![]({{site.url}}/_slides/slide-data/melb-2018/iaai19.png)

* Multi-class and binary classification of microbiome samples

* Relevance Vector Machine (RVM), Support Vector Machines (SVM), Random Forests (RF), Naive Bayes (NB)

> Carrieri , AP., Rowe, WPM et al. [A Fast Machine Learning Workflow for Rapid Phenotype Prediction from Whole Shotgun Metagenomes. IAAI 2018]()


----

| Uni. of Birmingham |
| :-----------------: |
| Nick Loman |
| Sam Nicholls |
| Claire McMurrary |
| Jo Stockton |
| Radoslaw Poplawski |
| the ARTIC Network |

***

|    UKRI/IBM        |  Quadram Institute   | Imperial College London |
| :---------:        | :------------------: | :---------------------: |
| Martyn Winn        |     Lindsay Hall     |       Simon Kroll       |
| Anna Carrieri      | Cristina Alcon-Giner |        Alex Shaw        |
| Edward Pyzer-Knapp |    Shabhonam Caim    |      Kathleen Sim       |
|                      |                         |

</script>

<section>
    <pre><code data-trim data-noescape>
    </code></pre>
</section>
