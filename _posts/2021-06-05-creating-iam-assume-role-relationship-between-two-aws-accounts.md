---
title: "Creating IAM assume-role relationship between two AWS accounts"
date: 2021-06-05 12:00:00 +0000
categories: []
tags: []
---

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">In this post, I discuss step by step using AWS CLI how to create a <strong>trust relationship</strong> between a user in the <strong>Trusted account</strong> and a role in the <strong>Trusting account</strong>. The idea is, in the end, we will have the credentials of a user in the Trusted AWS account that can manage resources in the Trusting AWS account.</span></span></span>
<!--more-->
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Note:</strong> If you work with AWS, IAM is a necessary concept, and I think time spent understanding that is rewarding. AWS docs are the best place to learn in detail about them. Here is the link to <span style="text-decoration: underline"><a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html">AWS Docs</a></span>. </span></span></span><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I was particularly interested in AWS IAM Roles and found this <span style="text-decoration: underline"><a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html">note interesting</a></span>.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">So that we are all on the same page, here are some entities that I'll be referring to. Two AWS accounts -a <strong>trusted account</strong> and a<strong> trusting account</strong>, an AWS <strong>IAM user</strong> created in the trusted account, a <strong>role</strong> created in the trusting account, an <strong>STS policy</strong> attached to the user in the trusted account, and an <strong>assume rule policy</strong> and a <strong>permissions policy</strong> attached to the role in the trusting account.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">A role in AWS IAM is similar to an AWS IAM user, with specific policies attached that determine the level of access to the resources listed in the policy. However, <em>a role does not have an access key and secret key that an IAM user has</em>. A role is assumed by an IAM user from the same account or from a different account with whom a trust relationship is set.</span></span></span>

<strong><span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">How does this work?</span></span></span></strong>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This concept involves two AWS accounts, and hence there is a list of actions to be performed on the trusting account and the trusted account. Before we delve deeper, let me differentiate between the two. <strong>A trusting account hosts the role </strong>to perform operations in the account. <strong>The trusted account hosts the user, </strong>which assumes the role in the trusting account.</span></span></span>
<img class="alignnone size-full wp-image-1302" src="https://skundunotes.com/wp-content/uploads/2021/06/42.-image-2.png" alt="42. Image-2" width="570" height="343" />
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">In 8 steps, I list how to setup the relationship using AWS CLI. Steps 1 to 4 are performed on the <strong>trusting account side </strong>and the rest on the <strong>trusted account side</strong>. And since we're using the AWS CLI, ensure that we are logged into the correct account.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 1:</strong> Create a policy document JSON-file that has a Principal associated with the <code>sts:AssumeRole</code> action.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">This policy states that any user that belongs to <code>Principal</code> can assume a role using this policy.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Here is the gist of that file.</span></span></span>
https://gist.github.com/kunduso/2449d127c428cc53e619b0bfa7e88d3a
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I stored that file as <code>assume-role-policy.json</code> in a folder where I ran the rest of the AWS CLI commands.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 2:</strong> Create a role and associate the above policy document with that role.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The association implies that the role can now be assumed by any user that belongs to the <code>Principal</code> specified in the policy. The command to run is:</span></span></span>
<code>aws iam create-role --role-name "Assume-Role-1" --assume-role-policy-document file://assume-role-policy.json</code>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The ARN of the role was included in the output I received. Please take note of that. It will be something like: <code>"Arn": "arn:aws:iam::$(TrustingAccountID):role/Assume-Role-1"</code></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 3:</strong> Create a policy from a policy document JSON-file that lists the kind of access and to which AWS resources.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Here is the JSON file of the policy which provides full access to S3.</span></span></span>
https://gist.github.com/kunduso/0b031b9735e96aaf819ca7172b817df4
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I saved the policy document as <code>assume-role-policy.json</code> and used the below command to create the policy from the above policy document.</span></span></span>
<code>aws iam create-policy --policy-name Custom-Policy-For-Role-1 --policy-document file://role-policy.json</code>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The ARN of the policy was included in the output. Please take note of that. It will be like:<code>"Arn": "arn:aws:iam::$(TrustingAccountID):policy/Custom-Policy-For-Role-1"</code></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 4:</strong> Attach policy to the role</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Attach the policy created in step 3 to the role created in step 2. This implies that the role has (a) permissions to perform specific actions on particular AWS resources only and (b) can be assumed by a user from the trusted account. The command to run that is:</span></span></span>
<code>aws iam attach-role-policy --role-name "Assume-Role-1" --policy-arn "arn:aws:iam::$(TrustingAccountID):policy/Custom-Policy-For-Role-1"</code>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">There is no output if the association is successful.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Now switch to the trusted account on AWS CLI.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 5:</strong> Create a user in the account</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I used the below command to create a user.</span></span></span>
<code>aws iam create-user --user-name "user-1"</code>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The ARN of the user was included in the output. Please take note of that. It will be something like: `"Arn": "arn:aws:iam::$(TrustedAccoutID):user/user-1"`</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 6:</strong> Create an <code>sts:AssumeRole</code> action policy from a policy document
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Create a policy from a policy document JSON-file that allows the <code>sts:AssumeRole</code> action on the role in the trusting account.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Here is the gist of that file.</span></span></span>
https://gist.github.com/kunduso/ecd91fb780ffaf4cd5dbe5bc80ceeaec
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The command to run is:</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><code>aws iam create-policy --policy-name Custom-Assume-Role-Policy --policy-document file://assume-role-policy-trusted.json</code></span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">The ARN of the policy was included in the output. Please take note of that. It will be like: <code>"Arn": "arn:aws:iam::$(TrustedAccoutID):policy/Custom-Assume-Role-Policy"</code></span></span></span></span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 7:</strong> Attach policy to the user</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">Attach the policy created in step 6 to the user created in step 5. I used the below command to attach the policy with the user.</span></span></span>
<code>aws iam attach-user-policy --policy-arn "arn:aws:iam::$(TrustedAccountID):policy/Custom-Assume-Role-Policy" --user-name "user-1"</code>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">There is no output if the association is successful.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000"><strong>Step 8:</strong> Generate access and secret key for the user and store them securely.</span></span></span>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I used the below command to generate access keys for the user.</span></span></span>
<code>aws iam create-access-key --user-name "user-1"</code>
<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">I received the output that contained the <strong>AccessKeyId</strong> and <strong>SecretAccessKey.</strong> Store these keys securely.</span></span></span>

<span style="font-size: 18px"><span style="font-family: calibri"><span style="color: #000000">After following the above eight steps, <strong>I managed resources in the trusting account using the credentials of the user in the trusted account. </strong>I hope you found this note helpful, and let me know if there are any questions or suggestions.</span></span></span>
