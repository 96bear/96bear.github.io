---
title: "Chromebook折腾全攻略"
date: "2020-02-12"
categories: 
  - "chromebook"
---

Chromebook系统为ChromeOS，是Google推出的为网络而生的笔记本，把轻巧便携这个特点展现到了极致。 最开始在考虑入手树莓派的时候无意间在知乎看到有人推广ChromeBook，好奇之下开始调查，越查越是眼睛发亮，我喜欢，我想要，一时冲动就买了一台二手。 最初我看中的是新鲜感，可玩性，使用过程中感觉到续航七八小时实在是舒服，又想学习linux的相关知识，走上了软件方面折腾的不归路，终于把一台快10岁的机子折腾到日常使用的程度。 本子是DELL的Chromebook 11,4+16G，闲鱼200元入手，伊拉克成色，经过折腾感觉性价比还不错。 ![订单](images/20200212225406.jpg)

#### 折腾分为几个阶段(点击即可访问)

1. [**拯救伊拉克战损外观**](https://bear962464.cn/2020/02/13/833/ "拯救伊拉克战损外观")
2. [**激活熟悉chromeos（罗列使用的插件）**](https://bear962464.cn/2020/02/13/835/ "激活熟悉chromeos（罗列使用的插件）")
3. [**通过crouton安装Ubuntu**](https://bear962464.cn/2020/02/13/839/ "通过crouton安装Ubuntu")
4. [**初始环境配置**](https://bear962464.cn/2020/02/13/841/ "初始环境配置")
5. [**xfce4美化，安装日常软件**](https://bear962464.cn/2020/02/13/843/ "xfce4美化，安装日常软件")**（等待更新）**
6. [**系统转移sd卡**](https://bear962464.cn/2020/05/26/954/ "系统转移sd卡等待更新")

### 伊拉克战损笔记本DELL Chromebook 11物理美化

![订单](images/20200212225406.jpg) 伊拉克成色 ![旧照1](images/20200212224955.jpg) ![旧照2](images/20200212224956.jpg) ![旧照3](images/20200212224954.jpg) 外屏碎裂，键盘缺←掉J，磨损不必多说，但是200元还是挺值得哈

##### 下面开始物理美化

1. 修复屏幕 一开始想用亚克力板贴在屏幕上代替外屏，后来发现对于我来说实现难度过高，于是购买了一张15.6的高清软膜，经过测量后裁剪高清膜内膜外框（方便粘在屏幕上），也较好的嵌入外边框。
    
2. 三面贴膜 因为国内不流行，淘宝商家没有这个模，这次淘宝也不是万能的咯，买了三面14寸（本子13.3），自己动手丰衣足食。
    
3. 修复键盘 理由同上，问了很多店铺找不到相同的，自己做了一个超简陋的LOL。
    
    ###### 效果展示
    
    ![屏幕](images/20200212234002.jpg) ![外壳](images/20200212233020.jpg) ![键盘](images/20200212233019.jpg)
    
    ###### 个人评价
    
    6/10分，勉强能带出去不嫌弃丢人现眼了，功能上还算顺手
    
    ### 激活熟悉chromeos（罗列使用的插件）
    
    #### 关于激活
    
    由于是登录谷歌账号，需要**接入万维网**，这个就需要八仙过海各显神通了，给出几个建议，**更加具体请百度吧**：
    
4. 通过路由器代理（需要技术）
    
5. **局域网ipv4共享ss** 需要使用ss，在win本上的ss客户端开启“允许来自局域网的连接”，然后把你的Chromebook和win本连接同一个WiFi，在win本上查看本机在局域网上的IP（win+r，cmd，回车，ipconfig，回车），然后在cb的WiFi设置里启用代理，在“手动配置代理”里面勾选“对所有协议使用相同代理”，然后把你win本的IP输入进去，端口填：1080“，再重连一下cb的WiFi，就可以激活了
    
6. 手机软件分享，通过**手机热点分享**（我用的方法） 使用两个app，主app联万维网，辅助app分享主软件联的网。[辅助app下载地址](https://www.lanzous.com/i9ass5a)
    

登录好google 会自动同步所有的记录，比如书签，密码，扩展，**建议在设置同步中关闭同步**，避免影响pc端扩展使用。

#### 推荐插件

**下载地址：** [谷歌应用商店](https://chrome.google.com/webstore/category/extensions?utm_source=chrome-ntp-icon) [应用商店镜像站](https://www.gugeapps.net)

1. Hoxx γPŅ Proxy 联网用的，虽然慢点但还算稳定且**免费**
2. Ads Killer Adblocker Plus 就像它的说明：从**所有**网站中移除广告！
3. 侧边翻译 选中后在侧边栏进行翻译，适应不需要全文翻译的情况
4. Evernote Web Clipper 印象笔记一键剪藏插件，个人很喜欢
5. Tampermonkey 通常叫做油猴，网页**神器**。建议脚本源[Greasy Fork](https://greasyfork.org/zh-CN)和github

推荐扩展 暂时没找到好用的(^.^)

### 通过crouton安装Ubuntu

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

### 4\. 环境配置，xfce4美化，安装日常软件

先看结果 ![美化](images/20200213025436.png)

#### 环境配置

进入桌面打开命令行，快捷键ctrl+T没反应的话alt+F3搜索term，建议使用xfce终端

##### 建议换源

不同版本换的源不同，建议了解后自行更改，不改也可，用国内网apt会比较慢

##### 首次进入桌面

```
sudo apt update
sudo apt upgrade
```

##### 设置中文环境

```
sudo apt-get install language-pack-zh-hans language-pack-zh-hans-base language-pack-gnome-zh-hans language-pack-gnome-zh-hans-base

sudo apt-get install &#x60;check-language-support -l zh&#x60;

sudo localectl set-locale LANG=zh_CN.UTF-8
```

//重启后生效

对于中文乱码是空格的情况，安装中文字体解决

```
sudo apt-get install fonts-droid-fallback ttf-wqy-zenhei ttf-wqy-microhei fonts-arphic-ukai fonts-arphic-uming
```

##### 卸载不必要的软件

```
sudo apt-get remove libreoffice-common -y          //删除 libreoffice
sudo apt-get remove unity-webapps-common -y        //删除 amazon
sudo apt-get remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku  landscape-client-ui-install onboard deja-dup -y
```

##### 安装常用软件

```
sudo apt install g++ git unrar unzip vim nano 
//以上最好安装，以下可选
sudo apt install gedit
```

##### 主题配置

```
wget -q -O - http://archive.getdeb.net/getdeb-archive.key | sudo apt-key add -
sudo sh -c &#039;echo &quot;deb http://archive.getdeb.net/ubuntu xenial-getdeb apps&quot; &gt;&gt; /etc/apt/sources.list.d/getdeb.list&#039;
sudo add-apt-repository ppa:noobslab/themes
sudo add-apt-repository ppa:noobslab/icons
sudo apt-get update
sudo apt-get install ubuntu-tweak -y
sudo apt-get install flatabulous-theme -y
sudo apt-get install ultra-flat-icons -y
sudo apt-get install vpnc git -y
```

# xfce4美化等待更新

# [系统转移sd卡](https://bear962464.cn/2020/05/26/954/ "系统转移sd卡等待更新")

材料：一个U盘（FAT32格式，其他格式可能出错），sd卡一张（高速，较大容量，作为系统盘）

## 1.设置sd卡

插入sd卡，ctrl+alt+t shell

```shell
sudo -i
#列出当前所有设备
fdisk -l
#找到SD卡，比如我的是/dev/sdc1,将其卸载
umount /dev/sdc1
#把设备格式化成ext4
mkfs.ext4 /dev/sdc1
#添加linux文件夹
mkdir /media/removable/linux
#把设备挂载到linux文件夹下
mount /dev/mmcblk1 /media/removable/linux
chmod 777 -R /dev /media/removable
```

## 2.备份系统

```shell
cd /usr/local/chroots/
#找到安装的linux名
#如/usr/local/chroots/xenial，所以chrootname为xenial
sudo edit-chroot -b chrootname
#当前目录生成文件xxxxx.tar.gz，将该文件mv到Downloads下面即可
#空间紧张就移动到U盘
#删除原有系统（可选）
sudo delete-chroot chrootname
```

## 3.恢复系统到SD卡

```shell
#-f指向备份文件，-p指向安装位置（sd卡）
sudo sh ~/Downloads/crouton -f mybackup.tar.gz -p /media/removable/linux
```

## 4.运行命令

每次重启后都要运行以下命令来启动ubuntu，建议./xxx.sh运行

```shell
#!/bin/sh
sudo umount /dev/sdb1
sudo mkdir /media/removable/linux
sudo mount /dev/sdb1 /media/removable/linux
sudo media/removable/linux/bin/startxfce4
```

进入终端把line5改成

`sudo media/removable/linux/bin/enter-chroot`

#### 致谢和资料

**感谢几位的无畏折腾和无私奉献，替后来人趟过了各种坑！**

1. [**chromebook折腾开发历程**](https://github.com/holdjohnh/Chromebook4China)——主要参考
2. [**ChromeBook 折腾小记**](https://www.zybuluo.com/chopsticks/note/68204)
3. [**一天一夜chromebook折腾心得**](https://blog.csdn.net/woodenrobot/article/details/53931220)
4. [**【教程】在SD卡或任意存储设备安装ubuntu14.04+steam游戏**](http://tieba.baidu.com/p/3519371851?pn=1)——系统转移参考
5. [**chromebook激活及通过crouton安装ubuntu**](https://www.jianshu.com/p/25c261ec9fef)——解决sudo密码问题
