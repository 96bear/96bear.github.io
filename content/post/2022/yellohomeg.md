---
title: "黄嘉琪诈骗升级"
date: "2022-02-06"
categories: 
  - "zatan"
---

# 黄嘉琪诈骗升级，伪装链接教学

![图片描述](images/1806_458d82e801bab3faaac946296a27114e.jpg)  
点进去是我老婆的芒种捏

## 先说结论

非常简单易懂 如芒种 [https://b23.tv/av380439292?BV1Qa411175n](https://b23.tv/av380439292?BV1Qa411175n)  
固定短链接+目标视频av号+英文?+伪装视频bv号

## 原理

一个猜测不一定对，评论是通过正则表达式提取bv/av号的，并且在同一个链接内的bv号优先于av号（毕竟以后以bv为主了），拿到bv/av号后返回视频信息在评论区显示标题。  
而点击结论里的链接时?后的参数是不起作用的，也就是说进入的是av号的视频。  
注意一个链接含两个bv号时会显示第一个的信息，大概类似于使用re.findall(find\_bv,url)\[0\]

## 怎么发现的

1. 以前见过有人这么干过，知道有可行性(方法可能不同）
2. 有人揭露过走接口生成b23短链接任意跳转的漏洞(已修复)，猜测后端使用正则提取信息，适当发散一下
3. b23.tv/av.+和b23.tv/bv.+在APP/pc页面会显示相关视频信息
4. 多试试就出结论了

## 怎么找av号

百度一下有很多源码/api可以用，油猴插件也行就不一一说了，py写了个小工具  
bv转av算法来自于csdn

```python
import math  
def BvToAv(Bv):  
    # 1.去除Bv号前的"Bv"字符  
    BvNo1 = Bv[2:]  
    keys = {  
        '1':'13', '2':'12', '3':'46', '4':'31', '5':'43', '6':'18', '7':'40', '8':'28', '9':'5',  
        'A':'54', 'B':'20', 'C':'15', 'D':'8', 'E':'39', 'F':'57', 'G':'45', 'H':'36', 'J':'38', 'K':'51', 'L':'42', 'M':'49', 'N':'52', 'P':'53', 'Q':'7', 'R':'4', 'S':'9', 'T':'50', 'U':'10', 'V':'44', 'W':'34', 'X':'6', 'Y':'25', 'Z':'1',  
        'a': '26', 'b': '29', 'c': '56', 'd': '3', 'e': '24', 'f': '0', 'g': '47', 'h': '27', 'i': '22', 'j': '41', 'k': '16', 'm': '11', 'n': '37', 'o': '2',  
        'p': '35', 'q': '21', 'r': '17', 's': '33', 't': '30', 'u': '48', 'v': '23', 'w': '55', 'x': '32', 'y': '14','z':'19'  

    }  
    # 2. 将key对应的value存入一个列表  
    BvNo2 = []  
    for index, ch in enumerate(BvNo1):  
        BvNo2.append(int(str(keys[ch])))  

    # 3. 对列表中不同位置的数进行*58的x次方的操作  

    BvNo2[0] = int(BvNo2[0] * math.pow(58, 6));  
    BvNo2[1] = int(BvNo2[1] * math.pow(58, 2));  
    BvNo2[2] = int(BvNo2[2] * math.pow(58, 4));  
    BvNo2[3] = int(BvNo2[3] * math.pow(58, 8));  
    BvNo2[4] = int(BvNo2[4] * math.pow(58, 5));  
    BvNo2[5] = int(BvNo2[5] * math.pow(58, 9));  
    BvNo2[6] = int(BvNo2[6] * math.pow(58, 3));  
    BvNo2[7] = int(BvNo2[7] * math.pow(58, 7));  
    BvNo2[8] = int(BvNo2[8] * math.pow(58, 1));  
    BvNo2[9] = int(BvNo2[9] * math.pow(58, 0));  

    # 4.求出这10个数的合  
    sum = 0  
    for i in BvNo2:  
        sum += i  
    # 5. 将和减去100618342136696320  
    sum -= 100618342136696320  
    # 6. 将sum 与177451812进行异或  
    temp = 177451812  

    return sum ^ temp  

if __name__ == '__main__':  
    Bv = input("请输入视频Bv号:")  
    Bbv = input("请输入用于伪装的Bv号\n以提供标题，留空不伪装:")  
    av = str(BvToAv(Bv))  
    print(Bv + "的Av号为:av" + av)  
    print("链接地址为：https://www.bilibili.com/video/av" + av)  
    print("短链接地址：b23.tv/av" + av)   
    if Bbv:  
        print("伪装链接为：https://b23.tv/av" + av+"?"+Bbv)
```
