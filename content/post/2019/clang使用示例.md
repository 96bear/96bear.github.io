---
title: "clang使用示例"
date: "2019-10-27"
categories: 
  - "termux"
tags: 
  - "clang"
---

_**Clang是一个**_[_**C语言**_](https://baike.baidu.com/item/C%E8%AF%AD%E8%A8%80)_**、**_[_**C++**_](https://baike.baidu.com/item/C%2B%2B)_**、**_[_**Objective-C**_](https://baike.baidu.com/item/Objective-C)_**语言的轻量级**_[_**编译器**_](https://baike.baidu.com/item/%E7%BC%96%E8%AF%91%E5%99%A8/8853067)_**。**_[_**源代码**_](https://baike.baidu.com/item/%E6%BA%90%E4%BB%A3%E7%A0%81)_**发布于**_[_**BSD**_](https://baike.baidu.com/item/BSD)_**协议下。Clang将支持其普通**_[_**lambda表达式**_](https://baike.baidu.com/item/lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F/4585794)_**、返回类型的简化处理以及更好的处理constexpr关键字。**_

```
本文可直接wget得到，配合tmux使用体验更佳。（文章末尾）
```

编译命令：  
 gcc/clang -g -O2 -o log ffmpeg\_log.c -I -L -l(第一竖线是大写的i，第三个竖线是小写的L)  
 示例clang -g -O2 -o log ffmpeg\_log.c -I …/ffmpeg -L …/ffmpeg/libavutil -lavutil

解析：  
 -g 输出文件中的调试信息  
 -O2 对输出文件做指令优化（默认是-O1是不对指令进行优化，-O2编译器会按照自己的理解优化指令，让指令运行的更快）  
 -o 输出文件的名字  
 -o后面跟的.c文件就是要编译的文件的名字  
 -I 指定头文件的位置  
 -L 指定库文件的位置  
 -l 指定引用的库文件名字

https://bear962464.cn/2019/10/27/640/
