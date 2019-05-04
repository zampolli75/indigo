---
title: "Create a Lambda Function on AWS"
layout: post
date: 2019-02-22 08:00
image: /assets/images/markdown.jpg
headerImage: false
projects: false
hidden: false # don't count this post in blog pagination
tag:
- aws
- cli
- lambda
star: false
category: blog
author: jrod
description: This guide explains how create a Lambda function on AWS
externalLink: false
---

# Lambda
Lambda functions are the best way I know to release a sotware product that needs to be fast, scalable, secure, always available, and inexpensive.  

Lambda functions are serverless, meaning that they don't require the users to provision, scale, and manage any server. Lambda functions are available in a variety of runtimes (e.g., node.js, python). Unfortunately, at the moment there is not R runtime, although there are some interesting workarounds to execute R code.

# Configuration
There are several ways to create Lambda functions. However, to my satisfy my use cases the best approach is a combination of the [Serverless]() and [Virtualenv]() services.  

## Serverless
Serverless allows the deployment of Lambda functions directly from the terminal of your client. Serverless frees the user from managing the security roles, package creation, and updating of the project. This allows a fast and reliable deplyment service. Serverless requires the installation of Docker, as when 

## Virtualenv
Virtualenv is funcdamental for my use cases as it allows to upload to Lambda some packages that are not available in the runtime (e.g., numpy). 

Following are some useful resources:

-  [Serverless python requirements](https://www.npmjs.com/package/serverless-python-requirements)

-  [Serverless framework guide](https://serverless.com/framework/docs/providers/aws/guide/deploying)

-  [Virtual env documentation](https://virtualenv.readthedocs.io/en/latest/installation/)

-  [Installation of python packages in virtualenv](https://packaging.python.org/guides/installing-using-pip-and-virtualenv/)



Command to install packages required by the project.

{% highlight shell %}
pip install -r requirements.txt
{% endhighlight %}

