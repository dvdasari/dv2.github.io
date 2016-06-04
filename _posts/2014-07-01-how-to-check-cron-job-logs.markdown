---
layout: post
title:  How to check cron job logs?
date:   2014-07-01 18:34:23 -0500
comments: true
keywords: jekyll update
description: How to check cron job logs?
---

Option #1: <br/>
By default cron jobs log to `/var/log/syslog` <br/>
You can see them by: `grep CRON /var/log/syslog`

Option #2: <br/>
Open file `/etc/rsyslog.d/50-default.conf` <br/>
Find the line that starts with `#cron.* /var/log/cron.log` <br/>
Uncomment that line and save the file <br/>
restart rsyslog: `sudo service rsyslog restart` <br/>
Now the cron logs appear in `/var/log/cron.log` <br/>

Option #3: <br/>
in crontab at the end of the task add `>> /tmp/cron.log 2>&1`
