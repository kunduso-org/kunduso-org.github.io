---
title: "My experiments with git and github"
date: 2019-03-03 06:19:59 +0000
categories: []
tags: []
---

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">What is git?</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><img class="  wp-image-126 alignleft" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image1.jpg" alt="gitandgithub-image1" width="323" height="215" />A few years back I heard about this super awesome distributed version control tool that did not need me to be connected to the server all the time I was working on code (I was/am a TFVC/Azure Devops user). So not being connected to the server sounded really cool because that was not the case for me. I not only had to have Visual Studio open but also have the files checked-out before I could start working on them. So a version control system that kept track of the files as I made changes to them along with me not being connected to the server was awesome. And its free? What? So code stays on my laptop under version control... what if I lose my laptop or it crashes? How do I share my work with others? Do I lose all the work? Is there a centralized repo? Enter github.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">What is GitHub?</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">An online repository for your local git repository. Kind of like the server location of your local repository (as long as you've added them. More later in use cases).</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">How do the two tie up? Do I need both to be set up?</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Both could be set up individually without depending on the other. However, once the setup is done, it is a good idea to tie them up when you have to work on your code on your laptop and save it elsewhere.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">There is a lot of good documentation around git, and I refer to that from time to time.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><a href="https://git-scm.com/book/en/v2">https://git-scm.com/book/en/v2</a></span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">For the purpose of this exercise, I am going to create a folder and use that as my sandbox. C:\MyTemporaryGitSandbox</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">UseCase: Setup a GitHub profile</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Please visit <a href="https://github.com/">https://github.com/</a> to create your profile.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">UseCase: Setup a new remote repository in GitHub</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">(you need to have a GitHub profile already created)</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">After creating a profile or after logging in, at the far top right corner, you will see an option to create a repository</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><img class=" size-full wp-image-127 alignleft" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image2.png" alt="gitandgithub-image2" width="185" height="187" /></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This is like creating a server location for your project with a unique name. Also, notice that this repository gets created under your user id.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The setup is quite intuitive and easy to understand. Now let us install git on our local (laptop).</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">UseCase: Install and set up git on your computer</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><a href="https://git-scm.com/download/win">https://git-scm.com/download/win</a> ...installation is quite simple and easy to follow.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Once installation is done, open a command prompt and type the following command:</span></span></span>
<pre><code>git --version</code></pre>
<code>
</code>

<code></code>
<img class="alignnone size-full wp-image-128" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image3.png" alt="gitandgithub-image3" width="567" height="157" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">(-if you are familiar with vs code and prefer using the terminal (Ctrl+`) and type in same command)</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This will list the version of git that is installed on your local.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">After checking the version lets create your local git profile. You do that by the following commands:</span></span></span>
<pre><code>git config --global user.name "FirstName LastName"</code></pre>
<code>
</code>

<code></code>
<pre><code>git config --global user.email youremailid</code></pre>
<code>
</code>

<code></code>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Once done, check your local configs by the command:</span></span></span>
<pre><code>git config --list</code></pre>
<code>
</code>

<code></code>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This will list all the settings that git will be using for your local repository.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">So up until this point you have a remote github repository created and you have a installed and configured git on your local. Now lets checkin some code into our local repository.</span></span></span>
<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">UseCase: How do I add/checkin code to my local git repo?</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I created a folder inside the sandbox folder after navigating there, ran following command:</span></span></span>
<pre><code>git init</code></pre>
<code>
</code>

<code></code>
<pre><code><img class="alignnone size-full wp-image-129" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image4.png" alt="gitandgithub-image4" width="680" height="47" /></code></pre>
<code>
</code>

<code></code>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Behind the scene, a new Git repository was created in FirstGitProject folder along with a .git hidden folder. There is a .config file that stores information related to this repository.</span></span></span>
<img class="alignnone size-full wp-image-130" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image5.png" alt="gitandgithub-image5" width="454" height="185" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">After initializing an empty git repository, I ran the next set of commands which pertain to creating a README.md file and then committing the changes into my local git repository.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">These set of commands created a README.md file and checked that into my local git repository.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Commands:</span></span></span>
<pre><code>git add README.md</code></pre>
<code>
</code>

<code></code>
<pre><code>git commit -m "first commit"</code></pre>
<code>
</code>

<code></code>
<img class="alignnone size-full wp-image-131" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image6.png" alt="gitandgithub-image6" width="674" height="135" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">To check your commit history, type in the command:</span></span></span>
<pre><code>git log --oneline</code></pre>
<code>
</code>

<code></code>
<img class="alignnone size-full wp-image-132" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image7.png" alt="gitandgithub-image7" width="503" height="44" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">In this case, since there is only one commit, we are seeing that. Now we are going to upload our code from our local git repo to a remote repo and for that we need to first link them.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">UseCase: Link your github repo with your local git repo</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">When we created a remote repository in github we landed on a page somewhat similar to:</span></span></span>
<img class="alignnone size-full wp-image-133" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image8.png" alt="gitandgithub-image8" width="985" height="501" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">To link our local empty repository with our remote repository run the command:</span></span></span>
<pre><code>git remote add origin https://github.com/kunduso/TestProjects.git</code></pre>
<code>
</code>

<code></code>
<img class="alignnone size-full wp-image-134" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image9.png" alt="gitandgithub-image9" width="747" height="39" /><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This updates the config file in our local git repo as shown below:</span></span></span>
<img class="alignnone size-full wp-image-135" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image10.png" alt="gitandgithub-image10" width="489" height="227" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">next we run the command:</span></span></span>
<pre><code>git push -u origin master</code></pre>
<code>
</code>

<code></code>
<img class="alignnone size-full wp-image-136" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image11.png" alt="gitandgithub-image11" width="809" height="130" /><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This adds more details to our config file in our local git repo:</span></span></span>
<img class="alignnone size-full wp-image-137" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image12.png" alt="gitandgithub-image12" width="512" height="273" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">With this change we have successfully pushed out local changes to remote github repository. You may check that in your remote repository.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Note: If you try to add a remote repository without having a local git repository setup this should throw an error:</span></span></span>
<img class="alignnone size-full wp-image-138" src="https://skundunotes.com/wp-content/uploads/2019/10/gitandgithub-image13.png" alt="gitandgithub-image13" width="747" height="63" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This means that we need to have a local repository created before we can link that to our github remote repository. Go to  UseCase: How do I add/checkin code to my local git repo?</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Reference:</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><a href="https://git-scm.com/downloads">https://git-scm.com/downloads</a></span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><a href="https://github.com/features">https://github.com/features</a></span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><a href="https://git-scm.com/docs/git-init">https://git-scm.com/docs/git-init</a></span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><a href="https://git-scm.com/docs/git-add">https://git-scm.com/docs/git-add</a></span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><a href="https://git-scm.com/docs/git-commit">https://git-scm.com/docs/git-commit</a></span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><a href="https://git-scm.com/docs/git-log">https://git-scm.com/docs/git-log</a></span></span></span>
