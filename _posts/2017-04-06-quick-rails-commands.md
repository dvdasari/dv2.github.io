---
layout: post
title: Quick Rails Commands
---

#### Deploy a branch other than `master` to staging using capistrano?
in `config/deploy/staging.rb` add line `set :branch, 'branch_name'` <br />
then run `bundle exec cap staging deploy`

----
<p></p>

#### Installing the Xcode Command Line Tools for Mac

1. Check if Xcode Command Line Tools are already installed, run <br />
   `$ xcode-select -p`  <br />
   if the output is something like `/Applications/Xcode.app/Contents/Developer`, the tools are already installed, nothing to do

1. Otherwise install with the below command  <br />
   `$ xcode-select --install`

----
<p></p>
