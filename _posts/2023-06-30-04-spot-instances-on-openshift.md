---
layout: post
title: "Reduce Cloud Spend for Managed Red Hat Openshift Clusters With Spot Instances"
date: 2023-06-25
last_modified_at: 2023-06-30
cover_image: 2023-06-30-04-cover.png
author: Ankur Singh
categories: Kubernetes, Openshift
excerpt_separator: <!--more-->
---

> Photo by [softco.com](https://softco.com/)

Red Hat Openshift is emerging as one of the prominent managed service based on Kubernetes for building and managing cloud-native enterprise grade applications. Along with many advantages with respect to application deployment and management process like container orchestration, automatic scaling, load balancing, continuous integration/continuous deployment (CI/CD), it also provides options for leveraging respective cloud features to ensure cost optimization. <br>

One such integrated feature gaining traction is the utilization of spot instances for running workloads in openshift clusters. With steps for hands-on demo, this blog equips readers with the knowledge around the terms & terminologies, concepts, benefits and best practices to leverage spot instances and drive cost efficiencies in their managed Red Hat OpenShift clusters.
<!--more-->
<br>
Red Hat Openshift is available as a managed service on major cloud platforms provided by AWS, Microsoft, Google & IBM. We will be using Azure Red Hat Openshift(ARO) for our practical use-case, however, the process remains similar with some specific changes for respective cloud platforms. In order to interact with our ARO cluster, along with cli, Red Hat provides web based access which will be using to carry out deployments on our cluster easily. 

<br><br>


## **Pre-requisites**
- Understanding of [spot instances](https://learn.microsoft.com/en-us/azure/virtual-machines/spot-vms).
- A [public](https://learn.microsoft.com/en-us/azure/openshift/tutorial-create-cluster) or [private](https://learn.microsoft.com/en-us/azure/openshift/howto-create-private-cluster-4x) openshift cluster deployed on Azure. 

 <br><br>

## **Briefing On Terminologies & Concepts**

- **Something**<br>
  - something something
  - another thing

<br><br>


<iframe src="https://scribehow.com/embed/Guide_to_replace_a_worker_Vm_with_a_spot_instance__CMgYh7vSRUa7QigAY5-dGA?as=scrollable&skipIntro=true" width="100%" height="640" allowfullscreen frameborder="0"></iframe>


