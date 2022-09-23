---
layout: post
title: "Deploy your static web app on local using kubernetes"
date: 2022-09-24
last_modified_at: 2022-09-24
cover_image: 2022-09-13-01-cover.png
author: Ankur Singh
categories: Kubernetes
excerpt_separator: <!--more-->
---
> Photo by [buddy.works](https://buddy.works)

Our tech industry has always promoted great innovations by adopting it for better, quicker and reliable performance. [Kubernetes](https://kubernetes.io/), known as k8s, has been one of those popular tools that has gained huge importance and community support in recent years.

In this blog, we will be looking into setting up a local kubernetes cluster, understanding required architecture & components and actually deploying a static website on it.

<br><br>

## Overview of tools that will be in use for setting up a static website using kind

- **Docker**<br>
  - [Docker](https://docs.docker.com/get-started/overview/) is an open-source tool that provides feasibility to **create, package, run** the software/application independent of the underlying infrastructure
  - It provides the best option to isolate the application and be executable irrespective of different environments
  - To understand docker thoroughly, please check upon this detailed [Docker architecture](https://docs.docker.com/get-started/overview/#docker-architecture) in their documentation.
  - CLI commands for creating images
- **Docker Image**<br>
  - Docker Image can be called a static **template** or **blueprint** of an application that is created using instructions mentioned in a file called **Dockerfile**
  - Docker Images are in a layered structure where each **RUN** command in **Dockerfile** will append new layer on top of previously executed command 
- **Docker Container**<br>
  - Running instance of Docker Image is called as **Container**
  - Docker is also called as Containerization tool where application images are self-contained with all pre-requisites of source code, libraries, configurations, dependent packages, etc..
- **Docker Registry**<br>
  - [Docker Registry](https://docs.docker.com/get-started/overview/#docker-registries) is the storage for Docker Images from where we can pull and push docker images
  - Docker Hub is the default public registry where the user's can push their local images or pull the publicly available remote images
<br><br>
![Docker Architecture](/assets/images/2022-09-22-00-docker-architecture.png)

> Photo by [docs.docker.com](https://docs.docker.com/get-started/overview/)

<br><br>
- **kind**<br>
  - **kind** called as **kubernetes in docker**
  - There are multiple tools used which helps in creating k8 cluster on local system for setting up development environment
    - [kind](https://github.com/kubernetes-sigs/kind)
    - [minikube](https://github.com/kubernetes/minikube)
    - [CodeReady Containers](https://github.com/code-ready/crc)
    - [minishift](https://github.com/minishift/minishift)
  - kind can create, manage , run the k8 local cluster using docker **container** as **k8 nodes**
- **make**<br>
  - make is a build tool which ease the creation of executables from source code or executes set of programs/instructions/commands which are written in **Makefile**
  - Each block in **Makefile** is referred as target where set of instruction are executed
    ```
    #!/usr/bin/env make
    .PHONY: test_target

    test_target: dependent_on_target
        command1 && \
          command2
    ```
  - Command to execute any target : `make <target name>`
- **kubectl**<br>
  - It is a command line tool using which commands can be executed on Kubernetes clusters 
  - `kubectl` is a CLI that is run with many options available for managing or getting descriptions for any components of k8(pods, deployments, services, ingress, configMap, etc...) 
  - In the backend,it makes an API call to performs all managing actions on k8 clusters
- **Kubernetes manifest/components**<br>
  - ***Pods***<br>
    - In the K8 cluster the applications are deployed as containers on worker nodes
    - They are not deployed directly on worker nodes -> ENCAPSULATED in k8 object called POD
    - POD is a single instance of an application, the smallest object that can be created in the K8 cluster
  - ***Deployment***<br>
    - k8 Deployment is an encapsulation of Pods where its manifest declaration includes which image to be used, the number of Pods(application containers) to be run, number of replicas for the same application to maintain high availability
    - Deployment provides features:
      - Rolling Updates (Pods are replaced in rolling fashion ensuring high availability of application)
      - Seamless Undo/Resume changes
      - Rollbacks
    - Deployments encapsulates ReplicaSet --> Deployment( ReplicaSet ( Pod ( Container )  ) )
  - ***Service***<br>
    - Pods are dynamically created and destroyed due to which it does not have a persistent IP address
    - However, the Deployment component abstracts the pods but internal access to the pods cannot rely on the dynamic change of pods lifecycle(creation & deletion)
    - Here comes a resource of k8 called as Service which logically group the pods with `selector` keyword
    - The multiple micro-services within the k8 cluster can access other applications(pods) through the Service component as a point of re-direction
<br><br>
![Kubernetes Architecture](/assets/images/2022-09-22-00-kubernetes-architecture.png)

> Photo by [phoenixnap.com](https://phoenixnap.com/kb/wp-content/uploads/2021/04/full-kubernetes-model-architecture.png)

<br><br>
  - ***Ingress***<br>
    - Ingress act as Reverse Proxy in the k8 cluster
    - It's the resource that is an interface between the outside network(external access) and the Kubernetes cluster
    - So if the outside world needs to interact/access any application in the k8 cluster their request will go through the ingress component
    - An Ingress manifest includes the hosts, re-direction path or rules that will internally make a request to the Service component
  - ***Ingress Controller***<br>
    - An Ingress controller is required to manage and controls the ingress component
    - It defines how ingress should work
    - Example: AWS Load Balancer controller which controls the Elastic Load Balancing for K8 clusters
<br><br>
![Kubernetes Ingress](/assets/images/2022-09-22-00-k8-ingress.png)

> Photo by [eksworkshop.com](https://www.eksworkshop.com/beginner/130_exposing-service/ingress/)

<br><br>

## Hosting Your Static Web Application On Local

Now without any further delay let's hop into deploying your static website on local kubernetes cluster. 

To ease up the process, we will use a [boiler-plate](https://github.com/sankur-codes/k8s-basic-project) which is specifically created for our use case and clone it in our local machine. The `README.md` file gives a step-by-step approach for making your static website up and running on local.

- Login to your github account
- Open [this](https://github.com/sankur-codes/k8s-basic-project) repository in new tab
- Click on **Fork** on the top right corner
- Scroll to the end and click on **Create Fork**
- Now the repository is in your github account. Click on **Clone** and copy the URL
- Open a terminal window on your machine and run `git clone <copied_URL>` on the cli. <copied_URL> is the URL you copied in previous step.
- Change directory to that of the cloned repository by running `cd k8s-basic-project`

> Note : Please refer steps mentioned in `README.md` file of the repository hereafter. We will be thoroughly understanding each step mentioned in the file as we move ahead. So fire up those brain cells !!

<br><br>

First and foremost, we will make sure all **pre-requisites are met** as mentioned in the `README.md` file of the repository. Installations on their respective documentations are pretty straight-forward and easy to follow. later, verify your installations through cli.

![Installation Verification](/assets/images/2022-09-13-01-intallations.png)

<br><br>

Now, as we know that k8s is a _container orchestration tool_ so we will be running our website in an isolated **container**. To run our application in containers we will have to create an **image** bundled with our application code & all dependencies needed to run our website.

Formerly, copy your static website code in `website` folder of the repository <br>
`cp -r <path_to_your_website_code>/* website/`

<br>

To build a docker image, there is a generic Dockerfile already present in the repository. Let's break it out for understanding : 
- `FROM nginx` : Pulls latest [nginx](https://hub.docker.com/_/nginx/tags) image from docker hub
- `LABEL maintainer="Ankur Singh"` : Just a label. you can add your name as maintainer if you want
- `COPY website/ /usr/share/nginx/html/` : Copies your website code to appropriate folder
- `EXPOSE 80` : Doesn't actually expose any port but gives an idea about container ports to be exposed

<br>

Let's go ahead to build the image and run your application in a docker container with a handy command <br>
`make run imageName=mywebapp imageTag=1.0 containerName=container_app hostPort=5000`

What will this command do ?
- It will refer the `Makefile` file and execute the commands mentioned in `run` target. It will pass the parameters provided during execution to the commands being executed withing the target. If no arguments are provided, default values are taken which are mentioned in the beginning of the `Makefile`
- First is `sudo docker build -t $(imageName):$(imageTag)`. It will refer the `Dockerfile` and build an image called **mywebapp:1.0**
- Second command is `sudo docker run -d --rm --name $(containerName) -p $(hostPort):80 $(imageName):$(imageTag)` which creates the container. It will use the image **mywebapp:1.0** and create a container named **container_app** whose port **80** is exposed to port **5000** of your system

![WebApp Containerization](/assets/images/2022-09-13-01-containerization.png)

Also visit `127.0.0.1:5000` in your browser to check if your static web application is accessible