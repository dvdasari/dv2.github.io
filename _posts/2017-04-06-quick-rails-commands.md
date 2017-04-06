---
layout: post
title: Quick Rails Commands
---

Deploy a branch other than `master` to staging using capistrano? <br />
in `config/deploy/staging.rb` => add line `set :branch, 'branch_name'` <br />
then `bundle exec cap staging deploy`
