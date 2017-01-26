---
layout: post
title:  "Running the LIFE708 Docker container"
author: Will Rowe
group: teaching-life708
permalink: /running-Docker/
---

This is a *brief* overview of Docker for LIFE708 students. The aim of this page is to:

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

Useful Docker information can be found [here][docker-1] and [here][docker-2].


## What does all the terminology mean?

Docker comes with it's own set of terminology - we'll list a few of the important ones for now:

### container

 * A container is an autonomous, runtime instance of a docker image. It consists of a Docker image, an execution environment and a standard set of instructions.

### image

 * We use a Docker image to create a container - an image does not have a state / it doesn't change. An image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime.

### Docker Hub

 * The Docker Hub is a centralized resource for working with Docker and its components.

### pulling

 * Pulling is when we download an image from the Docker Hub so we can run the container on our machine.


---

# Getting started on the teaching computers

## Pulling the Docker image

First, we need to pull the life708 container image from the Docker Hub:

```terminal
docker pull wpmr/life708:latest
```

Check that you can see the life708 container image:

```terminal
docker images
```

Your output should look something like this:

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
wpmr/life708        latest              4a5e668fbc60        3 minutes ago       3.466 GB
```

## Starting the container

Now we have our container image, we can start the container and enter it:

```terminal
docker run -itP -m 2g --name life708-$USER -v ~/Desktop/LIFE708-WORKSHOP/:/MOUNTED-VOLUME-LIFE708 wpmr/life708:latest
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

You should now see a screen looking like this:

```
###################################################################
Fri Jan 20 12:46:48 UTC 2017
Welcome to BioDock for LIFE708 . . .
View the GitHub page for an overview of how to use this container
Contact: will.rowe@liverpool.ac.uk
###################################################################

root@26aebcc43a1c:/MOUNTED-VOLUME-LIFE708#
```

## Working inside the container

Our container is running bash and our operating system is Ubuntu - if you need any pointers on using UNIX, please ask us or check out this [cheat sheet][cheat sheet].

While inside our container, all files that we make in **/MOUNTED-VOLUME-LIFE708** will be available on the Desktop of our machines in the **LIFE708-WORKSHOP** directory.


## Useful commands

Just type **exit** to leave the container. We can re-enter the container if needed by typing:

```terminal
docker start life708-$USER

docker exec -it life708-$USER bash
```

Some other useful commands (particularly if running Docker yourself later):

 * View all containers (both running and stopped) using:

```terminal
docker ps -a
```

 * Stop or remove all containers:

```terminal
docker stop $(docker ps -aq)

docker rm $(docker ps -aq)
```

 * Delete the Docker image:

```terminal
docker rmi [IMAGE ID]
```

---

# Closing

You should be set up now to work through the workshop material. Please ask us if you're not sure on anything or something hasn't worked.

You can access the workshops from [here][workshops] or on [Vital][vital].


## Running the workshops at home

All the workshop materials are available on this website ([{{ site.url }}][website]) and on [Vital][vital]. You will need to have Docker installed on your computer.

The easiest way if to install Docker if you have Mac / Windows is to download the applications:

* [for Mac][docker-mac]
* [for Windows][docker-windows]

Additional help on the LIFE708 container can be found [here][github-repo].


---

[docker-1]: https://en.wikipedia.org/wiki/Docker_(software)
[docker-2]: https://www.docker.com/what-docker
[cheat sheet]: http://www.rain.org/~mkummel/unix.html
[website]: {{ site.url }}
[workshops]: {{ site.url }}/teaching-materials/
[vital]: https://vital.liv.ac.uk/
[github-repo]: https://github.com/will-rowe/LIFE708
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/
