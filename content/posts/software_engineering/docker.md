---
title: An introduction to Docker
date: 2018-11-10
menu:
  sidebar:
    name: Intro to Docker
    identifier: docker
    parent: software_engineering
    weight: 4
---

**Topics:** Docker
 

I have been wanting to learn more about Docker for months. However, when starting to read Dockers [get started page](https://docs.docker.com/v17.09/get-started/) I quickly had to find out that I'm lacking knowledge in too many other concepts. Since my background isn't computer science I often run across this problem. Luckily, this allows me to constantly learn new things! After writing a post on [virtual machines](http://alpopkes.com/posts/2018/09/intro-to-virtual-machines/) and another about [containers](http://alpopkes.com/posts/2018/10/intro-to-containers/) this one is finally about Docker!

## Docker

### What is Docker?

Docker is a tool that makes it easy to use [containers](http://alpopkes.com/posts/2018/10/intro-to-containers/). We already learned that containerization allows us to package an application and all its dependencies into a little box - the container - which can then run anywhere. This allows developers to be sure that their software will run independently of where it is deployed.  

Of course, there were other technologies for containerization before Docker. However, Docker made it much easier and also safer to deploy containers than previous approaches. Another major benefit of Docker is that it's open source. This means that anyone can contribute to Docker and that Docker can be customized to one's own needs.


### How Docker works

A basic Docker architecture consists of three major parts:

1. Docker daemon (`dockerd`)     
The Docker daemon is responsible for creating, running, and monitoring Docker objects like containers or images. It can communicate with other daemons to manage Docker services. Usually, the daemon is launched by the host operating system.

2. Docker client (`docker`)     
The Docker client talks to the Docker daemon. For most people, the Docker client is the main way to interact with Docker. When calling commands like `docker run`, the Docker client sends them to the Docker daemon, who carries out the commands. It's also possible to interface directly with the daemon without using the Docker client.

3. Docker registries      
A registry stores and distributes Docker images. The big public registries are [Docker Hub](https://hub.docker.com/) and [Docker Cloud](https://www.docker.com/sites/default/files/Docker%20Cloud.pdf) and can be used by anyone to pull or push publically available images. You can also run your own private registry. However, by default `docker pull, docker push` and `docker run` commands will look for the required images on Docker Hub.

{{< img src="/posts/software_engineering/images/docker_architecture.png" width="80%" align="center" >}}


### Docker objects

When using Docker we create and use different kinds of objects, for examples images, containers, networks or plugins. Images and containers are the most important objects, so let's take a closer look at them. For more details on containers, take a look at the [blog post on containers](http://alpopkes.com/posts/2018/10/intro-to-containers/).

#### Images
- Template that contains instructions for creating a Docker container    
- We can create our own images or use those created by others and published in a registry    
- To create an image we first have to create a *Dockerfile*    
- The Dockerfile defines the steps required to create and run the image     

#### Containers
- Runnable instance of an image     
- A container is defined by its image and the configuration options that we provide when creating or starting the container    
- To learn more about containers, look at the [corresponding blog post](http://alpopkes.com/posts/2018/10/intro-to-containers/)    


### Sources
- [Docker documentation](https://docs.docker.com/engine/docker-overview/#the-docker-client)     
- [Docker get started](https://docs.docker.com/get-started/part2/#your-new-development-environment)      
- [Blog](https://blog.knoldus.com/docker-architecture/)     
- [O'Reilly book](https://www.oreilly.com/library/view/using-docker/9781491915752/ch04.html)     

