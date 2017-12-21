---
layout: post
title: Setting permissions SSH Keys
cover: cover.jpg
date:   2017-03-14 12:00:00
categories: Utilities
---

I was having some trouble accessing my AWS instance from the terminal. I realized that the server was not validating the SSH key because it was not an only read file.  

In fact, SSH keys have to be read-only files because of security reasons. Therefore, any key with a write permission will be rejected when trying to connect to the server.  

To set the read only permission we have to use the following command from the bash:

{% highlight shell %}
chmod 400 ~/path_ssh_key
{% endhighlight %}  

After setting the correct permission the server will accept the SSH key.


For more information about SSH Keys go to the following [website](http://bodhizazen.net/Tutorials/SSH_keys)