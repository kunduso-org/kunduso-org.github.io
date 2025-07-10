---
title: "TFS2018 to AzureDevops 2019 Upgrade -Part 3"
date: 2020-01-17 22:02:05 +0000
categories: []
tags: []
---

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Table of Contents:</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/01/17/tfs2018-to-azuredevops-2019-upgrade-part-1/">TFS2018 to AzureDevops 2019 Upgrade Part 1: Approach</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/01/17/tfs2018-to-azuredevops-2019-upgrade-part-2/">TFS2018 to AzureDevops 2019 Upgrade Part 2: Create TFS Sandbox</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">TFS2018 to AzureDevops 2019 Upgrade Part 3: Upgrade sandbox instance to Azure Devops 2019 (this article)</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/01/17/tfs2018-to-azuredevops-2019-upgrade-part-4/">TFS2018 to AzureDevops 2019 Upgrade Part 4 Upgrade production instance to Azure Devops 2019</a></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In Part 1, I outlined the approach I had in mind to upgrade our TFS infrastructure. In Part 2, I narrated the steps to create a sandbox TFS instance. In this part, I will demonstrate the steps that were taken to upgrade that instance by installing AzureDevops2019. As of writing the URL for the installer is available at https://azure.microsoft.com/en-us/services/devops/server/</span></span></span>

<!--more--><img class="alignnone size-full wp-image-327" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image0.png" alt="AzureDevopsUpgrade-Part3Image0" width="492" height="200" />
<em><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Note: Prior to installation, I ensured with help from Data Center Operations that there was no way sandbox TFS instance (application and database tier) was able to communicate with production TFS instance. Moreover before the installation, I requested Data Center Operations to take a snapshot of the application and database tier virtual machines. The snapshots become handy if I have to revert and re-try the installation. I would strongly advocate taking a snapshot before proceeding with an important upgrade like Azure Devops.</span></span></span></em>
<img class="alignnone size-full wp-image-328" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image1.png" alt="AzureDevopsUpgrade-Part3Image1" width="462" height="646" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Azure Devops 2019 installation took 35 minutes on sandbox instance.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">After successful installation the application tier had to be restarted.</span></span></span>
<img class="alignnone size-full wp-image-329" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image2.png" alt="AzureDevopsUpgrade-Part3Image2" width="462" height="645" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Following the restart, the below portal opened to configure Azure Devops (no more TFS!!)</span></span></span>
<img class="alignnone size-full wp-image-330" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image3.png" alt="AzureDevopsUpgrade-Part3Image3" width="924" height="634" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I selected the second option (image below) since this was not a fresh installation but an upgrade to the existing TFS instance.</span></span></span>
<img class="alignnone size-full wp-image-331" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image4.png" alt="AzureDevopsUpgrade-Part3Image4" width="919" height="632" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">On the databases tab I did not select anything other than confirming that I had a backup... rest of the values were picked up by the configuration wizard.</span></span></span>
<img class="alignnone size-full wp-image-332" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image5.png" alt="AzureDevopsUpgrade-Part3Image5" width="922" height="633" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">On the deployment scenario confirmation tab I went ahead with Production Upgrade since this was a sandbox environment which didn't interfere with production TFS instance. (<em>I would select that option while upgrading production TFS instance as well</em>)</span></span></span>
<img class="alignnone size-full wp-image-333" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image6.png" alt="AzureDevopsUpgrade-Part3Image6" width="922" height="633" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Confirmed the login credentials for the service account for AzureDevops</span></span></span>
<img class="alignnone size-full wp-image-334" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image7.png" alt="AzureDevopsUpgrade-Part3Image7" width="923" height="633" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Followed by configuring the application tier</span></span></span>
<img class="alignnone size-full wp-image-335" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image8.png" alt="AzureDevopsUpgrade-Part3Image8" width="922" height="628" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Search option was disabled</strong> so there was nothing to do there. This was because Search was not installed in the previous version (TFS2018.Update2). This can however be configured after the rest of the application is configured and upgraded.</span></span></span>
<img class="alignnone size-full wp-image-336" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image9.png" alt="AzureDevopsUpgrade-Part3Image9" width="921" height="632" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Same for Reporting</strong> and this too can be configured after the upgrade.</span></span></span>
<img class="alignnone size-full wp-image-337" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image10.png" alt="AzureDevopsUpgrade-Part3Image10" width="924" height="631" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Once all the configuration settings had been updated, a confirmation page loaded up that listed all the values that were selected.</span></span></span>
<img class="alignnone size-full wp-image-338" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image11.png" alt="AzureDevopsUpgrade-Part3Image11" width="923" height="837" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I verified that all was good and triggered a readiness check that ran for a couple of minutes.</span></span></span>
<img class="alignnone size-full wp-image-339" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image12.png" alt="AzureDevopsUpgrade-Part3Image12" width="924" height="471" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">All our XAML builds have been ported to task based build and release pipeline system a couple of years back and we were not using any XML builds. In case your team still is using, there is some helpful information provided here: https://docs.microsoft.com/en-us/azure/devops/pipelines/build/migrate-from-xaml-builds?view=azure-devops</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I acknowledged the warning and proceeded to Configure.</span></span></span>
<img class="alignnone size-full wp-image-341" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image13-a.png" alt="AzureDevopsUpgrade-Part3Image13-a" width="1416" height="675" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">It took about 13 minutes to configure and another 13 minutes to upgrade for a database of size 1.7TB.</span></span></span>
<img class="alignnone size-full wp-image-342" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image16-a.png" alt="AzureDevopsUpgrade-Part3Image16-a" width="993" height="485" />
<img class="alignnone size-full wp-image-343" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image18.png" alt="AzureDevopsUpgrade-Part3Image18" width="924" height="460" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">… and got the console view I was waiting for.</span></span></span>
<img class="alignnone size-full wp-image-344" src="https://skundunotes.com/wp-content/uploads/2020/01/azuredevopsupgrade-part3image19.png" alt="AzureDevopsUpgrade-Part3Image19" width="986" height="425" />
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The super-awesome Azure Devops Server Administration Console.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Notes:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">It took less than 75 minutes to install, configure and upgrade the instance; way lower than what my previous few experiences have been working with TFS2010, TFS 2012, TFS 2015 and 2017 -if I remember right.</span></span></span>
