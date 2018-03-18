---
title: "Creating aliases in terminal for Mac (iCloud)"
layout: post
date: 2017-02-03 12:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- utilities
- terminal
star: false
category: blog
author: jrod
description: How to create an alias to access from the terminal a path
---

The iCloud folder has an unusual path. ("~/Library/Mobile\ Documents/com~apple~CloudDocs").  
Having to access iCloud files from the terminal is a little bit of a pain. Therefore, to ease the access to the iCloud folder, you can create an alias in the ".bash_profile" file.  

Firstly, we will have to open the *bash_profile* file. I usually use *vim* as a text editor inside the bash.  

{% highlight shell %}
vim ~/.bash_profile
{% endhighlight %}

Once we open the *bash_profile* we have to add the alias to the iCloud path. This will enable to use the alias instead of having to type the whole path every time.  
Type the following code to create the alias inside the *bash_profile*.

{% highlight shell %}
alias icloud="cd ~/Library/Mobile\ Documents/com~apple~CloudDocs"
{% endhighlight %}

After saving the change in the *bash_profile* file you will be able to access the iCloud folder from the bash by using the alias (i.e. 'icloud'). 

I suggest using the same approach for folders that you use recurrently in your work.  

# References:
- [http://usabilityetc.com/2015/07/access-icloud-drive-folder-in-terminal/]()
- [https://www.moncefbelyamani.com/create-aliases-in-bash-profile-to-assign-shortcuts-for-common-terminal-commands/]()