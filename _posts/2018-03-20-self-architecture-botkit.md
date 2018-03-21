---
title: "Self-host Slack bot using Botkit"
layout: post
date: 2018-03-20 08:00
image: /assets/images/markdown.jpg
headerImage: false
projects: false
hidden: false # don't count this post in blog pagination
tag:
- nginx
- bots
- aws
- messenger
- conversational
- interface
star: false
category: blog
author: jrod
description: This guide explains how to protect with SSL an HTTP node app.
externalLink: false
---

Recently I migrated to my own architecture a slack bot based on the botkit platform. As usual, what seemed a simple process turn out to take more time than expected. Here are some of the things I learned during the process. The code in the post is specific to an Ubuntu server.

# Self-host
Is all about control, control, control... Although botkit allows you to easily host your bot on Glitch or Heroku, I believe that having full control of the environment where is hosted is beneficial for several reasons. 1) Efficiency: if you already pay for an EC2 instance, as I do, this comes at no additional cost. 2) Control over the node and npm libraries versions. 3) Easiest integration with other services. For example, my bot sends data to users depending on the values stored in a MongoDB hosted in the same instance.

# Slack bot
Slack is agnostic about where you host your application as long as you provide their API with a valid HTTPS URL that directs traffic to and from your bot. Being able to provide to the Slack API with such an URL is where I waste more time during this process.
In fact, the botkit application runs natively on HTTP. Therefore I had to find a way to secure the connection. From what I find out surfing around, the best way to do so is through a Nginx reverse proxy. 

## Nginx
Nginx is a web server that among other features enables to set up a reverse proxy server. A reverse proxy server is a type of server that intermediates between your local applications and clients that are across the Internet. Therefore, Nginx can listen on specifics ports to outside clients, and eventually redirect these requests to different local ports. In our case, we want to configure Nginx to listen to the outside world on a port secured with SSL and redirect traffic to our local node application that runs on HTTP.

The first step is to install the Nginx application. 
```shell
sudo apt-get update
sudo apt-get install nginx
```

When installed, the Nginx server creates a default configuration file. For most cases, this file will come handy. However, on my server, I already have some applications that run on the default ports, such as 80 and 443, which are the ones specified in the Nginx default file. Therefore, the following steps are specific to my case. 
Since the default configuration is not useful to me, I deleted it and created a new one. 

```shell
sudo rm /etc/nginx/sites-available/default
sudo touch  /etc/nginx/sites-available/myurlconfig
```
By default, the Nginx server will read all the configuration files located in the folder `/etc/nginx/sites-available` when it starts. I then open the config file I just created and insert my configurations.

```text
server {
    listen 8443 ssl http2;
    listen [::]:8443 ssl http2;

    # certs sent to the client in SERVER HELLO are concatenated in ssl_certificate
    ssl_certificate     /etc/nginx/certificates/ssl-bundle.crt;
    ssl_certificate_key /etc/nginx/certificates/ddslab.key;    ssl_session_timeout 1d;
    ssl_session_cache shared:SSL:50m;
    ssl_session_tickets off;


    # modern configuration. tweak to your needs.
    ssl_protocols TLSv1.2;
    ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
    ssl_prefer_server_ciphers on;

    # HSTS (ngx_http_headers_module is required) (15768000 seconds = 6 months)
    add_header Strict-Transport-Security max-age=15768000;

    # OCSP Stapling ---
    # fetch OCSP records from URL in ssl_certificate and cache them
    ssl_stapling on;
    ssl_stapling_verify on;

    ## verify chain of trust of OCSP response using Root CA and Intermediate certs
    ssl_trusted_certificate /path/to/root_CA_cert_plus_intermediates;

    resolver <IP DNS resolver>;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
       }
}
```

There are three main things to consider in the above configurations.  
-  The port in which the Nginx server is listening. We find this configuration at the top of the file, and in our case is port 8443. Notice that we set the port to listen using `ssl` and `http2` protocols.  
-  To listen using an SSL connection we need to provide Nginx with valid SSL certificates. I bought my certificates at  [ssls.com](https://www.ssls.com). I place the certificates under the same folder of the Nginx application. We need to provide Nginx with the ssl_certificate and the ssl_certificate_key. Generating a ssl_certificate that provides full SSL authentication requires some extra steps, see the paragraph below.
-  The local port where the traffic to and from the Internet was to be redirected. This is the port where our local node bot application runs. This configuration is under the location section at the bottom of the config file. In my case, my app runs on port 3000, so I set `proxy_pass http://localhost:3000;`.

## Open port AWS
Depending on your configurations, you will have to specifically open the port 8433 of your EC2 instance. To do so go to the security group of the instance and insert a new Inbound port.

## Full certificate
To be valid for the Slack API, the SSL certificate has to concatenate intermediate and root certificates into one. This information is specific to ssls.com. However, I assume other certificate issuers follow the same practice. For precise information on how to combine the two certificates follow [these steps](https://helpdesk.ssls.com/hc/en-us/articles/203427642-How-to-install-a-SSL-certificate-on-a-NGINX-server).

## Run Nginx server
Finally, if all the steps were successfull you should be able to start the server, which will start redirecting all the traffic on the HTTPS port 8433 to the localhost:3000. 

```shell
sudo service nginx restart
```