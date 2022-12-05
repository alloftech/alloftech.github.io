---
layout: post
title: "Containerization Deep Dive"
date: 2022-11-20
last_modified_at: 2022-11-20
cover_image: 2022-11-20-03-cover.jpg
author: Ankur Singh
categories: Kubernetes
excerpt_separator: <!--more-->
---

> Photo by [pinterest](https://in.pinterest.com/)

Massive spike in interest in Linux Containers has been a driving force for need to adopt various containerization technologies. **Containerization** is not a recent term but a series of innovations in the open source software community, especially in the _operating system_. This blog talks about the When, What, Why & How of the containerization world and also discusses a few prominent containerization tools used by top organizations.

<!--more-->
<br><br>

### <span style="text-decoration: underline"> History Of Containers </span>

The main reason containers came into existence was to provide a way to isolate multiple processes along with their resource utilization on the same host without any need for virtualization making it light weight and easily portable. <br>

- An Early implementation of container technology, [**_jails_**](https://wiki.freebsd.org/Jails), was added in FreeBSD during early 2000.
- Further, in 2001, the [Linux-VServer](http://linux-vserver.org/Welcome_to_Linux-VServer.org) was introduced with an intention to _allow several general purpose Linux servers on a single box with high degree of independence_. This was the beginning of the efforts to create distinct units that felt and looked like real server to the internally running process.
- The important part of container solution was introduced in 2006 with _generic process containers_ renamed as [**Cgroups**](https://www.kernel.org/doc/html/v4.18/admin-guide/cgroup-v2.html?highlight=user%20namespace). Cgroups ensured that none of the containers monopolizes the CPU, memory or disk I/O by allowing processes to be grouoped together and giving each container its share.
- In 2008, another vital part of the conainer solution, _Kernel namespaces_, was coined along with [**user namespaces**](https://man7.org/linux/man-pages/man7/user_namespaces.7.html) patch. Containers _having their own users_ and _getting root privileges inside of them_ without being provided outside is the boon of user namespaces. 
- With such growth and flexibility, security was the rising concern for the container users. To eliminate the doubt, containers were added with support for [SELinux](https://www.redhat.com/en/topics/linux/what-is-selinux) & [Seccomp](https://www.kernel.org/doc/html/v4.18/userspace-api/seccomp_filter.html) in 2015
- Current Linux Container built on all developments done before is undoubtedly the initiative of the open source **Docker Project** and associated _Docker Container format_. In late 2015, Docker (the company), which is the largest contributor to Docker (the project), donated it to Open Container Initiative (Linux Foundation) to prevent fragmentation and promote open standards.

### <span style="text-decoration: underline"> What Exactly Are Containers & Why Are They So Popular ? </span>

In Linux perspective a container is an isolated process with restrictions which rather than running a single binary as any simple process would have, it runs an image. An image can be underdstood as a file-system bundled with all dependencies viz file system, available resources, source code, installed packages, kernel modules, etc. <br>

Images may have one or multiple [layers](https://blogs.cisco.com/developer/container-image-layers-1) to it which are non-writable. To create a container, the [container runtime](https://opensource.com/article/21/9/container-runtimes) executes the image. As a container is created using an image, it adds another writable layer, commonly called *container layer*,  over the immutable layers of the image and that is ephemeral i.e. it exists only till the container is running. Once container is deleted, its associated container layer is also deleted and all data written on it is lost. As each container creates its own layer to perform i/o operations, multiple containers using the same image can be created and ran in an isolated manner on the same host itself. 

|![Layers in Containerization](/assets/images/2022-11-20-03-image-layers.png) |
|:--:|
| *Layers in Containerization* (Photo by [ParTech](https://www.partech.nl/nl/)) |

Before talking about the popularity of containers, let's first know what major pain-points did they solve for the developers. 

Developers generating code on a version control tool in a distributed ecosystem. Further, for this code to be merged and pushed to production, it is to be thoroughly tested and certified on non-production servers to be sure that it won't have any impactful issue in production. 
While the code being the same, the infrastructure or environment in which it was running to be tested differed from production based on difference in processes around dependency installation & versions, startup procedures, build, configurations, operating systems, etc. 

Therefore, even after being tested, the code could malfunction in production due to inconsistency in the environment. Here comes **containers** for the rescue !!

Containerization in general gives a far better solution for shipping not only the code but all its configurations, dependencies, scripts, etc bundled in a single entity called an *image*. An image would provide a consistent and a self sufficient environment for the application making it possible to be executed the same way on production as it would be done on local or testing stages. Moreover, as discussed earlier, multiple instances of the same image can be created as containers to be running on the same host in an isolated manner making it easy to scale the application.

Thus, eliminating the overhead of developers thinking about why *IT RUNS ON MY MACHINE, NOT SURE WHAT"S WRONG WITH PRODUCTION !*

Other major reasons for wide acceptance of containers are :
- It is lightweight as compared to full-fledged VMs
- Highly supportive when it comes to managing & deploying micro-services
- Easily scalable and manageble using orchestration tools
- Huge support from top cloud vendors via managed services for applications
- Ensuring complete resource utilization of the virtual machine with multiple micro-services running on same host in an isolated manner if needed

### <span style="text-decoration: underline"> How To Work On Containers ? </span>

Let's begin with the most interesting part of the blog where we will be looking at creating containers. 

Amongst the major containerization tools, [**Docker**](https://www.docker.com/) has been the widely accepted. While Docker has an awesome documentaion to be referred for concepts of containerization and has been the easiest tool to be used, personally I've found [**Podman**](https://podman.io/) to be a [better](https://www.ionos.com/digitalguide/server/know-how/podman-vs-docker/) alternative. Among many other issues with Docker, its *complicated server-client architecture* and the *necessity for root previleges for the daemon process* has bugged me alot when it comes to efficient resource utilization & security risks.

Even if Podman is not familiar to you, don't worry !! The creators of Podman have made sure that, along with providing cool features like rootless containers and lower resource utilization, users of Docker can easily adapt to it as most of the cli commands are exactly same. Also, they've got Podman Desktop for you as well and most importantly Podman uses [CRI-O](https://cri-o.io/) (first implementation of [Open Container Initiative (OCI)](https://opencontainers.org/)  standards suite that passes all Kubernetes integration tests) as container runtime, so it seems like the future of containerization tool !!!

Without further ado, let's hop on for creating containers using Podman as our containerization tool. Ofcourse, you will have to [*install*](https://podman.io/getting-started/installation) podman first !

For our example we will be using a basic image of [*Apache HTTP Server Project*](https://hub.docker.com/_/httpd).

Firstly, we will use podman to check if httpd image exists and pull it in our local machine. By default, it will be referring remote image registry of Docker.

|![Image search & pull](/assets/images/2022-11-20-03-podman-1.1.png ) |
|:--:|
| *Image search & pull* |

Now, we will create a container using the image. It will run a simple http server inside the container on port `80` which we will forward on port `8080` of our host.

|![Running Server In Container](/assets/images/2022-11-20-03-podman-1.1.png ) |
|:--:|
| *Running Server In Container* |


For debugging purposes, we can use various commands and provide id of the container as parameter
> Note : The user `root` in last command isn't the root user of the host but of the container

|![Troubleshooting Container](/assets/images/2022-11-20-03-podman-1.1.png ) |
|:--:|
| *Troubleshooting Container* |

Lastly, if needed we can run commands inside of the container either directly or by accessing the shell prompt of the container

|![Accessing Running Container Files](/assets/images/2022-11-20-03-podman-1.1.png ) |
|:--:|
| *Accessing Running Container Files* |

### <span style="text-decoration: underline"> Conclusion </span>

With this, I would like to conclude this blog with below points :
- Containers are solution to proper resource utilization, easy scalability, high portability & light-weight application execution.
- Containers are not magic but alot of simple concepts being collaborated in an evolutionary pattern to create an ecosystem favourable for ever increasing advancements of IT world. There is more than just docker commands in containerization !!!
- Orchestration of containers is another boom in the IT market. Sooner or later, most traditional monolith applications will have to be managed using a micro-service architecture running on containers handled by tools like Kuberneters or RedHat OpenShift for better performance

