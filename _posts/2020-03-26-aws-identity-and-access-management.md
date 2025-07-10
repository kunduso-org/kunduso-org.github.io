---
title: "AWS identity and access management"
date: 2020-03-26 21:46:56 +0000
categories: []
tags: []
---

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Before you starting reading this post, my suggestion would be (provided you have time at hand) to re</span></span></span><span style="color:#000000;font-family:calibri;font-size:18px;">fer to information related to IAM on AWS site. These are my running notes that I was able to capture while working on IAM.</span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;"><span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">From AWS site – “AWS Identity and Access Management (IAM) enables you to manage access to AWS services and resources securely. Using IAM, you can create and manage AWS users and groups, and use permissions to allow and deny their access to AWS resources.”</span></span></span></span></span></span><!--more-->
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">IAM allows to manage access to compute, storage, database and application services in the AWS cloud. IAM uses access control concepts such as users, groups and permissions which get applied to individual API calls. This allows IAM to classify which user can access which specific service, the kind of action s/he can perform and which resources are available.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">In order to login I typed in https://us-east-1.signin.aws.amazon.com/ and then clicked on “sign in to console” button on the top right corner. This opened up a new page where I could login via two options:
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-using account ID, username and password or</span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">-using root user email</span></span></span></span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">The first time I wanted to create my account, I took the – Sign in using root user email –option. </span></span></span>
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I could create a new AWS account (new root user email) or sign in to an existing root user email. </span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">I created my user following the steps listed at <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html" target="_blank" rel="noopener">Getting Started</a>.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">Once the first user was created, I noted down the account ID, IAM username and password and logged out of my root login.
<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">My next login was using the IAM user that I created in the previous step.</span></span></span></span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">IAM is an interesting concept and AWS has really good resources around that. I think it is worth the time to understand how IAM manages security for anyone who wants to be proficient in managing resources in AWS.</span></span></span>

<span style="font-size:18px;"><span style="font-family:calibri;"><span style="color:#000000;">More information at https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html</span></span></span>
