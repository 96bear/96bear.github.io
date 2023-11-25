---
title: "ruoyi-cloud 学习笔记"
date: "2022-04-25"
categories: 
  - "centos"
  - "java"
  - "linux"
---

# ruoyi 微服务版本 学习上手

[http://doc.ruoyi.vip/ruoyi-cloud/](http://doc.ruoyi.vip/ruoyi-cloud/) ruoyi 微服务版本文档

## 目标

基于centos环境 自行搭建一套nacos微服务中心 然后运行起该项目 进行学习

## 搭建记录

- \[x\] ruoyi-cloud大致了解
    
- \[x\] Java ide选择安装
    
- \[x\] centos云服务器重装
    
- \[x\] 服务器安装openjdk8
    
- \[x\] [centos安装nacos2.0.3\_毅大师的博客-CSDN博客\_centos nacos](https://blog.csdn.net/qq_39648029/article/details/120760172)
    
- \[x\] [Maven-CentOS安装\_宇枫16的博客-CSDN博客\_centos maven安装](https://blog.csdn.net/rao991207823/article/details/118914715)
    
- \[x\] yum install maven （本地构建时发现会自动安装jdk8作为依赖）
    
- \[x\] 在bt面板进行[宝塔面板下载，免费全能的服务器运维软件 (bt.cn)](https://www.bt.cn/new/download.html)
    
    - \[x\] 安装
    
    ```text
    Mysql >= 5.7.0 (推荐5.7版本)
    Redis >= 3.0
    Node >= 12（PM2管理器 5.2）
    Nginx1.2
    ```
    
    - \[x\] 数据库导入
    
    ```
    3、创建数据库ry-cloud并导入数据脚本ry_2021xxxx.sql（必须），quartz.sql（可选）
    4、创建数据库ry-config并导入数据脚本ry_config_2021xxxx.sql（必须）
    ```
    
- \[x\] 了解mvn命令[Maven的使用入门 - jimisun - 博客园](https://www.cnblogs.com/jimisun/p/7842537.html)
    
- \[x\] ~~~shell // 构建jar mvn complile // 对于java工程执行package打成jar包，对于web工程打成war包 mvn package
    
- \[x\] \[Maven全局配置文件settings.xml详解 - 洪墨水 - 博客园 \]([https://www.cnblogs.com/hongmoshui/p/10762272.html#:~:text=settings.xml中包含类似本地仓库、远程仓库和联网使用的代理信息等配置](https://www.cnblogs.com/hongmoshui/p/10762272.html#:~:text=settings.xml中包含类似本地仓库、远程仓库和联网使用的代理信息等配置)。 ,settings.xml文件一般存在于Maven的安装目录的conf子目录下面，或者是用户目录的.m2子目录下面。 其实相对于多用户的PC机而言，在Maven安装目录的conf子目录下面的settings.xml才是真正的全局的配置。)
    
- \[x\] [如何在linux中编译写和运行java代码 - 编程语言 - 亿速云](https://www.yisu.com/zixun/131089.html)
    
- \[ \] 爆炸了，1C1G已重启三遍，不过应该算是成功了
    
- \[x\] 换一个思路，配置不够本地构建
    
- \[x\] \[VirtualBox\]VirtualBox中安装CentOS-8 - 简书 \]([https://www.jianshu.com/p/4aee7e81db11](https://www.jianshu.com/p/4aee7e81db11))
    
- \[x\] 配置ssh，zsh，建立快照
    
- \[x\] 文件转移
    
- \[x\] 踩坑https://blog.csdn.net/newbie\_R/article/details/115920761 要改nacos里ruoyi-system-dev.yml里的sql账密
    
- \[x\] 本地搭建成功
    

## 学习参考

[http://doc.ruoyi.vip/ruoyi-cloud/](http://doc.ruoyi.vip/ruoyi-cloud/)

bing等搜索引擎

## 学习任务

- \[x\] ruoyi-cloud文档
- \[x\] 搭建起centos环境
- \[x\] 搭建一套nacos微服务中心
- \[ \] 再读文档
- \[ \] 尝试框架更多的功能
- \[ \] 读源码，学java
