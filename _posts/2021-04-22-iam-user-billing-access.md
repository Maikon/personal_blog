---
title: My IAM admin user can't access billing. Why?
layout: post
tag:
- aws
- iam
- aws billing
- iam permissions
- documentation
category: blog
image: assets/images/puzzled.jpeg
---

TL;DR
- if you want IAM users to access billing, a key step is to [activate this option as a root user](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/control-access-billing.html#ControllingAccessWebsite-Activate)
- the above assumes that you also have the correct IAM policies attached on your IAM user that allow access to billing
- AWS documentation is verbose enough that you can miss key information
- these things happen even if you not new to AWS (I've been using AWS for some time now)

I recently decided to start experimenting again with a side project on AWS. I try as much as possible to follow AWS best practices, especially around security. One such practice is not interacting with AWS resources with your root user account but instead via a dedicated IAM user with only the necessary permissions.

After I created a dedicated IAM user, I gave this user admin permissions. I created my project using various services (i.e Lambda, API Gateway etc). After a month or so, I was curious to see how much all that activity on AWS was costing me. I went to the AWS console, clicked on "Billing" and low-and-behold, I see the message that usually means I'm about to enter another AWS IAM rabbit-hole. "You Need Permissions":

[![billing permissions error message](/assets/images/aws/iam/billing_0.png)](/assets/images/aws/iam/billing_0.png)

I had to do a double-take on that as I was convinced what I was seeing was wrong. My IAM user is an admin. An admin can see and do everything, I kept telling myself. Of course, when you analyse the best practice mentioned above, it's clear that a root user is, or should be, different to an IAM generated admin user, otherwise there's no point doing this process.

Developers of all experience levels are notorious for rushing and not properly analysing an error message and I'm no exception (pun intended). I had another look at the AWS permissions error message and clicked on the _**"this account allows IAM and federated users to access billing information"**_ link. This led me to click on another link and then wade through the sea of text to figure out what was wrong.

To save you time, here's what you need to do:

1. Click on **"My Account"** on the menu that unfolds on the top right when you click on your account name:
[![click "my account" on the AWS console](/assets/images/aws/iam/billing_1.png)](/assets/images/aws/iam/billing_1.png)


2. Scroll down until you find **"IAM User and Role Access to Billing Information"** and click on **"Edit"**:
[![click "edit" to edit access to billing information](/assets/images/aws/iam/billing_2.png)](/assets/images/aws/iam/billing_2.png)


3. Tick the checkbox titled **"Activate IAM Access"** and then hit **"Update"**:
[![tick checkbox to activate IAM access](/assets/images/aws/iam/billing_3.png)](/assets/images/aws/iam/billing_3.png)

You can now sign in with your IAM user and see the billing information 🎉
