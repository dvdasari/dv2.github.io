---
layout: post
title:  "Replacing MySQL with MariaDB on OS X for a Ruby on Rails App"
date:   2015-01-08 19:35:03 -0500
categories: mysql mariadb
---

First uninstall MySQL.

If you have installed it using brew, you uninstall as below
`$ brew uninstall mysql`

Next install mariadb
`$ brew install mariadb`

That is all it takes to install MariaDB

However, after this, starting my rails server gave me the below error

```
Incorrect MySQL client library version! This gem was compiled for 5.6.19 but the client library is 10.0.15-MariaDB. (RuntimeError)
```

This is how I fixed this error to make the Rails app work with MariaDB

From the root of the rails application

```
$ gem uninstall mysql2
$ gem install mysql2 -v=’0.3.17' -- --with-mysql-config=/usr/local/Cellar/mariadb/10.0.15/bin/mysql_config
```
