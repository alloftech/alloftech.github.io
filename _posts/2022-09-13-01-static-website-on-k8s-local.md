---
layout: post
title: "Deploy your static web app on local using kubernetes"
date: 2022-09-13
last_modified_at: 2022-09-13
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

> Note : We will be thoroughly understanding each step and each file mentioned in the repository as we move ahead. So fire up those brain cells !!


First and foremost, we will make sure all **pre-requisites are met** as mentioned in the `README.md` file of the repository. Installations on their respective documentations are pretty straight-forward and easy to follow. Verify your installations through cli.

![Installation Verification](/assets/images/2022-09-13-01-intallations.png)

Now, as we know that k8s is a _container orchestration tool_ so we will be running our website in an isolated **container**. To run our application in containers we will have to create an **image** bundled with our application code & all dependencies needed to run our website.






