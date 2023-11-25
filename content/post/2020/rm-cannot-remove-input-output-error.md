---
title: "rm: cannot remove Input/output error"
date: "2020-05-26"
categories: 
  - "chromebookubuntu"
  - "linux"
---

apt install设置触发python3.5报错 尝试删除python3.5报错信息如下

```shell
rm: cannot remove ‘/usr/lib/python3.5/__pycache__ /_compression.cpython-35.pyc ’: Input/output error
```

尝试手动删除失败 查询后得知对文件系统进行修复可以解决 使用命令fsck 我是在chromebook运行的chroot，于是在chromeos中使用sudo fdisk -l查询分区路径 得到路径/dev/sdb1

```shell
#检测命令,全部y，也可加入-a
fsck.ext3 /dev/sdb1
```

重启后可以删除\_compression.cpython-35.pyc 了，但是并没有解决apt install设置触发python3.5报错的问题

```shell
Errors were encountered while processing:
   python3.5
   dh-python
   python3
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

继续尝试
