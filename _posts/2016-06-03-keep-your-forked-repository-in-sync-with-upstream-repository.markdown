---
layout:      post
title:       How to keep your forked repository in sync with the upstream repository
date:        2016-06-03 19:35:03 -0500
comments:    true
keywords:    git fork rebase
description: How to keep your forked repository in sync with the upstream repository
---

Assuming that you forked a repository and cloned it on your local machine

1. To list the current remote repository for your fork:

```
$ git remote -v
origin	https://github.com/dv2/rails.git (fetch)
origin	https://github.com/dv2/rails.git (push)
```

2. Specify a new remote upstream repository that will be synced with the fork

```
$ git remote add upstream https://github.com/rails/rails.git
```

3. To verify the upstream repo

```
$ git remote -v
origin		https://github.com/dv2/rails.git (fetch)
origin		https://github.com/dv2/rails.git (push)
upstream	https://github.com/rails/rails.git (fetch)
upstream	https://github.com/rails/rails.git (push)
```

4. To sync local master with upstream master

```
git pull --rebase upstream master
git push -f origin master
```

5. To sync local branch `branch_name` with upstream master

```
git pull --rebase upstream master
git push -f origin branch_name
```

6. To sync local branch with upstream `branch_name` (other than master)

```
git pull --rebase upstream branch_name
git push -f origin branch_name
```
