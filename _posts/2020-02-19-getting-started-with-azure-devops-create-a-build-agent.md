---
title: "Getting started with Azure Devops -create a build agent"
date: 2020-02-19 20:45:43 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This is <strong>part 4 of a multipart series on getting started with Azure DevOps</strong>. Please refer to the below links to access other notes in this series.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="https://skundunotes.com/2019/11/07/getting-started-with-azure-devops/" target="_blank" rel="noopener">Part 1: Getting started with Azure DevOps</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2019/12/24/getting-started-with-azure-devops-work-items/" target="_blank" rel="noopener">Part 2: Getting started with Azure DevOps -work items</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/03/17/getting-started-with-azure-devops-add-a-repo/" target="_blank" rel="noopener">Part 3: Getting started with Azure DevOps -add a repo</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Part 4: Getting started with Azure DevOps -create a build agent (this note)</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/05/15/getting-started-with-azure-devops-create-a-build-pipeline-part-1-yaml-pipeline/" target="_blank" rel="noopener">Part 5: Getting started with Azure DevOps -create a build pipeline -Part 1 (YAML pipeline)</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/05/15/getting-started-with-azure-devops-create-a-build-pipeline-part-2-classic-editor/" target="_blank" rel="noopener">Part 6: Getting started with Azure DevOps -create a build pipeline -Part 2 (Classic Editor)</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2019/12/27/azure-devops-deployment-groups/" target="_blank" rel="noopener">Part 7: Getting started with Azure DevOps -create a deployment group</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/05/22/getting-started-with-azure-devops-create-a-release-definition/" target="_blank" rel="noopener">Part 8: Getting started with Azure DevOps -create a release definition</a></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Project teams will come to use Azure DevOps Server 2019 in one of two ways (i) either an upgrade from a previous version of TFS or (ii) a fresh setup of Azure DevOps Server 2019.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In case of a fresh setup of Azure DevOps Server 2019, it is a good idea to get a new (build) agent pool created and ready before a build definition requires to use the agent pool. In case a project team is upgrading from the previous version of TFS (version 2015 and later), the build agents tied to build pools in the earlier TFS installation should be able to pick up requests from build definitions (I have tested this for TFS2018.Update2 but not for an earlier version of TFS). Although, irrespective of which version of TFS you are upgrading from, my preference would be to create a new (build) agent pool in case of a TFS upgrade as well.</span></span></span><!--more-->

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">So let's roll back a bit and identify a few key components that I'm talking about.</span></span></span>
<img class="alignnone size-full wp-image-367" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image0.png" alt="AD-GS-BuildAgent-Image0" width="1142" height="569">

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As you can see, a build definition consumes source code and build steps. A build downloads the source code to the build agent machine and uses the software installed on that machine to run the build steps on the source code. In the end, a build produces a build status and build artifacts.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">What is a build agent pool?</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As the name suggests, its a collection (pool) of build agents. A single machine (VM) can host multiple build agents and multiple machines can belong to a single build agent pool. Also, the same machine can host build agents belonging to different agent pools. However, a build agent can belong to only one build agent pool. Agent pools are a level of abstraction provided by Azure DevOps so that you do not have to manually manage the agents from a build definition. When you queue a build, it executes on an agent from the agent pool specified in the build definition.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Why do I need to know about build agents?</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The build machine is where the actual builds will be executed. Ideally, you'd want it to be a separate machine from the App and DB server hosting Azure DevOps 2019. You may have multiple machines belonging to as many agent pools.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">How to create a build agent pool and build agent?</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The steps required are:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 1:</strong> Create an agent pool.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Navigate to Project Settings (gear at the bottom left corner) -&gt;Pipelines -&gt; Agent Pools </span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I named the pool: Sandbox Pool</span></span></span>
<img class="alignnone size-full wp-image-368" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image1.png" alt="AD-GS-BuildAgent-Image1" width="1135" height="460">

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 2:</strong> Once you have an agent pool created, click on the Download agent button.</span></span></span>
<img class="alignnone size-full wp-image-369" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image2.png" alt="AD-GS-BuildAgent-Image2" width="757" height="175">

<em><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">(I already had a couple of agents created and they are being listed here -Sandbox-agent1 and Sandbox-agent2)</span></span></span></em>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 3:</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">When you click on the Download agent button a new window pops out with agent commands to install on Windows, Mac, and Linux OSes. In my case, Windows is selected and (please read carefully) by default the version of agent architecture is x86. I categorically selected x64 since I was installing an agent on a Windows Server 2016 Standard.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Note: Do not install x86 version of agent on x64. You will be able to install and the agent will also come up, but your builds will fail without any clear error log.</span></span></span>
<img class="alignnone size-full wp-image-370" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image3.png" alt="AD-GS-BuildAgent-Image3" width="759" height="824">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 4:</strong> Download the agent and copy the zip file to the appropriate build agent. The first command block is to unzip and if the zip file is not located at the correct path the script will error out.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I unzipped the file: vsts-agent-win-x64-2.144.2 at E:\TFS-Sandbox-BuildAgent</span></span></span>
<img class="alignnone size-full wp-image-371" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image4.png" alt="AD-GS-BuildAgent-Image4" width="633" height="171">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 5:</strong> From here, open a Powershell command prompt as Admin and run config.cmd</span></span></span>
<img class="alignnone size-full wp-image-372" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image5.png" alt="AD-GS-BuildAgent-Image5" width="462" height="137">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I was prompted to enter the server URL and went ahead with the <strong><em>Integrated</em></strong> authentication type.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">After connecting to the server, we start with registering the agent.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 6:</strong> Although we could create an agent pool on the fly at this step, I would not recommend that. This is where we enter the name of the agent pool we created.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Agent pool: Sandbox Pool</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The next prompt was for agent name. I entered: Sandbox-agent3 to be consistent with the naming convention.</span></span></span>
<img class="alignnone size-full wp-image-373" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image6.png" alt="AD-GS-BuildAgent-Image6" width="503" height="244">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">At this point, if I go back and check Azure DevOps portal -&gt; Project Settings -&gt; Agent pools, I see the following:</span></span></span>
<img class="alignnone size-full wp-image-374" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image7.png" alt="AD-GS-BuildAgent-Image7" width="1083" height="358">

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As you can see, the agent is listed but still offline. We have a few more steps left.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 7:</strong> Coming back to the Powershell command the next set of prompts are for the work area, the service and the user under which to run the service as.</span></span></span>
<img class="alignnone size-full wp-image-375" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image8.png" alt="AD-GS-BuildAgent-Image8" width="737" height="393">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">If you open windows services, you will see this new service there:</span></span></span>
<img class="alignnone size-full wp-image-377" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image10.png" alt="AD-GS-BuildAgent-Image10" width="513" height="30">

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><strong>Step 8:</strong> After this, if we go back to the portal we will see that the agent is green now.</span></span></span>
<img class="alignnone size-full wp-image-376" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image9.png" alt="AD-GS-BuildAgent-Image9" width="1071" height="351">

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">And this is how we create an agent in Azure DevOps 2019 server.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">There are a few notes that I'd want people to be careful of:</span></span></span></strong>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Can I have multiple build agents installed on the same machine?</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Yes, we can have multiple (4/5) build agents, depending upon machine resources (memory/RAM). Although they can belong to separate build agent pools make sure they all have a unique agent name.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I am going to create multiple agents on a particular machine. Can I create copies of an existing agent folder and trigger the config.cmd file later?</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">There is a tendency to create a copy because it's easy, right. But please do not create a copy of an existing installed agent, instead, unzip again from the zip file that was downloaded. Every time an agent gets configured, there are a few hidden files added to that folder. See below:</span></span></span>
<img class="alignnone size-full wp-image-378" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image11.png" alt="AD-GS-BuildAgent-Image11" width="711" height="277">

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">On opening .agent file you will see configuration specific to the installed agent.</span></span></span>
<img class="alignnone size-full wp-image-379" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image12.png" alt="AD-GS-BuildAgent-Image12" width="424" height="196">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Similarly, the other three files too have agent specific values ...and that is why you should not copy an existing agent folder and instead take the unzip route.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">If at all you have to, my suggestion would be to create copies of the unzipped folder before configuring the first agent.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">What will happen if I create a copy of an existing agent folder and trigger the config.cmd file?</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I tried that.</span></span></span>
<img class="alignnone size-full wp-image-380" src="https://skundunotes.com/wp-content/uploads/2020/02/ad-gs-buildagent-image13.png" alt="AD-GS-BuildAgent-Image13" width="845" height="54">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">If I run "config.cmd remove" my existing installed agent will be removed. I do not want that. There is a use case for this command though. Say for example you configured the agent and something wasn't correct... maybe you want to change the agent name, maybe you want to connect the agent to a different Azure DevOps URL. Under those circumstances, it is recommended that you do a clean removal of the agent and start configuring again.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Which machine can act as a build agent?</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The machine that has all the necessary software installed so that the builds that run on that complete successfully. If the product needs .net and/or java to compile, have those installed before you install the agent on the machine. If you realized that you installed a software after installing the agent, restart the windows service under which the agent was running or better still (if possible) restart the build agent machine.</span></span></span>

<em><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Note: I also noticed a slight difference in URL to agent pool</span></span></span></em>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In TFS the url was $(TFSUrl)/DefaultCollection/_admin/_AgentPool</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In AzureDevops it is: $(TFSUrl)/DefaultCollection/_settings/agentpools</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Next, <a href="http://skundunotes.com/2020/05/15/getting-started-with-azure-devops-create-a-build-pipeline-part-1-yaml-pipeline/">Part 5: Getting started with Azure DevOps -create a build pipeline -Part 1 (YAML pipeline)</a></span></span></span>
