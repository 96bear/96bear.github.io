---
title: "增加右键选项“通过Pycharm打开”"
date: "2022-03-31"
categories: 
  - "python"
  - "windows"
  - "编程"
---

图中这个选项的增加，删除同理。直接上流程

![image-20220331112319665](images/KXrhZ2ULseqPQcw.png)

## 流程

1. win + R 输入 regedit 回车打开注册表编辑器
    
2. 进入此路径
    
    ```
    计算机\HKEY_CLASSES_ROOT\directory\background\shell
    ```
    
    ![image-20220331113015937](images/J7BGouEL1lwqiXP.png)
    
3. shell下新建项，名为pycharm
    
    pycharm下有个默认值，右键修改，数值数据改为“通过Pycharm打开”
    
    ![image-20220331113407108](images/SfOgG9PanHCRjJt.png)
    
4. pycharm下新建项，名为command
    
    command下有个默认值，右键修改，数值数据改为pycharm64.exe的**绝对路径**\+ **"%V"**
    
    （右键pycharm的快捷方式，点击属性查看路径
    
    ![image-20220331113326152](images/bmWnRUC9x8EIMuJ.png)
    
5. 可选：为“通过Pycharm打开”前增加图标
    
    pycharm下右键新建->字符串值，名称：icon，数值数据改为pycharm64.exe的**绝对路径**
    

![image-20220331113859358](images/VLT51e4NHqlA3ks.png)
