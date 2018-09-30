---
title: An introduction to virtual machines
date: 2018-09-29
permalink: /posts/2018/09/intro-to-virtual-machines/
tags:
  - virtual_machines
---

**Topics:** Virtual machines

 
I have been wanting to learn more about Docker for months. However, when starting to read Dockers [get started page](https://docs.docker.com/v17.09/get-started/) I quickly had to find out that I'm lacking knowledge in too many other concepts. Since my background isn't computer science I often run across this problem. Luckily, this allows me to constantly learn new things! With the goal of eventually reaching the topic of Docker, this post will introduce virtual machines, while the next one will be about containers.

## Virtual Machines

### What is a virtual machine?

The name "virtual machine" already points out what a virtual machine does: it emulates a "real" computer system. Let's think of it as a little box that completely and entirely believes that it is a full computer. And it has reason to do so: a virtual machine exhibits the behavior of a computer and is capable of performing tasks like a computer. 

You might think: but why should I want a virtual computer if I already have a "real", physical computer? The great thing about virtual machines is that they allow us to run different operating systems on a single physical computer! For example, you might have a Windows machine but would like to work with Linux from time to time. A virtual machine will allow you to do this, while being easy to manage and maintain. 

### How do virtual machines work?

Virtual machines are run as *guests* on another computer called *host*. Several of these guest virtual machines can exists within a single host.

To create a virtual machine we need a so called *virtual machine manager* which is also called *hypervisor*. The hypervisor is a type of software that allows us to run additional operation systems within the host operating system. 

Let's take a look at the entire picture. The bottommost part of our computer architecture is the infrastructure, for example a laptop. On top of this runs the host operating system, for example Windows 10. Next comes the hypervisor that manages our guest operating systems. The guest operating systems sit on top of the hypervisor. Guest operating systems might be Ubuntu and Windows XP.

[Link if you can't see the image displayed here](https://github.com/zotroneneis/resources/blob/master/virtual_machine.png)

![](https://github.com/zotroneneis/resources/blob/master/virtual_machine.png)


### What are disadvantages of virtual machines?

When creating a virtual machine, part of the host's resources are used to provide an isolated environment for the virtual machine. After assigning these resources, the main system can't use them for its own processes anymore. For example, when the virtual machine is assigned 1GB of memory but uses only half of that memory at a time, the remaining 50% will be wasted. This results in a degradation of the main systems performance since some resource sit around without being used. This is one of the disadvantages of virtual machines and at the same time one of the benefits of containers and docker.


<!-- ## Containers -->

<!-- ### What is a container? -->

<!-- Let's start by looking at physical containers. Containers are mostly used in the shipping industry where they allow for an organized and efficient way to transport goods from one place to another. Before we agreed to using containers and standardized container sizes, shipping goods in bulk was a lot messier. -->

<!-- Software containers have a similar function. They allow us to pack our code and all of its dependencies (libraries, frameworks, etc.) into a nice little box - a container. The container can then run anywhere. This allows developers to be sure that their software will run independently of where it is deployed. --> 

<!-- Containers solve the problem of how we can reliably run software when moving it from one computing environment to another (e.g. from a developers machine to a test environment). When the environments are not identical we can run into problems. For example, the developer might have written the code in Python 3 but the test environment still runs in Python 2 (shame on the test environment!). Also differences in network topology, storage and security policies can cause problems. -->

<!-- ## Why are containers useful? -->
<!-- Difference between virtual machines and containers -->

<!-- Before we had containers, the go-to technology for running different isolated applications on a single server were *virtual machines*. Different to a container, a virtual machine packs the operating system and code together. Each virtual machine thinks that is has its own server. However, in reality, several different virtual machines all share the same server without knowing anything about each other. The underlying host operating system makes sure that each guest virtual machine believes that -->
<!-- it is the most important one in the world. This can become problematic (especially when running multiple virtual machines), because each guest virtual machine is basically running on an emulated server. The emulation layer between the host and guest operating systems is called "hypervisor" -->

<!-- Containers are different. Because they only contain an application and its dependencies, lots of containers fit on a single host operating system. In this case, we also don't have several operating systems running on a single server. The only operating system is the one of the host and all containers communicate with it directly. The rough eqivalent to the hypervisor used by virtual machines is the *container engine*. At the moment, the *Docker Engine* is by far the mos popular one. -->


<!-- ## What is a container image? -->

<!-- ## What is docker? -->




<!-- Sources: -->
<!-- - https://techcrunch.com/2016/10/16/wtf-is-a-container/ -->
<!-- - https://stackoverflow.com/questions/23735149/what-is-the-difference-between-a-docker-image-and-a-container -->

