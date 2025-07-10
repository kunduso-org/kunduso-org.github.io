---
title: "CI/CD using Terraform and Azure Pipelines -ideation"
date: 2021-06-18 11:02:29 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">After writing a few notes on "Azure DevOps and Terraform," I thought of exploring the idea of integrating Azure DevOps and Terraform a little further.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Generally speaking, in Azure Pipelines (classic editor), a build definition (pipeline in Azure DevOps services) is used to compile and package an artifact. Then, under Releases, we have a release definition associated with the build definition used to deploy the artifacts across different environments like -Dev, Test, Stage, and Prod. These environments consist of different stages and can be mapped to separate deployment groups. The Azure DevOps pipeline enables us to build once and deploy multiple -across environments.</span></span></span>
<!--more-->
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Note: I have a <a href="https://skundunotes.com/2020/05/15/getting-started-with-azure-devops-create-a-build-pipeline-part-2-classic-editor/"><span style="text-decoration:underline;"><strong>separate note</strong></span></a> where I write in detail about Pipelines and their components.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Terraform, as we know, is an infrastructure automation tool that follows the sequence of <code>init -&gt; validate -&gt; plan -&gt; apply</code> (and if necessary, <code>destroy</code> too). So the idea was to use both Azure DevOps and Terraform to automate infrastructure provisioning across environments.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">That meant that I had to map the Terraform commands into Azure pipelines build definition and release definition. In a traditional CI/CD approach, we compile and create the package in CI (continuous integration). However, in the case of Terraform, there is no code to compile. And then, in CD (continuous deploy), we apply the package to an environment. I realize that terraform plan and terraform apply belong to CD because they are environment specific and will vary from environment to environment. And hence, by the rule of elimination, <code>terraform init</code> and <code>terraform validate</code> belong to CI. There is no hard and fast rule as such. I attempt to retrofit terraform workflow with Azure DevOps.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In the case of an Azure DevOps build definition, the workflow would look something like below:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-checkout files from code repo,</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-run <code>terraform init</code>, which downloads all dependencies and initializes the backend to store the remote state. It is at this stage that we also provide credentials (AWS or Azure) to be able to communicate with the remote state</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-run <code>terraform validate</code>, which checks for correctness of syntax,</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-create a package with the .terraform folder and the .tf files in it.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The package is then ready to have <code>terraform plan</code> and <code>terraform apply</code> run on them, which belongs to the continuous deploy step defined in a release definition.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">So the release definition workflow would look like this:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-download the package from the associated build definition,</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-run <code>terraform plan</code></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-run <code>terraform apply</code></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Conceptually, this would work but has certain limitations.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The idea of a CI/CD pipeline is to build once-deploy multiple. However, a terraform configuration has a remote state where terraform stores the state file of changes applied to an environment. Terraform stores the remote state configuration value in a backend.tf file, and that file cannot have any variables.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Here is an example of a backend.tf file in case of AWS:</span></span></span>
https://gist.github.com/kunduso/6e1dba046e7889ec08f6b931175757ae#file-backend-tf
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Effectively, there is an S3 bucket and a key, and these cannot be variables.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">And here is an example of a backend.tf file in case of Azure:</span></span></span>
https://gist.github.com/kunduso/8452cbd4f83f716dfd9f032f86a68473

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Let me explain through an example. Say I have a terraform configuration to provision a virtual machine in AWS. The requirement is to provision the VM in the following four environments: Dev, Test, Stage, and Prod. Generally, it is a good security practice to map environments to separate AWS accounts. And when it comes to a CI/CD pipeline, the changes need to flow from environment to environment. Hence, a typical Azure DevOps CI/CD pipeline can have a workflow like: deploy to Dev environment -&gt; validate the change in Dev environment-&gt;deploy to Test environment-&gt; validate the change in Test environment -&gt;deploy to Stage environment -&gt; validate the change in Stage environment-&gt;deploy to Prod environment-&gt; validate the change in Prod environment. By having separate stages, we can protect a higher environment from having un-validated changes introduced.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In such a scenario, how do we manage to apply the Terraform configurations across different environments?</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">And as I had mentioned, we cannot have variables in the backend.tf, which implies that we cannot extend the remote state such that for the Dev environment, the remote state is such, and for Test, it is such. There can be only one value there.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Hashicorp had thought of this, and they introduced the concept of <strong>workspaces</strong>. A terraform workspace allows the backend to extend such that we can use it to store the remote state of different environments.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I hope this note helped you understand the limitation that I presented. Please read <span style="text-decoration:underline;"><a href="https://skundunotes.com/2021/06/19/terraform-workspace-with-multiple-aws-accounts/" target="_blank" rel="noopener">my following note where I do a deep dive on terraform workspace.</a></span> I then implement the idea discussed using YAML-based Azure pipelines and Powershell in the next note titled  -<span style="text-decoration:underline;"><a href="https://skundunotes.com/2021/07/10/ci-cd-of-terraform-workspace-with-yaml-based-azure-pipelines/" target="_blank" rel="noopener">CI/CD of Terraform workspace with YAML based Azure Pipelines</a>.</span></span></span></span>
