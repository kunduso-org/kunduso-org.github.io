---
title: "My experiments with git and github -Part 2"
date: 2019-12-11 06:23:36 +0000
categories: []
tags: []
---

<span style="color:#000000;"><span style="font-size:18px;"><span style="font-family:calibri;">In my previous note, I documented a few simple use cases for someone who is getting started with using git and github. In this note I am going to focus on a few more use cases particularly around: clone, branch, merge and rebase.</span></span><span style="font-size:18px;"><span style="font-family:calibri;">For all the use cases below, I am going to assume that git for windows is installed</span></span><!--more--></span>

<span style="color:#000000;"><strong><span style="font-size:18px;"><span style="font-family:calibri;">UseCase: How do I download code from a remote git repo to work on that?</span></span></strong>
<span style="font-size:18px;"><span style="font-family:calibri;">A lot of times you'd want to work or maybe just review a code base that already exists (for e.g. in github). While you can always review the code in github but if you have to update the code that is already there you'd want the code downloaded onto your local computer. This is known as clone. Say we want to work on a code base: https://github.com/kunduso/TestProjects.git Lets get back to our command prompt and create a folder where we want to clone the git repo and call it "FirstGitProject-Clone" followed by a git clone command.</span></span></span>
<span style="color:#000000;"><img class="alignnone size-full wp-image-189" src="https://skundunotes.com/wp-content/uploads/2019/12/mewgagpart2-image1.png" alt="MEWGAGPart2-Image1" width="737" height="125" /></span>
<span style="font-size:18px;color:#000000;"><span style="font-family:calibri;">This will download the latest code base from github to your local folder from where the clone command was executed and you are ready to make changes to the code base and work on it as any other code base. Once your changes are in place, its the same sequence:</span></span>
<code></code>
<pre>git pull
git add .
git commit "${enter your comments}"
git push</pre>
<code></code>

<span style="color:#000000;"><img class="alignnone size-full wp-image-190" src="https://skundunotes.com/wp-content/uploads/2019/12/mewgagpart2-image2.png" alt="MEWGAGPart2-Image2" width="769" height="190" /></span>
<span style="font-size:18px;color:#000000;"><span style="font-family:calibri;">Now you should be able to view your comments on github along with history of how the codebase has evolved/changed.</span></span>
<span style="color:#000000;"><img class="alignnone size-full wp-image-191" src="https://skundunotes.com/wp-content/uploads/2019/12/mewgagpart2-image3.png" alt="MEWGAGPart2-Image3" width="991" height="370" /></span>
<span style="font-size:18px;color:#000000;"><span style="font-family:calibri;"><strong>UseCase: How do I create a branch in my local git repo?</strong></span></span>
<span style="color:#000000;"><span style="font-size:18px;"><span style="font-family:calibri;">Branching is a mid level feature in git. You might not have to use it if your code base is new and not yet released to production. However after your first release, you'd want master branch to be as close to production as possible. Alternatively, you might have a pretty updated code base in master and instead of committing your changes to master you feel comfortable to make them in a local branch and once the feature/user story/bug you were working on looks good, you are ready to merge your changes to master. In this case it is a good idea to create a branch (copy) of your code from master and work (make commits) there. There are two ways to create a new git branch.</span></span></span>
<span style="font-size:18px;color:#000000;"><span style="font-family:calibri;">Option 1: </span></span>
<pre><span style="color:#000000;"><code>git branch ${NewBranchName}
git checkout ${NewBranchName}</code></span></pre>
<span style="font-size:18px;color:#000000;"><span style="font-family:calibri;">Option 2: </span></span>
<pre><span style="color:#000000;"><code>git checkout -b ${NewBranchName}</code></span></pre>
<span style="font-size:18px;color:#000000;"><span style="font-family:calibri;"><strong><em>-b</em></strong> in git checkout command tells Git to create a new branch before checking it out.</span></span>
<span style="color:#000000;"><span style="font-size:18px;"><span style="font-family:calibri;">git branch <em><strong>-a</strong></em> is also a good command to review which branch you are working on. The * will be against the branch that is checked out (you are currently working on it).</span></span><img class="alignnone size-full wp-image-192" src="https://skundunotes.com/wp-content/uploads/2019/12/mewgagpart2-image4.png" alt="MEWGAGPart2-Image4" width="629" height="260" /></span>
<span style="color:#000000;"><span style="font-size:18px;"><span style="font-family:calibri;">Now that we are have the NewBranch checked-out let me make a couple of commits there to create some history.</span></span><img class="alignnone size-full wp-image-193" src="https://skundunotes.com/wp-content/uploads/2019/12/mewgagpart2-image5.png" alt="MEWGAGPart2-Image5" width="672" height="142" /></span>
<span style="color:#000000;"><span style="font-size:18px;"><span style="font-family:calibri;">With history, I can now push my changes to github</span></span><img class="alignnone size-full wp-image-194" src="https://skundunotes.com/wp-content/uploads/2019/12/mewgagpart2-image6.png" alt="MEWGAGPart2-Image6" width="599" height="209" /></span>
<span style="color:#000000;"><span style="font-size:18px;"><span style="font-family:calibri;">..and we can review this on github</span></span><img class="alignnone size-full wp-image-195" src="https://skundunotes.com/wp-content/uploads/2019/12/mewgagpart2-image7.png" alt="MEWGAGPart2-Image7" width="990" height="305" /></span>
<span style="color:#000000;"><strong><span style="font-size:18px;"><span style="font-family:calibri;">UseCase: How do I merge?</span></span></strong></span>
<span style="color:#000000;"><span style="font-size:18px;"><span style="font-family:calibri;">After branch we work on merge. There have been a lot of stackoverflow questions of what is the right way to merge and I will be pointing to one such that I found informative: </span></span><span style="font-size:18px;"><span style="font-family:calibri;">https://stackoverflow.com/questions/5601931/what-is-the-best-and-safest-way-to-merge-a-git-branch-into-master</span></span>
<span style="font-size:18px;"><span style="font-family:calibri;">In our case, we first move to master as our active branch and then issue a git merge command from there. Git merges the specified branch to its current active branch. The format is:</span></span></span>
<pre class="wp-block-preformatted"><span style="color:#000000;"><code>git checkout master
git merge ${NewBranchName}</code></span></pre>
<span style="color:#000000;"><span style="font-size:18px;"><span style="font-family:calibri;">If there are no merge conflicts, these changes are committed to master and you can do a <em><strong>git -log</strong></em> to check commit history</span></span><img class="alignnone size-full wp-image-196" src="https://skundunotes.com/wp-content/uploads/2019/12/mewgagpart2-image8.png" alt="MEWGAGPart2-Image8" width="582" height="292" /></span>
<span style="font-size:18px;color:#000000;"><span style="font-family:calibri;">Once merge is done, upload the code to your github using the command</span></span>
<span style="color:#000000;"><code>git push origin master</code></span>
<span style="color:#000000;"><img class="alignnone size-full wp-image-197" src="https://skundunotes.com/wp-content/uploads/2019/12/mewgagpart2-image9.png" alt="MEWGAGPart2-Image9" width="577" height="70" /></span>
<span style="font-size:18px;color:#000000;"><span style="font-family:calibri;">And this is how we merge. There is also something known as <em><strong>squash</strong></em>  where the command looks like:</span></span>
<pre class="wp-block-preformatted"><span style="color:#000000;"><code>git merge --squash NewBranch</code></span></pre>
<span style="font-size:18px;color:#000000;"><span style="font-family:calibri;">This command is followed by a git commit to commit the changes to master branch. As you have noticed in this example, when changes from NewBranch are merged to master, all the commits in the NewBranch are now logged under master branch -commit by commit. What if I do not want to see all the changes from the NewBranch? What if I am interested only in the final version of NewBranch rather than the individual commits in the branch. We use the <em><strong>squash</strong></em> option. Using this, you are combining all the commits into one commit which also makes it easier to cancel out the noise of individual commits.</span></span>
<span style="color:#000000;"><img class="alignnone size-full wp-image-198" src="https://skundunotes.com/wp-content/uploads/2019/12/mewgagpart2-image10.png" alt="MEWGAGPart2-Image10" width="906" height="359" /></span>
<span style="color:#000000;"><span style="font-size:18px;"><span style="font-family:calibri;">There are different scenarios when you'd want to use <em><strong>squash</strong></em> and when not. When merging from my individual branch to master, I prefer adding <em><strong>--squash</strong></em> option because that way, the commit history in master looks clean </span></span><span style="font-size:18px;"><span style="font-family:calibri;">...and then out of habit, I like to save this work in github.</span></span></span>
<pre><span style="color:#000000;"><code>git push origin master</code></span></pre>
<span style="color:#000000;"><img class="alignnone size-full wp-image-199" src="https://skundunotes.com/wp-content/uploads/2019/12/mewgagpart2-image11.png" alt="MEWGAGPart2-Image11" width="640" height="156" /></span>
<span style="font-size:18px;color:#000000;"><span style="font-family:calibri;">There is also something known as rebase and it is different than merge. Ideally you'd not want to rebase your master branch from any other branches but rebasing your feature/local branch from master should be totally okay. I'll try to list a few good urls that I used to understand this concept.</span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Reference:</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">https://git-scm.com/docs/git-branch</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">https://git-scm.com/docs/git-merge</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">https://git-scm.com/docs/git-rebase</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">https://www.youtube.com/watch?v=CRlGDDprdOQ</span></span></span>

