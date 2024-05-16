---
title:      "cron jobs"
date:       2024-04-14T11:38:00-04:00
tags:       ["linux", "shell", "tech"]
identifier: "20240414T113800"
---

a way to scheduale tasks

`crontab -l`
- list cron jobs

`crontab -e`
- edit cron jobs

minute, hour, day of month, month, day of week,
m,h,d,m,w

* * * * * command

`* * * * * /usr/bin/python /roo/rand.py`
- always supply full path to executable
