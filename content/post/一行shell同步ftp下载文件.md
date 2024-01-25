+++
title = '一行shell同步ftp下载文件'
date = 2024-01-25T09:38:39+08:00
draft = false

+++

## 背景

在干净的Linux或容器中，有时没有curl/wget等工具，需要同步文件、下载文件很麻烦，但python作为自带的组件提供了很方便的功能

## 命令

FTP文件下载 

~~~shell
python -c "from ftplib import FTP; ftp = FTP('192.168.1.25', 'FtpUserName', 'FtpUserPasswd'); ftp.retrbinary('RETR /Path/To/YourFile', open('YourFile', 'wb').write); ftp.quit()"
~~~

Http文件下载(自动获取文件名)

~~~shell
python -c "import urllib.request;url='http://download.redis.io/releases/redis-5.0.5.tar.gz'; urllib.request.urlretrieve(url, url.split('/')[-1])"
~~~

