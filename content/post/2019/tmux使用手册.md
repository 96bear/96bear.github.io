---
title: "tmux使用手册"
date: "2019-10-27"
categories: 
  - "termux"
tags: 
  - "tmux使用帮助"
---

tmux 在终端中输入tmux就可以打开一个新的 tmux session，tmux 的所有操作必须先使用一个前缀键（默认是 ctrl + b）进入命令模式，或者说进入控制台，就像 vim 中的 esc。  
下面列出了一些常用的基本操作：

信息查询

- tmux list-keys 列出所有可以的快捷键和其运行的 tmux 命令
- tmux list-commands 列出所有的 tmux 命令及其参数
- tmux info 列出所有的 session, window, pane, 运行的进程号，等。 ## 窗口控制 先来看看在 tmux 之外如何进行控制
- session 会话：session是一个特定的终端组合。输入tmux就可以打开一个新的session
    - tmux new -s session\_name 创建一个叫做 session\_name 的 tmux session
    - tmux attach -t session\_name 重新开启叫做 session\_name 的 tmux session
    - tmux switch -t session\_name 转换到叫做 session\_name 的 tmux session
    - tmux list-sessions / tmux ls 列出现有的所有 session
    - tmux detach 离开当前开启的 session
    - tmux kill-server 关闭所有 session
- window 窗口：session 中可以有不同的 window（但是同时只能看到一个 window）
    - tmux new-window 创建一个新的 window
    - tmux list-windows
    - tmux select-window -t :0-9 根据索引转到该 window
    - tmux rename-window 重命名当前 window
- pane 面板：window 中可以有不同的 pane（可以把 window 分成不同的部分）
    - tmux split-window 将 window 垂直划分为两个 pane
    - tmux split-window -h 将 window 水平划分为两个 pane
    - tmux swap-pane -\[UDLR\] 在指定的方向交换 pane
    - tmux select-pane -\[UDLR\] 在指定的方向选择下一个 pane

更常用的是在 tmux 中直接通过默认前缀 ctrl + b 之后输入对应命令来操作，具体如下（这里只列出输入默认前缀之后需要输入的操作）：

- 基本操作
    - ? 列出所有快捷键；按q返回
    - d 脱离当前会话,可暂时返回Shell界面
    - s 选择并切换会话；在同时开启了多个会话时使用
    - D 选择要脱离的会话；在同时开启了多个会话时使用
    - : 进入命令行模式；此时可输入支持的命令，例如 kill-server 关闭所有tmux会话
    - \[ 复制模式，光标移动到复制内容位置，空格键开始，方向键选择复制，回车确认，q/Esc退出
    - \] 进入粘贴模式，粘贴之前复制的内容，按q/Esc退出
    - ~ 列出提示信息缓存；其中包含了之前tmux返回的各种提示信息
    - t 显示当前的时间
    - ctrl + z 挂起当前会话
- 窗口操作
    - c 创建新窗口
    - & 关闭当前窗口
    - \[0-9\] 数字键切换到指定窗口
    - p 切换至上一窗口
    - n 切换至下一窗口
    - l 前后窗口间互相切换
    - w 通过窗口列表切换窗口
    - , 重命名当前窗口，便于识别
    - . 修改当前窗口编号，相当于重新排序
    - f 在所有窗口中查找关键词，便于窗口多了切换
- 面板操作
    - " 将当前面板上下分屏（我自己改成了 |）
    - % 将当前面板左右分屏（我自己改成了 -）
    - x 关闭当前分屏
    - ! 将当前面板置于新窗口,即新建一个窗口,其中仅包含当前面板
    - ctrl+方向键 以1个单元格为单位移动边缘以调整当前面板大小
    - alt+方向键 以5个单元格为单位移动边缘以调整当前面板大小
    - q 显示面板编号
    - o 选择当前窗口中下一个面板
    - 方向键 移动光标选择对应面板
    - { 向前置换当前面板
    - } 向后置换当前面板
    - alt+o 逆时针旋转当前窗口的面板
    - ctrl+o 顺时针旋转当前窗口的面板
    - z 最大化当前所在面板
    - page up 向上滚动屏幕，q 退出
    - page down 向下滚动屏幕，q 退出  
        因为 iTerm2 的支持，很多切换的操作可以直接用鼠标进行，非常方便。具体大家可以自己尝试一下。  
        配置  
        修改~/.tmux.conf即可
