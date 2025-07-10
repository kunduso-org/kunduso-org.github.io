---
title: "Protect master in Azure Repos using policies"
date: 2020-07-18 01:19:50 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">A couple of weeks or so back, I had an interesting work land into my sprint. The request was to <strong>protect the <em>'master'</em> branch and enable PR build for a set of Azure repositories.</strong></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This involved -for each repository:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"> -Require a minimum number of reviewers</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"> -Enable a certain set of policies</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"> -Check for linked work items</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"> -Check for comment resolution</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"> -Check for build validation</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"> -Add a conditional step in the associated build definition and update release definition accordingly</span></span></span>
<!--more-->

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I won’t dig deeper into -why we protect the <em>'master'</em> branch -in this note. Simply stated, the concept of <strong>branch protection is to prevent irrevocable changes from being committed</strong>. With more and more repos being hosted as public, there needs to be a standardized process in which the community benefits from the repository and at the same time is able to make meaningful contributions. And that is where branch protection is of value.</span></span></span>
<blockquote><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Generally speaking master branch is sacrosanct.</span></span></span></blockquote>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Different development teams treat the&nbsp;<em>'master'</em> branch differently. Some consider that once a code repository goes through a certain maturity phase, where the <em>'master'</em> branch becomes stable, any changes (commit) made to it should be on certain policies being met. These policies effectively work towards ensuring that the master branch is protected from changes that do not align. Or loosely stated, <strong>commit/s to the <em>'master'</em> branch is not allowed unless a specific set of conditions/policies are not met.</strong></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">How do we protect the <em>'master'</em> branch (in Azure DevOps)?</span></span></span>
<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Universal answer: By enabling Pull requests.</span></span></span></strong>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In Azure DevOps pull requests are enabled via the following steps.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">For any repository, click on branches -&gt; ellipsis (next to the branch that is being protected) -&gt; branch policies<img class=" size-full wp-image-573 aligncenter" src="https://skundunotes.com/wp-content/uploads/2020/07/prbwap-image2.png" alt="PRBwAP-image2" width="961" height="542"></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This navigates to the policies page for the branch. Here is an image of a few policies for this repository.<img class=" size-full wp-image-574 aligncenter" src="https://skundunotes.com/wp-content/uploads/2020/07/prbwap-image3.png" alt="PRBwAP-image3" width="522" height="550"></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As can be seen, above are a few policies (and I have not enabled any policies) for this repository in this image.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Coming back to the request at hand… it was to protect the <em>'master'</em> branch for a set of existing repositories (between 2-3 dozen) by enabling these policies.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Here is a little more information on the request. There were existing Azure DevOps build definitions (kept in a certain folder under All build pipelines). Each of these build definitions was associated with a repository in Azure DevOps. 1:1 mapping. The request was to protect the <em>'master'</em> branch ONLY in these repositories. Another critical piece of information: a few of these repositories already had some of the policies applied.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I was not going to work through this manually.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Algorithm:</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 1: Create a list of all build definitions under the ‘particular folder’</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 2: for each build definition in the list, gather information about the repository that is tied to it</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 3: here, I had a choice. For each repository</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">(a) either <strong>delete all existing policies</strong> and apply new ones. This was a lot easier since it would ensure that no checks (if policies exist) were need.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">(b) or check if a certain policy existed and <strong>apply only if it did not</strong> and move to the next policy until all the policies were applied.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Both approaches have benefits and drawbacks: (i) delete all and recreate does not consider <strong>special conditions unless explicitly stated</strong> and (ii) creating only missing policies would <strong>allow for existing deviations to persist</strong> (for already created policies). And I wanted a solution where-in it was possible to use both options rather than have two separate scripts for that purpose.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">My preferred scripting language is PowerShell and I was able to code the entire workflow with the help of some very good documentation from <strong>docs.microsoft.com</strong></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I have the code committed in my <strong>GitHub repository: <a href="https://github.com/kunduso/ApplyPoliciesToAzureRepos" target="_blank" rel="noopener">ApplyPoliciesToAzureRepos</a></strong> and I have ensured that as much information is available in the ReadMe file about how to use it.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">After I executed the PowerShell script with appropriate arguments, this is how the policies page looked.<img class="alignnone size-full wp-image-575" src="https://skundunotes.com/wp-content/uploads/2020/07/prbwap-image4.png" alt="PRBwAP-image4" width="943" height="811"></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">There are a few more policies such as case-enforcement, file-size, merge-strategy, and required-reviewer which I did not enable since it was not required in the repositories I was working with.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">More information related to these can be found at below links:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="https://docs.microsoft.com/en-us/cli/azure/ext/azure-devops/repos/policy/case-enforcement?view=azure-cli-latest">az repos policy case-enforcement</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="https://docs.microsoft.com/en-us/cli/azure/ext/azure-devops/repos/policy/file-size?view=azure-cli-latest">az repos policy file-size</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="https://docs.microsoft.com/en-us/cli/azure/ext/azure-devops/repos/policy/merge-strategy?view=azure-cli-latest">az repos policy merge-strategy</a></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><a href="https://docs.microsoft.com/en-us/cli/azure/ext/azure-devops/repos/policy/required-reviewer?view=azure-cli-latest">az repos policy required-reviewer</a></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The other part of the request was to add a conditional step in the associated build definition. I have that as a separate note <strong><a href="http://skundunotes.com/2020/07/17/pr-builds-in-azure-devops">here</a>.</strong></span></span></span>
