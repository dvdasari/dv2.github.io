---
layout:      post
title:       Setup CI on GitLab for a Ruby on Rails app
---

### This is still in progress...

GitLab comes with Continuous Integration support. We do have to setup a Runner to run the CI job. Runner is basically a daemon that runs on a server that executes the CI job.

### Setup Runner

GitLab supports Debian, Ubuntu and CentOS. <br>
We will setup the runner on Ubuntu 16.04 server. <br>
ssh into the server. <br>

* Add GitLab's official repository via yum:

{:.pad_left_30}
```
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-ci-multi-runner/script.deb.sh | sudo bash
```

* Install `gitlab-ci-multi-runner` : <br>

{:.pad_left_30}
```
sudo apt-get install gitlab-ci-multi-runner
```

* Register the runner:

{:.pad_left_30}
```
sudo gitlab-ci-multi-runner register

```

In order to use GitLab's continuous integration, we have to

1. add a `.gitlab-ci.yml` file to the root directory of the app
2. configure the GitLab project to use a Runner.

After that each merge request or push triggers CI.

### Creating `.gitlab-ci.yml` file

First make sure there is a `.ruby-version` file at the root of the app with the ruby version you want to use. <br>

```
touch .ruby-version
echo "2.3.1" > .ruby-version
```

Next, create `.gitlab-ci.yml` at the root of the rails app <br>

```
touch .gitlab-ci.yml
```

If you are using `rspec` for testing, update the above file with:

```
before_script:
  - rbenv install --skip-existing `cat .ruby-version`
  - bundle install
  - bundle exec rake db:migrate RAILS_ENV=test

rspec:
  script:
    - bundle exec rspec
  tags:
    - ruby
```

If `minitest` is used, update as below:

```
before_script:
  - rbenv install --skip-existing `cat .ruby-version`
  - bundle install
  - bundle exec rake db:migrate RAILS_ENV=test

test:
  script:
    - bundle exec rake test
  tags:
    - ruby
```

### Configuring a Runner

Runners run the builds that are defined in `.gitlab-ci.yml`. Runner can be virtual machine, docker container etc.

Two steps are involved

1. Install it
2. Configure it
