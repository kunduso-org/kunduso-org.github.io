---
title: "What is terraform import and why you too should know about it"
date: 2021-07-31 11:26:06 +0000
categories: []
tags: []
---

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">We want to do it right and do it right the first and every time, whether it is learning to play the guitar or to create software. Or whether it is creating a version-able, re-usable, repeatable, and testable approach to provisioning infrastructure on the cloud. AKA infrastructure as code.</span></span></span>
<!--more-->
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">And by the way, before you read any further, I am assuming that you understand how terraform and the AWS/Azure cloud infrastructure work together. To put it in one line, terraform configuration files contain information on the resources to provision in infrastructure providers such as AWS, Azure, etc.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This desire to provision resources using an IaC tool like terraform is fair and holds for most cases where infrastructure is being stood from the ground up. That is also known as a greenfield project -there was nothing, and then there was everything. However, there are times when infrastructure projects are not greenfield. There are times when infrastructure is partially or completely stood up using non-terraform based approach. And later, there might be a desire to convert that to terraform configuration to harness the benefits of IaC.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Well, no problem, I thought. I could quickly write up a terraform configuration project to provision resources. But when I started to execute the code (<code>terraform apply</code>), I got an error -the resource by that name exists. Hmm. <em>So I had a situation where cloud resources had already been provisioned using non-terraform based approach, and I was trying to find a way to create them using terraform.</em></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I could think of two options. <strong>Option A:</strong> I could delete the resource provisioned manually or provisioned using a non-terraform based approach. After deleting, I could execute the terraform configuration, and the resources would be provisioned correctly using the shiny new IaC approach. But what about situations where I could not or do not want to delete the cloud resource. It could be due to the effort that has already gone into creating the infrastructure, or it could be that the resources are currently in use and cannot be deleted. That brought me to <strong>Option B:</strong> <code>terraform import</code>. Using the command, I no longer had to destroy the cloud resource and instead could import their current state into the terraform state file of the configuration. Hashicorp has documented this well at <span style="text-decoration: underline"><a href="https://www.terraform.io/docs/cli/import/index.html" target="_blank" rel="noopener">Terraform import</a></span>. The link to the <span style="text-decoration: underline"><a href="https://www.terraform.io/docs/cli/import/usage.html" target="_blank" rel="noopener">usage of this command</a></span> is also helpful.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">How did I go about using terraform import?</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">As suggested in the usage doc (link shared above), I created a terraform configuration for the cloud resources I was importing, and then I ran the <code>terraform plan</code> command. I received the output that terraform would create all the resources specified in the terraform configuration. Well, that was because terraform did not know about the current state of infrastructure. Then I executed the <code>terraform import</code> command.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Note: Are you familiar with the <a href="https://registry.terraform.io/providers/hashicorp/aws/latest/docs" target="_blank" rel="noopener"><span style="text-decoration: underline">Terraform registry for AWS</span></a>? There is a <strong><code>import</code></strong> block for each resource type (scroll to the bottom of the page). The import command example displays how to uniquely identify a resource in the cloud infrastructure, e.g., name or id, and import that information into a terraform state.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">On successful execution of the <code>terraform import</code> command, I ran the <code>terraform plan</code> command once again. And this time, I got the message: <code>No changes. Infrastructure is up-to-date</code>. That is the desired end state of any terraform import project. <span style="text-decoration: underline">That meant all existing cloud resources for which I had written terraform configuration have been imported into the terraform state file.</span></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I'll explain that via an example I worked on. I have an AWS account where I manually created the following cloud resources: an Amazon VPC, public subnets, a route table, route table associations, and an internet gateway. And as I mentioned earlier, the task was to import them into terraform. So how did I go about that?</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I classified the approach into six high-level steps as follows.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 1:</strong> Create a list of all the resources that have to be imported. These resources already exist.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">In the above example, there were seven resources - an Amazon VPC, a couple of subnets, a route table, an internet gateway, and a couple of route table associations with the two subnets.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 2:</strong> Create a terraform configuration block for each resource.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I have the code checked in at my GitHub repository <a href="https://github.com/kunduso/terraform-import-aws-vpc" target="_blank" rel="noopener">terraform-import-aws-vpc</a></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 3:</strong> Run <code>terraform plan</code>. The output would include all the resources that terraform would have to provision.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">After executing <code>terraform plan</code> on the example above, terraform listed all the seven resources to create.</span></span></span>
<img class="alignnone size-full wp-image-1381" src="https://skundunotes.com/wp-content/uploads/2021/07/46.-image-2.png" alt="46. Image-2" width="356" height="44" />

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Note: If interested, open the <code>terraform.tfstate</code> file and check that out. That would be pretty much empty since, as per terraform these resources do not exist yet. Close the state file because it will be referenced in the next step.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 4:</strong> Run <code>terraform import</code> on one resource.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">If successful, terraform will update the state file accordingly. Open the <code>terraform.tfstate</code> file again and review if the resource state has been imported.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I imported the vpc using the <code>terraform import</code> command. I have an image of that, along with the output I received.</span></span></span>
<img class="alignnone size-full wp-image-1382" src="https://skundunotes.com/wp-content/uploads/2021/07/46.-image-3.png" alt="46. Image-3" width="710" height="173" />

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 5:</strong> Run <code>terraform plan</code> again.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This time the resource count should go down because the state file associated with the current terraform configuration is aware of the resource that was imported. And it did when I ran <code>terraform plan</code> again.</span></span></span>

<img class="alignnone size-full wp-image-1383" src="https://skundunotes.com/wp-content/uploads/2021/07/46.-image-4.png" alt="46. Image-4" width="363" height="30" />

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 6:</strong> Repeat step 4 for the rest of the resources until all the resources are imported.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Once all the resources are imported, the last run of <code>terraform plan</code> would display - <code>No changes. Infrastructure is up-to-date</code>. </span></span></span>

<img class="alignnone size-full wp-image-1384" src="https://skundunotes.com/wp-content/uploads/2021/07/46.-image-5.png" alt="46. Image-5" width="672" height="241" />

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I could also run a <code>terraform apply</code> that would not change anything since the terraform configuration, the state file, and the existing cloud infrastructure are all identical.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><span style="text-decoration: underline">I noted that terraform deleted none of the cloud resources -their ids remained the same.</span> Multiple runs of <code>terraform plan</code> and <code>terraform apply</code> resulted in no change -idempotent.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Once all the vpc changes were imported into a terraform configuration state file, I could manage (add, modify, remove) them using terraform. I also started realizing the benefits of infrastructure-as-code, namely, the consistent end state of infrastructure, re-usable modules, testable, version-controlled code even though, to begin with, these resources were not provisioned by terraform.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">And that is how I learned about <code>terraform import</code>. I hope you found this note helpful. Please let me know if there are any question or suggestions.</span></span></span>
