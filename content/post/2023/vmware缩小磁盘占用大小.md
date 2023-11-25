---
title: "VmWare缩小磁盘占用大小"
date: "2023-06-05"
categories: 
  - "centos"
  - "linux"
  - "ubuntu"
  - "windows"
  - "app"
---

VmWare越用约大，已经达到了52个G，这种情况下虚拟机的压缩传输都成了大问题

## 如何缩小

在已经安装VMtools的虚拟机中执行 `vmware-toolbox-cmd disk shrink /` 注意：先删除所有快照，否则会报错 静静等待进度达到100

## 成果

52 => 17.9G
