---
title: "Create Terraform pre-requisites for AWS using AWS CLI in 3 easy steps"
date: 2021-04-03 21:25:26 +0000
categories: []
tags: []
---

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Generally speaking, when we work with Terraform to provision resources in AWS Cloud, we have a few pre-requisites. These are  -a remote backend to store the Terraform state file, a lock table, and IAM user credentials that Terraform will require to provision the resources. I say "generally speaking" because you can get away with the remote backend bit by pursuing the default (local) option. However, that is not recommended from a collaboration, automation, and security standpoint. Since this setting up of the prerequisites is a one-time activity, there is no need to automate that bit beyond a point. Yes, that is true that there will be multiple active Terraform configuration projects in an organization over some time. And in such a situation, having an automated process to configure the pre-requisites is a step in the right direction. <strong>Using the AWS CLI to create a backend -Amazon S3 bucket and an Amazon DynamoDB table and an AWS </strong></span></span></span><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>IAM user should be reasonably easy from a repeatability and consistency objective.</strong></span></span></span>
<!--more-->
<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 1: Install AWS CLI on your local and log-in.</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><a href="https://aws.amazon.com/cli/" target="_blank" rel="noopener">Download the installer</a> and install it on your local. Once done, open a new windows command prompt and run "aws configure". If this is a new setup, you will be requested an access key, secret access, etc. There is detailed information on that at <a href="https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html" target="_blank" rel="noopener">AWS CLI Configuration basics docs</a>.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 2(a): Create an Amazon S3 bucket</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The Terraform configuration uses an Amazon S3 bucket to store the remote terraform.tfstate file. There are two steps to this process - (a) create an Amazon S3 bucket and (b) encrypt the bucket.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">On the AWS authenticated command prompt, key in the following statement, where "skundu-terraform-remote-state-two" is the bucket's name. On successful completion, the below statement in {} appears. Please note that the bucket name has to be globally unique. More info at <span style="text-decoration: underline"><a href="https://docs.aws.amazon.com/cli/latest/reference/s3api/create-bucket.html" target="_blank" rel="noopener">aws-cli-create-bucket</a></span>.</span></span></span>

https://gist.github.com/kunduso/f575395837c5ae303291c48cb9edc6af

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">After creating the bucket, I added server-side encryption using the information available at <span style="text-decoration: underline"><a href="https://docs.aws.amazon.com/cli/latest/reference/s3api/put-bucket-encryption.html" target="_blank" rel="noopener">aws-cli-put-bucket-encryption</a></span>.</span></span></span>
https://gist.github.com/kunduso/1681cf3c766afd5611da9d14cddce01f

<em><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Note: If you struggled as I did with error messages then check out possible issues at <a href="https://stackoverflow.com/questions/60880646/error-while-enabling-server-side-encryption-policy-for-aws-s3-bucket-through-cli" target="_blank" rel="noopener">Stack-Overflow</a>.</span></span></span></em>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 2(b): Create an Amazon DynamoDB Table</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">A Terraform configuration uses the Amazon DynamoDB table to store the lock to prevent concurrent access to the terraform.tfstate file.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I followed the steps listed <a href="https://docs.aws.amazon.com/cli/latest/reference/dynamodb/create-table.html" target="_blank" rel="noopener">here</a> to create the command to use.
</span></span></span>
https://gist.github.com/kunduso/cc445a7d9dccc6f0e6dc2f10af927852
<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 3: Create an AWS IAM user with the required permissions</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Creating an AWS IAM user for Terraform configuration is a <strong>multipart process</strong> that comprises of the following:</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Create a policy from a JSON file.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Create a user.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Create an access key associated with the user.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Attach the policy to the user.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">All these steps are listed below.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Here is an example policy file to provide restricted access to Terraform to only communicate with the Amazon S3 and Amazon DynamoDB table.</span></span></span>
https://gist.github.com/kunduso/bf94f1aa5e683ed66539458a9a44138d
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I store the same in a backend-role-policy.json file and use it in the step below</span></span></span>
https://gist.github.com/kunduso/b9e8c47e46216dbf239818b6c45a165e

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">After executing the above steps, we will have access to a few required variables while working with Terraform configurations. <strong>These are the Amazon S3 bucket name and location, the Amazon DynamoDB table name, and the AWS IAM user's <code>access-key</code> and <code>secret-access</code>.</strong></span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">These values will be referred to in the backend.tf file and while executing "terraform init", "terraform plan", and "terraform destroy" steps.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Below is a descriptive example of a backend.tf configuration</span></span></span>
https://gist.github.com/kunduso/6e1dba046e7889ec08f6b931175757ae
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">As you can see in the code above, I replaced the values of $(BackendBucketName) with the name of the Amazon S3 bucket, $(BucketRegion) with the region where the bucket's region, and $(BackendLockTableName) with the DynamoDB table's name. The value in $(PathToTFStateFile) is relative to the s3 bucket. E.g., if we prefer to store the terraform.tfstate file at the root of the bucket, the value of $(PathToTFStateFile) will be "terraform.tfstate".</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Here is an example of the IAM access key used in "terraform init"</span></span></span>
https://gist.github.com/kunduso/4816eeecabb7c81553dec939b204119f
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Note: The above Terraform command belongs to an azure-pipelines.yaml file. I am using Powershell to run them.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">And that brings us to the end of creating AWS resources referred to in a Terraform project. I hope you found this post useful. Please let me know if you have any questions or suggestions. Until then, happy terraforming!</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Note Two:</strong> In the example above, the AWS IAM user has access only to the resources required to manage the backend. The AWS IAM user's access needs to be set accordingly depending on what resources need to be provisioned in a Terraform configuration. E.g., suppose the Terraform configuration is to create an Amazon VPC, subnet, route table, etc... <strong>In that case, the AWS IAM user will require additional permissions to manage those resources.</strong></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Note Three:</strong> make sure that the resources do not exist already in your AWS account. Also, log out of the AWS console, once the resources are created.</span></span></span>
