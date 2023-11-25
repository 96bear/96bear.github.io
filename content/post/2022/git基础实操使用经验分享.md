---
title: "Git基础实操使用经验分享"
date: "2022-03-21"
categories: 
  - "linux"
  - "app"
  - "zatan"
---

# Git使用经验

## 最基础

```shell
git clone url
git pull

git push
```

## Git Config相关

### 1.设置代理

```shell
git config --global http.proxy http://192.168.1.20:20172
git config --global https.proxy http://192.168.1.20:20172
```

### 2.关闭ssl验证

```shell
git config --global http.sslVerify "false"
```

### 3.查看设置

```shell
git config --global -l
```

## branch相关

### 1.创建分支

```shell
git branch branch_name
```

### 2.查看所有分支

```shell
git branch -l
```

### 3.切换分支

```shell
git branch branch_name
```

### 4.删除分支

```shell
git branch -d branch_name
# 强行删除 -d换-D
```

## 实际使用

### 1.如何贡献代码

转载自[一步一步教你怎样给Apache Spark贡献代码 | Parallel Labs](http://www.parallellabs.com/2014/08/05/how-to-contribute-to-spark-step-by-step/)

**以Apache Spark为例**

1. 到 Apache Spark 的github 页面内点击 **fork** 按钮
2. 你的github仓库中会出现 spark 这个项目
3. **本地电脑上**， 使用

```bash
git clone [你的 spark repository 的 github 地址]
例如：
git clone git@github.com:gchen/spark.git
```

本地得到一个叫 **spark 的文件夹**

4. **进入该文件夹**，使用

```text
git remote add upstream https://github.com/apache/spark.git
```

添加 Apache/spark 的远程地址

5. **使用\*\***

```text
git pull upstream master 
```

得到目前的 Apache/spark 的最新代码，现在我们在 你自己fork的Spark代码仓库的master 这个分支上，以后这个分支就留作跟踪 upstream 的远程代码

6. **好了**，现在你可以开始贡献自己的代码了。

按照开发惯例，我们一般**不在**自己代码仓库的master上提交新的代码，而是需要为每一个新增的功能或者bugfix新增一个**新的branch**。使用：

```text
git checkout -b my_change
```

创建新的分支，现在我们可以在这个分支上更改代码

7. **添加代码**，**并提交代码**：

```text
git add .
git commit -m “message need to be added here”
```

8. 提交Pull Request**前合并冲突**

在我们提交完我们的代码更新之后，一个常见的问题是远程的upstream（即apache/spark)已经有了**新的更新**，从而会导致我们提交Pull Request时会导致conflict。为此我们可以在提交自己这段代码前手动先把远程其他开发者的commit与我们的commit合并。使用：

```text
git checkout master
```

切换到我们自己的**主分支**，使用

```text
git pull upstream master 
```

拉出apache spark的**最新**的代码。**切换回 my\_change 分支**，使用

```text
git checkout my_change
git rebase master
```

然后把自己在my\_change分支中的代码更新到在自己github代码仓库的my\_change分支中去：

```text
git push origin my_change 
```

将代码提交到自己的仓库。

9. 提交**Pull** **Request**

这时候可以在自己的仓库页面跳转到自己的my\_change分支，然后点击 new pull request。按照Spark的风格规定，我们需要在新的Pull Request的标题最前面加上JIRA代号。所以我们需要在System Dashboard上创建一个新的JIRA，例如\[SPARK-2859\] Update url of Kryo project in related docs)。然后把SPARK-2859这个代号加到你的Pull Request的标题里面。

例如：[\[SPARK-2859\] Update url of Kryo project in related docs by gchen · Pull Request #1782 · apache/spark · GitHub](https://github.com/apache/spark/pull/1782)

Pull Rquest的描述的写法很重要。有几个要点：

（1）在Pull Request的描述中，一定记得加上你提交的JIRA的url，方便JIRA系统自动把Pull Request的链接加进去，例如[\[SPARK-2859\] Update url of Kryo project in related docs](https://issues.apache.org/jira/browse/SPARK-2859)。

（2）PR的描述要言简意赅，讲清楚你要解决的问题是什么，你怎么解决的。大家可以多参考其他committer提交的PR。

10. 等待Spark committer审核你的PR。

如果需要进一步的代码修改，你可以继续在本地的my\_change分支下commit新的代码，所有新的代码会在”git push origin my\_change”之后自动被加入你之前提交的Pull Request中，方便进行问题的跟踪和讨论。

11**.** 如果一切顺利，具有apache/spark.git 写权限的commiter就会把你的代码merge到apache/spark.git的master里面去了！

ps. 你的代码被merge完之后，就可以把my\_change这个分支给删掉了:)

### 2.Git push 过的东西，后悔了怎么办

1. 查看你要回到的那个版本
    
    ```shell
    git log
    ```
    
2. 回退到上个版本
    
    ```shell
    git reset --hard HEAD^ 
    ```
    
3. 退到/进到指定commit\_id
    
    ```shell
    git reset --hard commit_id
    ```
    
4. 修改提交到远程
    
    ```shell
    git push origin HEAD --force
    ```
