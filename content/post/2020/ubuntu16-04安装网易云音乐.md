---
title: "ubuntu16.04安装网易云音乐"
date: "2020-02-29"
categories: 
  - "zatan"
---

### ubuntu16.04安装网易云音乐

有时候网易云音乐打不开可能是因为下载的版本不对导致缺少系统底层接口的支持

#### 下载

官网已经更新为18.04版本 [16.04在官网此位置下载](http://s1.music.126.net/download/pc/netease-cloud-music_1.0.0_amd64_ubuntu16.04.deb)

#### 安装

下载好后在下载路径打开终端

```
sudo dpkg -i netease-cloud-music_1.0.0_amd64_ubuntu16.04.deb
```

#### 如果出现数据库出错问题

请看[网易云音乐数据库出错问题](https://bear962464.cn/2020/02/29/900/)

#### 太长不看(一行命令)

````
//懒人命令,打开终端直接执行并输入密码
sudo -i && wget http://s1.music.126.net/download/pc/netease-cloud-music_1.0.0_amd64_ubuntu16.04.deb && sudo dpkg -i netease-cloud-music_1.0.0_amd64_ubuntu16.04.deb
```
````
