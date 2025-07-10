---
title: "Delete IIS Logs using Azure Devops"
date: 2019-04-13 04:27:04 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">How many times have you been in a situation where you are waiting for an important  ci/cd pipeline delivery to finish only to find that it had failed with the error -</span></span></span>
<h4><img class="alignnone size-full wp-image-17" src="https://skundunotes.com/wp-content/uploads/2019/03/photo1.png" alt="Photo1" width="298" height="48" /></h4>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This happened to me for a few times and there were two usual approaches to resolution:</span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-request Data Center team for more disk space and immediately delete some folders to make code deploy successful</span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-create an automated process that ran periodically and deleted logs</span></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The second option sounded interesting, provided I had time at hand. <em>Ultimately, it came to such that creating an automated process would take less time than freeing up the disk space manually. That was the tipping point.</em>
</span></span></span></span></span></span><!--more-->

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="font-size:18px;"><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">When it comes to automation, there are multiple ways of doing so and having used Azure Devops(aka TFS) for more than a decade, that was my goto orchestrator.
For example say we have more than a few dozen Dev/Test/QA boxes and its the C: drive that was getting filled up every once in a while with IIS Logs. To clear the logs files I had to create a release definition with below steps: </span></span></span></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 1: Create a pipleline</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 2: Add deployment group</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 3: Add task</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step 4: Trigger release</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step1: This is where we name the individual environments where we want a task (or group of tasks) to be executed. Via pipeline, we orchestrate the deployment process.</span></span></span>
<img class="alignnone size-full wp-image-34" style="color:var(--color-neutral-600);" src="https://skundunotes.com/wp-content/uploads/2019/04/pipeline_environment.png" alt="Pipeline_Environment" width="1888" height="381" />

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In this case, the task that execute on Prod would have to be successful on PreProd; the task that execute on PreProd would have to be successful on QA and so forth all the way to Dev. This way we ensure that the process is well tested from environment to environment. Here is an image of pre-deployment condition for Test environment.</span></span></span>
<img class="alignnone  wp-image-41" src="https://skundunotes.com/wp-content/uploads/2019/04/deploymentcondition-1.png" alt="DeploymentCondition" width="465" height="302" />

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As can be seen, to deploy to Test, the deployment process had to be successful in Dev. After creating a pipeline, we move onto Deployment group (that is under Tasks tab)</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step2: Tasks tab shows a drop down arrow that lists all the environments that were added in the previous step.</span></span></span>

<img class="alignnone size-full wp-image-38" style="color:var(--color-neutral-600);" src="https://skundunotes.com/wp-content/uploads/2019/04/tasks.png" alt="Tasks" width="1795" height="412" />

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Under each environment there is a default agent phase available. In our case we prefer a deployment group phase. Here we add the deployment groups that contain the names of the actual machines that belong to this group.</span></span></span>

<img class="alignnone size-full wp-image-40" src="https://skundunotes.com/wp-content/uploads/2019/04/deploymentgroup-1.png" alt="DeploymentGroup.PNG" width="1697" height="542" />

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step3: On the left hand pane we now search for Powershell and add that task.</span></span></span>

<img class="alignnone size-full wp-image-32" src="https://skundunotes.com/wp-content/uploads/2019/04/addingtask.png" alt="AddingTask" width="1679" height="664" />

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The code to include inside Inline Script is included in the github link that I shared below.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Step4: Save the release definition and trigger a release</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Now wait for magic!</span></span></span>

<img class="alignnone size-full wp-image-36" src="https://skundunotes.com/wp-content/uploads/2019/04/pipelinedeploy.png" alt="PipelineDeploy" width="817" height="479" />

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">As seen, the list of environments are available with the deployment status. One after the other as deployment to a particular environment succeeds, the next one gets picked up.</span></span></span>

<img class="alignnone size-full wp-image-37" src="https://skundunotes.com/wp-content/uploads/2019/04/pipelinedeploy-approval.png" alt="PipelineDeploy-approval" width="824" height="515" />

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Bonus Item #1: Approval. Here in the above image you will see that there were no approvals necessary for Dev, Test and QA environments, but for PreProd there is an approval pending. That is because I put in a pre-deployment approval condition for that environment. The below image shows that option.</span></span></span>

<img class="alignnone  wp-image-31" src="https://skundunotes.com/wp-content/uploads/2019/04/addapprover.png" alt="AddApprover" width="668" height="356" />

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Once approval was provided, powershell command was executed on Preprod and Prod environment. We could have had an approval for Prod as well.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">After the release definition was executed on all the environments, the deployment status ended with the below image.</span></span></span>

<img class="alignnone size-full wp-image-35" src="https://skundunotes.com/wp-content/uploads/2019/04/pipeline_successful.png" alt="Pipeline_Successful" width="742" height="447" />

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Bonus Item #2: This release definition eliminated a need for me to manually delete logs files which was great, but I still had to trigger this. There is an option to schedule a release using which, we eliminate the need for manual trigger. Using scheduling the release definition will be auto-triggered which will then delete these log files at regular interval.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Github: <a href="https://github.com/kunduso/infrastructureascode/tree/master/DeleteIISLogs" target="_blank" rel="noopener">DeleteIISLogs</a></span></span></span>
