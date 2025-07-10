---
title: "TFS2018 to AzureDevops 2019 Upgrade -Part 1"
date: 2020-01-17 22:00:01 +0000
categories: []
tags: []
---

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Table of Contents:</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">TFS2018 to AzureDevops 2019 Upgrade Part 1: Approach (this article)</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/01/17/tfs2018-to-azuredevops-2019-upgrade-part-2/">TFS2018 to AzureDevops 2019 Upgrade Part 2: Create TFS Sandbox</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/01/17/tfs2018-to-azuredevops-2019-upgrade-part-3/">TFS2018 to AzureDevops 2019 Upgrade Part 3: Upgrade sandbox instance to Azure Devops 2019</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/01/17/tfs2018-to-azuredevops-2019-upgrade-part-4/">TFS2018 to AzureDevops 2019 Upgrade Part 4 Upgrade production instance to Azure Devops 2019</a></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The purpose of this article is to describe the process of <strong>upgrading TFS infrastructure from TFS 2018.Update2 to Azure Devops 2019</strong>. As could be understood from the version number in the previous statement, our setup was on premises and remained so after the upgrade.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Here is additional information on the infrastructure hosting TFS:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">TFS application on Windows 2012 Standard Edition</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">TFS database on Windows 2012R2 on SQL 2016 SP1</span></span></span>
<img class="alignnone size-full wp-image-275" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part1image1.png" alt="AzureDevopsUpgrade-Part1Image1" width="433" height="327" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Almost all members of our development department used TFS as a source code repository, build and release management (CI/CD pipeline) and work item tracking tool. Hence all stakeholders were informed before the upgrade to TFS infrastructure.</span></span></span><!--more-->

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Considering that TFS was (is) so widely used, there was a desire to know a few things prior to the upgrade:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-for how long is the environment unavailable?</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-how is the new environment going to react (navigation, look and feel) after the upgrade?</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-in case we have to bailout (rollback to current TFS environment) mid-way in the upgrade process, do we have the capability and how?</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Except for the last one, the answers to the questions were available only after an upgrade. So, the approach that we were comfortable taking was to create a parallel and standalone TFS (application and database) instance that is identical to (but separate from) production and use that environment as our upgrade candidate. For the sake of clarity, let’s call the standalone instance as TFS sandbox instance. And upgrade that sandbox instance with Azure Devops 2019.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This approach had three benefits:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">• the current <strong>TFS production setup did not get interrupted</strong> and the department continued to work as usual,</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">• the upgrade process took place on a <strong>setup as close to production</strong> and so the metrics collected were relatable to an actual production upgrade and</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">• the department got to <strong>navigate/review the new environment</strong> before the actual production upgrade and hence has a better idea of what to expect.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">To answer the last question (if we have to bailout midway), before proceeding with TFS upgrade we took a <strong>snapshot of the application and database servers</strong>. That snapshot allowed us to revert in case we had to. After a couple of weeks or so, these snapshots were deleted.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The next set of steps are as follows and since these are quite detailed, I am creating separate notes for them. Click on them to read further:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">• <a href="https://skdevops.wordpress.com/2020/01/17/tfs2018-to-azuredevops-2019-upgrade-part-2">standup a parallel TFS instance (sandbox)</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">• <a href="https://skdevops.wordpress.com/2020/01/17/tfs2018-to-azuredevops-2019-upgrade-part-3">upgrade the sandbox instance</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">• <a href="https://skdevops.wordpress.com/2020/01/17/tfs2018-to-azuredevops-2019-upgrade-part-4">upgrade TFS production instance</a></span></span></span>
