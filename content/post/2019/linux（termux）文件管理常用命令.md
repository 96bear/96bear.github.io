---
title: "linux（termux）文件管理常用命令"
date: "2019-10-27"
categories: 
  - "linux"
  - "termux"
tags: 
  - "linux"
  - "termux命令"
---

```
本文可直接wget得到，配合tmux使用体验更佳。（文章末尾）
```

文件管理--增, 删, 改, 查

pwd 显示工作目录的绝对路径（类似于windows电脑窗口的地址栏内容）

mkdir,touch(创建新的文件);

新建文件：touch 文件名 例：touch a.txt

新建文件夹：mkdir 文件夹名(即目录名) 例：mkdir test

mkdir -p /a/b/c #递归依次创建 /a , /a/b , /a/b/c  
cp：

复制文件：cp 源文件 目标位置或新文件名

例：cp -v a.txt aa.txt 复制文件并显示(Verbose)执行过程

复制文件夹：cp -rv 源文件夹名 目标位置或新文件夹名

例：cp -rv test test-1 说明：-r是递归的意思，复制文件夹时必须用此选项

重命名（移动mv）: mv 源文件名 目标位置或新文件名

例：mv -v test-1 test-2

删remove(rm)：

用法： rm -rfv 文件名

例：删除当前文件夹中的test、test-2、a.txt、aa.txt文件

rm -rfv test test-2 a.txt aa.txt  
说明：-r 是递归操作，删除目录时必须使用此选项

\-f 是强制(force)删除

\-v 是显示(verbose)执行过程  
文件查找命令:cat ,head, tail, more, less

1.看全文：cat 文件名 例1: cat /etc/hosts

例2：cat -n /etc/passwd 看文件内容并显示行号(number)  
2.看头几行(默认看头10行)：head -行数 文件名

例：head -3 /etc/passwd 看头3行内容

3.看最后几行(默认看尾部10行)：tail -行数 文件名

例：tail -3 /etc/passwd 看尾部3行内容

4.看更多(向下翻页显示)：more 文件名

例：more /etc/passwd

快捷键：空格键下翻一页，回车键下翻一行，q键退出(quit)

5.看更少(上下翻页显示)：less 文件名

例：less /etc/passwd

快捷键：方向键上下左右翻，空格键或pagedown下翻一页，pageup键向上翻一页，回车键下翻一行，q键退出(quit)

https://bear962464.cn/2019/10/27/640/
