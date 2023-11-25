---
title: "通过crouton安装Ubuntu"
date: "2020-02-12"
categories: 
  - "chromebook"
---

#### 1.关于crouton

Chrome OS系统基于linux内核，crouton安装是基于chroot的。利用crouton可以在Chrome OS系统内装入精简的Ubuntu系统（可自行扩充），这种方式两系统的切换非常方便，[crouton文档](https://github.com/dnschneid/crouton)。 不过**注意**了，想达到**日常使用**的程度，**机器的硬盘是有限且绝对不够用的**，这就是为什么装入的ubuntu是精简的，为什么我要把系统转移到128g的sd卡上——**不用担心**，对于已经安装在硬盘的系统也可以像我一样把**系统转移到便携储存**上，具体在后详细叙述。

#### 2.备份Chrome OS

**折腾以前一定要对原Chrome OS系统进行备份** Chrome网上应用商店搜索并下载Chromebook Recovery Utility，准备一个4G以上的U盘插入电脑，根据提示备份（U盘格式化）——话说我没做这一步，没出问题也是幸运。

#### 3.进入开发者模式

**警告: 进入开放模式后，会清除此前硬盘中的所有数据！** 按住Esc键、刷新键和电源按钮——顶部第一个、第四个、最后一个——进入恢复模式。听说有的Chromebook需要切换的物理开发者开关。 恢复屏幕上，按Ctrl+D，同意提示，启动进入开发人员模式。 步骤： 进入恢复模式——按 Ctrl+D——反正不是 Ctrl+D就是Enter，回车重启一次，再Ctrl+D，等一会，上面的进度条走完了之后再自动重启，接下来设置系统语言，开启调试模式(**密码记牢了**)，就进入开发者模式了。 以后每次开机都需要摁Ctrl+D跳过警告，否则迎接你的是30s后一声刺耳的尖鸣再进入系统——我承认，吓到过我，并且有了心理阴影。

#### **_动手能力强有基础的同学接下来可以参考本文首Crouton官方文档进行自定义安装_**

### 以下是我安装时的步骤：

1. [下载crouton](https://www.lanzous.com/i9atvwb)请解压后仍然放入Downloads文件夹
2. Ctrl+Alt+T打开crosh，输入shell并回车
3. 这一步很关键：**Ctrl+ALT+forward**，然后登录**root**,密码之前设置的那个调试密码。之后输入命令`chromeos-setdevpasswd` 设置新密码，这个就是下一步sudo需要的密码。Ctrl+ALT+back回到浏览器。
4. **确保CB全局接入万维网**（不然可能在安装过程中卡住），输入`sudo sh ~/Downloads/crouton -t xfce` 输入**上一步设置的密码**开始安装，如果你想装unity桌面的话也可以`sudo sh ~/Downloads/crouton -t unity`，之后的教程默认xfce桌面
5. 安装进行中，网好30分钟，网差一两小时。喝杯咖啡或者泡壶茶，休息一会。如果异常中断试试sudo startxfce能否继续。
6. 安装的最后要求填入用户名和密码，建议与调试密码一致（非必须一致）
7. 在shell中
    
    ```
    
    sudo enter-chroot 进入终端
    sudo startunity 启动桌面
    sudo startxfce4 启动 xfce 桌面 
    ```
    

在 Chrome os 和 Linux 之间切换： 从 C 到 L：shift + ctrl + alt + 前进键 从 L 到 C：shift + ctrl + alt + 后退键

```
### 其他命令（备用）
#### 备份与恢复
默认是存储在/usr/local/chroots/下，每个版本对应一个目录，***如/usr/local/chroots/xenial，所以chrootname为xenial***。
所以chroots的备份和恢复：
```

- 备份命令：sudo edit-chroot -b chrootname (生成文件precise-20141230-0321.tar.gz，将该文件mv到Downloads下面即可)
- 恢复命令:sudo edit-chroot -r chrootname //chrootname: 所安装 ubuntu 的版本代号
    

#### 删除ubuntu

```
sudo delete-chroot chrootname 
//chrootname: 所安装 ubuntu 的版本代号,详见备份与恢复
```

#### crouton 进阶使用

使用 xiwi 先在 chrome 网上应用商店里下载 cronton-integration

```
sudo sh ~/Downloads/crouton -t xfce,extension,xorg,xiwi //我是在 xfce 桌面下使用的 unity 也可行 但要求下载 extension,xorg,xiwi
sudo startxfce4 -X xiwi // 使用 xiwi 来实现 窗口化
```

可以做到左边 chrome 右边 xfce 桌面

[**返回Chromebook折腾全攻略**](http://bear962464.cn/2020/02/13/831/)
