---
title: "Azure DevOps to Amazon EC2 instance in 4 steps"
date: 2020-09-10 11:34:04 +0000
categories: []
tags: []
---

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">After having completed the first few articles on "Getting started with Azure DevOps", I thought of working on an interesting use case. The objective I set for myself was to create a CI/CD pipeline to deploy Azure DevOps build artifacts to Amazon EC2 instances. Later on, I realized that the timing of the use case couldn't have been better as we were able to utilize the experience.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">In the process of working on the use case, I took notes of what I learned, and I share them here.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Note: if you are new to Azure DevOps, I'd suggest reading through the 8-part series on <a href="https://skundunotes.com/2019/11/07/getting-started-with-azure-devops/">Getting started with Azure DevOps</a>. I borrow heavily from the concepts discussed in those notes.</span></span></span><!--more-->

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Broadly, I can classify this use case into 4 steps:</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 1: Create a build definition and build artifact in Azure DevOps</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 2: Create an Amazon EC2 instance in AWS</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 3: Add an Amazon EC2 instance to a deployment group in Azure DevOps</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 4: Create a release definition to deploy build artifact to the Amazon EC2 instance in a deployment group</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 1: Create a build definition and build artifact</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">A build definition creates build artifacts that can be used to deploy to a resource (in this case, an Amazon EC2 instance). In Azure DevOps, there are two ways to create a build definition. I have two detailed articles on this at <a href="http://skundunotes.com/2020/05/15/getting-started-with-azure-devops-create-a-build-pipeline-part-1-yaml-pipeline/" target="_blank" rel="noopener">Getting started with Azure DevOps -create a build pipeline -Part 1 (YAML pipeline)</a> and <a href="http://skundunotes.com/2020/05/15/getting-started-with-azure-devops-create-a-build-pipeline-part-2-classic-editor/" target="_blank" rel="noopener">Getting started with Azure DevOps -create a build pipeline -Part 2 (Classic Editor)</a></span></span></span>
<em><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Please note: Although the reference articles state that they are part of an 8-part series, you do not need to go through each to understand how to create a build definition.</span></span></span></em>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 2: Create an Amazon EC2 instance in AWS</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Amazon Elastic Compute Cloud (EC2) is Amazon web services' way of identifying virtual machines. I created a Windows virtual machine with a key pair so that I was able to log in. I did not specify any particular security group and went ahead with the default one. Here is a link to <a href="https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/get-set-up-for-amazon-ec2.html">AWS documentation</a>.</span></span></span>
<em><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Note: Any virtual machines provisioned in cloud infrastructure must have proper security to protect from malicious access. The default security group does not provide that protection.</span></span></span></em>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 3: Add an Amazon EC2 instance to a deployment group</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">A deployment group is a collection of machines with the same lifecycle -all virtual machines in an environment can belong to the same deployment group.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This step too is covered in detail in the following article <a href="https://skundunotes.com/2019/12/27/azure-devops-deployment-groups/" target="_blank" rel="noopener">Getting started with Azure DevOps -create a deployment group</a>.</span></span></span>
<em><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Please note: As stated in the previous note, you do not have to go through all 8 articles to understand how to add a VM to a deployment group.</span></span></span></em>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 4: Create a release definition to deploy build artifact to the Amazon EC2 instance in a deployment group</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">A release definition is the last part of this chain wherein the build artifacts are deployed to the VMs of a deployment group which is the Amazon EC2 instance that we added in the previous step. This step, too (sorry for sounding like a broken record on this post), is covered in detail in the following article <a href="https://skundunotes.com/2020/05/22/getting-started-with-azure-devops-create-a-release-definition/" target="_blank" rel="noopener">Getting started with Azure DevOps -create a release definition</a>.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Conclusion:</strong> Having followed the above steps extensively at work (private cloud), I must say that deploying to an Amazon EC2 instance (public cloud) was not a novelty or any different than deploying to a VM on-premise (private cloud). My doubts were whether Azure DevOps would be able to communicate with an instance in a separate virtual cloud. With appropriate security groups in place, it was not too difficult.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">If you are new and want to attempt this, I highly recommend you share your findings. If you have any questions, please do not hesitate to ask. It'll be an opportunity for both of us to learn something new.</span></span></span>
