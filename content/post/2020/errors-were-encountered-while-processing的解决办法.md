---
title: "Errors were encountered while processing的解决办法"
date: "2020-05-26"
categories: 
  - "chromebookubuntu"
  - "linux"
  - "ubuntu"
---

sudo apt-get upgrade报错

```shell
3 not fully installed or removed.
```

```shell
#解决方法
cd /var/lib/dpkg
sudo mv info info.bak
sudo mkdir info
sudo apt-get upgrade
```

完美解决！
