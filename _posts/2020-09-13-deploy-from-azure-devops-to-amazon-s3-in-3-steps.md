---
title: "Deploy from Azure DevOps to Amazon S3 in 3 steps"
date: 2020-09-13 11:25:48 +0000
categories: []
tags: []
---

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">While working on my previous note on how to deploy from <a href="http://skundunotes.com/2020/09/10/azuredevops-to-ec2-instance-in-4-steps/" target="_blank" rel="noopener">Azure DevOps to an Amazon EC2 instance</a>, I came across another use case -<strong>how to deploy from Azure DevOps to an Amazon S3 bucket</strong>. This note is about the steps I followed to do so.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Here is a recap so that we're all on the same page.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Azure DevOps is Microsoft's solution to a software development process that aids collaboration, traceability, and visibility using components like Azure Boards (work items), Azure Repos (code repository), Azure Pipelines (build and deploy), Azure Artifacts (package management), Azure Test Plans along with a plug and play module to integrate an awesome number of third party tools like Docker, Terraform, etc.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Amazon S3 stands for simple storage service, and as the name suggests, it is used to store and retrieve artifacts -files, and folders. Here is a link to the <a href="https://aws.amazon.com/s3/" target="_blank" rel="noopener">official documentation</a>.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Coming back to the use case at hand, I wanted to create a release definition (pipeline) to <strong>store artifacts from Azure DevOps into an Amazon S3 bucket.</strong> This can be broken down into three steps:</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 1: Create an AWS IAM user with appropriate permissions</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 2: Create a service connection in Azure DevOps</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 3: Create a release definition</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Please note that there are a couple of pre-requisites to this use case.</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">-knowledge about AWS IAM -how to create/manage a user</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">-knowledge about Amazon S3 bucket -how to create/manage a bucket</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">-knowledge about Azure DevOps pipelines -how to create a build and release definition</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 1: Create an AWS IAM user with appropriate permissions</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">As we know, to work with resources in AWS, we need proper access -to read/modify. In this case, we need an IAM user with programmatic access permission (full access) to S3. Please attach the appropriate policy (<em>AmazonS3FullAccess</em>) and store the Access key ID and Secret Access key securely. We need those in the next step.</span></span></span>

<img class="alignnone size-full wp-image-741" src="https://skundunotes.com/wp-content/uploads/2020/09/24.-aztos3-image1.png" alt="24. AZtoS3-image1" width="804" height="364" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Note: I am purposefully staying out of going into details about IAM -user, group, policy, and role in this post.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 2: Create a service connection in Azure DevOps</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Now let's go back to our Azure DevOps portal and launch the project where we want to set up the connection. Click on the gear icon at the bottom left corner. This will bring up the project settings and under Pipelines, we find the Service connections options. If you do not have any existing connections, you'll get a welcome message.</span></span></span>

<img class="alignnone size-full wp-image-743" src="https://skundunotes.com/wp-content/uploads/2020/09/24.-aztos3-image3.png" alt="24. AZtoS3-image3" width="526" height="324" />

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Click on <strong>Create service connection</strong>.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This will launch a new side panel with all the new connections available. Chances are that the AWS connection is not available, as I have in the below image.</span></span></span>

<img class="alignnone size-full wp-image-744" src="https://skundunotes.com/wp-content/uploads/2020/09/24.-aztos3-image4.png" alt="24. AZtoS3-image4" width="483" height="544" />

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This means that we'll have to install the AWS Connection from the marketplace.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Let us close the new service connection panel and return to our project portal. On the top right corner you'll see a shopping bag icon -Marketplace -&gt; Browse marketplace.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This launches a new page where we may add extensions for our Azure DevOps project. Search for AWS here.</span></span></span>

<img class="alignnone size-full wp-image-745" src="https://skundunotes.com/wp-content/uploads/2020/09/24.-aztos3-image5.png" alt="24. AZtoS3-image5" width="186" height="254" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Click on the icon to install the extension to get it for free. Make sure you are logged into the right organization when you are installing this service connection (ensure your email ID displayed at the top is the same email ID tied to your Azure DevOps project).</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">As instructed on the next page, select the Azure DevOps organization for which you want to install the extension. and click on Install. After installation, you'll see a message to "Proceed to organization". Let's click on that.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Now, lets again navigate to the project and click on the gear icon at the bottom left followed by Service connections and then "Create service connection". This time we see AWS as the first option to connect to. Yay!</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Select AWS and click on Next.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">On this page, we are requested to provided mandatory authentication details along with a few optional details. For now, we'll proceed with <strong>Access Key ID</strong> and <strong>Secret Access Key</strong> that we saved from the previous step when we created our IAM user along with the Service Connection name and description.</span></span></span>

<img class="alignnone size-full wp-image-747" src="https://skundunotes.com/wp-content/uploads/2020/09/24.-aztos3-image6a.png" alt="24. AZtoS3-image6a" width="454" height="345" />

<img class="alignnone size-full wp-image-746" src="https://skundunotes.com/wp-content/uploads/2020/09/24.-aztos3-image6.png" alt="24. AZtoS3-image6" width="448" height="276" />

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Once you have the connection created, you'll see that under Service connections.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">That brings us to the end of step 2 and now we proceed with creating a release definition to copy artifacts (files and folders) from Azure DevOps to an S3 bucket in AWS. How exciting!</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Step 3: Create a release definition</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Let's navigate back to our project portal by clicking on Overview (vertical right panel) and then click on Pipelines -&gt; Releases.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">It'd be good if we already have a Pipeline/Build definition created or some code in the repository that we can upload to S3.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Assuming you have, we proceed to the step when we add a new stage to a release definition.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Click on the + to add a task to agent job and search for S3.</span></span></span>

<img class="alignnone size-full wp-image-749" src="https://skundunotes.com/wp-content/uploads/2020/09/24.-aztos3-image8.png" alt="24. AZtoS3-image8" width="645" height="265" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This will list two tasks. We select "Amazon S3 Upload" and click on Add.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Under this task, we will be required to fill details like the AWS Credential, Region, BucketName, and Source Folder. Once that is done, save the release definition and trigger a run.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Note: Here is an image of the task group from a separate project with all values.</span></span></span>

<img class="alignnone size-full wp-image-750" src="https://skundunotes.com/wp-content/uploads/2020/09/24.-aztos3-image9.png" alt="24. AZtoS3-image9" width="410" height="665" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">After a successful run of the release definition, I was able to view the artifacts in the S3 bucket.</span></span></span>

<img class="alignnone size-full wp-image-751" src="https://skundunotes.com/wp-content/uploads/2020/09/24.-aztos3-image10.png" alt="24. AZtoS3-image10" width="667" height="295" />

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Conclusion:</strong> There are multiple options available for populating the Amazon S3 bucket from Azure DevOps, depending on the specific use case. Similar to uploading to S3, we can also download content from an Amazon S3 bucket using the "Amazon S3 download" task. </span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">As I like to state at the end of my notes, I hope you enjoyed reading the article as much as I wanted to write it. And if you have any questions or clarifications to seek, please do not hesitate to ask. I will be glad to explore those with you.</span></span></span>
