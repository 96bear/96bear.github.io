---
title: "centos7 matlab runtime 部署流程"
date: "2022-09-22"
categories: 
  - "centos"
  - "linux"
---

## 环境

centos7 虚拟机环境  
内存6G cpu 2\*4 硬盘100G  
x86\_64

## 下载

(约30分钟)  
在 [页面](https://ww2.mathworks.cn/products/compiler/matlab-runtime.html) 选择版本下载  
测试时使用的是：  
[https://ssd.mathworks.com/supportfiles/downloads/R2022a/Release/5/deployment\_files/installer/complete/glnxa64/MATLAB\_Runtime\_R2022a\_Update\_5\_glnxa64.zip](https://ssd.mathworks.com/supportfiles/downloads/R2022a/Release/5/deployment_files/installer/complete/glnxa64/MATLAB_Runtime_R2022a_Update_5_glnxa64.zip)  
![](images/b5df423990674a0fcbefed40f4acdf82.png)

## 解压

(约20分钟)

unzip MATLAB\_Runtime\_R2022a\_Update\_5\_glnxa64.zip

```
unzip MATLAB_Runtime_R2022a_Update_5_glnxa64.zip
```

## 安装

(约8分钟)

./install -mode silent -agreeToLicense yes #静默模式安装

```
./install -mode silent -agreeToLicense yes #静默模式安装
```

## 环境变量

**本步骤永久固化环境变量，不建议执行，原因见问题2**  
(约1分钟)

vi /etc/profile

```
vi /etc/profile
```

编辑文件加入  
`export LD_LIBRARY_PATH=/usr/local/MATLAB/MATLAB_Runtime/v912/runtime/glnxa64:/usr/local/MATLAB/MATLAB_Runtime/v912/bin/glnxa64:/usr/local/MATLAB/MATLAB_Runtime/v912/sys/os/glnxa64:`

source /etc/profile

```
source /etc/profile
```

## 测试

### 占用

cpu 4x4核虚拟机未开启虚拟化100%  
mem 2g左右  
![](images/ccc2e11415380a79cb972b47dbf72385.png)

### 耗时

196秒，处理文件大小2.3m  
（惨不忍睹）  
![](images/4a51e4e40b59c00778d9a9543c874002.png)

## 可能出现的问题

### 1、GLIBCXX\_3.4.21

$ java -jar testJar.jar
java: symbol lookup error: /usr/local/MATLAB/MATLAB\_Runtime/v912/bin/glnxa64/../../sys/os/glnxa64/libstdc++.so.6: undefined symbol: \_\_cxa\_thread\_atexit\_impl

```
$ java -jar testJar.jar
java: symbol lookup error: /usr/local/MATLAB/MATLAB_Runtime/v912/bin/glnxa64/../../sys/os/glnxa64/libstdc++.so.6: undefined symbol: __cxa_thread_atexit_impl
```

尝试用本地的库链接解决：  
`sudo ln -sf /usr/lib64/libstdc++.so.6.0.19 /usr/local/MATLAB/MATLAB_Runtime/v912/sys/os/glnxa64/libstdc++.so.6`

$ java -jar testJar.jar
Exception in thread "main" java.lang.UnsatisfiedLinkError: /usr/local/MATLAB/MATLAB\_Runtime/v912/bin/glnxa64/libnativedl.so: /usr/local/MATLAB/MATLAB\_Runtime/v912/bin/glnxa64/../../sys/os/glnxa64/libstdc++.so.6: version \`GLIBCXX\_3.4.21' not found (required by /usr/local/MATLAB/MATLAB\_Runtime/v912/bin/glnxa64/libnativedl.so)

```
$ java -jar testJar.jar
Exception in thread "main" java.lang.UnsatisfiedLinkError: /usr/local/MATLAB/MATLAB_Runtime/v912/bin/glnxa64/libnativedl.so: /usr/local/MATLAB/MATLAB_Runtime/v912/bin/glnxa64/../../sys/os/glnxa64/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by /usr/local/MATLAB/MATLAB_Runtime/v912/bin/glnxa64/libnativedl.so)
```

查询：  
strings /usr/lib64/libstdc++.so.6 |grep GLIBCXX  
![](images/e0bab23ee0867d15c6b35c0edb6feae4.png)  
了解 `libstdc++.so.6` 版本低,该库来自gcc v9以上版本编译，本地gcc版本8.5

#### 解决

升级GCC（编译安装约一小时），参考 [https://zhuanlan.zhihu.com/p/498529973](https://zhuanlan.zhihu.com/p/498529973)  
[http://www.45fan.com/article.php?aid=1yHfFmWE9y3w8MG9](http://www.45fan.com/article.php?aid=1yHfFmWE9y3w8MG9) 升级10.2  
随后  
`sudo ln -sf /usr/lib64/libstdc++.so.6.0.28 /usr/local/MATLAB/MATLAB_Runtime/v912/sys/os/glnxa64/libstdc++.so.6`  
![](images/b492fc8c428a690dad6b3a9b89fb279f.png)  
问题已经解决  
![](images/df0693e5adaae4eceae808a308854a58.png)

### 2、yum报错 .so冲突（动态链接库

由于matlab runtime的依赖库造成包括但不限于 yum（基于py2） 的崩溃  
pycurl.so There was a problem importing one of the Python modules required to run yum.  
![](images/c5cb53e1c65995b745da1ba6b75de99e.png)

检查命令  
`ldconfig -p | grep curl`  
`ldd /usr/lib64/python2.7/site-packages/pycurl.so`  
![](images/5587027ae1c371052f3f117c06af63ff.png)

很明显 需要的依赖跑偏了

### 处理建议

取消永久固化 $LD\_LIBRARY\_PATH  
仅在所需会话中  
`export LD_LIBRARY_PATH=/usr/local/MATLAB/MATLAB_Runtime/v912/runtime/glnxa64:/usr/local/MATLAB/MATLAB_Runtime/v912/bin/glnxa64:/usr/local/MATLAB/MATLAB_Runtime/v912/sys/os/glnxa64:`  
\[官方建议\]([https://ww2.mathworks.cn/help/compiler/m](https://ww2.mathworks.cn/help/compiler/m) xa64/jre/lib/fonts\`  
下载 [链接](https://jaist.dl.sourceforge.net/project/wqy/wqy-microhei/0.2.0-beta/wqy-microhei-0.2.0-beta.tar.gz)

\# 下载
wget https://jaist.dl.sourceforge.net/project/wqy/wqy-microhei/0.2.0-beta/wqy-microhei-0.2.0-beta.tar.gz
# 解压保存
tar -zxvf wqy-microhei-0.2.0-beta.tar.gz
mv wqy-microhei /usr/share/fonts/
# 复制到matlab jre路径
cp /usr/share/fonts/wqy-microhei/wqy-microhei.ttc /usr/local/MATLAB/MATLAB\_Runtime/v912/sys/java/jre/glnxa64/jre/lib/fonts
# 进入相应目录
cd /usr/local/MATLAB/MATLAB\_Runtime/v912/sys/java/jre/glnxa64/jre/lib/fonts
# 生成文件 fonts dir
mkfontdir
# 建立字体索引
mkfontscale

```
# 下载
wget https://jaist.dl.sourceforge.net/project/wqy/wqy-microhei/0.2.0-beta/wqy-microhei-0.2.0-beta.tar.gz
# 解压保存
tar -zxvf wqy-microhei-0.2.0-beta.tar.gz
mv wqy-microhei /usr/share/fonts/
# 复制到matlab jre路径
cp /usr/share/fonts/wqy-microhei/wqy-microhei.ttc /usr/local/MATLAB/MATLAB_Runtime/v912/sys/java/jre/glnxa64/jre/lib/fonts
# 进入相应目录
cd /usr/local/MATLAB/MATLAB_Runtime/v912/sys/java/jre/glnxa64/jre/lib/fonts
# 生成文件 fonts dir
mkfontdir
# 建立字体索引
mkfontscale
```

再次运行，中文字体修复
