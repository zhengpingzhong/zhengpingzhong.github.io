---
layout: post
title:  "some tips of logrotate"
date:   2025-06-11 22:00:00 +0800
categories: jekyll update
---

## Adding a daily logrotate

```bash
cd /etc/logrotate.d/

touch blackboxlog

vim blackboxlog

```

## Edit the log ruler

```text

/data1/blackbox_exporter/log.log {
    size 50M
    rotate 7
    compress
    missingok
    notifempty
    copytruncate
}

```
