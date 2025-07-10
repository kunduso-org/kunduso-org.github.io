---
title: "Authenticating Terraform to Azure using Service Principal"
date: 2020-11-03 20:27:34 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Continuing on my journey to learn Terraform, I wanted to explore the idea of authenticating Terraform to Azure. Terraform, as we know, is an infrastructure automation tool, and this authentication technique allows us to create/manage resources on the Azure cloud platform. I came across two insightful articles on Azure Service Principals that helped me understand the how's and what's of the service principal. Here are the links to those -<a href="https://nedinthecloud.com/2019/07/16/demystifying-azure-ad-service-principals/">Ned Belavance's Demystifying Azure AD Service Principals</a> and <a href="https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli" target="_blank" rel="noreferrer noopener">Microsoft Docs</a></span></span></span>
<!--more-->
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Following the instructions there, I identified three steps to the objective.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 1: Create a Service Principal</strong></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Here is code of the service principal provisioning command I ran on Azure portal command prompt:</span></span></span>
https://gist.github.com/kunduso/580b0c13875f1d841f252fe482d5db41
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">With these values in hand, it was now time to head over to Terraform and provide those credentials for Terraform to be able to access my Azure subscription.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 2: Update terraform configuration files</strong></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I followed the instructions <a href="https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/guides/service_principal_client_secret" target="_blank" rel="noreferrer noopener">here to create the Azure provider usage and authentication</a>.</span></span></span>
https://gist.github.com/kunduso/915f67b1a20233d36c88294a1209d2d0
https://gist.github.com/kunduso/a301d2616e7eede2cd33b30a60f139fa
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The documentation was precise on what values were required. I also know that these are secured credentials and that they need to be managed carefully. I came across an approach to declare variables in a variables.tf file and place actual values in a .tfvar</span></span></span>
https://gist.github.com/kunduso/bae6d4688c646776afd901ae5785b197
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 3: Execute terraform trio commands (init -&gt; plan -&gt; apply)</strong></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">At the end of <code>terraform apply</code> I was able to verify that a resource group was created under my subscription on the Azure portal.</span></span></span>
<img class="alignnone size-full wp-image-921" src="https://skundunotes.com/wp-content/uploads/2020/11/terraform-azure-rg.png" alt="terraform-azure-rg" width="751" height="357">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Conclusion:</strong>The purpose of this note was to authenticate Terraform, and we saw that with the creation of a resource group in Azure.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Other ideas to explore:</strong>Is this the best method to be able to authenticate Terraform? How to authenticate Terraform to AWS using an IAM user?</span></span></span>
