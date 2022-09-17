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

<!--more-->

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