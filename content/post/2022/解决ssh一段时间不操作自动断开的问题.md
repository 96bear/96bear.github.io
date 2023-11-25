---
title: "解决ssh一段时间不操作自动断开的问题"
date: "2022-03-14"
categories: 
  - "zatan"
---

```shell
#edit this config_file
sudo vi /etc/ssh/ssh_config

#add
ServerAliveInterval 50 
ServerAliveCountMax 3

#restart
sudo /etc/init.d/ssh restart
```
