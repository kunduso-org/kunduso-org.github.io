---
title: "Authenticating Terraform to AWS using IAM user"
date: 2020-11-04 17:09:38 +0000
categories: []
tags: []
---

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">In my previous note, I mentioned the <a href="http://skundunotes.com/2020/11/03/authenticating-terraform-to-azure-using-service-principal/" target="_blank" rel="noopener">steps to authenticate Azure</a>. In this note, I'll list the steps to authenticate to AWS. The approach will be pretty similar -we create an IAM user with appropriate policies, create/update terraform configuration files, and run the configuration files.</span></span></span>
<!--more-->
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 1: Create an IAM user</strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">To work with resources in AWS, we need appropriate access -read/modify. In this case, we need an IAM user with programmatic access permission (full access) to Amazon S3. Please attach the appropriate policy (AmazonS3FullAccess) and store the Access key ID and Secret Access key securely. We need those in the next step.</span></span></span></span></span></span>

<img class="alignnone size-full wp-image-941" src="https://skundunotes.com/wp-content/uploads/2020/11/terraform-aws-image2.png" alt="Terraform-AWS-Image2" width="926" height="243" />

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 2: Update terraform configuration files</strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I followed the instructions <a href="https://registry.terraform.io/providers/hashicorp/aws/latest/docs" target="_blank" rel="noopener">here</a> to create the AWS provider usage, authentication, and the instructions to create an Amazon S3 bucket were <a href="https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket" target="_blank" rel="noopener">provided here</a>. As stated in my previous note, the secured credentials (access_key and secret_key) are stored in a .tfvars file. This .tfvars file should not be added to the repository (update .gitignore accordingly).</span></span></span></span></span></span>
https://gist.github.com/kunduso/91c4a8eb850e9ba9d4e63263f5e51511
https://gist.github.com/kunduso/7416b5da596e93d567fc637c60e5b6a5
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 3: Execute terraform trial commands (init -&gt; plan -&gt; apply)</strong>
</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><img class="alignnone size-full wp-image-943" src="https://skundunotes.com/wp-content/uploads/2020/11/terraform-aws-image4.png" alt="Terraform-AWS-Image4" width="557" height="125" /></span></span></span></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">After <code>terraform apply</code> I was able to verify that an Amazon S3 bucket was created under my AWS profile using Terraform configuration files.
<img class="alignnone size-full wp-image-942" src="https://skundunotes.com/wp-content/uploads/2020/11/terraform-aws-image3.png" alt="Terraform-AWS-Image3" width="1056" height="339" />
</span></span></span></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Conclusion:</strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The purpose of this note was to authenticate Terraform, and we saw that with the creation of the bucket in Amazon S3.
</span></span></span></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Other ideas to explore:</strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Is this the best method to authenticate Terraform?
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">How to provision an EC2 instance in AWS using Terraform?</span></span></span></span></span></span></span></span></span></span></span></span></span></span></span>
