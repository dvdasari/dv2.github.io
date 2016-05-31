---
layout: post
title:  "Search across all git branches"
date:   2014-09-22 19:35:03 -0500
categories: search git branches
---

I was looking for a way to search within all the git branches for some text and here is how I ended up doing that. I wrote a ruby program that I run inside the git repo.

{% highlight ruby %}
ruby gitsearch.rb
{% endhighlight %}

{% gist dv2/ac6369d8653a2a7b3eca %}

Search results/output of the program is displayed in the terminal as well as written to the file ‘search_results.txt’

Output in the file looks as below:

```
############################
Search Term: elasticsearch
File Types: *.rb
############################
add_fulltext_search:config/initializers/tire.rb:log_file = Rails.root.join “log”, “elasticsearch_#{Rails.env}.log”
add_fulltext_search:spec/spec_helper.rb: puts “2. elasticsearch -f -D es.config=/usr/local/opt/elasticsearch/config/elasticsearch.yml “
calculate_estimates:config/initializers/tire.rb:log_file = Rails.root.join “log”, “elasticsearch_#{Rails.env}.log”
calculate_estimates:spec/spec_helper.rb: puts “2. elasticsearch -f -D es.config=/usr/local/opt/elasticsearch/config/elasticsearch.yml “
add_blog_posts:config/initializers/tire.rb:log_file = Rails.root.join “log”, “elasticsearch_#{Rails.env}.log”
add_blog_posts:spec/spec_helper.rb: puts “2. elasticsearch -f -D es.config=/usr/local/opt/elasticsearch/config/elasticsearch.yml “
add_blog_posts_refactor:config/initializers/tire.rb:log_file = Rails.root.join “log”, “elasticsearch_#{Rails.env}.log”
add_blog_posts_refactor:spec/spec_helper.rb: puts “2. elasticsearch -f -D es.config=/usr/local/opt/elasticsearch/config/elasticsearch.yml “
cron_jobs:config/initializers/tire.rb:log_file = Rails.root.join “log”, “elasticsearch_#{Rails.env}.log”
cron_jobs:spec/spec_helper.rb: puts “2. elasticsearch -f -D es.config=/usr/local/opt/elasticsearch/config/elasticsearch.yml “
download_buttons:config/initializers/tire.rb:log_file = Rails.root.join “log”, “elasticsearch_#{Rails.env}.log”
download_buttons:spec/spec_helper.rb: puts “2. elasticsearch -f -D es.config=/usr/local/opt/elasticsearch/config/elasticsearch.yml “

```
