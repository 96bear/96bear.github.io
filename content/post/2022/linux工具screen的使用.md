---
title: "linux工具screen的使用"
date: "2022-02-25"
categories: 
  - "kali"
  - "linux"
  - "termux"
  - "ubuntu"
---

# screen 介绍

使用telnet或SSH远程登录linux时，如果连接非正常中断（远程机器关闭），重新连接时，系统将开一个新的session，无法恢复原来的session.screen命令可以解决这个问题。Screen工具是一个终端多路转接器，在本质上，这意味着你能够使用一个单一的终端窗口运行多终端的应用。

## 我的常用命令

```shell
#开启一个screen
screen -S ssh111
#这里用-S表示给screen起名字

#退出该screen
Ctrl-a d

#screen内切换前后window
Ctrl-a #这里的ctrl-a表示同p/n

#screen内查看所以window
Ctrl-a w
时按ctrl键和a键，然后再单独按d键。这时退到根终端。

#查看开启的所有screen：
screen -ls

#进入某个screen：
screen -r ssh111#name或id

#关闭该session：
exit
Ctrl-d
#在该screen中退出

#强制连接，踢掉原来的
screen -D -r 20791
screen -x -r 20791 #（共享原来的）

#screen内创建window
Ctrl-a c

#screen内切换前后window
Ctrl-a p/n

#screen内查看所以window
Ctrl-a w

#切换到第 0..9 个window
Ctrl-a 0..9

#kill window，强行关闭当前的 window
Ctrl-a k
```

## 完整汉化帮助信息

```
语　　法：
screen [-AmRvx -ls -wipe][-d <作业名称>][-h <行数>][-r <作业名称>][-s ][-S <作业名称>]

补充说明：
screen为多重视窗管理程序。此处所谓的视窗，是指一个全屏幕的文字模式画面。通常只有在使用telnet登入主机或是使用老式的终端机时，才有可能用到screen程序。

参　　数：
-A 　将所有的视窗都调整为目前终端机的大小。
-d <作业名称> 　将指定的screen作业离线。
-h <行数> 　指定视窗的缓冲区行数。
-m 　即使目前已在作业中的screen作业，仍强制建立新的screen作业。
-r <作业名称> 　恢复离线的screen作业。
-R 　先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。
-s 　指定建立新视窗时，所要执行的shell。
-S <作业名称> 　指定screen作业的名称。
-v 　显示版本信息。
-x 　恢复之前离线的screen作业。
-ls或--list 　显示目前所有的screen作业。
-wipe 　检查目前所有的screen作业，并删除已经无法使用的screen作业。
 python app.py -mongo=local-test -redis=local-test -my_url=local-test
常用screen参数：
在每个screen session 下，所有命令都以 ctrl+a(C-a) 开始。
C-a ? -> Help，显示简单说明
C-a c -> Create，开启新的 window
C-a n -> Next，切换到下个 window 
C-a p -> Previous，前一个 window 
C-a 0..9-> 切换到第 0..9 个window
Ctrl+a [Space] -> 由視窗0循序換到視窗9
C-a C-a -> 在两个最近使用的 window 间切换 
C-a x -> 锁住当前的 window，需用用户密码解锁
C-a d -> detach，暂时离开当前session，将目前的 screen session (可能含有多个 windows) 丢到后台执行，并会回到还没进 screen 时的状态，此时在 screen session 里    每个 window 内运行的 process (无论是前台/后台)都在继续执行，即使 logout 也不影响。 
C-a z -> 把当前session放到后台执行，用 shell 的 fg 命令則可回去。
C-a w -> Windows，列出已开启的 windows 有那些 
C-a t -> Time，显示当前时间，和系统的 load 
C-a K -> kill window，强行关闭当前的 window
C-a [ -> 进入 copy mode，在 copy mode 下可以回滚、搜索、复制就像用使用 vi 一样
    C-b Backward，PageUp 
    C-f Forward，PageDown 
    H(大写) High，将光标移至左上角 
    L Low，将光标移至左下角 
    0 移到行首 
    $ 行末 
    w forward one word，以字为单位往前移 
    b backward one word，以字为单位往后移 
    Space 第一次按为标记区起点，第二次按为终点 
    Esc 结束 copy mode 

C-a ] -> Paste，把刚刚在 copy mode 选定的内容贴上
```
