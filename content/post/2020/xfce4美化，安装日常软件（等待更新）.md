---
title: "xfce4美化，安装日常软件（等待更新）"
date: "2020-02-12"
categories: 
  - "chromebook"
---

先看结果 ![美化](images/20200213025436.png)

##### 主题配置

```
wget -q -O - http://archive.getdeb.net/getdeb-archive.key | sudo apt-key add -
sudo sh -c &#039;echo &quot;deb http://archive.getdeb.net/ubuntu xenial-getdeb apps&quot; &gt;&gt; /etc/apt/sources.list.d/getdeb.list&#039;
sudo add-apt-repository ppa:noobslab/themes
sudo add-apt-repository ppa:noobslab/icons
sudo apt-get update
sudo apt-get install ubuntu-tweak -y
sudo apt-get install flatabulous-theme -y
sudo apt-get install ultra-flat-icons -y
sudo apt-get install vpnc git -y
```

[**返回Chromebook折腾全攻略**](http://bear962464.cn/2020/02/13/831/)
