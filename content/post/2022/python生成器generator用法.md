---
title: "Python生成器generator用法"
date: "2022-03-17"
categories: 
  - "python"
---

# 生成器generator

从[高天视频](https://www.bilibili.com/video/BV1KS4y1D7Qb)处学习

## 简单示例

- 知识点1 使用yield定义生成器函数，调用得到生成器对象
- 知识点2
    
    ```python
    def gen(num):
    while num > 0:
        yield num
        num -= 1
    return
    # g是一个生成器对象
    g = gen(5)
    print(next(g))
    print("---------")
    print(next(g))
    print("test done")
    for i in g:
    print(i)
    '''output
    5
    分割线
    4
    test done
    3
    2
    1
    '''
    ```
    
    ## 详细解释和生成器的send()
    
    ```python
    def gen(num):
    while num > 0:
        tmp = yield num
        if tmp is not None:
            num = tmp
        num -= 1
    return
    # g是一个生成器对象
    g = gen(5)
    print(next(g)) # 等价 print(g.send(None))
    print("---------")
    print(next(g))
    print("test done")
    print(f"send:{g.send(10)}")
    # for loop 等价 i=next(g) loop
    for i in g:
    print(i)
    '''output
    5
    分割线
    4
    test done
    send:9
    8
    7
    6
    5
    4
    3
    2
    1'''
    ```
    

<iframe src="//player.bilibili.com/player.html?aid=724754059&amp;bvid=BV1KS4y1D7Qb&amp;cid=551523345&amp;page=1" allowfullscreen="true"></iframe>
