---
title: "scoop—Windows命令行包管理工具"
date: "2019-10-28"
categories: 
  - "windows"
tags: 
  - "windows"
---

### 安装前的准备：

- 用户名不含中文字符
- Windows 7 SP1+ / Windows Server 2008+
- [PowerShell 3+](https://www.microsoft.com/en-us/download/details.aspx?id=34595)
- [.NET Framework 4.5+](https://www.microsoft.com/net/download)  
    **若Powershell或.NET Franmework版本过旧，更新后重启即可。  
    若不清楚版本号，可`Win+R`运行powershell，输入以下命令获取版本号。**

$PSVersionTable.PSVersion.Major   #查看Powershell版本
$PSVersionTable.CLRVersion.Major  #查看.NET Framework版本

### 安装scoop：

```
Set-ExecutionPolicy RemoteSigned -scope CurrentUser
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```

**如果下载scoop的过程中断，那么必须先删除(C:\\Users<user>\\scoop)文件夹，再执行以上命令安装。**  
下载完成后，输入`scoop`出现如下帮助即安装成功。

![](blob:https://bear962464.cn/ad2490b1-6779-47ed-abc4-600b88619daf)

- 安装成功
- 输入`scoop`获取帮助
