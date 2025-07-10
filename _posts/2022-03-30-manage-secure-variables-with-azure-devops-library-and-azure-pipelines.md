---
title: "Manage secure variables with Azure DevOps Library and Azure Pipelines"
date: 2022-03-30 07:33:00 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Quite often, I use <strong>secure variables in Azure Pipelines</strong>. However, though I want to use them, I do not intend on sharing the values of the variables in the code or logs due to security. Hence I required a variable store that was secure and, at the same time, easily accessible by Azure Pipelines.</span></span></span>
<!--more-->
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I found the solution in <strong>Azure DevOps Library</strong>. In this note, <span style="text-decoration:underline;">I'll demonstrate how to create a variable group, store secure values, and reference the variables in the Azure Pipelines YAML file.</span> I'll also show you that the Azure pipelines log won't expose the variable's value.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The <strong>variable group</strong> option is available under Azure DevOps project&nbsp; -&gt; Pipelines&nbsp; -&gt; Library&nbsp; -&gt; Variable groups.
A variable group has a name, a set of variables and associated values, and pipeline permissions. Pipeline permissions is a list of pipelines in the project that have access to the variables and corresponding values in the variable group.
The below image is of a variable group with the name <code>demo-one</code>.</span></span></span>
<img class="alignnone size-full wp-image-1770" src="https://skundunotes.com/wp-content/uploads/2022/03/63.image-2.png" alt="63.Image-2" width="576" height="341">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">This variable group has two variables <code>SecureVariable</code> and <code>UnsecureVariable</code>. The value of <code>SecureVariable</code> is masked due to the lock I placed on the value.</span></span></span>
<img class="alignnone size-full wp-image-1771" src="https://skundunotes.com/wp-content/uploads/2022/03/63.image-3.png" alt="63.Image-3" width="835" height="173">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The last step is to associate the variable group with a pipeline. Please note that the pipeline should exist before adding permissions to the variable group.</span></span></span>
<img class="alignnone size-full wp-image-1775" src="https://skundunotes.com/wp-content/uploads/2022/03/63.image-6.png" alt="63.Image-6" width="551" height="307">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I clicked on the pipeline permissions and provided the pipeline name that required access to this variable group.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In the <code>azure-pipelines.yaml</code> file, I accessed the variable group via the variables keyword. You may read more about that at <span style="text-decoration:underline;"><a href="https://docs.microsoft.com/en-us/azure/devops/pipelines/library/variable-groups?view=azure-devops&amp;tabs=yaml#use-a-variable-group" target="_blank" rel="noopener">use-a-variable-group</a></span>.</span></span></span>
<img class="alignnone size-full wp-image-1772" src="https://skundunotes.com/wp-content/uploads/2022/03/63.image-4.png" alt="63.Image-4" width="337" height="44">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">You may access the <code>azure-pipelines.yaml</code> file from my GitHub repository: <a href="https://github.com/kunduso/TestProjects" target="_blank" rel="noopener"><span style="text-decoration:underline;">TestProjects</span></a>. This pipeline only displays the values of variables accessed from the library variable group.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I triggered a pipeline run with the pipeline and library variables in place. You may access the same here - <span style="text-decoration:underline;"><a href="https://littlecoding.visualstudio.com/Open-Project/_build/results?buildId=266&amp;view=results" target="_blank" rel="noopener">kunduso.TestProjects.add-library-variables</a></span>.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">And if you access the jobs, you will see that the value of the <code>SecureVariable</code> was masked, even in the logs. While the value of <code>UnsecureVariable</code> was not.</span></span></span>
<img class="alignnone size-full wp-image-1773" src="https://skundunotes.com/wp-content/uploads/2022/03/63.image-5.png" alt="63.Image-5" width="688" height="368">
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">There are several ways to pass variables (secured and unsecured) to Azure DevOps pipelines. I found the option of using the Library variable group <strong>simple to use and sufficiently secure for my needs</strong>. I hope this note helped you understand the underlying concept of associating and accessing variables in Library and Azure Pipelines. Please do not hesitate to reach out if you have any questions or suggestions.</span></span></span>
