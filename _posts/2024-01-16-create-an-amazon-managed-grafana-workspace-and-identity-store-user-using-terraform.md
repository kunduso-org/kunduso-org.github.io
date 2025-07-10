---
title: "Create an Amazon Managed Grafana workspace and Identity store user using Terraform"
date: 2024-01-16 14:47:26 +0000
categories: []
tags: []
---

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This note is an extension of my previous note on <a href="https://skundunotes.com/2022/11/12/create-an-amazon-managed-grafana-workspace-using-terraform/" target="_blank" rel="noopener">creating an Amazon Managed Grafana workspace</a> with one more resource added to the configuration. In my earlier note, there was a pre-requisite manual step to create the IAM Identity Center user before creating the Amazon Managed Grafana workspace. At that time, the AWS Terraform provider version <code>4.29.0</code> did not support that specific resource type. As of <span style="text-decoration: underline">January 2024</span>, the AWS Terraform provider version <code>5.32.0</code> supports that resource.</span></span></span>
<!--more-->
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">In this note, <span style="text-decoration: underline">I cover only that change</span>. Please refer to the previous note for the rest of the configuration (to create the Amazon Managed Grafana workspace). Here is the link to my <strong>GitHub repository</strong> with the Terraform code: <a href="https://github.com/kunduso/aws_managed_grafana_workspace_dashboard/tree/main/amg_workspace" target="_blank" rel="noopener">aws_managed_grafana_workspace_dashboard/amg_workspace.</a></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">As mentioned in my previous note, you must enable <span style="text-decoration: underline">"IAM Identity Center"</span> in your AWS account, where I have the details. The below Terraform code creates an <code>aws_identitystore_user</code> resource.</span></span></span>
<img class="alignnone size-full wp-image-3351" src="https://skundunotes.com/wp-content/uploads/2024/01/89-image-2.png" alt="89-image-2" width="1092" height="542" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Once the resource is created, the <code>user_id</code> property can be populated in the <code>aws_grafana_role_association</code> resource using the code below.</span></span></span>
<img class="alignnone size-full wp-image-3352" src="https://skundunotes.com/wp-content/uploads/2024/01/89-image-3.png" alt="89-image-3" width="796" height="164" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Previously, when the Identity store user was manually created, the <code>user_id</code> value was passed into the Terraform configuration. But now, with the AWS resource being supported by the AWS provider, there is no need to pass the absolute value.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">After Terraform provisioned all the resources, I logged into my AWS console and found the message below.</span></span></span>
<img class="alignnone size-full wp-image-3353" src="https://skundunotes.com/wp-content/uploads/2024/01/89-image-4.png" alt="89-image-4" width="860" height="112" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I sent the email verification link and configured a password for the user. Then, using the same username and password, I logged into the Amazon Managed Grafana workspace.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">If you used my Terraform configuration to provision the Amazon Managed Grafana workspace previously, try to add the above <code>aws_identitystore_user</code> resource. That will prevent you from manually keeping track of user management. Using the allowed roles in the link <a href="https://docs.aws.amazon.com/grafana/latest/APIReference/API_UpdateInstruction.html#ManagedGrafana-Type-UpdateInstruction-role" target="_blank" rel="noopener">managed-grafana-role</a> you can also manage users and their permissions using Terraform.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">And that brings us to the end of this note. Please see how easy user management with Amazon Managed Grafana workspace has become with the latest resource support.</span></span></span>
