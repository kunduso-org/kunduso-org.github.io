---
title: "Create Terraform pre-requisites for Azure using Azure CLI in 3 easy steps"
date: 2021-03-27 19:56:15 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Any Terraform project configuring resources in Azure has pre-requisites. These are (i) a storage account, a container in the storage account, and the access key to the storage account, and (ii) a service principal credential to be able to communicate with Azure to create-update-delete resources. In this post, I describe the process to set up Azure cloud resources using Azure CLI that is required before using/referring them in Terraform configuration.</span></span></span>
<!--more-->
<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 1: Install Azure CLI on your local laptop and log in.</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Azure CLI docs: The Azure command-line interface (Azure CLI) is a set of commands used to create and manage Azure resources. The Azure CLI is available across Azure services and is designed to get you working quickly with Azure, with an emphasis on automation. More info at <span style="text-decoration:underline;"><a href="https://docs.microsoft.com/en-us/cli/azure/" target="_blank" rel="noopener">Azure CLI</a></span>.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Once installed, open the "Windows Azure SDK Environment" command prompt and key in "az login". That opens up a web browser requesting you to log in, and once done, the command prompt displays the subscription details.</span></span></span>
<img class="alignnone size-full wp-image-1233" src="https://skundunotes.com/wp-content/uploads/2021/03/39.tf-pr-az-image1.png" alt="39.TF-PR-Az-image1" width="750" height="451" />
<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 2: Create a storage account and storage container.</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Before we create a storage account, we create a resource group to host that. I have the commands that need to be run to create a resource group, storage account, and storage container.</span></span></span>
https://gist.github.com/kunduso/70bb5e869a349d3d6c3a5ff2dbe9589c
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Note:</strong> Store the access key to the <strong>storage account securely</strong>. That key is required when configuring the Azure backend to use the Terraform remote state.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 3: Create a service principal with required permissions</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">There is a single line command to create a service principal that will be sufficient to provision resources in Azure. Below is the command to do so from the Azure CLI.</span></span></span>
https://gist.github.com/kunduso/580b0c13875f1d841f252fe482d5db41
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">That command attaches a <strong>"contributor"</strong> role to the service principal, and the output is presented below that. As per the console output, these are credentials that you must protect. <strong>These credentials will be used by Terraform (in Terraform plan and destroy steps) to communicate with the Azure cloud to provision resources.</strong></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">That is all that is required to set up before using Terraform. On the Terraform project side, we refer to these pre-requisites in the <strong>"terraform init", "terraform plan"</strong>, and if applicable <strong>"terraform destroy"</strong> commands. I say "if applicable" because we do not <em>"generally speaking"</em> destroy all the resources; we alter them via the Terraform configuration as the project matures and evolves.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Below is an example of a Terraform backend configuration in Azure.</span></span></span>
https://gist.github.com/kunduso/8452cbd4f83f716dfd9f032f86a68473
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Below is the <strong>"terraform init"</strong> command required to set up a remote backend in Azure.</span></span></span>
https://gist.github.com/kunduso/be8330bde2925b546540e1cd1d2b45f8
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Below is the <strong>"terraform plan"</strong> command with the secured credentials being passed via the command line.</span></span></span>
https://gist.github.com/kunduso/7c71cb963a4804ad9248e8880bf1651e
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As an organization continues to use Terraform to automate the environment provisioning process, there is a tendency to reuse these resources like the storage account and the service principal. And, right at the beginning, <strong>every possible attempt should be made to keep the pre-requisites of a Terraform configuration project as isolated as possible.</strong></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I am not suggesting that we keep resources isolated just for the sake of doing so. If we are careful, we can manage the reuse of these resources. We can use the same resource group to host all the storage accounts belonging to different Terraform configuration projects. We can even reuse the same storage account to host the state file of various Terraform configuration projects. <strong>However, the container inside the storage account must be different for different Terraform configuration projects.</strong> The service principal "generally speaking" should also be unique per Terraform configuration project. I'll write a detailed note later on that.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">And that brings us to the end of how to create Azure resources referred to in a Terraform project. I hope you found this post useful. Please let me know if you have any questions or suggestions. Until then, happy terraforming!</span></span></span>

<em><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Note: The above Terraform commands are steps extracted out of an azure-pipelines.yaml file. I am using Powershell to run them.</span></span></span></em>

<strong><em><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Another Note: Please end the cloud session once the storage and service principal is created with the command "az logout".</span></span></span></em></strong>
