---
title: "Getting started with Azure Devops -add a repo"
date: 2020-03-17 18:00:11 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This is <strong>part 3 of a multipart series on getting started with Azure DevOps</strong>. Please refer to the below links to access other notes in this series.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="https://skundunotes.com/2019/11/07/getting-started-with-azure-devops/" target="_blank" rel="noopener">Part 1: Getting started with Azure DevOps</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2019/12/24/getting-started-with-azure-devops-work-items/" target="_blank" rel="noopener">Part 2: Getting started with Azure DevOps -work items</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Part 3: Getting started with Azure DevOps -add a repo (this note)</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/02/19/getting-started-with-azure-devops-create-a-build-agent/" target="_blank" rel="noopener">Part 4: Getting started with Azure DevOps -create a build agent</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/05/15/getting-started-with-azure-devops-create-a-build-pipeline-part-1-yaml-pipeline/" target="_blank" rel="noopener">Part 5: Getting started with Azure DevOps -create a build pipeline -Part 1 (YAML pipeline)</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/05/15/getting-started-with-azure-devops-create-a-build-pipeline-part-2-classic-editor/" target="_blank" rel="noopener">Part 6: Getting started with Azure DevOps -create a build pipeline -Part 2 (Classic Editor)</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2019/12/27/azure-devops-deployment-groups/" target="_blank" rel="noopener">Part 7: Getting started with Azure DevOps -create a deployment group</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="http://skundunotes.com/2020/05/22/getting-started-with-azure-devops-create-a-release-definition/" target="_blank" rel="noopener">Part 8: Getting started with Azure DevOps -create a release definition</a></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">There are three ways we can add code to our project in Azure DevOps. We could either <strong>create a new repo, upload an existing local git repo, or import one.</strong></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Whether to use TFVC or Git as a version control tool is determined when we create a project in our organization. Here is an example as I was creating a new project.</span></span></span>
<img class="alignnone size-full wp-image-415" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image1.png" alt="AD-GS-Repos-Image1" width="549" height="635">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I proceeded with selecting Git as the version control for this project.</span></span></span><!--more-->
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Once the project was created the Repos tab provided me three options to populate the repository:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-clone this empty repo to my local computer</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-push an existing (local) repo that I have on my computer</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-import a repository (from an external source)</span></span></span>
<img class="alignnone size-full wp-image-416" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image2.png" alt="AD-GS-Repos-Image2" width="865" height="644">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In the notes below I walk through all the options one by one. Let's start with Option A.</span></span></span>
<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Option A: Clone to your computer.</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This is by far the simplest of the approaches. Select this option if you currently have no code related to your project in any repository... as in, if this is a green-field project on which no work has started; no code exists.</span></span></span>
<em><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Note: I had to create a separate project for this example.</span></span></span></em>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">There are two ways to clone this empty repository.</span></span></span>
<img class="alignnone size-full wp-image-426" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image12.png" alt="AD-GS-Repos-Image12" width="899" height="238">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Click on the copy button and grab the URL (as shown above)</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">On our local, I created an empty folder where I'd want to host this repository. For e.g. C:\Git\kundusoRepo\Projects</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I then opened a command prompt and pasted the command to clone the repository</span></span></span>
<code>git clone https://littlecoding.visualstudio.com/Project-04/_git/Project-04</code>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As soon as I hit enter, I was requested for my Microsoft credentials.</span></span></span>
<img class="alignnone size-full wp-image-427" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image13.png" alt="AD-GS-Repos-Image13" width="961" height="449">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">After I entered them, I got a message (warning) that I cloned an empty repository. Which is true.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">On my local, I just had a new folder created inside "Projects" titled Project-04 with only a .git folder.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Here is how the .config file looked</span></span></span>
<img class="alignnone size-full wp-image-428" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image15.png" alt="AD-GS-Repos-Image15" width="787" height="257">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">We're all set after which I followed the same approach as with any git setup which is:</span></span></span>
<code>git add .
git commit -m ""
git pull
git push</code>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">...and after the last command, the code from our local repo would be available on the AzureDevops project repository.</span></span></span>
<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Option B: Push an existing repository from the command line.</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I had a windows service project that was in progress and in this example I tried to add that project to this repository. I had a couple of commits to that project on my local git instance.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">On my local machine, here is my project directory.</span></span></span>
<img class="alignnone size-full wp-image-433" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image3-1.png" alt="AD-GS-Repos-Image3" width="550" height="183">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The .git folder is hidden, with a config file inside. Here is the image of the contents of the file.</span></span></span>
<img class="alignnone size-full wp-image-418" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image4.png" alt="AD-GS-Repos-Image4" width="420" height="175">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As can be seen, there is no remote URL segment, only the standard core. I then ran the git remote add origin command and captured the changes.</span></span></span>
<img class="alignnone size-full wp-image-419" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image5.png" alt="AD-GS-Repos-Image5" width="654" height="53">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Two changes took place -the config file got updated and I was prompted for my Microsoft account login since the project I have is under my MS ID.</span></span></span>
<img class="alignnone size-full wp-image-434" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image6-1.png" alt="AD-GS-Repos-Image6" width="415" height="165">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">...and here is a trail of what happened after I confirmed my account.</span></span></span>
<img class="alignnone size-full wp-image-421" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image7.png" alt="AD-GS-Repos-Image7" width="652" height="308">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">...and this is how the config file looked after the above git commands were executed (note the extra remote and branch segments that got added)</span></span></span>
<img class="alignnone size-full wp-image-422" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image8.png" alt="AD-GS-Repos-Image8" width="645" height="323">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I navigated back to our Azure DevOps project and refreshed the Repos tab. This showed the project and commit history of our project.</span></span></span>
<img class="alignnone size-full wp-image-423" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image9.png" alt="AD-GS-Repos-Image9" width="1091" height="316">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">...and that is how I added a local git repo to the Azure Dev)ps instance.</span></span></span>
<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Option C: Import a repository</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Click on Import on the Repos landing page. We got two options:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-import a Git repository</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-import from TFVC (and there are some suggestions on how to import from TFVC to Git)</span></span></span>
<img class="alignnone size-full wp-image-429" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image16.png" alt="AD-GS-Repos-Image16" width="881" height="319">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In this example, I imported from a Github repository that I created a while back.</span></span></span>
<img class="alignnone size-full wp-image-430" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image17.png" alt="AD-GS-Repos-Image17" width="440" height="336">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">It took a few seconds to import the repository (since there was little history) and once that was done... I was able to view history on Azure DevOps project's portal.</span></span></span>
<img class="alignnone size-full wp-image-431" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image18.png" alt="AD-GS-Repos-Image18" width="1123" height="419">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Now that I had a repository on Azure DevOps, I could clone that to my local and work off of that repository. Or if I already had cloned an empty one (as shown in Option A), I could do a git pull and get all the artifacts along with history onto my local.</span></span></span>
<img class="alignnone size-full wp-image-432" src="https://skundunotes.com/wp-content/uploads/2020/03/ad-gs-repos-image19.png" alt="AD-GS-Repos-Image19" width="913" height="174">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Once I make updates (git add. -&gt; git commit) to our local git repository it is a good idea to push to our Azure DevOps repository.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The same sequence is followed... git pull, resolve conflicts if any, git push.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">These are the three options for how I was able to add code to my Azure DevOps project.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Next, <a href="http://skundunotes.com/2020/02/19/getting-started-with-azure-devops-create-a-build-agent/">Part 4: Getting started with Azure DevOps -create a build agent</a></span></span></span>
