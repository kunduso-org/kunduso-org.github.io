---
title: "Terraform Remote State -Part 2: Using Azure"
date: 2021-03-25 11:30:46 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This note is in continuation to the previous one  -<a href="https://skundunotes.com/2021/03/12/terraform-remote-state-part-1-using-aws/" target="_blank" rel="noopener">Terraform Remote State -Part1: Using AWS</a>, where I've covered the <strong>what and how of a terraform remote state</strong>. So that we are all on the same page, I'll list down a few important points related to that:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-the current state of infrastructure as referred to by a terraform configuration is stored in a state file called <strong>terraform.tfstate</strong></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-the file gets created on command "terraform apply" and referred by each subsequent "terraform plan" command</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-the default location of the terraform.tfstate file is the <strong>exact directory where the rest of the terraform configurations</strong> are stored</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-terraform remote state refers to the storage of the terraform configuration state file in a location that <strong>assists automation, encourages collaboration, and enhances security</strong></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-configuring a remote-state is done by<strong> adding a backend block</strong> to the existing terraform configuration</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In my previous post, I explored the idea of "terraform remote state" using AWS. In this post, I'll list the steps to achieve the same using Azure.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As is standard, <strong>there are two pre-requisites to terraform remote state</strong>. Although it's obvious, when I state pre-requisites, these are resources that should already exist before they are used in a particular Terraform configuration. In the case of Azure, they are - (i) <strong>a storage container to store the key (terraform.tfstate file) and</strong> (ii) <strong>access key to the storage container.</strong></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 1:</strong> Add the following block to existing terraform configuration file. I have seen repositories (on GitHub) where there is a separate file by the name of "backend.tf" where the backend configuration is stored. I believe storing backend configuration in a separate file keeps the configuration accessible and readable, rather than adding that block to an existing Terraform configuration.</span></span></span>

https://gist.github.com/kunduso/8452cbd4f83f716dfd9f032f86a68473

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">If you've worked on Azure, you know that every resource belongs to a resource group and that is what we set in $(ResourceGroupToStoreTerraformBackendResources). Terraform stores the key (terraform.tfstate) in a storage container $(StorageContainerName) that belongs to a storage account $(UniqueStorageAccountName) that belongs to a resource group $(ResourceGroupToStoreTerraformBackendResources). The $(StorageAccountAccessKey) is the storage account access key.</span></span></span>
<em><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Note: If you recall, in AWS, we also stored a lock in a dynamoDB table to restrict concurrent access. In the case of Azure, that is not required since simultaneous access of the terraform.tfstate file is not allowed in a storage container.</span></span></span></em>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 2:</strong> After the backend block is added to our Terraform configuration, we have to inform Terraform to initiate the backend. That is done with the "<strong>terraform init</strong>" command. Whether you already have an existing terraform.tfstate in your local workspace or initializing the configuration for the first time, we have to start with the "terraform init" command. </span></span></span><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Any subsequent changes made to the backend block should always be followed by a "terraform init".</span></span></span>

https://gist.github.com/kunduso/be8330bde2925b546540e1cd1d2b45f8

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Again, as is customary, I have a code repository (<span style="text-decoration:underline;"><a href="https://github.com/kunduso/Terraform-Azure-RemoteBackend" target="_blank" rel="noopener">Terraform Azure RemoteBackend</a></span>) where I demonstrate the backend configuration pointing to my Azure subscription. This repository has a backend.tf file with the details of an Azure backend. The skeleton code is to configure a Virtual network. The <strong>azure-pipelines.yaml</strong> file has all the Terraform steps in detail.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">If you noticed, <strong>I did not mention service principal here. We do not require service principal credentials to store remote state in the storage container. We instead, use the storage account access key to do so.</strong> Terraform requires service principal credentials to interact with Azure cloud to provision resources. Particularly, service principal credentials are required at "terraform plan" step.</span></span></span>

https://gist.github.com/kunduso/7c71cb963a4804ad9248e8880bf1651e

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As I mentioned in my previous note -remote backend in Terraform is a crucial concept to understand as you move towards adopting an infrastructure-as-code mindset.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I hope you found this post helpful, and let me know if you have any questions or comments.</span></span></span>
