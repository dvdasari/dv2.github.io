---
layout: post
title: Configure NGINX for multiple websites on a VPS
date: 2014-01-29 00:15:37 -0600
comments: true
keywords: NGINX, Ruby on Rails, VPS
description: How to configure NGINX for multiple websites on a VPS
---

In this post I will show how to configure NGINX to host multiple domains on a VPS

**I will be using** <br />
NGINX  
Passenger  
Digital Ocean (for hosting, but any hosting provider like Linode, Dreamhost, etc can be used)  


**I will show how to host 3 domain names. Lets say they are:** <br />
domain1.com  
domain2.com  
domain3.com  

First we will setup our DNS records for each of the domains. Here is how they look.
![Nginx Domain]({{ site.url }}/assets/nginx_domain1.png)
![Nginx Domain]({{ site.url }}/assets/nginx_domain2.png)
![Nginx Domain]({{ site.url }}/assets/nginx_domain3.png)

I have shown how to deploy a Ruby on Rails app in my previous post - [Deploy Ruby on Rails 4 Application on Ubuntu 12.04 With Nginx and Passenger](/2014/01/26/deploy-ruby-on-rails-to-linode-on-ubuntu-12-dot-04/)

I have deployed 3 apps for each domain and they are in the below paths: <br />
{% highlight ruby %}
/home/user_name/domain1  
/home/user_name/domain2  
/home/user_name/domain3  
{% endhighlight %}

On the server, Nginx has a *default* configuration file at /etc/nginx/sites-available
{% highlight ruby %}
root@user_name:/etc/nginx/sites-available$ ls -l
-rw-r--r-- 1 root root 356 Jan 28 05:29 default
{% endhighlight %}

and a symlink to this file at /etc/nginx/sites-enabled
{% highlight ruby %}
root@user_name:/etc/nginx/sites-enabled$ ll
lrwxrwxrwx 1 root root   34 Jan 28 04:15 default -> /etc/nginx/sites-available/default
{% endhighlight %}

We will remove the symlink to the default file (which is equal to disabling the default configuration) and create 3 configuration files for the 3 domains
{% highlight ruby %}
root@user_name:/etc/nginx/sites-enabled$ rm default
{% endhighlight %}

{% highlight ruby %}
root@user_name:/etc/nginx/sites-available$ touch domain1.com
root@user_name:/etc/nginx/sites-available$ touch domain2.com
root@user_name:/etc/nginx/sites-available$ touch domain3.com
{% endhighlight %}

Now lets create symlinks for these files in /etc/nginx/sites-enabled
{% highlight ruby %}
root@user_name:/etc/nginx/sites-enabled$ pwd
/etc/nginx/sites-enabled

root@user_name:/etc/nginx/sites-enabled$ ln -s /etc/nginx/sites-available/domain1.com domain1.com
root@user_name:/etc/nginx/sites-enabled$ ln -s /etc/nginx/sites-available/domain2.com domain2.com
root@user_name:/etc/nginx/sites-enabled$ ln -s /etc/nginx/sites-available/domain3.com domain3.com

root@user_name:/etc/nginx/sites-enabled$ ls -l
lrwxrwxrwx 1 root root 38 Jan 29 16:15 domain1.com -> /etc/nginx/sites-available/domain1.com
lrwxrwxrwx 1 root root 38 Jan 29 16:15 domain2.com -> /etc/nginx/sites-available/domain2.com
lrwxrwxrwx 1 root root 38 Jan 29 16:15 domain3.com -> /etc/nginx/sites-available/domain3.com
{% endhighlight %}

We will have the below configuration in these files
{% highlight ruby %}
server {
  listen 80;

  server_name www.domain1.com domain1.com;
  passenger_enabled on;
  rails_env production;
  root /home/user_name/domain1/current/public;

  error_page 500 502 503 504  /50x.html;
  location = /50.x.html {
    root html;
  }
}
{% endhighlight %}

{% highlight ruby %}
server {
  listen 80;

  server_name www.domain2.com domain2.com;
  passenger_enabled on;
  rails_env production;
  root /home/user_name/domain2/current/public;

  error_page 500 502 503 504  /50x.html;
  location = /50.x.html {
    root html;
  }
}
{% endhighlight %}

create `/etc/nginx/sites-available/domain3.com`
{% highlight ruby %}
server {
  listen 80;

  server_name www.domain3.com domain3.com;
  passenger_enabled on;
  rails_env production;
  root /home/user_name/domain3/current/public;

  error_page 500 502 503 504  /50x.html;
  location = /50.x.html {
    root html;
  }
}
{% endhighlight %}

Restart NGINX
{% highlight ruby %}
sudo service nginx restart
{% endhighlight %}

To verify if all the configurations are correct, run *sudo nginx -t*
{% highlight ruby %}
root@user_name:/etc/nginx/sites-enabled$ sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
{% endhighlight %}

That is all we have to do.

Open a browser and check for domain1.com, domain2.com, domain3.com. They should load the sites.


**If anything is not working you can do a couple of things to see what is wrong** <br>
1. Verify if all the configurations are correct by running *sudo nginx -t* <br>
2. Check NGINX log files at */var/log/nginx/*

{% highlight ruby %}
root@user_name:/etc/nginx/sites-enabled$ sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
{% endhighlight %}

{% highlight ruby %}
root@user_name:/etc/nginx/sites-enabled$ cd /var/log/nginx/
root@user_name:/var/log/nginx$ ll
-rw-r--r--  1 root root  9838 Jan 29 15:30 access.log
-rw-r--r--  1 root root 17762 Jan 29 16:38 error.log
{% endhighlight %}
