---
title: "Create VPC Flow logs to publish to Amazon CloudWatch Logs using Terraform"
date: 2024-11-20 15:41:42 +0000
categories: []
tags: []
---

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">VPC Flow Logs is a feature in Amazon Web Services (AWS) that enables capturing information about IP traffic going to and from network interfaces in the Virtual Private Cloud (VPC). These logs provide detailed visibility into network traffic, helping to monitor, troubleshoot, and analyze traffic patterns, security issues, and performance within the VPC. The logs can be stored in Amazon CloudWatch Logs or Amazon S3 for further analysis.</span></span></span>
<!--more--><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">To enable VPC Flow Logs, an IAM role is required to publish the log data to Amazon CloudWatch Logs or Amazon S3. This role must have specific permissions, typically including <code style="background-color: #dcdcdc;font-size: 15px;color: #000000">logs:CreateLogGroup</code>, <code style="background-color: #dcdcdc;font-size: 15px;color: #000000">logs:CreateLogStream</code>, and <code style="background-color: #dcdcdc;font-size: 15px;color: #000000">logs:PutLogEvents</code> for CloudWatch Logs or appropriate S3 permissions if logs are stored in an S3 bucket.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I describe enabling flow logs for Amazon VPC using Terraform in this note. If you are interested, please refer to my GitHub repository: <a href="https://github.com/kunduso/terraform-aws-vpc" target="_blank" rel="noopener"><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #0000ff">kunduso/terraform-aws-vpc</span></span></span></a>.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Enabling flow logs for an Amazon VPC <strong>involves four steps:</strong></span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>1.</strong> Create an IAM role to publish flow logs to CloudWatch Logs</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>2.</strong> Create a KMS Key to encrypt the CloudWatch Log Group</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>3.</strong> Create an Amazon CloudWatch Logs group</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>4.</strong> Attach the CloudWatch Logs group to the VPC to capture flow logs</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Let me now explain each step in detail.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 1: Create an IAM role to publish flow logs to CloudWatch Logs</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">An IAM role has two policies: (i) the trust (also known as the assume) policy and (ii) the permission policy.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The Trust Policy (also called an Assume Role Policy) is the policy attached to an IAM role to define the trusted entities that can assume the role. The below trust policy states that the service principal, <code style="background-color: #dcdcdc;font-size: 15px;color: #000000">vpc-flow-logs.amazonaws.com</code>, can assume this role, which is the AWS service responsible for VPC Flow Logs.</span></span></span>
<img class="alignnone size-full wp-image-5088" src="https://skundunotes.com/wp-content/uploads/2024/11/105-image-1.png" alt="105-image-1" width="821" height="537" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Apart from the trust policy, an IAM role requires a permission policy. The below permission policy provides a list of capabilities that enable the IAM role to publish logs to AWS CloudWatch Logs.</span></span></span>
<img class="alignnone size-full wp-image-5089" src="https://skundunotes.com/wp-content/uploads/2024/11/105-image-2.png" alt="105-image-2" width="987" height="657" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">For more information on this concept, please choose  <a href="https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-iam-role.html" target="_blank" rel="noopener"><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #0000ff">-flow-logs-iam-role</span></span></span></a>.</span></span></span>
<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 2: Create a KMS Key to encrypt the CloudWatch Log Group</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The AWS KMS Key encrypts the data stored in the AWS CloudWatch Logs. I provisioned a tighter KMS key policy specifically for this use case using the <code style="background-color: #dcdcdc;font-size: 15px;color: #000000">aws_kms_key_policy</code>. Please note that adding the AWS KMS key, although optional, helps create a secure solution.</span></span></span>
<img class="alignnone size-full wp-image-5090" src="https://skundunotes.com/wp-content/uploads/2024/11/105-image-3.png" alt="105-image-3" width="1171" height="685" />
<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 3: Create an Amazon CloudWatch Logs group</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The logs group stores log data that captures detailed information about the IP traffic going to and from your VPC's network interfaces.</span></span></span>
<img class="alignnone size-full wp-image-5091" src="https://skundunotes.com/wp-content/uploads/2024/11/105-image-4.png" alt="105-image-4" width="890" height="178" />
<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 4: Attach the CloudWatch Log group to the VPC for flow logs</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The last step is to attach the Amazon CloudWatch Logs group to the VPC flow logs via the below Terraform code.</span></span></span>
<img class="alignnone size-full wp-image-5092" src="https://skundunotes.com/wp-content/uploads/2024/11/105-image-5.png" alt="105-image-5" width="1011" height="365" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The resource properties include the VPC ID, the CloudWatch Logs group, and the IAM role to enable sending the logs.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Once Terraform provisioned all the AWS cloud resources, I logged into my AWS account and navigated to VPC → selected the specific VPC → Flow Logs and examined the VPC Flow Logs property.</span></span></span>
<img class="alignnone size-full wp-image-5093" src="https://skundunotes.com/wp-content/uploads/2024/11/105-image-6.png" alt="105-image-6" width="1017" height="604" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">And that concludes this note. If you have any questions or suggestions, please do not hesitate to contact me via the comments below.</span></span></span>
