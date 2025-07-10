---
title: "Migrate code from TFVC to Azure Repos"
date: 2020-04-23 03:53:15 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Over the last few years organizations are moving closer to their customers in order to remain relevant and useful. This helps them understand their customer better which in turn allows them to serve their customers well. Moving closer to customers becomes effective with capability to deliver value quicker. This is what lead to organizations adopting devops best practices (among other like lean, agile etc).</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Devops best practices are enabled by software development tools which allow an organization to be able to deliver value as frequently (more) and as quickly(fast) to end customers. One such tool that has become the de-facto standard of developers is git and its numerous hosting platforms like Github, Azure Repos (part of AzureDevops), bitbucket etc.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Newer organizations find it easy to setup their repos using git. But how does an organization using Microsoft TFS (TFVC) as their code repo harness the benefits of using git? How do they move their code from TFVC to git?</span></span></span>
<!--more-->
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The most common (and to an extend logical) solution is to leave history behind in TFVC and move tip (latest changeset) to git and start creating history from that point on. But what if Azure Repos provides an option to move history (maximum up to 180 days back) from TFVC to Azure Repos?</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I moved a few TFVC repositories to Azure Repos and came across a few interesting ideas on what can and what cannot be moved. All in all, I think it’s a good option to be aware of, if there arises a requirement to do so.</span></span></span>
<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 1: Create an empty repository in Azure Devops</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Make sure you do not add a Read Me or a git .ignore file because if you do so, your repo gets initiated and there is not an easy way to use the import functionality once the repo is initiated.</span></span></span>
<img class="alignnone size-full wp-image-495" src="https://skundunotes.com/wp-content/uploads/2020/04/20.-mtar-image1.png" alt="20. MTAR-Image1" width="504" height="469">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Add them once code has been migrated from TFVC.</span></span></span>
<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 2: Add some code</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Once our repo is ready we are greeted by the customary welcome note of an empty repo.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Select the third option: import a repository</span></span></span>
<img class="alignnone size-full wp-image-496" src="https://skundunotes.com/wp-content/uploads/2020/04/20.-mtar-image2.png" alt="20. MTAR-Image2" width="659" height="336">
<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 3: Git or TFVC</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Under the import option we have a choice -Git or TFVC. Select TFVC</span></span></span>
<img class="alignnone size-full wp-image-497" src="https://skundunotes.com/wp-content/uploads/2020/04/20.-mtar-image3.png" alt="20. MTAR-Image3" width="555" height="393">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Note:</strong> this path has to be a project hosted in TFS in the same collection.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The migrate history option allows to take a maximum of 180 days history from TFVC -&gt;Azure Devops.</span></span></span>
<img class="alignnone size-full wp-image-498" src="https://skundunotes.com/wp-content/uploads/2020/04/20.-mtar-image4.png" alt="20. MTAR-Image4" width="552" height="502">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Also note that one branch in TFVC is one repo. This means that if you have a parent folder and inside that three TFVC repos like Dev, Test and Release where Dev and Release are branched out of Test then you won’t be able to migrate that branch relationship (parent-child) into Git. Contents of only one branch will be consumed by one repo.</span></span></span>
<img class="alignnone size-full wp-image-499" src="https://skundunotes.com/wp-content/uploads/2020/04/20.-mtar-image5.png" alt="20. MTAR-Image5" width="554" height="167">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I also noticed that Microsoft does not recommend taking history from TFVC but just the tip (latest changeset).</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This transfer request took a few minutes and once it was done I could see history form TFC in Azure Repos. The first commit had a reference to the source of this repo in TFVC; the location from where it was imported among with the changeset# in the commit message. After that, the rest of the changesets of TFVC were converted to commit# with the same author, commit date and comment. I however did not find any work items being attached to the commits that were attached with the original changeset in TFVC.</span></span></span>
<img class="alignnone size-full wp-image-500" src="https://skundunotes.com/wp-content/uploads/2020/04/20.-mtar-image6.png" alt="20. MTAR-Image6" width="621" height="411">
<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">A few points to ponder:</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-one branch in TFVC can be converted to one repo in Azure Repos</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-attached work items of TFVC changesets do not carry over to commits of Azure Repos</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-branch relationship in TFVC do not carry over to Azure Repos</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-code in TFVC can continue to live side by side the migrated code to Azure Repos; development can continue in both repos (TFVC and Azure Repos) but these are not visible to each other -no relationship</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">On the overall this is a good feature for a project that has code (with history) in TFVC and wants to start using Azure Repos by migrating code with history.</span></span></span>
