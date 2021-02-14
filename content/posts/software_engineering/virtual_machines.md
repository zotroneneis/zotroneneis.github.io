---
title: An introduction to virtual machines
date: 2018-09-29
menu:
  sidebar:
    name: Intro to virtual machines
    identifier: virtual_machines
    parent: software_engineering
    weight: 4
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

{{< img src="/posts/software_engineering/images/virtual_machine.png" width="30%" align="center" >}}


### What are disadvantages of virtual machines?

When creating a virtual machine, part of the host's resources are used to provide an isolated environment for the virtual machine. After assigning these resources, the main system can't use them for its own processes anymore. For example, when the virtual machine is assigned 1GB of memory but uses only half of that memory at a time, the remaining 50% will be wasted. This results in a degradation of the main systems performance since some resource sit around without being used. This is one of the disadvantages of virtual machines and at the same time one of the benefits of containers and docker.
