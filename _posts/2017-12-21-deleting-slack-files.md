---
title: "Deleting Slack files in an automated manner"
layout: post
date: 2017-12-22 12:00
image: /assets/images/posts/brilliant-idea.gif
headerImage: false
projects: false
hidden: false # don't count this post in blog pagination
tag:
- slack
star: false
category: blog
author: jrod
description: How to detele in bulk all the slack files
externalLink: false
---

Reaching the file storage limit allowed on Slack the free plan (5GB) is quite a pain. Every time you try to upload something after hitting the limit you get a message alert reminding you about it. Also, deleting files manually is time-consuming and annoying. 
Luckily others came to the rescue writing automated scripts that can delete files older than 30 days automatically leveraging Slack API.

## Get the API token
The first step is to get your private Slack token to use in the script. You can get yours from the [Slack API](https://api.slack.com) site in the [Legacy tokens section](https://api.slack.com/custom-integrations/legacy-tokens).

## Download the script
The script that contains the code to delete the files can be downloaded from [my Github account](https://gist.github.com/zampolli75/f303029d16e55e3a5366e4a2e110e4c8).  The script runs on Python; therefore we assume you have it [installed on your machine](http://docs.python-guide.org/en/latest/starting/install3/osx/) and know how to [install the libraries used in the script](https://packaging.python.org/tutorials/installing-packages/).
From the original code of [jackcarter](https://gist.github.com/jackcarter/d86808449f0d95060a40) I change a line of code that was not compatible with Python 3. Therefore, be aware you might have to change this part of the code (line 31-32) depending on the version you are using.

Open the file and paste your private API token in line five where you find the following code:

```python
token = 'paste your token here'
```

Once you are done save the file.

## Run the script
The easiest way to run the script is from the terminal. To run it on my Mac I have to write:

```shell
python slack_delete.py 
```

You might have a different alias for python, such as python2 or python3 (depending on the version).