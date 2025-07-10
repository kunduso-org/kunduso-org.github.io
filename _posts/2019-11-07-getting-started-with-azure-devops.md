---
title: "Getting started with Azure DevOps"
date: 2019-11-07 19:41:03 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I have been working on TFS (aka Azure DevOps now) since 2008. Earlier as an SCM-Build engineer and from 2013 as an admin as well. Back in the days, I used TFS as a source code repository, build automation, and work item management tool predominantly and later with the addition of release management as an automated deployment tool (not for long though). Those who were fortunate enough to have used TFS RM when it was newly released would remember how difficult it was to use it. However, the opportunity was promising. Long story short and with more and more open-source development tools making inroads, Microsoft also bit the bullet (maybe a little late, according to me) and now we have Azure DevOps. So one of these days I thought of taking a look around to see what does it have to offer.</span></span></span>

<img class="alignnone size-full wp-image-152" src="https://skundunotes.com/wp-content/uploads/2019/11/ad-gettingstarted-image1-1.png" alt="AD-GettingStarted-Image1" width="897" height="484">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Start free? I mean, why not? All I needed was to log-in via my Microsoft account.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">After logging in, I was prompted to create an organization. What is an organization? I googled around and read a couple of docs on that. Basically, it's your support system to build your software. You get:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-access to a board to track your work items, and sprint</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-add a few people to your team if you want to (but not beyond 5 if you want it to be free),</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-link to your code repository -TFVC or Git</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-create build and release pipeline and such.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">More details here and here.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/organization-management?view=azure-devops</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">https://docs.microsoft.com/en-us/azure/devops/user-guide/plan-your-azure-devops-org-structure?view=azure-devops</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">If you are new to Azure DevOps, I think it is worth spending time and going through these links.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">After the organization, it was time to create a project. You need a project to keep all your work together, right?</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><img class="alignnone size-full wp-image-150" src="https://skundunotes.com/wp-content/uploads/2019/11/ad-gettingstarted-image2.png" alt="AD-GettingStarted-Image2" width="643" height="645"></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The options are quite descriptive and I selected what made sense to me.... and my empty project was ready!</span></span></span>
<img class="alignnone size-full wp-image-222" src="https://skundunotes.com/wp-content/uploads/2019/12/ad-gettingstarted-workitem-image1.png" alt="AD-GettingStarted-WorkItem-Image1" width="825" height="507">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As can be seen on the left, you get access to a pretty decent suite of products to get started. I've explored them in detail in the following subsequent posts.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2019/12/24/getting-started-with-azure-devops-work-items/" target="_blank" rel="noopener">Part 2: Getting started with Azure DevOps -work items</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/03/17/getting-started-with-azure-devops-add-a-repo/" target="_blank" rel="noopener">Part 3: Getting started with Azure DevOps -add a repo</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/02/19/getting-started-with-azure-devops-create-a-build-agent/" target="_blank" rel="noopener">Part 4: Getting started with Azure DevOps -create a build agent</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/05/15/getting-started-with-azure-devops-create-a-build-pipeline-part-1-yaml-pipeline/" target="_blank" rel="noopener">Part 5: Getting started with Azure DevOps -create a build pipeline -Part 1 (YAML pipeline)</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/05/15/getting-started-with-azure-devops-create-a-build-pipeline-part-2-classic-editor/" target="_blank" rel="noopener">Part 6: Getting started with Azure DevOps -create a build pipeline -Part 2 (Classic Editor)</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2019/12/27/azure-devops-deployment-groups/" target="_blank" rel="noopener">Part 7: Getting started with Azure DevOps -create a deployment group</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/05/22/getting-started-with-azure-devops-create-a-release-definition/" target="_blank" rel="noopener">Part 8: Getting started with Azure DevOps -create a release definition</a></span></span></span>
