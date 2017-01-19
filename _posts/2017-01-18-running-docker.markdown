---
layout: post
title:  "Running the LIFE708 Docker container"
author: Will Rowe
categories: main
group: teaching
permalink: /Docker/
---

This is an *brief* overview of Docker for LIFE708 students. The aim of this page is to:

 * get you set up with everything you need to do the LIFE708 practicals in the teaching cluster

 * give you a set of tips / point of reference for trying some of the course content on your own computers

---

<h1>Contents</h1>

* TOC
{:toc}

---

# Introduction

## What is Docker and why are we using it?

Docker is a tool that let's you make, deploy and run applications inside containers. This allows developers to easily package up applications with everything that they will need to run, meaning an application can run on virtually any system.

We are using Docker as it is an easy way to get everyone using the same software and files, regardless of what computer they are running these workshops from - so you can use our Docker container to try the workshops again at home!

Useful Docker info can be found [here][docker-1] and [here][docker-2].


## What does all the terminology mean?

container, image, pulling,

---

# Getting started on the teaching computers

## Pulling the Docker container image

First, we need to pull the life708 container image from the Docker Hub:

```terminal
docker pull wpmr/life708:latest
```

Check that you can see the life708 container image:

```terminal
docker images
```


## Starting the container

Now we have our container image, we can start the container and enter it:

```terminal
docker run -itP -m 2g --name life708-$USER -v ~/Desktop/LIFE708-WORKSHOP/:/SCRATCH wpmr/life708:latest
```

This command is doing quite a few things (which we don't need to worry too much about now) but we have ended up inside a running container!

<details style="cursor: pointer;">
<summary>Click for explanation of the above command</summary>
<br />
<p>-i = keep STDIN open even if not attached</p>
<p>-t = allocate a pseudo-tty</p>
<p>-P = publish all exposed ports to the host interfaces</p>
<p>-m = memory limit (2gb)</p>
<p>--name = name for container at runtime (easy to use for later exec commands)</p>
<p>-v = bind mount a volume (for data transfer etc. between container and host machine)</p>
<p>--rm = makes container ephemeral (not used in the above command but useful to keep things tidy)</p>

</details>
<br />


## Working inside the container

While inside our container, all files that we make in `/SCRATCH` will be available on the Desktop of our machines under `LIFE708-WORKSHOP`


---

# Closing

## Running the workshops at home


easiest way if to install Docker if you have Mac / Windows:

https://docs.docker.com/docker-for-mac/
https://docs.docker.com/docker-for-windows/


---


[docker-1]: https://en.wikipedia.org/wiki/Docker_(software)
[docker-2]: https://www.docker.com/what-docker
