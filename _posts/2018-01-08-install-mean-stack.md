---
title: "Install Conponents to Run Locally a MEAN Stack Application"
layout: post
date: 2018-01-08 08:00
image: /assets/images/markdown.jpg
headerImage: false
projects: false
hidden: false # don't count this post in blog pagination
tag:
- mean
- mongo
- node
star: false
category: blog
author: jrod
description: How to install configure your machine to run MEAN applications locally (MAC)
externalLink: false
---

# The Project
I have been working on a team project to develop an Application that runs on the MEAN stack (actually it does not use Angular.js but to simplify I will refer to the MEAN stack anyway).  
The project is hosted as a private repository in Github.com and therefore requires Git to pull and push new changes.  
This post is intended as a quick-start to the new collaborators on the project that do not have any previous knowledge of the MEAN stack, Git, or the use of the Terminal. The first version of this post is specific to MacOS. However, I plan to do a similar post for Windows users in the future.

# Install Sublime Text Editor
In case you do not already have a text editor for the code I suggest to install [Sublime](http://www.sublimetext.com). 

# Install Xcode
Xcode is an integrated development IDE used mainly to develop programs for the MacOS and iOS. When installing Xcode however it makes available a lot of functionalities that are useful when coding and programming (e.g. Git). Although Xcode installs a version of Git, we can always install a different one using Homebrew (see below). Installing a different version of Git might be useful if you want to have full control over the version used and when to update it. In case you want to install a second version of Git you will have to define the default version in the [```.bash_profile```](https://apple.stackexchange.com/questions/93002/how-to-use-the-homebrew-installed-git-on-mac).

# Install Homebrew
[Homebrew](https://brew.sh) is a package manager for MacOS that allows to effortlessly install packages with a simple command from the terminal line.  
To install Homebrew open the terminal and run the following command:  

{% highlight shell %}
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

Afterward, type the commands to check that you successfully install Homebrew:

{% highlight shell %}
$ brew update
$ brew doctor
{% endhighlight %}


In additions to installing new libraries, Homebrew makes really easy their update process.

# Git and Github
The next step in configuration process is to configure Git and Github to be able to clone locally the project repository.  

## Git
To check that Git was correctly installed with Xcode we can open the terminal and execute the following commands:

{% highlight shell %}
$ git --version
$ which git
{% endhighlight %}

Using Git from the terminal can be harsh without any visual cue of the branch you are working on, or the state of the changes performed. Also, autocomplete features are quite useful when working from the terminal. To set up the terminal in the best way to work with Git you should follow these steps:  
-  For autocomplete features place in the home directory of your Mac [this file](https://gist.github.com/zampolli75/2b623bcfba93f366cc446e3d2e176672) with name ```git-completion.bash```.
-  For prompt help features place in the home directory of your Mac [this file](https://gist.github.com/zampolli75/3024ae4aca5f52aa37ef2a5a78996787) with name ```git-prompt.sh```.
-  From the home directory of your, Mac check whether you already have a file named ```.bash_profile```. If you have it already, you will have to open it and modify it. If you do not have it create it typing ```touch .bash_profile```. Following open the file create or already present and add the [following commands](https://gist.github.com/zampolli75/fd7562568c90f7a42932a73d54d236d1).  

Afterward, restart the terminal to apply the changes. 

Other handy configurations to Git that I appreciate are the following:  
-  Set Sublime as default text editor:
{% highlight shell %}
$ git config --global core.editor "'/Applications/Sublime\ Text.app/Contents/MacOS/Sublime\ Text' -n -w"
{% endhighlight %}
-  Set a [conflict resolution style](http://psung.blogspot.com/2011/02/reducing-merge-headaches-git-meets.html) that allows to easily interpret conflicts:  
{% highlight shell %}
$ git config --global merge.conflictstyle diff3
{% endhighlight %}
-  The following config to consider the current branch as default when doing a push. The name of the current branch will have to match the name of the remote branch in order to run successfully ```$ git push remote```.
{% highlight shell %}
$ git config --global push.default upstream
{% endhighlight %}


Now Git is correctly installed and configured to work efficiently and effectively.

## Github
As a student, you can access for free the [Github Educational Pack](https://education.github.com) to have access to unlimited private repositories. The pack contains other nice perks, such as Travis Premium account and AWS credits.  
To allow to easily push and pull changes to and from the remote we can configure your Github credentials locally so you do not have to specify them each time. I prefer to use the SSH connection to connect to Github, so this is the configuration I will explain. You can also configure other forms of authentication using the [Keychain app present in your Mac](https://help.github.com/articles/caching-your-github-password-in-git/).  

First of all you have to check whether you already have [stored on your machine an SSH key](https://help.github.com/articles/checking-for-existing-ssh-keys/#platform-mac). To do that open the terminal and type:
{% highlight shell %}
$ ls -al ~/.ssh
{% endhighlight %}

If you do not have already a private key you will have to [generate one and add it to the shh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/). When choosing a passphrase the golden security rule is the longer, the better. So go ahead and type a line of your favorite song or quote.  
Know that you have a private SSH key you have to [add it to your Github account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/#platform-mac).  
[Now lets test the SSH connection](https://help.github.com/articles/testing-your-ssh-connection/).  

Finally, set your [email address](https://help.github.com/articles/setting-your-commit-email-address-in-git/) and [username](https://help.github.com/articles/setting-your-username-in-git/) in Git.

Remember that you will have to use the remotes URL that is compatible with the [SSH connection](https://help.github.com/articles/changing-a-remote-s-url/) (not HTTPS).

# Mongo
Installing Mongo using Homebrew is quite easy and it requires just one command. Before installing Mongo we update Homebrew.
{% highlight shell %}
$ brew update
$ brew install mongodb
{% endhighlight %}

To start using Mongo you have to enter the following command:
{% highlight shell %}
$ sudo mongod
{% endhighlight %}

If Mongo does not start it is likely that you might see an error related to the nonexistence of the ```/data/db``` folder. So go ahead and [create the folder using ```sudo``` permissions](https://stackoverflow.com/questions/7948789/mongodb-mongod-complains-that-there-is-no-data-db-folder).
{% highlight shell %}
$ sudo mkdir -p /data/db
{% endhighlight %}

Now you can try to start again the Mongo server using ```$ sudo mongod```.  
A great introduction to Mongo is provided by [Daniel Gynn](https://www.danielgynn.com/getting-started-mongodb/) in his blog.

# Node
Thanks to Homebrew also installing Node is quite easy. Go to your terminal and enter the following command:  
{% highlight shell %}
$ brew install node
{% endhighlight %}

To check that the installation was successful check the version of node you just installed with ```node -v```.  
Treehouse provides a [complete guide to the installation of Node](http://blog.teamtreehouse.com/install-node-js-npm-mac).

# Conclusion
Now you have all the tools to start working locally on a web application that uses Mongo, Express, and Node.