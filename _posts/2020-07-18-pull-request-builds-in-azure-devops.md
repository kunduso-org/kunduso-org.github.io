---
title: "Pull Request Builds in Azure DevOps"
date: 2020-07-18 01:20:17 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Picking up from where I left in the <a href="http://skundunotes.com/2020/07/17/protect-master-in-azure-repos-using-policies">previous note</a>… the other part of the request was to add a conditional step in the associated build definition and update the release definition accordingly. I will elaborate on that here.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Our typical code cycle is something like… code gets committed into a repository, a continuous integration (CI) build kicks off and on successful completion, a continuous deployment release definition kicks off and deploys the build artifact to an environment.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">With PR (pull request) builds, however, things can be a little different. The purpose of adding build validation as part of PR is to ensure that the pull request does not cause a CI build to fail. In this case, the feedback is immediate, and even before a reviewer spends time reviewing the PR. Generally, there is no point in reviewing a PR if it is going to break a CI build. BOCTAOE -but of course there are obvious exceptions. Ideally, the developer who submits a PR can fix that and ensure that CI build passes before it is ready for a reviewer.</span></span></span><!--more-->

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">There were a couple of options to consider enabling PR builds. To make this point clear it is necessary to elaborate on the steps in the build definition.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 1: Get code from the repository</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 2: NuGet restore</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 3: build a solution</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 4: test code for vulnerabilities</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 5: package code</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 6: upload package as a build artifact</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In the case of a PR build, the objective was to only check till step 4. There was no point in wasting time and resources in packaging and uploading the artifact.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Moreover, on the release definition side, there was no requirement to auto-trigger a release in case the build was caused by someone who submitted a pull request.</span></span></span>

<strong><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">How do you address those in Azure DevOps?</span></span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">We use <strong>conditions in build definitions</strong>. Conditions allow us to trigger a step provided the associated condition is met.</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Here is an image of a custom condition in a task that states that the steps will <strong>NOT</strong> take place if all the previous steps have succeeded (passed) and the build is triggered due to a pull request and the destination branch of the pull request is ‘master’ branch.</span></span></span>
<img class="alignnone size-full wp-image-585" src="https://skundunotes.com/wp-content/uploads/2020/07/prbwap-image8.png" alt="PRBwAP-image8" width="556" height="200">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">At my work, I enabled custom conditions for the last two steps (5 and 6 -package and upload code as build artifact) such that these steps were not executed when the build was triggered due to a pull request and where the target branch of the pull request was ‘master’</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">And on the <strong>release definition</strong> side click on pipeline -&gt; artifacts -&gt; continuous deployment trigger -&gt; <strong>pull request trigger: disable</strong></span></span></span>
<img class="alignnone size-full wp-image-583" src="https://skundunotes.com/wp-content/uploads/2020/07/prbwap-image6.png" alt="PRBwAP-image6" width="1057" height="726">

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As you can see, the <em>Continuous deployment trigger</em> is enabled so that if a build is triggered not due to a pull request, the artifacts associated with that build are deployed.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">These changes ensured that when a build was triggered due to a pull request, they did not create artifacts and successful completion of a PR build did not trigger a release to deploy the artifacts (that are non-existent). The concept of PR builds could also be extended such that there is a set of steps in a build definition that are triggered specifically if it is a PR build… maybe extended unit testing of the code or vulnerability testing. In that case, the condition will be something like:</span></span></span>
<img class="alignnone size-full wp-image-584" src="https://skundunotes.com/wp-content/uploads/2020/07/prbwap-image7.png" alt="PRBwAP-image7" width="552" height="176">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">…which would imply that the task would be triggered <strong>ONLY</strong> if all the previous tasks were successful and the build was triggered due to a pull request and the target branch of the pull request was ‘master’</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I hope as a reader you found the two notes useful.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">PS: A week after applying these changes, another request came up which is documented at <a href="https://skundunotes.com/2020/07/31/pr-builds-in-azure-devops-part2/">Pull Request Builds in Azure Devops -Part&nbsp;2</a></span></span></span>
