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
- [az cli](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) installed on your system.
- A [public](https://learn.microsoft.com/en-us/azure/openshift/tutorial-create-cluster) or [private](https://learn.microsoft.com/en-us/azure/openshift/howto-create-private-cluster-4x) openshift cluster deployed on Azure.
- Understanding of [MachineSets](https://docs.openshift.com/container-platform/4.8/machine_management/creating_machinesets/creating-machineset-azure.html) in [Openshift](https://www.redhat.com/en/technologies/cloud-computing/openshift)

 <br><br>

## **Briefing On Terminologies & Concepts**

- **Something**<br>
  - something something
  - another thing

<br><br>

## **Why To Use Spot Instances ?**

In general, spot instances have a tremendously lower price as compared to on-demand virtual machines on any cloud platform varying with respect to regions. In our case, lets do our math right for expenses related to virtual machines in default case for a Openshift cluster on Azure. <br> 

- During cluster creation, *Azure Red Hat Openshift* managed service creates on-demand virtual machines for
computation requirements as nodes of type `Standard_D8s_v3`.
- Looking at the [documentation](https://azure.microsoft.com/en-in/pricing/details/virtual-machines/red-hat/#pricing) for cost of these virtual machines in *East US* region, it is estimated to be **~$280/month**. With increasing traffic & workload, the number and size of instances is meant to increase for the cluster to be able to serve smoothly. Let's keep the scaling part aside for now.
- Coming to spot instances for type `Standard_D8s_v3`, the estimated price drops significanty to **~$83/month**. That's nearly 350% lesser than that of on-demand instances. This would further drop for regions which do not have high demand for compute resources.
- So, with replacement of one on-demand virtual machine with spot instance, we will be saving **~$200/month**. 
- Lastly, let's consider a cluster with 10 worker nodes which usually is not a huge cluster but normal for a small/mid size enterprise application. To balance cloud cost optimization and SLA of application, even if 5 nodes would be replaced with spot instances under resilient architecture design, around **~$1000/month** could be saved. Imagine there exist clusters with 1000+ worker nodes as well !!

<br><br>

## **How To Use Spot Instances For ARO Clusters ?**
Azure Red Hat Openshift has provisions to leverage the usage of spot instances as nodes for clusters. <br>
After creation of cluster, as we will be using web interface provided by Red Hat, URL for the console will be required. Further, we will require creadentials to access the cluster and login to the web interface. <br>
Before beginning with the step-by-step practical, make sure to have `CLUSTER` & `RESOURCEGROUP` environment variable created with correct values of name of cluster and resource group, respectively.<br><br>

Lets go !!

<br><br>

<iframe src="https://scribehow.com/embed/Replacing_a_VM_node_with_a_Spot_node_on_Azure_Red_Hat_Openshift__CMgYh7vSRUa7QigAY5-dGA?skipIntro=true" width="100%" height="650" allowfullscreen frameborder="0"></iframe>

<br><br>


