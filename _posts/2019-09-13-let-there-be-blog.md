---
layout: post
title:  "Data sketching for genomics"
date:   2019-09-13 12:00:00 -0000
author: Will
tag-icon: fas fa-pencil-alt
tagline: a review of algorithms and bioinformatic software
featured-image: https://upload.wikimedia.org/wikipedia/en/2/26/Led_Zeppelin_-_Led_Zeppelin_IV.jpg
---

I recently had my first review paper published - [When the levee breaks: a practical guide to sketching algorithms for processing the flood of genomic data](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1809-x). This is a quick post to summarise some of the thoughts behind the paper.
 
As the title suggests, the review offers an introduction to the world of `data sketching algorithms` and their application in genomics. Like many, I've arrived to bioinformatics from a wet lab background. As I progressed from [hacky perl pipelines](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0133492) to writing [software that people actually use](https://github.com/will-rowe/groot), I've had to learn a lot of computer science 101. At times I have been pretty overwhelmed by some of the concepts and algorithms, which was the case for sketching. Sometimes I wasn't sure how best to parameterise some software, or I knew that a sketching algorithm would be a good fit for some task but I wasn't sure which particular flavour of MinHash to implement.

I ended up writing a lot of notes on sketching, including the advantages/disadvantages, commonly used algorithms and the software which uses them. These ended up forming the paper, which I hope is a useful guide to those in a similar position to my former self. The paper covers several common sketching algorithms:

* MinHash
* Bloom filters
* CountMin sketches
* HyperLogLog

As well as the review itself, I included some [online notebooks](https://github.com/will-rowe/genome-sketching) and [an updateable list of sketching software](https://github.com/will-rowe/genome-sketching/blob/master/references.md) - both of which have gone down really well. I'm grateful to those who have already given feedback and updated the software list. The notebooks can be run interactively thanks to [Binder](https://mybinder.org/), which is a really great resource for making papers reproducible and accessible. It was particularly useful for this paper as Binder restricts you to 1-2Gb of RAM, which is ideal for showing off the low memory requirements of sketching!

