---
title: "转移linux系统到SD卡"
date: "2020-05-26"
categories: 
  - "chromebook"
  - "chromebookubuntu"
---

材料：一个U盘（FAT32格式，其他格式可能出错），sd卡一张（高速，较大容量，作为系统盘）

## 1.设置sd卡

插入sd卡，ctrl+alt+t shell

```shell
sudo -i
#列出当前所有设备
fdisk -l
#找到SD卡，比如我的是/dev/sdc1,将其卸载
umount /dev/sdc1
#把设备格式化成ext4
mkfs.ext4 /dev/sdc1
#添加linux文件夹
mkdir /media/removable/linux
#把设备挂载到linux文件夹下
mount /dev/mmcblk1 /media/removable/linux
chmod 777 -R /dev /media/removable
```

## 2.备份系统

```shell
cd /usr/local/chroots/
#找到安装的linux名
#如/usr/local/chroots/xenial，所以chrootname为xenial
sudo edit-chroot -b chrootname
#当前目录生成文件xxxxx.tar.gz，将该文件mv到Downloads下面即可
#空间紧张就移动到U盘
#删除原有系统（可选）
sudo delete-chroot chrootname
```

## 3.恢复系统到SD卡

```shell
#-f指向备份文件，-p指向安装位置（sd卡）
sudo sh ~/Downloads/crouton -f mybackup.tar.gz -p /media/removable/linux
```

## 4.运行命令

每次重启后都要运行以下命令来启动ubuntu，建议./xxx.sh运行

```shell
#!/bin/sh
sudo umount /dev/sdb1
sudo mkdir /media/removable/linux
sudo mount /dev/sdb1 /media/removable/linux
sudo media/removable/linux/bin/startxfce4
```

进入终端把line5改成

`sudo media/removable/linux/bin/enter-chroot`

## 删除sd卡chroot方法

先挂载上chroot

```shell
sudo delete-chroot -y -ac /media/removable/linux/chroots/xenial/ 
```

## 2020 1 15增加bash脚本方便每次进入chroot

请查看本站文章[crouton shell(sd)](https://bear962464.cn/2020/11/28/988/)
