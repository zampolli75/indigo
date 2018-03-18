---
title: "Deactivate Microsoft Office License on MacOS"
layout: post
date: 2018-01-28 08:00
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
description: This guide explains how to deactivate your current Office license
externalLink: false
---

# License Nightmare
Microsoft holds to their standards with a fabulous user experience failure whenever you want to deactivate your current Office license on the Mac.  
You might wonder, why would you ever need to deactivate a valid license (apparently this question seemed unimportant to Microsoft developers) if you want to continue using the Microsoft suite. In my specific case, my University provides all students starting college with an institutional "free" license (Office 365 license) linked to their email account as part of a bundle. Unexpectedly (really?!) though, many students arrive at college with their Office products associated with an existing license (Volume license), and therefore they need to switch accounts and license type.  
Changing your current license type appears to be quite a hard job if you don't know where the Office suite stores the files with your existing account information. Luckily, an [Engineer](https://github.com/pbowden-msft) at Microsoft wrote a script to allow us to deactivate the Office license with one command from the terminal.  
In this quick guide, I'll explain the steps you have to follow to use the script successfully assuming that you don't have experience with the terminal.  

## 1. Sign out from your Office account
The first step in the deactivation process is to sign out from the account currently associated with your Office suite. To do so open one of the application from the Office suite (e.g., Excel, Word) and click on the top left icon on the left sidebar of the application window. From the menu, you will find the sign out option.

![logout](/assets/images/posts/deactivate-office-license-mac/logout.png)  

Now **close all** the Office applications currently open.

## 2.  Download the Unlicense script  
In my Github account, you can find the [`Unlicense`](https://github.com/zampolli75/Unlicense/blob/master/Unlicense) script that I cloned from [`pbowden-msft`](https://github.com/pbowden-msft).  
From the repository click on the `Raw` button to access the file content.

![rawdata](/assets/images/posts/deactivate-office-license-mac/githubraw.png)

Then `save` the file to your default `Download` folder. Set the filename as `Unlicense.sh`. **You have to change the file extension of the file to `.sh`**.  
If you set a different folder as default for your downloaded files modify accordingly the following steps.  


![saveas](/assets/images/posts/deactivate-office-license-mac/saveas.png)


## 3. Run the script from the terminal  
We run the script from the terminal. Open the terminal application and navigate to the `Downloads` folder where we saved the `Unlicenced` file.

To move to the `Downloads` directory from the terminal use the following command.

{% highlight shell %}
$ cd Downloads
{% endhighlight %}

After entering the command check that successfully changes the directory.

![downloads](/assets/images/posts/deactivate-office-license-mac/downloads.png)

To give execute permissions to the file we have to change the file permissions with `chmod`. From the terminal we prompt the following command:

{% highlight shell %}
$ chmod +x ./Unlicense.sh
{% endhighlight %}

Now we can execute the script.

## 3. Detect and deactivate your license
Before deactivating your license, we want to confirm that you have a license associated with your Office suite. To detect current licenses we run the following command from the terminal:

{% highlight shell %}
$ ./Unlicense.sh --DetectOnly
{% endhighlight %}

![downloads](/assets/images/posts/deactivate-office-license-mac/detect.png)

After confirming that you have a license associated, we can finally deactivate it with the following command. Before running the command close all your Microsoft Office applications.

{% highlight shell %}
$ ./Unlicense.sh --All --ForceClose
{% endhighlight %}

After successfully deactivating your license you will be prompted to input your account Office credentials the next time you open an application from the Office suite. Now when you input the credentials of the new account you want to associate your license will be correctly updated. Finally, you can close the terminal.