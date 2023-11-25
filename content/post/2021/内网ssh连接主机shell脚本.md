---
title: "内网ssh连接主机shell脚本"
date: "2021-01-04"
categories: 
  - "chromebook"
  - "chromebookubuntu"
  - "linux"
  - "ubuntu"
  - "windows"
---

[![](images/image-20210102194340579.png)](http://bear962464.cn)

## 起因

我有两台本本，日常一台性能本，win系统。还有一台chromebook，常用chroot-ubuntu系统，便携，看看资料啥的，也能编译一些东西，为了两台电脑的协作，想学一下用ssh连接

## 配置

- 安装**openssh-server**,**openssh-clicent**,**iptables**
- 使用 **iptables**开放**22端口**
- 看情况配置**/etc/ssh/sshd\_config**，我加了个免密码
- 开启ssh服务，ifconfig查看内网ip，win本xshell连接成功！
- 为了方便使用和备忘写了个shell脚本

## Shell

```shell
#!/bin/sh

#任意键继续的模块
get_char()
        {
        SAVEDSTTY=`stty -g`
        stty -echo
        stty cbreak
        dd if=/dev/tty bs=1 count=1 2> /dev/null
        stty -raw
        stty echo
        stty $SAVEDSTTY
        }

echo "ssh相关脚本，请选择："
echo "1) Install openssh-server openssh-clicent iptables"
echo "2) open22 & service ssh restart"
echo "3) open localhost:22"
echo "4) service ssh stop"
echo "5）check 22（requir lsof）"
echo "6) Edit ur sshd_config（requir micro/vim/nano）"
echo "7) Edit ur ip（requir micro/vim/nano）"
echo "8) nothing"
read number
case $number in
1)echo "首次使用，安装ssh服务,按任意键继续"
char=`get_char`
sudo apt install openssh-client openssh-server iptables  
;;
2)echo "开机一次性启动ssh-service，按任意键继续"
char=`get_char`
sudo iptables -I INPUT -p tcp --dport 22 -j ACCEPT
sudo service ssh restart
;;
3)echo "单独开启22端口，按任意键继续"
char=`get_char`
sudo iptables -I INPUT -p tcp --dport 22 -j ACCEPT
;;
4)echo "结束ssh服务，按任意键继续"
char=`get_char`
sudo service ssh stop
;;
5)echo "查看22端口是否启用，按任意键继续"
char=`get_char`
sudo lsof -i:22
echo 如果上边有列表信息代表22开启
;;
6)echo "编辑ssh服务配置（含更改22，密码，root选项）按任意键继续"
echo "Please edit /etc/ssh/sshd_config"
char=`get_char`
sudo micro /etc/ssh/sshd_config
;;
7)echo "设置固定ip-test，按任意键继续"
echo "Please edit /etc/network/interfacesces"
char=`get_char`
sudo micro /etc/network/interfacesces
;;
8)echo "关于"
echo "作者：96bearli"
echo "Blog:https://bear962464.cn"
echo "Blog:https://96bearli.github.io"
echo "Time:2021.01.04 19:05"
char=`get_char`
;;
esac
```
