---
layout: post
title: "Reduce Cloud Spend for Managed Red Hat OpenShift Clusters With Spot Instances"
date: 2023-06-25
last_modified_at: 2023-06-30
cover_image: 2023-06-30-04-cover.png
author: Ankur Singh
categories: Kubernetes, OpenShift
excerpt_separator: <!--more-->
---

> Photo by [softco.com](https://softco.com/)

[Red Hat OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift) is emerging as one of the prominent managed service based on Kubernetes for building and managing cloud-native enterprise grade applications. Along with many advantages with respect to application deployment and management process like container orchestration, automatic scaling, load balancing, continuous integration/continuous deployment (CI/CD), it also provides options for leveraging respective cloud features to ensure cost optimization. <br>

One such integrated feature gaining traction is the utilization of spot instances for running workloads in OpenShift clusters. With steps for hands-on demo, this blog equips readers with the knowledge around the terms & terminologies, concepts, benefits and best practices to leverage spot instances and drive cost efficiencies in their managed Red Hat OpenShift clusters.
<!--more-->
<br>
Red Hat OpenShift is available as a managed service on major cloud platforms provided by AWS, Microsoft, Google & IBM. We will be using [Microsoft Azure Red Hat OpenShift(ARO)](https://www.redhat.com/en/technologies/cloud-computing/openshift/azure) version 4.10.54 for our practical use-case. However, the process remains similar with some specific changes for respective cloud platforms. In order to interact with our ARO cluster, along with cli, Red Hat provides an elegant web based user interface which we will be using to carry out actions on our cluster easily. 

<br><br>


## **Pre-requisites**
- [az cli](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) installed on your system.
- A [public](https://learn.microsoft.com/en-us/azure/OpenShift/tutorial-create-cluster) or [private](https://learn.microsoft.com/en-us/azure/OpenShift/howto-create-private-cluster-4x) OpenShift cluster deployed on Azure.

 <br><br>

## **Briefing On Technologies & Concepts**

- **Spot Instance**<br>
  - The concept of [spot instances](https://learn.microsoft.com/en-us/azure/virtual-machines/spot-vms) is based on a dynamic pricing model. Cloud providers allocate their excess or unused capacity to spot instances, which are made available to users through a bidding system. 
  - Users specify the maximum price they are willing to pay per hour for the instance, and if the current spot price is below their bid, they can acquire the instance.
- **OpenShift**<br>
  - [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/OpenShift) is a containerization platform developed by Red Hat. It provides a complete ecosystem for deploying, managing, and scaling containerized applications.
  - It is built upon Kubernetes, an open-source container orchestration platform, and adds additional features and capabilities to make it more accessible and developer-friendly.
- **MachineSets**<br>
  - In OpenShift, a [MachineSet](https://docs.OpenShift.com/container-platform/4.8/machine_management/creating_machinesets/creating-machineset-azure.html) is a Kubernetes object used to manage and control the deployment and scaling of worker nodes within a cluster.
  - A MachineSet defines the desired state of a group of machines, specifying the number of replicas, the machine template (which includes details such as the machine image, instance type, and other configurations), and the rules for scaling up or down based on resource requirements.

<br><br>

## **Why To Use Spot Instances ?**

In general, spot instances have a tremendously lower price as compared to on-demand virtual machines on any cloud platform varying with respect to regions. In our case, lets do our math right for expenses related to virtual machines in default scenerio for an OpenShift cluster on Azure. <br> 

- During cluster creation, *Azure Red Hat OpenShift* managed service creates on-demand virtual machines for
computation requirements as nodes of type `Standard_D8s_v3`.
- Looking at the [documentation](https://azure.microsoft.com/en-in/pricing/details/virtual-machines/red-hat/#pricing) for cost of these virtual machines in *East US* region as when writing this blog, it is estimated to be **~$280/month**. With increasing traffic & workload, the number and size of instances is meant to increase for the cluster to be able to serve smoothly. Let's keep the scaling part aside for now.
- Coming to spot instances for type `Standard_D8s_v3`, the estimated price drops significanty to **~$83/month**. That's nearly 70% lesser than that of on-demand instances. This would further drop for regions which do not have high demand for compute resources.
- So, with replacement of one on-demand virtual machine with spot instance, we will be saving **~$200/month**. 
- Lastly, let's consider a cluster with 10 worker nodes which usually is not a huge cluster but normal for a small/mid size enterprise application. To balance cloud cost optimization and SLA of application, even if 5 nodes would be replaced with spot instances under resilient architecture design, around **~$1000/month** could be saved. Imagine there exist clusters with dozens of worker nodes as well !!

<br><br>

## **How To Use Spot Instances For ARO Clusters ?**
Azure Red Hat OpenShift has provisions to leverage the usage of spot instances as nodes for clusters. <br>
After creation of cluster, as we will be using web interface provided by Red Hat, URL for the console will be required. Further, we will require creadentials to access the cluster and login to the web interface. We will use `az`cli to get these information.<br>
Before beginning with the step-by-step practical, make sure to have `CLUSTER` & `RESOURCEGROUP` environment variables exported with correct values of name of cluster and resource group, respectively.<br><br>

Lets go now !! 🚀

<br><br>

<iframe src="https://scribehow.com/embed/Replacing_a_VM_node_with_a_Spot_node_on_Azure_Red_Hat_OpenShift__CMgYh7vSRUa7QigAY5-dGA" width="100%" height="640" allowfullscreen frameborder="0"></iframe>

<br><br>

## **Some Best Practices To Keep In Mind**
- Even though it seems tempting to do, but master nodes must not be run as a spot instance especially in the context of a managed OpenShift service such as ARO.
- Thorough research and testing is required to create a resilient architecture integrating spot instances to maintain a balance between optimizing cost and enabling high availability during eviction of instances.
- Ensure only non-critical pods or applications like one involving batch processing or stateless applications are scheduled on the spot instances as there won't be any loss of data or packet drops in case of nodes being not ready during eviction.
- Distribute your spot instance creation in different availability zones to enhance availability & resilience.
- Periodically review and update your spot instance bids to remain competitive in the market and take advantage of potential cost savings.
- Continuously monitor instance terminations, spot instance pricing trends, and system metrics to make informed decisions and quickly respond to any changes or interruptions.

<br><br>

## **Conslusion**<br>
In conclusion, utilizing spot instances in your managed Red Hat OpenShift clusters on the cloud can significantly reduce your cloud spend while maintaining the necessary performance and availability. Spot instances are particularly useful for workloads that can tolerate interruptions and have flexible resource requirements. They are commonly utilized for tasks such as batch processing, big data analytics, research simulations, or any workload that can be distributed across multiple instances or restarted without significant data loss.

<br><br>
