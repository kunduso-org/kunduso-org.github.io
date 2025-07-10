---
title: "Exploring Azure Pipelines, Terraform, and Powershell"
date: 2021-02-25 20:19:22 +0000
categories: []
tags: []
---

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">As of this writing [Feb 2021], if you've used the <a href="https://marketplace.visualstudio.com/items?itemName=ms-devlabs.custom-terraform-tasks" target="_blank" rel="noopener">Terraform extension from Microsoft DevLabs</a>, you'd have noticed that there is support for only a tiny set of Terraform commands out of the box. These are <code>init</code>, <code>validate</code>, <code>plan</code>, <code>validate and apply</code>, and <code>destroy</code>.</span></span></span>
<!--more--><img class="alignnone size-full wp-image-1143" src="https://skundunotes.com/wp-content/uploads/2021/02/35.exploring-ap-tf-ps-image2.png" alt="35.Exploring-AP-TF-PS-image2" width="761" height="282" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>But Terraform has many commands: <code>fmt</code>, <code>import</code>, <code>output</code></strong>, <code>show</code>, <code>taint</code>, <code>workspace</code>, etc.,<strong> which are not referenced by the Terraform task (from Microsoft DevLabs).</strong> <strong>What are the options if I want to use those?</strong> Yes, a few more extensions are available, like the one by Charles Zipp -but as of now, those are to provision resources in Azure Cloud only. I, however, was interested in provisioning and managing resources in AWS. And so I explored an option -my favorite automation and scripting ally --<strong>POWERSHELL</strong>!!!</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Working with Powershell, <strong>I achieved my integration goal, namely using Azure Pipelines and Terraform with Powershell to provision resources in AWS.</strong> In this post, I list the steps to deploy that.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Before we begin with the automation process, there are a few <strong>pre-requisites</strong> we need to have. These are:</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">-AWS IAM user with permission to create (administer) an Amazon VPC and manage (administrator) an Amazon S3 bucket (for remote state)</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">-existing Amazon S3 bucket details to store the remote state</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">-a GitHub repo containing Terraform configuration files</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">-an Azure DevOps project</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>The first step is to install Terraform.</strong> There are two paths that a project can take while deciding the build infra, self-hosted, or Azure Pipelines hosted.</span></span></span>
<img class="alignnone size-full wp-image-1144" src="https://skundunotes.com/wp-content/uploads/2021/02/35.exploring-ap-tf-ps-image3.png" alt="35.Exploring-AP-TF-PS-image3" width="873" height="330" />

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">If you build on an Azure-supplied VM, then the installer task makes sense since we do not manage the build machine.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">If you have a self-hosted box, you may decide to have a dedicated VM to build all Terraform configuration pipelines. In that case, you may have Terraform installed on that VM. The installation steps are easy to follow.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">-select the TF version from <a href="https://releases.hashicorp.com/terraform/" target="_blank" rel="noopener">Terraform download</a></span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">-download the installer and unzip</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">-add the path to Terraform.exe to PATH environment variable</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">-open a new PowerShell console and type in "terraform version". You should get the same version that you selected to download.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Here is a link for detailed steps: <a href="https://learn.hashicorp.com/tutorials/terraform/install-cli" target="_blank" rel="noopener">Install Terraform</a></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The other option, even though a self-hosted build VM is used, is to use the Terraform tool installer task (from Microsoft DevLabs) as the first step in a pipeline.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">On the other hand, if you build on an Azure-supplied VM, then the installer task makes sense since we do not manage the build machine.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Either way, I think the installer task provides more flexibility since we are not tied to a single version to compile all the Terraform Configuration files.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Once Terraform is installed, while configuring an Azure pipeline, we can follow the same sequence of steps we would when working from on our personal laptop/local.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I had worked on provisioning an Amazon S3 bucket in my <a href="https://skundunotes.com/2021/02/17/azure-pipelines-yaml-and-terraform-to-provision-aws-s3/" target="_blank" rel="noopener">previous two notes</a>, and so here, I provision an Amazon VPC and add two subnets in the exercise. As is good practice, I have a remote backend for the configuration.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I stored the Terraform configuration files along with the Azure-pipelines.yaml file. The flow in a yaml file is described below.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 1: Install Terraform</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I installed Terraform with the Terraform tool installer task because I have the build machine hosted in Azure pipelines. You need to have the Terraform extension installed for the Azure DevOps project to use this step.</span></span></span>
https://gist.github.com/kunduso/1fccf22ea04b8ce07691331c9beb08c5

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 2: Terraform init</span></span></span></strong>
https://gist.github.com/kunduso/4816eeecabb7c81553dec939b204119f
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Using Azure DevOps as an orchestrator, I provided the remote backend bucket to store the tfstate file, the key (tfstate file), and the user's region and credentials. Here the bucket (<em>skundu-terraform-remote-state</em>) and key (<em>tf/ADO-TF-VPC-Int/terraform.tfstate</em>) values are specific to the current Terraform configuration. The credentials (access key and secret key) are specific to the IAM user that is being used to provision the resources in AWS.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 3: Terraform validate</span></span></span></strong>
https://gist.github.com/kunduso/a614b8f61898d1af1446febaed159136
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The validate command with the " -json" flag displays the result of a syntatic and consistency check on the Terraform configuration files.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 4: Terraform plan</span></span></span></strong>
https://gist.github.com/kunduso/fb533998747779c78d78abe4ee0e79da
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Terraform plan creates an execution plan. The "-out application.tfplan" creates a file by that name which is then used in the next step to provision the resource.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 5: Terraform apply</span></span></span></strong>
https://gist.github.com/kunduso/ce873303449dc9446f191097f4f4d200
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Terraform apply is the last step in provisioning a resource and the "application.tfplan" generated in the previous step is referred here.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">And that brings us to the end of how to use Terraform, Azure DevOps, and Powershell to provision a resource in AWS. It is exciting that the terraform commands under the Powershell block in azure-pipelines.yaml run just as they would from the command line on our local laptop. Using Powershell to execute Terraform steps opened up many possibilities as I tried automating that using Azure DevOps. I hope you found this post useful.</span></span></span>
