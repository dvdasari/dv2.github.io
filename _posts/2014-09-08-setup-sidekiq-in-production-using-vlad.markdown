---
layout: post
title:  Setup Sidekiq in production using VLAD
date:   2014-09-08 19:35:03 -0500
comments: true
keywords: sidekiq vlad
description: Setup Sidekiq in production using VLAD
---

Here is the official documentation for Sidekiq deployment 
 — [https://github.com/mperham/sidekiq/wiki/Deployment](https://github.com/mperham/sidekiq/wiki/Deployment)

Add sidekiq to Gemfile
{% highlight ruby %}
gem ‘sidekiq’, ‘3.2.1'
{% endhighlight %}

Create the file `config/initializers/sidekiq.rb`
{% gist dv2/6e1ea0e69d0050581fb8 %}

Vlad `config/deploy.rb`
{% gist dv2/7c4662583761baf11001 %}

On the server create files

`/etc/init/sidekiq.conf`
{% gist dv2/7072761632fc88209dbc %}

`/etc/init/workers.conf`
{% gist dv2/ed710898437287eef1df %}

To deploy: `bundle exec rake vlad:deploy`
