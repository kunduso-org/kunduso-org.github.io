---
title: "Migrating a repository from bitbucket to Azure Repos -UI based"
date: 2020-07-10 11:01:23 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">A few weeks back I got an opportunity to migrate code from Bitbucket to Azure Repos. I had 2 options – (i) <strong>Azure Repos UI based migration tool</strong> or (ii) <strong>automation; powershell is my go-to tool for that</strong>. The UI based approach appeared intuitive, so I gave that a try to get a look and feel as to how long the migration took and how the repo looked after the migration.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">There were a little more than 3 dozen repositories in total to be migrated and I realized that the approach -UI based (although do-able) was not quite exciting. I then ended up spending time working on an automated approach.</span></span></span>
<!--more-->
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In these notes, I list the two steps that were involved to migrate the repositories from Bitbucket to Azure Repos. This being part 1, I list the steps to migrate using the UI tool. If you want to read the approach using powershell please refer to <a href="http://skundunotes.com/2020/07/03/migrating-a-repository-from-bitbucket-to-azure-repos-powershell-based">this article</a>&nbsp;</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Prerequisites:</strong> </span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I had read permission to the bitbucket repository and admin permissions in Azure Repos.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Approach:</strong> </span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Azure Repos and Bitbucket have Git as underlying source control so moving a repository was simply a matter of cloning an existing repository and changing the host.</span></span></span>
<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 1:</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Access Azure Repos and click on Import repository (this is the second last option in a drop down list of existing repositories)</span></span></span>
<img class="alignnone size-full wp-image-553" src="https://skundunotes.com/wp-content/uploads/2020/07/21.-migratingfrombbtoar-image1.png" alt="21. MigratingFromBBtoAR -image1" width="554" height="450">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Bitbucket’s source type is Git and so moving the repository is straightforward. Clone URL is bitbucket URL when you access a repository. Here is an image of the same.</span></span></span>

<img class="alignnone size-full wp-image-554" src="https://skundunotes.com/wp-content/uploads/2020/07/21.-migratingfrombbtoar-image2.png" alt="21. MigratingFromBBtoAR -image2" width="891" height="208">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Copy the Clone URL. This auto fills the Name to the repository -although not mandatory but try not to change that.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Click on Requires authorization and provide your bitbucket username and password/PAT.</span></span></span>
<img class="alignnone size-full wp-image-555" src="https://skundunotes.com/wp-content/uploads/2020/07/21.-migratingfrombbtoar-image3.png" alt="21. MigratingFromBBtoAR -image3" width="555" height="574">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 2:</strong></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Click on Import.</span></span></span>
<img class="alignnone size-full wp-image-556" src="https://skundunotes.com/wp-content/uploads/2020/07/21.-migratingfrombbtoar-image4.png" alt="21. MigratingFromBBtoAR -image4" width="593" height="442">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">It hardly took a minute to migrate the repository and on Azure Repos portal I was able to view that.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The repository was migrated with all branches, commits and tags which was quite helpful. Just to give a sense of size: this was a 15 MB repository with total 22 branches.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Conclusion:</strong> Migrating repository from bitbucket to Azure Repos is super easy and quick but not quite exciting if you have to migrate a large number of repositories. I have a separate note for a powershell based migration approach which is handy if there are more number of repositories to move. Click <a href="http://skundunotes.com/2020/07/10/migrating-a-repository-from-bitbucket-to-azure-repos-using-powershell">here</a> to read. I think at the end it comes down to productivity and purpose -take the approach that makes sense.</span></span></span>
