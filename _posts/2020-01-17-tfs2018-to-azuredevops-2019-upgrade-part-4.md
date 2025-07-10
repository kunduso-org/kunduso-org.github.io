---
title: "TFS2018 to AzureDevops 2019 Upgrade -Part 4"
date: 2020-01-17 22:03:31 +0000
categories: []
tags: []
---

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Table of Contents:</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/01/17/tfs2018-to-azuredevops-2019-upgrade-part-1/">TFS2018 to AzureDevops 2019 Upgrade Part 1: Approach</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/01/17/tfs2018-to-azuredevops-2019-upgrade-part-2/">TFS2018 to AzureDevops 2019 Upgrade Part 2: Create TFS Sandbox</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/01/17/tfs2018-to-azuredevops-2019-upgrade-part-3/">TFS2018 to AzureDevops 2019 Upgrade Part 3: Upgrade sandbox instance to Azure Devops 2019</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">TFS2018 to AzureDevops 2019 Upgrade Part 4 Upgrade production instance to Azure Devops 2019 (this article)</span></span></span>
<!--more-->
<del><em><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Thank you for stopping by. This article is still in progress.</span></span></span></em></del>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><img class=" size-full wp-image-459 alignleft" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image0.png" alt="AD-U-Image0" width="233" height="260" /><em>Updated as of March 8th 2020</em></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In the last three posts I mentioned about the series of steps that were taken to standup a parallel independent AzureDevops instance for the project teams to try their hands on. The project teams were requested to test out the new UI and familiarize themselves so that once the production instance was updated there was minimal learning curve. In the course of testing the new AzureDevops instances, a few fixes/upgrades were identified in order for the teams to be able to function efficiently. These were then classified into two categories – (i) fixes that can be implemented on current TFS instance that will carry into Azure Devops and (ii) fixes that need to be applied only after the upgrade.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">For example changing a build logic could be done prior to upgrade while changing support for certain activities require the new features to be available before they can be applied.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Once all the fixes were in place, it was decided to upgrade to Azure Devops 2019 Update 1 (instead of Azure Devops 2019).</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">On the day the upgrade was being carried out, all project teams were requested to check-in/shelve their code changes, save their task groups, build definitions, work items, boards views, queries etc. and preferably close visual studio client on their machines.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Closer to the upgrade time, I logged into the database server and queried active connections from the tbl_command table and reminded people to log off and close connection.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 1:</strong> I then stopped all build agents so that no last minute checkin could trigger a build. This was to ensure that there is as less talking while the tool goes down.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">go to this url for that: http://${TFS-URL}/DefaultCollection/ed/_admin/_AgentQueue</span></span></span>
<img class="alignnone size-full wp-image-437" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image1.png" alt="AD-U-Image1" width="653" height="344" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 2:</strong> Triggered TfsServiceControl quiesce</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This command was triggered on the application server were TFS was installed. This took approximately 6-7 minutes. I verified that the service was turned off by trying to access the TFS homepage which returned a service not available page.</span></span></span>
<img class="alignnone size-full wp-image-438" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image2.png" alt="AD-U-Image2" width="646" height="732" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 3:</strong> Shutdown application and database server</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This step was to prepare for a contingent and is not a mandatory step for Azure Devops upgrade. After the shutdown, a snapshot of both the servers were taken so that, if during the upgrade due to some unforeseen issued we had to bail out, we could just restore the snapshots and revisit the upgrade later. After the snapshots were taken, both the servers were powered back up.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 4:</strong> Install Azure Devops 2019 update 1</span></span></span>
<img class="alignnone size-full wp-image-439" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image3.png" alt="AD-U-Image3" width="461" height="646" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The installation took about 30 mins or so after which I was required to restart the application server.</span></span></span>
<img class="alignnone size-full wp-image-440" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image4.png" alt="AD-U-Image4" width="461" height="647" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 5:</strong> After the server was restarted, when I logged in, I got the below Configuration Center window ready.</span></span></span>
<img class="alignnone size-full wp-image-441" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image5.png" alt="AD-U-Image5" width="894" height="623" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 6:</strong> Click on Start Wizard </span></span></span>
<img class="alignnone size-full wp-image-442" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image6.png" alt="AD-U-Image6" width="922" height="633" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Since this was an updated to an existing instance (TFS2018.Update2) I selected the options that I have existing databases. Click Next.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 7:</strong> Specify Configuration database</span></span></span>
<img class="alignnone size-full wp-image-443" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image7.png" alt="AD-U-Image7" width="920" height="631" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This value was picked up from the existing setup.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 8:</strong> Deployment scenario:</span></span></span>
<img class="alignnone size-full wp-image-444" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image8.png" alt="AD-U-Image8" width="923" height="633" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This being a production upgrade, I selected the default option.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 9:</strong> Provide service account under which Azure Devops will run</span></span></span>
<img class="alignnone size-full wp-image-445" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image9.png" alt="AD-U-Image9" width="920" height="629" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 10:</strong> Verify the application url and SSH Settings</span></span></span>
<img class="alignnone size-full wp-image-446" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image10.png" alt="AD-U-Image10" width="922" height="631" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 11:</strong> Configure search</span></span></span>
<img class="alignnone size-full wp-image-447" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image11.png" alt="AD-U-Image11" width="922" height="861" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 12:</strong> After selected all appropriate configuration values, I got to the review page</span></span></span>
<img class="alignnone size-full wp-image-448" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image12.png" alt="AD-U-Image12" width="922" height="968" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 13:</strong> I reviewed all and it looked good. Clicked Verify which started readiness check.</span></span></span>
<img class="alignnone size-full wp-image-449" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image13.png" alt="AD-U-Image13" width="917" height="547" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 14:</strong> After the readiness check completed a confirmation was requested</span></span></span>
<img class="alignnone size-full wp-image-450" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image14.png" alt="AD-U-Image14" width="916" height="545" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Since we had already migrated all our builds, I selected the checkbox (which enabled the Configure button) and proceeded.</span></span></span>
<img class="alignnone size-full wp-image-451" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image15.png" alt="AD-U-Image15" width="914" height="546" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">It took about 10 mins to configure</span></span></span>
<img class="alignnone size-full wp-image-452" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image16.png" alt="AD-U-Image16" width="917" height="546" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 15:</strong> I clicked on Next and the upgrade was already in progress</span></span></span>
<img class="alignnone size-full wp-image-458" style="color:var(--color-text);" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image17-1.png" alt="AD-U-Image17" width="917" height="546" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The upgrade step took a little less than 18 minutes and when I clicked Next, I got the Success page! Azure Devops upgrade done.</span></span></span>
<img class="alignnone size-full wp-image-455" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image19.png" alt="AD-U-Image19" width="918" height="546" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Clicked on Close.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I verified that the product version was correct and then reviewed the other configuration values.</span></span></span>
<img class="alignnone size-full wp-image-456" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image20.png" alt="AD-U-Image20" width="980" height="278" />
<img class="alignnone size-full wp-image-457" src="https://skundunotes.com/wp-content/uploads/2020/01/ad-u-image21.png" alt="AD-U-Image21" width="1155" height="947" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 16:</strong> I enabled all the build agents (that were disabled prior to the upgrade) and then started my smoke test to ensure that all the features and functionalities are working as expected.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Conclusion:</strong> </span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The upgrade process took close to 2 hours which isn’t bad at all considering our 1.7 TB database size. Practicing the upgrade on sandbox instance was a confidence booster. Also having project teams try out the sandbox instance ensured that there was just one issue reported post the upgrade.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Notes:</strong> </span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The application and database server snapshots were deleted once all project teams were comfortable with the upgrade.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Azure Devops build definitions were working fine with build agents that were configured on TFS 2018</span></span></span>
