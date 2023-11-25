---
title: "Midas学习笔记"
date: "2020-02-19"
categories: 
  - "大学课程重点"
---

## Midas学习笔记

### **目录**

\[TOC\]

### 程序界面

#### 横向菜单

![](images/6zcmksVLgFbexUX.png) ![](images/vqJxHT3eIkXlgi6.png) ![](images/apYPXAegwZrxR6S.png)

1. 常用单位系
2. MCT命令流
3. SCP截面特性计算器

* * *

#### 竖向菜单

![](images/aLTqF4uJpg9tNd5.png) 树形菜单-工作：建立模型的每一步操作 可能用到 定义组 ![](images/lcS9CEaWsgfHDVi.png) ![](images/hpKnlj7vZyEoGS3.png) 用户自定义-左右两个树形菜单，作用：边操作边检查![](images/svezXnUHLDmtaQ9.png)

* * *

![](images/AZvgxXb7yEuPdzD.png) 信息窗口：方便查看操作和错误信息

* * *

可以查看结构，荷载。右上角视角。 ![](images/gqoeO29CpKwMGtm.png)

* * *

方便实用小菜单 常用的激活（F2）和钝化（ctrl+F2） ![](images/9aPSDHZ3jLy1tqW.png)

* * *

### 材料定义

![](images/Geukj5WsLP6Bci7.png) **材料特性值就是定义材料** **材料定义有三种方法**

#### 方法一：程序includes

![](images/sdBkYvgTC1XR2qn.png) **演示** ![](images/M3DvWNJ2Hjs1hX7.png) 点击**适用**后按序号显示在左侧窗口

#### 方法二：用户自定义

![](images/WmRMLOiqdkFIDPf.png) **演示** ![](images/5DFp6wfoMQBSAU3.png)

#### 方法三：导入现有模型里的材料

![](images/9f4PpSxv8qFeiLz.png) **演示** 导入——.mcb——打开——选择材料（留需要的在右侧）——编号方式 ![](images/N7JGvs8WAruw4Uj.png)![](images/JwvR6gEPaTKCnFS.png)

#### 结果可视

![](images/arZXeqcTKE4CIAo.png)

### 截面定义

![](images/ge2QwJAa7qthjZI.png) ![](images/ifYT6GH94ch3UXC.png)

也是有多种方法

#### 内嵌数据库

内嵌数据库的截面定义 ![](images/KY84DdE97gPIOB1.png) ![](images/EiOpPQjkZUhsCmc.png) 演示 ![](images/fmcCxOAFwQWEV8s.png) 截面特性值 ![](images/L63zvTNKbFStrYx.png) 点击适用

#### 用户定义

![](images/pM8VauglvdPwfFh.png) 演示 ![](images/x62abmCy8AQD5p3.png)

#### 数值

更关心截面特性值 ![](images/rcBE2Taze31M4OH.png) 演示 ![](images/lm2XZieKH3cfGFb.png) 点击计算——计算结果可更改——输入名称——适用

#### 导入已有模型

文件类型.mcb 不需要的放左边 ![](images/nxeCm9h4SrqtpAI.png)

#### 另：编辑可修改偏心

![](images/QOAEtTdLZm35wRx.png)

### 定义节点定义

| **横栏节点单元** 需要的截图过多，还是看视频吧 最简单的是输入每个节点坐标 较方便的是通过Excel | x | y | z |
| --- | --- | --- | --- |
| 0.09 | 0 | 0 |
| 0.795 | 0 | 0 |

然后点击**节点表格** [具体的操作和其他的方法还是看视频吧](https://www.bilibili.com/video/av47639045?p=6) 还有一个重排节点序号

### 建立单元

![](images/ZuT1pbMegvaxsBX.png) 单元可以是点线面 ![](images/2GfKvWbBIjPi1X9.png) 可以复制，扩展，分割，镜像（方向xyz，长度就是间距） 对于镜像的单元通过 /修改参数/统一单元坐标轴，参考原单元，xyz 进行方向的统一

导入CAD的.dxf文件生成相应的单元（导入前统一单位） 选择需要的图层导入 点击适用 ![](images/7TSn1JrjPRpV3Lb.png) ![](images/BJ1snbHuFLYrCSl.png)

### 定义边界

#### 解释

![](images/rWQ8w56ecINRk9U.png) 单个节点约束——前三栏 两个节点连接——弹性刚性 梁两端释放条件——释放/偏心栏目的释放梁端约束，比如铰接滑动滚动

#### 具体演示

1. 一般支撑 三个平动三个转动一个Rw单梁其自由度，选中节点，勾选进行约束自由度（默认参考整体坐标轴） ![](images/cwDbvWtkp8sRaOi.png)
2. 节点弹性支撑 类似1，不同在可自行输入弹簧刚度值（自行计算），也可打钩 ![](images/RUlEKVzoIf82uML.png)
3. 节点之间 弹性连接 三个平动三个转动 6个约束分量，或直接类型刚性（按照单元坐标系，注意方向）![](images/EkVWcvlHLFCXpbf.png) 设置好后选点![](images/lRzxnw4iXFGtfhq.png) 刚性连接:选择主节点——设置自由度（or类型）——定义从属节点（善用小菜单栏） ![](images/thOcxmNuf4bBFSZ.png) ![](images/vtubAT9V1INwSCs.png)
4. 杆端 ![](images/gfAFXt3QhBRZDys.png) 铰接滑动滚动 勾选要定义的约束分量（相对值和数值）——选择杆件 相对值和数值：传递出去的抗弯刚度为百分比还是定值 比如My=1为完全连接，=0为铰接 ![](images/SQTH97AVJzideNa.png)
    
    ### 定义荷载
    
    分为两步：
    
    #### 第一步 静力荷载工况
    
    名称类型 ![](images/dPxBhc15RvgIoO8.png) ![](images/oMUuNirOpC9Dk2X.png) ![](images/YrK8tfnaNTpZ6Ml.png)
    
    #### 第二步 给荷载赋予内容
    
    1.节点荷载 ![](images/x4y38JpZIrEmLf2.png) ![](images/mNEQ5PZLOdlpUiz.png) 选择要定义的节点，给分量赋值（按照整体坐标系） 2.单元荷载 ![](images/cs8Amfa3zZp6gbN.png) ![](images/SDsLf1FcrHmdJ8M.png) ![](images/xk7U1gil4Sw6nuj.png) 单元荷载——工况名称——荷载类型——选择单元——数值 相对值——相对于整体的？%位置受到P的？力 均布荷载添加后 ![](images/lNrJE2oyFzL5WCm.png) 3.面荷载 和2大同小异，注意选择对应的单元类型 ![](images/WYLMQrqlh9214Dn.png) ![](images/5naDWoztEPMxIK9.png) 演示结果 ![](images/T4kWuYC5RoOPBN8.png)
    
    ### 定义自重
    
    civil是通过截面面积、材料容重、单元构建的长度和自重系数四个参数计算自重的 定义自重荷载工况——添加自重——输入自重系数（如Z：-1）——点击添加——结果 ![](images/pef2q86Eh1zB4Ri.png) ![](images/7iA83zKHYgBFVpu.png) ![](images/B1GpIqhy4eS9ZuQ.png) ![](images/ICG6VjL8ztHQ52c.png) 当自重作用时考虑结构的容重与材料定义时的容重不同，输入自重容重/材料容重。 比如素混凝土25，自重容重钢筋混凝土26，输入-26/25，点击添加。修改用编辑 ![](images/vyMUxca2QuWimBS.png)
    
    ### 定义移动荷载
    
    四个步骤 ![](images/39YchxfPoI5AluX.png)
    
    #### 选择规范
    
    China ![](images/TzqSuK8plAHUiLw.png)
    
    #### 定义车道
    
    ##### 车道线——线单元模型
    
    偏心距——车轮间距——桥跨（冲击）——车辆移动方向——通过 两点or鼠标点取or单元号 选车道线 偏心距：车道线和建模线的差距，没有为0 对于连续梁，输入最大跨的长 ![](images/Qq1WLDCM4sHIZSi.png) ![](images/fDRJXEc9btzVOpq.png) ![](images/jXybBMtQqEZFdD2.png)
    
    ##### 车道面——板单元模型
    
    和线单元大同小异 ![](images/QajnRD8cIEo1sbt.png) ![](images/mU2ldoxkNOpiC9P.png) ![](images/vqg69jCf2ctKiWL.png)
    
    #### 定义移动荷载
    
    标准车辆（规范） 用户自定 ![](images/wEf2C6K9ecQIGyO.png) ![](images/PDShdKsZcfbNUWG.png)
    
    #### 定义移动荷载工况
    
    目的：移动荷载——》车道 ![](images/3lOvtHexwiCrNSu.png) ![](images/ehrNT7uPyJiFg8Q.png)
