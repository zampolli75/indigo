---
title: "Start Node app with dotenv library"
layout: post
date: 2018-01-12 08:00
image: /assets/images/markdown.jpg
headerImage: false
projects: false
hidden: false # don't count this post in blog pagination
tag:
- node
- mongo
star: false
category: blog
author: jrod
description: Start Node app with dotenv library
externalLink: false
---

To deploy a Node application in multiple environments I decided to use the PM2 manager. This decision revealed detrimental when using the development environment. In fact, every time I make a change PM2 has to restart the application, and I have to log into it every time to visualize the changes I just made.  
After some research, I came across a Node library that allows defining variable names when starting the process and that does not require to reboot the application everytime you make changes to the app.  

Following I will explain how to install and run the application form the terminal using [dotenv](https://github.com/motdotla/dotenv).  

# Install
Since I will be using the dotenv package only in the development environment, I added the package dependency in the `package.json` file under `devDependencies`.

{% highlight json %}
  "devDependencies": {
    "dotenv": "^4.0.0"
  }
{% endhighlight %}

To install the package, you have to run from the terminal (in the directory) of the project the following command:

{% highlight shell %}
$ npm install
{% endhighlight %}

# Configure
To define the environment variables, we create a file in the project directory named ```.env```. The dotenv library by default looks for the ```.env``` filename. If you want to use a different filename you have to specify its path and name as an argument when you run the application.  

# Run Application
Once the variables are defined in the `.env` file we are ready to run the app from the terminal line.  
Since we are only using this library in the development environment, we do not want to upload this library to the test and production environments. To avoid having to require the package inside the `app.js` we can preload the library from the terminal when starting the application. This solution allows to use the library only when requested (in the development environment in our case) and do not undermine the performance of the other environments with unnecessary libraries.  

{% highlight shell %}
$ sudo node --require dotenv/config app.js
{% endhighlight %}

Note: Since the application, I am using requires Mongo to authenticate the users, we need to have in parallel in a different terminal tab the `mongod` process running.  

If you did not get any errors the application should be running and accessible from your browser at your localhost address (by default `127.0.0.1:4000`.)