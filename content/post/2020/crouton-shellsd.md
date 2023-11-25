---
title: "crouton shell(sd)"
date: "2020-11-28"
categories: 
  - "chromebook"
  - "chromebookubuntu"
---

# 关于

我在Chromebook上通过crouton安装了ubuntu，安装方法请看之前博客文章，为了扩展储存，我将系统转移到了sd卡。本博文就是为了解决每次进入sd卡chroot多条命令的省略

# 优点

- 省略每次命令的输入
- 每次休眠后变化后的sd卡路径自动获取
- 一键重设开发者root密码

# 脚本

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

#sh printtime.sh
#该shell直接输出本地时间
time1=$(date +%Y-%m-%d\ %H:%M:%S)
#currentTimeStamp=`date +%s -d "$time1"`
#time1=$(($currentTimeStamp+28800))
#time1=$(date +%Y-%m-%d\ %H:%M:%S -d "1970-01-01 UTC $time1 seconds");
echo 现在是北京时间：$time1

sleep 0.6
echo "管理员Bear您好"
echo "您即将进入chroot"
echo "您安装的是ubuntu16.04"
echo "请输入序号选择要进入的方式"
echo "1) Start xfce_sd"
echo "2) Start shell_sd"
echo "3) echo shell_sd"
echo "4) xfce in window"
echo "5) Start xfce_local"
echo "6) Start shell_local"
echo "7) app in window"
echo "8) xfce in tab"
echo "9) resolve.sh"

read number
case $number in
1)echo "正在启动桌面xfce_sd"
sdpath=$(sudo fdisk -l |grep -C 0 "HPFS/NTFS/exFAT"  |cut -c1-9)
echo 正在挂载$sdpath到/media/removable/linux
sudo umount $sdpath
sudo mkdir /media/removable/linux
sudo mount $sdpath /media/removable/linux
echo 正在启动xfce桌面，请输入密码1
sudo /media/removable/linux/bin/startxfce4
;;

2)echo "正在进入命令行shell_sd"
sdpath=$(sudo fdisk -l |grep -C 0 "HPFS/NTFS/exFAT"  |cut -c1-9)
echo 正在挂载$sdpath到/media/removable/linux
sudo umount $sdpath
sudo mkdir /media/removable/linux
sudo mount $sdpath /media/removable/linux
echo 正在进入命令行，请输入密码1
sudo /media/removable/linux/bin/enter-chroot
;;

3)echo "显示sd shell"
grep -A 8 "正在启动桌面xfce_sd" enterchroot.sh
;;

4)echo "以窗口模式启动桌面xfce"
sdpath=$(sudo fdisk -l |grep -C 0 "HPFS/NTFS/exFAT"  |cut -c1-9)
echo 正在挂载$sdpath到/media/removable/linux
sudo umount $sdpath
sudo mkdir /media/removable/linux
sudo mount $sdpath /media/removable/linux
sudo /media/removable/linux/bin/startxfce4 -X xiwi
;;

5)echo "正在启动桌面xfce_local"
sudo startxfce4
;;

6)echo "正在进入命令行shell_local"
sudo enter-chroot
;;

7)echo "以下为窗口运行ubuntu软件的命令,手动复制执行"
echo "sudo /media/removable/linux/bin/enter-chroot -b xiwi typorae"
echo 按任意键继续显示...
char=`get_char`
sleep 0.6
echo "help:   https://github.com/dnschneid/crouton/wiki/crouton-in-a-Chromium-OS-window-%28xiwi%29"
;;

8)echo "以下为标签运行xfce的命令并自动运行"
echo "sudo /media/removable/linux/bin/startxfce4 -X xiwi-tabtee"
sleep 0.6
echo 确认继续执行请按任意键继续...
char=`get_char`
sdpath=$(sudo fdisk -l |grep -C 0 "HPFS/NTFS/exFAT"  |cut -c1-9)
echo 正在挂载$sdpath到/media/removable/linux
sudo umount $sdpath
sudo mkdir /media/removable/linux
sudo mount $sdpath /media/removable/linux
echo 正在启动xfce桌面，请输入密码1
sudo /media/removable/linux/bin/startxfce4 -X xiwi-tabtee
;;

9)echo "常见问题通道"
#sh /resolve.sh
echo "Bear遇到的问题并完美解决（懒得记）的代码"
echo "任务开始执行没有再次确认，且无法挽回，建议看代码"
sleep 1
echo "所以你要解决什么问题？"
sleep 0.6
echo "1）重设开发者root密码"
echo "2) 解决Read-only file system"
echo "3) 锁启动签名"
read number
case $number in
1)echo "正在重设开发者root密码"
echo 确认继续执行请按任意键继续...
char=`get_char`
echo "请进行2-3次的密码输入"
sudo chromeos-setdevpasswd
echo "执行完毕，password已修改"
;;
2)echo "正在设定为WR读写"
echo 确认继续执行请按任意键继续...
char=`get_char`
sudo /usr/share/vboot/bin/make_dev_ssd.sh --remove_rootfs_verification --partitions 4
echo "执行完毕，重启生效"
;;
3)echo "或许你做了个不明智的决定"
echo 确认继续执行请按任意键继续...
char=`get_char`
sudo crossystem dev_boot_signed_only=1
echo "执行完毕，重启生效"
;;
esac
;;
esac
```
