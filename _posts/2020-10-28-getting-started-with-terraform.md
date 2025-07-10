---
title: "Getting started with Terraform"
date: 2020-10-28 16:31:30 +0000
categories: []
tags: []
---

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Provisioning and de-provisioning computing resources happen at a faster rate as the process of doing so become accessible. And cloud technologies like AWS and Azure have made it easy, cheap, and quick to do so. However, provisioning resources manually (although possible) is not scalable and also not efficient. Any organization pursuing its DevOps journey must focus on infrastructure-as-code (IAC) once their application CI/CD pipelines become robust and automated. Stated clearly, IAC is the infrastructure stored in a coded form that can be parsed by any compiler to provision (a) resource/resources. Terraform is one such tool that I started learning a little while back.</span></span></span>
<!--more-->
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">If you are interested, you may read up about IAC in a blog <a href="http://skundunotes.com/2020/01/03/what-is-infrastructure-as-code/" target="_blank" rel="noopener">I wrote a year back</a>.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Here are my running notes as I learn more about Terraform. I hope you find them useful.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Why Terraform?</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Terraform provides a platform to create/manage/destroy resources in cloud infrastructure. So if we want to create a virtual machine in AWS or Azure cloud, Terraform can be used.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Create a VPC in the cloud? Terraform can be used to do so.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Create storage? Create users? Yes.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Manage all these resources? Yes, Terraform can help us.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Delete/destroy them. Yup, Terraform again.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">But if I can do all these through the AWS or Azure UI, why use Terraform? I think two essential factors tilt the balance in Terraform's favor.</span></span></span>
<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Consistency:</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Being an IAC tool, Terraform brings the benefits associated with that principle. Namely, a consistent and repeatable approach to environment provisioning. The other associated services are the ability to automate deployments and testability of process, to name a few.</span></span></span>
<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Productivity:</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Managing one VPC or one VM is easy and quick. Managing ten VMs is not as fast as managing one is, but still doable in a reasonable time frame. Managing 20 or more manually is not fun anymore. It not only takes more time but also is error-prone since its manual. Terraform tackles both these concerns since it uses the same (coded) approach to manage all resources, and it does so in parallel. Terraform also supports the concepts associated with reusable modules -write once, use multiple.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">(I'm sure there are more benefits, but these are the ones that I could think of).</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">How does Terraform work?</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">After installation, we write terraform code (.tf files) on our local machine in HashiCorp Configuration Language (HCL). Terraform supports a plugin/provider-based approach. We use providers to assist us in provisioning and managing resources.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Generally speaking, the below image lists the steps that we'd follow while using terraform<img class="alignnone size-full wp-image-883" src="https://skundunotes.com/wp-content/uploads/2020/10/terraform-skundunotes-1.gif" alt="Terraform-skundunotes" width="853" height="480" /></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Below are some exciting concepts associated with Terraform that I came across:</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">terraform core/installer and terraform plugin:</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">There are two important units to terraform -the installer on our local machine and the providers that we communicate with over remote procedural calls to https://releases.hashicorp.com/</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Terraform references the providers in the .tf configuration files via plugin blocks</span></span></span>
<img class="alignnone size-full wp-image-863" src="https://skundunotes.com/wp-content/uploads/2020/10/45.-terraform-gs-image1.png" alt="45.-terraform-gs-image1" width="439" height="148" />

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">When we run <code>terraform init</code> Terraform scans the .tf configuration file and downloads any new providers plugin that is listed. If we added a new provider after running "terraform init" earlier, we need to run that again. </span></span></span><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Terraform stores these provider plugins in the .terraform\plugins\ folder.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">terraform (.tf) files:</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">These are the configuration files where we store our infrastructure in code, which typically will have one or more -$(LogicalFilename).tf, variables.tf, backend.tf (optional), terraform.tfvars (do not commit this to the repository; add to .gitignore)</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><em>$(LogicalFilename)</em> could be a Main.tf or an aws-ec2.tf or azure-vm.tf</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><em>variables.tf</em> is not mandatory. We can declare variables in the Main.tf file, but keeping them separate keeps the project modular.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><em>backend.tf</em> is the file where we state the location to store the terraform.state file. The default location is local (where Main.tf is stored).</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">state file:</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The terraform.state file is generated after we trigger the command "terraform plan". A state file stores the current state of the configuration that has been provisioned or deployed, or destroyed. When we make changes to an existing .tf configuration file and want to re-provision resources, it is compared with the current state file to identify the changes that have to be made. That way, terraform only provisions the resources that are changing to bring that to the desired state. This concept is known as idempotency.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">remote state and backend:</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The terraform.state file is generated as part of <code>terraform apply</code> command. The state file is in JSON format and stores the current state of the resources that are provisioned from the .tf configuration file. The terraform.state file is important in that regard and needs to be stored securely. Backend is the location where this state file is stored. The default location is the same directory where we have the .tf configuration file. However, if we add a backend.tf file to the folder and specify a location in it, that is where the terraform.state file will be stored. The backend is initialized as part of <code>terraform init</code> command.</span></span></span>
<img class="alignnone size-full wp-image-865" src="https://skundunotes.com/wp-content/uploads/2020/10/45.-terraform-gs-image2.png" alt="45.-terraform-gs-image2" width="571" height="174" />

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">storing terraform files in GitHub:</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">GitHub is the de facto standard when it comes to storing and sharing code. Terraform, being an infrastructure provisioning tool, requires credentials to be able to interact with the provisioners. These credentials "could" also be coded. However, nobody should share these credentials via accidental commit to a repository. I like to store such secure variables in the .tfvars (or .tfvars.json) file and then add that file to the .gitignore file so that git does not track that file and commit that to the repository. Also, the terraform.state or *.tfplan file should not be committed to the code repository.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Here is a <a href="https://github.com/github/gitignore/blob/master/Terraform.gitignore" target="_blank" rel="noopener">list of files</a> that should be in the .gitignore file.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Further reading:</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I found this <a href="https://www.youtube.com/watch?v=CRueD4fU0AI&amp;list=PLD7svyKaquTlE9dErhMazFhWbSSCfMP_4" target="_blank" rel="noopener">12 part youtube series</a> by Skylines Academy insightful and informative. </span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I have also gone through Ned Belavance's tutorial on Pluralsight  (<a href="https://www.pluralsight.com/courses/getting-started-terraform" target="_blank" rel="noopener">Terraform - Getting Started</a>). I highly recommend both these content.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Please do share your thoughts in the comment section. Also, please mention links to other helpful blogs on terraform that you have come across.</span></span></span>
