---
title: "ubuntu libssl1.0.0:amd64降级失败"
date: "2020-07-18"
categories: 
  - "zatan"
---

## 起因

想降级libssl1.0.0,下载libssl1.0.0\_1.0.2g-1ubuntu4.15\_amd64单独降级导致dpkg卡住

## 报错

```sh
# sudo apt-get install libssl-dev                      :( 1 20-07-18 - 23:54:55
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
libssl-dev 已经是最新版 (1.0.2g-1ubuntu4.16)。
libssl-dev 已设置为手动安装。
您可能需要运行“apt-get -f install”来纠正下列错误：
下列软件包有未满足的依赖关系：
 libssl-dev : 依赖: libssl1.0.0 (= 1.0.2g-1ubuntu4.16) 但是 1.0.2g-1ubuntu4.15 正要被安装
 libssl1.0.0 : 破坏: libssl1.0.0:i386 (!= 1.0.2g-1ubuntu4.15) 但是 1.0.2g-1ubuntu4.16 正要被安装
 libssl1.0.0:i386 : 破坏: libssl1.0.0 (!= 1.0.2g-1ubuntu4.16) 但是 1.0.2g-1ubuntu4.15 正要被安装
E: 有未能满足的依赖关系。请尝试不指明软件包的名字来运行“apt-get -f install”(也可以指定一个解决办法)。

###

# dpkg --remove --force-remove-reinstreq libssl1.0.0:amd64 
dpkg: 依赖问题阻止了卸载 libssl1.0.0:amd64 的操作：
 libpython3.5-minimal:amd64 依赖于 libssl1.0.0 (&gt;= 1.0.2~beta3).
 libpython2.7-stdlib:amd64 依赖于 libssl1.0.0 (&gt;= 1.0.2~beta3).
 openssh-client 依赖于 libssl1.0.0 (&gt;= 1.0.2).
 libpodofo0.9.3 依赖于 libssl1.0.0 (&gt;= 1.0.0).
 libsasl2-modules:amd64 依赖于 libssl1.0.0 (&gt;= 1.0.0).
 libwinpr-sspi0.1:amd64 依赖于 libssl1.0.0 (&gt;= 1.0.0).
 wget 依赖于 libssl1.0.0 (&gt;= 1.0.1).
 sbsigntool 依赖于 libssl1.0.0 (&gt;= 1.0.0).
 openssl 依赖于 libssl1.0.0 (&gt;= 1.0.2g).
 libssh-4:amd64 依赖于 libssl1.0.0 (&gt;= 1.0.0).
 calibre-bin 依赖于 libssl1.0.0 (&gt;= 1.0.0).
 ntpdate 依赖于 libssl1.0.0 (&gt;= 1.0.1d).
 libfreerdp-core1.1:amd64 依赖于 libssl1.0.0 (&gt;= 1.0.0).
 netsurf-gtk 依赖于 libssl1.0.0 (&gt;= 1.0.0).
 gstreamer1.0-plugins-bad:amd64 依赖于 libssl1.0.0 (&gt;= 1.0.1).
 libfreerdp-crypto1.1:amd64 依赖于 libssl1.0.0 (&gt;= 1.0.0).
 wpasupplicant 依赖于 libssl1.0.0 (&gt;= 1.0.1).
 libssl-dev:amd64 依赖于 libssl1.0.0 (= 1.0.2g-1ubuntu4
dpkg: 处理软件包 libssl1.0.0:amd64 (--remove)时出错：
 依赖问题 - 不会执行卸载
在处理时有错误发生：
 libssl1.0.0:amd64

###

# dpkg -r --force-architecture libssl-dev:amd64             20-07-18 - 23:58:16
dpkg: 依赖问题阻止了卸载 libssl-dev:amd64 的操作：
 nodejs-dev 依赖于 libssl-dev (&gt;= 1.0.0g).
dpkg: 处理软件包 libssl-dev:amd64 (--remove)时出错：
 依赖问题 - 不会执行卸载
在处理时有错误发生：
 libssl-dev:amd64
```

## 解决

```sh
#解决执行命令
wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.0.0_1.0.2g-1ubuntu4.16_amd64.deb
sudo -i
dpkg -r --force-all libssl-dev:amd64
dpkg -i --force-all libssl1.0.0_1.0.2g-1ubuntu4.16_amd64.deb 
```

再次尝试sudo apt install -f,自动修复成功 问题解决！

## 反思

没事瞎搞啥。。。
