---
layout: post
title:  "Setting up Sphinx in a Ruby on Rails application"
date:   2014-09-16 19:35:03 -0500
categories: sphinx ruby on rails
---

Thinking Sphinx — [http://pat.github.io/thinking-sphinx/](http://pat.github.io/thinking-sphinx/)

Installing Sphinx -  [http://pat.github.io/thinking-sphinx/installing_sphinx.html](http://pat.github.io/thinking-sphinx/installing_sphinx.html)

Installing on MacOS X - [http://pat.github.io/thinking-sphinx/installing_sphinx/mac.html](http://pat.github.io/thinking-sphinx/installing_sphinx/mac.html)

Using Homebrew: `brew install sphinx`

Add ‘thinking-sphinx’ to Gemfile - [https://github.com/pat/thinking-sphinx](https://github.com/pat/thinking-sphinx)
{% highlight ruby %}
gem ‘thinking-sphinx’, ‘~> 3.1.1'
bundle install
{% endhighlight %}

Quick Start Quide — [http://pat.github.io/thinking-sphinx/quickstart.html](http://pat.github.io/thinking-sphinx/quickstart.html)

{% highlight ruby %}
rake ts:index ==> index data
rake ts:start ==> Run Sphinx
rake ts:rebuild ==> rebuild indexes
{% endhighlight %}
