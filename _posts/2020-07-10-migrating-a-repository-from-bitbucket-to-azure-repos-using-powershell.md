---
title: "Migrating a repository from bitbucket to Azure Repos -using powershell"
date: 2020-07-10 11:02:01 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Picking up from where I left in the&nbsp;</span></span></span><span style="color:#000000;font-family:calibri;font-size:18px;"><a href="http://skundunotes.com/2020/07/10/migrating-a-repository-from-bitbucket-to-azure-repos-ui-based">previous note</a></span><span style="color:#000000;font-family:calibri;font-size:18px;">, here are the steps involved in migrating a code repository from bitbucket to Azure Repos using powershell.</span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I’m going to spend some time noting how bitbucket team, project and repos are related to each other. <strong>If you want to go the code directly, here is the <a href="https://github.com/kunduso/MigrateToAzureRepos">link to the github repository</a>.</strong> However, I would advise reading this note (5 mins or less) to understand the internal working of the tool.</span></span></span>
<!--more-->
<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">What is a team in bitbucket?</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Per bitbucket -bitbucket Teams makes it simple to create a shared account to consolidate your team-owned repositories and organize your group’s work.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">What is a project in bitbucket from a repository perspective?</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Per bitbucket support -a project is a place where you organize your repositories. They add the ability to categorize and group repositories.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Loosely stated -a project can be imagined as a host for the repositories. A repository can belong to only one project.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">What is a repository in bitbucket?</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Apart from the obvious, a repository in bitbucket is associated to bitbucket project and bitbucket team.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">So you can imagine the relationship between team, project and repositories to be something like:<img class="alignnone size-full wp-image-563" src="https://skundunotes.com/wp-content/uploads/2020/07/21.-migratingfrombbtoar-image5.png" alt="21. MigratingFromBBtoAR -image5" width="728" height="506"></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">On the Azure Devops side, we have a similar structure -organization, project and repository. And (like bitbucket), a team project is associated to only one organization and can host multiple repositories.</span></span></span>

<em><strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In this case the requirement was to move all the git repositories in all the projects from one team in Bitbucket to one team project of an organization in Azure Devops.</span></span></span></strong></em>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Prerequisites:</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-I had access to all projects and all repositories in bitbucket</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-I had admin access in AzureDevops -able to create repositories and push</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In order to work with creating Azure repos from the commandline I had to install Azure CLI tool. Accessible <a href="https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest" target="_blank" rel="noopener">here</a>.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Algorithm:</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Create local folder for clone work</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Login to Bitbucket</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Get a list of bitbucket teams</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">For each bitbucket team, get a list of projects associated with that team</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-For each projects, get a list of repositories associated with that project</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-For each repository,</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-clone the repo to local</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-check if a repository exists in Azure Repos, if not create one</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-push code from local to repo in Azure Repos</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">continue until all repositories in particular project are migrated</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">continue until all projects in particular bitbucket team are migrated</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">continue until all teams in particular bitbucket login are migrated</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Logout of bitbucket</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Delete local folder</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Delete AzureDevps PAT from environment</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Conclusion:</strong> </span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Before you give the powershell script a try, please understand the internal working and logic used to migrate the repositories. If needed, before using it, place appropriate checks in place like – just create a list of repositories in bitbucket and on Azure Devops and review if the list is complete. Once you are confident and certain, go ahead and use it to migrate ONLY ONE repository. You can manage that by proper exit statements.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">All in all, I’ve found this script quite robust and have used it to migrate all the repositories and (after multiple fixes) worked perfectly fine. I am storing the final version in github for our community to use it.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Some useful links:</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">https://community.atlassian.com/t5/Bitbucket-questions/What-is-the-difference-between-a-repository-and-a-project/qaq-p/399774</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">https://confluence.atlassian.com/bitbucket/group-repositories-into-projects-792497956.html</span></span></span>
