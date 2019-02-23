---
title: An introduction to containers
date: 2018-10-07
permalink: /posts/2018/10/intro-to-containers/
tags:
  - docker
  - containers
---

**Topics:** Containers
 

I have been wanting to learn more about Docker for months. However, when starting to read Dockers [get started page](https://docs.docker.com/v17.09/get-started/) I quickly had to find out that I'm lacking knowledge in too many other concepts. Since my background isn't computer science I often run across this problem. Luckily, this allows me to constantly learn new things! With the goal of eventually reaching the topic of Docker, the [first post](http://alpopkes.com/posts/2018/09/intro-to-virtual-machines/) introduced virtual machines. This one is about containers.

## Containers

### What is a container?

Let's start by looking at physical containers. Containers are mostly used in the shipping industry where they allow for an organized and efficient way to transport goods from one place to another. Before humans agreed to using containers and standardized container sizes, shipping goods in bulk was a lot messier.

Software containers have a similar function. They allow us to pack our code and all of its dependencies (libraries, frameworks, etc.) into a nice little box - a container. The container can then run anywhere. This allows developers to be sure that their software will run independently of where it is deployed. 

Usually, when moving software from one computing environment to another (e.g. from a developers machine to a test environment) we run into problems, especially if the environments are not identical. For example, the developer might have written the code in Python 3 but the test environment still runs in Python 2. Also differences in network topology, storage and security policies can cause problems.
Containers solve this problem and allow us to reliably run software in different computing environments. Using containers to deploy applications is called *containerization*.

### How containers work and how they differ from virtual machines

Let's look at the entire picture. Similar to virtual machines, containers sit on top of a host operating system (e.g. Windows 10) which in turn sits on an infrastructure (e.g. a laptop). The rough eqivalent to the hypervisor used by virtual machines is the *container engine* which sits on top of the host operating system. At the moment, the *Docker Engine* is by far the most popular one. 

Several containers can sit on top of the same host operating system. Different to virtual machines, all containers share access to the the same operating system, namely the one of the host. This has the major advantage that only one operating system needs to be managed and maintained. Also, the shared components are read-only. This allows containers to be small in size (megabytes) and to be able to start within seconds (as opposed to gigabytes and minutes for a virtual machine). So to point this out again: in the case of virtual machines we have several operating systems running on a single server. In the case of containers, we only have the host operating system.

<img src="https://github.com/zotroneneis/resources/blob/master/vm_vs_container.png"
     alt="Container vs. Virtual Machine"
     style="float: left; margin-right: 10px;" />

[Link if you can't see the image displayed here](https://github.com/zotroneneis/resources/blob/master/vm_vs_container.png)

### What is the difference between a container and a container image?

A container is the runtime instance of a (container) image. Our image is the "recipe": a package that includes everything we need for running a specific application including the code, the file system , libraries and configuration of our application. The container is the "cake": a running instance of the image, that is, what the image becomes in memory when executed.


<img src="https://github.com/zotroneneis/resources/blob/master/image_vs_container.png"
     alt="Container vs. Image"
     style="float: left; margin-right: 10px;" />

[Link if you can't see the image displayed here](https://github.com/zotroneneis/resources/blob/master/image_vs_container.png)

### Advantages of containerization

The [Docker Get Started](https://docs.docker.com/get-started/#docker-concepts) page contains a nice overview of the main advantages of using containers. Containers are:

- Flexible: Even the most complex applications can be containerized.
- Lightweight: Containers leverage and share the host kernel.
- Interchangeable: You can deploy updates and upgrades on-the-fly.
- Portable: You can build locally, deploy to the cloud, and run anywhere.
- Scalable: You can increase and automatically distribute container replicas.
- Stackable: You can stack services vertically and on-the-fly.




### Sources
- [Techcrunch](https://techcrunch.com/2016/10/16/wtf-is-a-container/)
- [StackOverflow](https://stackoverflow.com/questions/23735149/what-is-the-difference-between-a-docker-image-and-a-container)
- [Electronic Design](https://www.electronicdesign.com/dev-tools/what-s-difference-between-containers-and-virtual-machines)
- [Docker Get Started](https://docs.docker.com/get-started/#docker-concepts)
