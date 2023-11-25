---
title: "小技巧:termux打印二维码"
date: "2019-10-28"
categories: 
  - "linux"
  - "termux"
tags: 
  - "termux"
  - "termux命令"
---

![](images/Screenshot_2019-10-29-00-30-28-006_com.termux-576x1024.jpg)

示例

代码:

```
echo "网址|文本" |curl -F-=\<- qrenco.de
```
