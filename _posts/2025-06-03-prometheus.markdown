---
layout: post
title:  "some tips of prometheus "
date:   2025-06-03 22:52:44 +0800
categories: jekyll update
---

## Reloading configuration

```bash
pidof prometheus | xargs -r kill -s SIGHUP
```

## Using multiple scrape_config_files

```yml
scrape_config_files:
  - scrape_config_files/*.yml

```

### scrape_configs in the *.yml file example

```yml
scrape_configs:
  # 监控业务kafka
  - job_name: "kafka"
    static_configs:
    - targets:
        - 172.18.1.16:9308
        - 172.18.1.17:9308
        - 172.18.1.18:9308
        - 172.18.1.112:9308
        - 172.18.1.162:9308
        - 172.18.1.197:9308
```

## configure https and authentication

``` shell

yum install httpd-tools

htpasswd -nBC 10 "" | tr -d ':\n'


```

| 参数    | 含义                                                           |
| ------- | -------------------------------------------------------------- |
| `-n`    | **不要写入文件**，只在终端输出 `username:hashed_password` 形式 |
| `-B`    | 使用 **bcrypt 加密算法**（更安全）                             |
| `-C 10` | 设置 bcrypt 的加密“成本”（cost factor），10 是推荐值之一       |
| `""`    | 空用户名，这里故意写成空（只要哈希值，不需要用户名）           |
