---
title: "markdown图片批量处理到本地|base64 or  local"
date: "2021-01-25"
categories: 
  - "zatan"
---

# MdDealWithImg2-Base64OrLocal-、

仓库 [GitHub](https://github.com/96bearli/MdDealWithImg2-Base64OrLocal-)

# markdown图片批量处理到本地|base64 or local

写的发GitHub的英文版readme不能浪费是不是 **\>\_<**

## What's this?

Localization of image links referenced in markdown documents . And you have two options:

1. Save in Base64 format;
2. Reference local images.

## 这是个啥？

将markdown文档引用的图片链接本地化 你有两种选择：

1. 保存img为base64格式;
2. 引用下载到本地的图片.

ps：不太推荐1，特别是大量图片的文件，更容易出现种种问题，而且文档打开速度，1.2倍的大小等等，这些都是不利条件

## And How to use it?

- Put the document you want to process in the folder "before".
- Then run main.py.
- You can find the new documents in the folder "output".

## 怎么用？

- 把你想处理的.md文档放在before文件夹中
- 运行主程序main.py.
- 输出文件会保存到output文件夹

ps: 两种模式图片都会保存到output内同名文件夹，图片可能会请求失败或404，但是会创建一个空的图片文件顶位置。

另外每次运行output文件夹**自动重置清空**

## Import

```python
import re
import os
import urllib.error
import urllib.request
import shutil
import base64
```

## 当前版本的源码（最好前往github查看最新）

```python
# -*- codeing = utf-8 -*-
# @Time : 2021/1/25 13:29
# @Author : 96bearli
# @File : main.py
# @Software : PyCharm
import re
import os
import urllib.error
import urllib.request
import shutil
import base64

# python读取文件夹内文件名
def findMd(path):
    try:
        files = os.listdir(path)
        if len(files) == 0:
            print("指定目录下无文件，请检查重试")
            exit()
    except FileNotFoundError as error:
        print(error)
        print("文件夹./before不存在，已自动创建，请移动文件并重试")
        os.mkdir(path)
        exit()
    for name in files:  # 在列表中移除非.md文件
        if ".md" not in name:
            files.remove(name)
    if len(files) == 0:
        print("./before文件夹下不包含可处理的markdown文件，请检查重试")
        exit()
    return files

def findImgUrl(filePath):
    with open(filePath, "r", encoding="utf-8") as f:
        md = f.read()
    findImg = re.compile(r"!\[.*?\]\((http.+?)\)")
    # findImg = re.compile('http.+?jpg')
    urls = re.findall(findImg, md)
    return urls

def getImg(urls):
    dataList = []
    for url in urls:
        headers = {
            "User-Agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1 Edg/88.0.4324.96"
        }
        try:
            req = urllib.request.Request(url=url, headers=headers)
        except Exception as error:
            print(error)
            print("构造请求出错")
        try:
            imgData = urllib.request.urlopen(req,timeout=5)
            dataList.append(imgData)
        except Exception as error:
            print(error)
            print("Get image error! retry!")
            try:
                imgData = urllib.request.urlopen(req,timeout=5)
                dataList.append(imgData)
            except:
                print("Again!")
            dataList.append(" ")
    return dataList

def saveImg(fName, Datas):
    imgPath = "./output/%s" % fName.replace(".md", "")  # 文件保存路径，如果不存在就会被重建
    if not os.path.exists(imgPath):  # 如果路径不存在
        os.makedirs(imgPath)
    count = 0
    for Data in Datas:
        try:
            with open("%s/%s.jpg" % (imgPath, str(count)), 'wb') as f:
                f.write(Data.read())
        except Exception as imgError:
            print(imgError)
            print("图片保存出错，正在重试")
            try:
                with open("%s/%s.jpg" % (imgPath, str(count)), 'wb') as f:
                    f.write(Data)
            except Exception as imgError:
                print(imgError)
                print("还是不行")
                with open("%s/%s.jpg" % (imgPath, str(count)), 'w') as f:
                    f.write("Data")
        count += 1

def img2Base64(name, count):
    # image_base64 = str(base64.b64encode(image))
    # return image_base64

    with open("./output/%s/%s.jpg" % (name.replace(".md", ""), str(count)), 'rb') as f:
        image = f.read()
    image_base64 = str(base64.b64encode(image), encoding='utf-8')
    # print("%s.jpg转为base64格式成功" % str(count))
    str1 = str("data:image/jpg;base64," + image_base64)
    return str1

def saveMdBase64(urls, name, path):
    try:
        with open(path + "/" + name, 'r', encoding='utf-8') as f:
            text = f.read()
    except Exception as fileError:
        print(fileError)
        print("???文件呢，你给我藏哪去了？刚才明明还在的，得，下一个")
        return 0
    for i in range(len(urls)):
        try:
            data = img2Base64(name, i)
            # text = re.sub(urls[i], data, text)
            text = text.replace(urls[i], data)
        except Exception as Terror:
            print(Terror)
            print("图片转base64失败")
            continue
    with open("./output/%s" % name, "w", encoding='utf-8') as f:
        f.write(text)
    print("* %s处理完毕" % name)
    return 1

def saveMdLocal(urls, name, path):
    try:
        with open(path + "/" + name, 'r', encoding='utf-8') as f:
            text = f.read()
    except Exception as fileError:
        print(fileError)
        print("???文件呢，你给我藏哪去了？刚才明明还在的，得，下一个")
        return
    for i in range(len(urls)):
        try:
            text = text.replace(urls[i], "./%s/%s.jpg" % (name.replace(".md", ""), str(i)))
        except Exception as Terror:
            print(Terror)
            print("就这么一会儿文件内容就动过了？")
            continue

    with open("./output/%s" % name, "w", encoding='utf-8') as f:
        f.write(text)
    print("* %s处理完毕" % name)

if __name__ == '__main__':
    readPath = "./before"
    fileList = []
    fileList = findMd(readPath)
    print("请注意:output文件夹即将自动重置，请关闭output文件夹或该文件夹下的任何正在使用程序")
    print("-" * 30)
    choose = input("请选择转存模式(默认base64):\n* 输入1:本地图片\n* 输入其他:base64(如果图片太多会打开困难，请谨慎选择)\n")
    outputPath = "./output"  # 文件保存路径，如果不存在就会被重建
    if os.path.exists(outputPath):  # 如果路径存在
        shutil.rmtree(outputPath)
        # os.remove(outputPath)  # 这个不对，应该是递归删除非空
    for fileName in fileList:
        print("当前进度:%i/%i"%(fileList.index(fileName)+1,len(fileList)))
        try:
            imgUrls = findImgUrl(readPath + '/' + fileName)  # 用获取的文件路径查找文件中的img链接
            if len(imgUrls) == 0:
                print("%s内未引用图片链接,跳过" % fileName)
                print("-" * 30)
                continue
            elif len(imgUrls) >= 10:
                print("当前文件需要处理%i个链接，可能需要较长时间，请耐心等待"%len(imgUrls))
        except Exception as error:
            print(error)
            print("文件%s获取图片链接出错" % fileName)
            continue
        try:
            imgDatas = getImg(imgUrls)
        except Exception as error:
            print("图片数据获取失败")
            continue
        try:
            saveImg(fileName, imgDatas)
        except Exception as error:
            print(error)
            print("图片保存出错")
            continue
        if choose == "1":
            try:
                saveMdLocal(imgUrls, fileName, readPath)
            except Exception as error:
                print(error)
                print("文件%s保存出错，图片转Local出错" % fileName)
                continue
        else:
            try:
                if saveMdBase64(imgUrls, fileName, readPath) == 0:
                    print("-" * 30)
                    continue
            except Exception as error:
                print(error)
                print("文件%s保存出错，图片转base64出错" % fileName)
                continue
        print("新的文件%s已保存到output文件夹" % fileName)
        print("-" * 30)
    print("全部完成")
```
