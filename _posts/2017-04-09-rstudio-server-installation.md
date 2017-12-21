---
title: "Install RStudio Server (Ubuntu)"
layout: post
date: 2017-04-09 12:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- markdown
- elements
star: false
category: blog
author: jrod
description: Summary of how to install Rstudio on a Linux machine
---

# The Server
This post assumes that you have already available a server. I usually work with Amazon Web Services (AWS) instances, therefore there might be some specificities. However, the procedure suggested should work for every type of server (local or on the cloud).  

The procedure suggested is specific for the installation of RStudio Server Pro running on Ubuntu (12.04+).  

# Installing R
Before installing R we need to add the CRAN repository to indicate to the OS from where does it has to download the R installation files. The CRAN will also be used after the installation to download packages and updates.  

We have to add the CRAN url to the *sources.list* file, which contains all the sources from which packages can be obtained. The version of Ubuntu in this case is *xenial*, you should use subtitute the last part of the second command if you're using a different version. For the details you can visit [this post on RStudio](https://cran.rstudio.com/bin/linux/ubuntu/README.html). For more information about the purpose of the *sources.list* file you can visit [this resource](https://wiki.debian.org/SourcesList).  
We do so with the following commands:  

{% highlight shell %}
## Open the sources.list file
$ sudo vim /etc/apt/sources.list

## Add the path to define R CRAN
$ deb https://cran.cnr.berkeley.edu/bin/linux/ubuntu xenial/
{% endhighlight %}


After correctly setting the CRAN repository we can procedeed to download and install R.  
However, before downloading the R installation files we have to add the key used to signed the CRAN archives. The key serves as a security measure to validate the file source. For thorough details on how to add the key you can visit the [CRAN website](https://cran.r-project.org/bin/linux/ubuntu/#secure-apt). For more details on the *secure-apt* concept you can look thorugh [this documentation](https://wiki.debian.org/SecureApt).

With the following commands we add the server key:  

{% highlight shell %}
$ gpg --keyserver pgp.mit.edu --recv-key 51716619E084DAB9
$ gpg -a --export 51716619E084DAB9 > MRutter_cran.asc
$ sudo apt-key add MRutter_cran.asc
{% endhighlight %}

After we sucessfully add the key to the system we can proceeed to install base R. It is adviced to update your system before installing R.

{% highlight shell %}
## Update the system
$ sudo apt-get update

## Install R
$ sudo apt-get install r-base
{% endhighlight %}

# Installing RStudio Server Pro
After installing base R we can proceed to install R Studio Server Pro.  
Before installing RStudio we have to install *gdebi-core*, which is used to RStudio and its dependencies. For more details on how to install RStudio Server you can visit [RStudio website](https://www.rstudio.com/products/rstudio/download-commercial/).  
We use the following commands to install RStudio:

{% highlight shell %}
$ sudo apt-get install gdebi-core
$ wget https://download2.rstudio.org/rstudio-server-pro-1.0.143-amd64.deb
$ sudo gdebi rstudio-server-pro-1.0.143-amd64.deb
{% endhighlight %}

# Setting Up the RStudio Server
After installing RStudio Server you might want to install the dependencies of some popular R libraries (at least among my workgroup). I'm assuming your server does not already have these dependencies. You might want to check if you already have them before executing the following code.  

## Installing and Configuring Java
Java is a dependency for some popular libraries, such as *OpenNLP*.  
With the following command you can install Java 8.  

{% highlight shell %}
$ sudo apt-get install openjdk-8-*
{% endhighlight %}

In order to configure Java for R we have set all Java variables.
Here are some resources gather while understanding how to configure Java correctly: [IBM - Installing R](https://www.ibm.com/support/knowledgecenter/en/SSPT3X_3.0.0/com.ibm.swg.im.infosphere.biginsights.install.doc/doc/install_install_r.html), [Stack Overflow](http://stackoverflow.com/questions/34212378/installation-of-rjava), [Stack Overflow](http://stackoverflow.com/questions/16438073/unable-to-install-rjava-in-r-3-0-in-ubuntu-13-04).  


{% highlight shell %}
## Configuration of variables
$ sudo R CMD javareconf -e

## Setting the java home path in bashrc file
## Open the bashrc file
$ sudo vim ~/.bashrc

## Add the following paths in the bashrc file
$ export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
$ export PATH=$PATH:$JAVA_HOME/bin
{% endhighlight %}

## Install GLS
GLS is the GNU scientific library. Some R libraries depend on this library. For some reason that I wasn't able to understand I had also to install the development package in order to succeed to install the R libraries.  
Here are some resources I used when figuring out how to correctly install GLS: [Ask Ubuntu](https://askubuntu.com/questions/490465/install-gnu-scientific-library-gsl-on-ubuntu-14-04-via-terminal).

{% highlight shell %}
$ sudo apt-get install libgsl2
$ sudo apt-get install libgsl0-dev
{% endhighlight %}


# Other Resources:
- [RStudio server manual](http://docs.rstudio.com/ide/server-proserver-management.html#administrative-dashboard)
- [Upgrade Rstudio Server to Pro](https://support.rstudio.com/hc/en-us/articles/216079967-Upgrading-RStudio-Server)
