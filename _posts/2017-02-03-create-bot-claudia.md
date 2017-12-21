---
layout: post
title: Creating a Bot on Messenger
cover: cover.jpg
date:   2017-02-03 12:00:00
categories: Conversational
---

# Conversational Interfaces
Last semester I started experimenting with conversational interfaces. The first Bot I developed was an Alexa skill for my department (which is still under development). I quickly realized the potentialities of Bots as a means to facilitate all those low-level activities which are better performed without any human interaction. Why would you want to send an e-mail or call a 1-800 when you can just chat with someone (or something in our case)?!  
There are multiple platforms where you can create bots, but the turning point was the release of FB Messenger SDK in April 2016. So excited by the momentum of conversational interfaces I decided to join the revolution and start developing Messenger bots. Following are the best practices I have learning so far.

# Create a Bot with Claudia-Bot-Builder
One of the most significant features an app/service must have in 2017 is scalability. Therefore I decided to host my Bot on Amazon Web Services (AWS). The Lambda service was just perfect for my needs, as it offered quick deployment time and no need for maintenance.  
<span style="color:green">Node.js</span> has emerged as the standard for developing bots, in fact it is fully supported by Slack, Messenger, Alexa, and many others.  
I soon discovered a Node.js library that was perfect for the deployment of bots on AWS, namely Claudia-Bot-Builder. The library enables to develop the bot using semplified templates wich increase development time and readabily of the code. In addition to Messenger, the library offers the possibility to create bots on multiple platforms (e.g. Slack, Telegram, Kik). What is great about claudia-bot-builder is that it automatically generates the Webhook URL and the security key required by Messenger to communicate to AWS.  

Following are the main steps to deploy your bot.

## Deploy your bot on AWS using Claudia-Bot Builder
When creating a new project the first thing to do is start a new npm project.
{% highlight shell %}
#initiate a new npm project
npm init
#download the dependencies
npm install
{% endhighlight %}  

<br>
To deploy the bot on AWS we'll need to use the claudia library. I intalled the library as a global one and use it only as a development dependency for my project. If you already have claudia installed on your machine you might want to update it to the lastest release.

{% highlight shell %}
#install claudia
npm install claudia -g
#update claudia
npm update claudia -g
{% endhighlight %}  
<br>
In the file package.json, we have to add the following dependencies:

{% highlight json %}
 "dependencies": {
    "claudia-bot-builder": "^"
  },
  "devDependencies": {
    "claudia": "^"   
  }
{% endhighlight %}  
<br>
Once our bot is developed we're ready to deploy it on AWS ([see the code for one of the bots I created](https://github.com/zampolli75/berna_personalization_bot)). Firstly we will launch the claudia create command, which will automatically create a new IAM role, and will deploy our code on Lambda.  

{% highlight shell %}
claudia create --region us-east-1 --api-module bot
{% endhighlight %}
<br>
The next step is to to get the Webhook URL and verification token to connect to Messenger. To do this we'll launch the update command for Messenger.

{% highlight shell %}
claudia update --configure-fb-bot
{% endhighlight %}
<br>
The command will return the Webhook URL and the verificantion token to input on Messenger. Afterwards you will be prompted to input the Facebook page access token, and the Facebook App Secret.  

That is the last step! You can start testing your bot on Messenger. 