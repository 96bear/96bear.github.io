---
title: "ubuntu新装环境配置"
date: "2020-02-12"
categories: 
  - "chromebook"
---

#### 环境配置

进入桌面打开命令行，快捷键ctrl+T没反应的话alt+F3搜索term，建议使用xfce终端

##### 建议换源

不同版本换的源不同，建议了解后自行更改，不改也可，用国内网apt会比较慢

##### 首次进入桌面

```
sudo apt update
sudo apt upgrade
```

##### 设置中文环境

```
sudo apt-get install language-pack-zh-hans language-pack-zh-hans-base language-pack-gnome-zh-hans language-pack-gnome-zh-hans-base

sudo apt-get install &#x60;check-language-support -l zh&#x60;

sudo localectl set-locale LANG=zh_CN.UTF-8
```

//重启后生效

对于中文乱码是空格的情况，安装中文字体解决

```
sudo apt-get install fonts-droid-fallback ttf-wqy-zenhei ttf-wqy-microhei fonts-arphic-ukai fonts-arphic-uming
```

##### 卸载不必要的软件

```
sudo apt-get remove libreoffice-common -y          //删除 libreoffice
sudo apt-get remove unity-webapps-common -y        //删除 amazon
sudo apt-get remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku  landscape-client-ui-install onboard deja-dup -y
```

##### 安装常用软件

```
sudo apt install g++ git unrar unzip vim nano 
//以上最好安装，以下可选
sudo apt install gedit
```

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
