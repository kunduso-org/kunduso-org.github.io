---
title: "Create Azure DevOps iterations using PowerShell"
date: 2021-12-26 09:02:00 +0000
categories: []
tags: []
---

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I start planning for the following year in the last two weeks of December. I use Azure DevOps to track my work  -break down goals into features, and features into user stories, which I map into sprints. Creating iterations and adding them to projects is something that anyone can automate. I learned that from <span style="text-decoration: underline"><a href="https://vinijmoura.medium.com/how-to-create-all-iterations-to-your-project-on-azure-devops-cedadb045705" target="_blank" rel="noopener">Vinicius Moura's blog -How to: Create all iterations to your project on Azure DevOps.</a></span></span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Using that approach, I created a year's worth of iterations and added them to my project. In this note, I document my changes for my specific requirement.</span></span></span>
<!--more-->
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Update Dec 2022:</strong> I automated this process using Azure DevOps. You may read about that at <a href="https://skundunotes.com/2022/12/29/automate-azure-boards-iteration-using-powershell-and-azure-pipelines/" target="_blank" rel="noopener">automate-iterations-in-Azure-Board-using-Azure-pipelines</a>.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Pre-requisite:</strong> It is necessary to have an Azure DevOps organization, a project to host the iterations, and admin credentials of the project to generate PAT.</span></span></span>
<em><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Note: I ran this script from my local laptop and did not attempt to create an Azure DevOps pipeline for that since I had to do it only once.</span></span></span></em>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I classified this use case into five steps:</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 1:</strong> Install Azure DevOps CLI</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 2:</strong> Create Azure DevOps PAT</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 3:</strong> Identify the start date, number of days in each iteration, and number of iterations to create and add to the team</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 4:</strong> Create and add iterations to the team</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 5:</strong> Review iteration and revoke PAT</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I stored the PowerShell script in my GitHub repo: <span style="text-decoration: underline"><a href="https://github.com/kunduso/add-iterations-to-azure-devops-project" target="_blank" rel="noopener">add-iterations-to-azure-devops-project</a>.</span></span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 1: Install Azure DevOps CLI</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The details on the installation are available at <span style="text-decoration: underline"><a href="https://github.com/Azure/azure-devops-cli-extension" target="_blank" rel="noopener">azure-devops-cli-extension</a></span>.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 2: Create Azure DevOps PAT</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Please check the following note on <span style="text-decoration: underline"><a href="https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&amp;tabs=preview-page#create-a-pat" target="_blank" rel="noopener">creating a PAT.</a></span> I selected <code>Read &amp; write</code> for the Work Items under the Scope of the PAT.</span></span></span>
<img class="alignnone size-full wp-image-1678" src="https://skundunotes.com/wp-content/uploads/2021/12/60-image-2.png" alt="60-image-2" width="477" height="103" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I copied the PAT value and stored it securely because that won't be accessible later.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 3: Identify the start date, number of days in each iteration, and number of iterations to create and add to the team</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I use Azure DevOps regularly, and the last sprint of 2021 ends on the 7th of Jan, 2022. Hence, the start date of the first sprint in 2022 is the 8th of Jan 2022. I use a two-week sprint cadence; thus, the last date of the sprint is <code>$StartDateIteration.AddDays(13)</code>, which adds 13 more days to the start date of the sprint. Moreover, since I need iterations for the whole year, I selected the number of iterations as 26 (52 weeks in a year and 2-week sprints).</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 4: Create and add iterations to the project</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I like arranging the iterations such that it is easily accessible. Hence I added the <code> --path $RootPath</code> flag to the <code>az boards iteration project create</code> command. The <code>$RootPath</code> value is the year, which is <code>2022</code> for the current use case.
After creating the iterations, I added them to the project team. Here is an image of the first three iterations.</span></span></span>
<img class="alignnone size-full wp-image-1680" src="https://skundunotes.com/wp-content/uploads/2021/12/60-image-3.png" alt="60-image-3" width="500" height="200" />
<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 5: Review iteration and revoke PAT</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I triggered the PowerShell script, and it created the iterations and added them to the team project. I quickly reviewed and found that the iterations were created correctly and added to the team. Since my use case was complete, I revoked the PAT. Here is the MS Docs article on <span style="text-decoration: underline"><a href="https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&amp;tabs=preview-page#revoke-a-pat" target="_blank" rel="noopener">how to revoke PAT.</a></span></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Last year, it took me over an hour to create the iterations and add them to the project, and I did not enjoy the process. This year, it took me the same time to understand the code, make tweaks to address my specific requirement, and run the code to auto-create the iterations and add them to the project team. However, I enjoyed the process this time, and from next year onwards, this automation should save me time.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">If you manually manage iterations in Azure DevOps, I recommend you look into this automated process. I've tried to be descriptive in this note, and if you have questions, please use the comments section to ask.</span></span></span>
