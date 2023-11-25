---
title: "从mysql可视化工具、excel表格等复制的数据转化为标准的markdown表格"
date: "2022-08-31"
categories: 
  - "linux"
  - "python"
  - "windows"
  - "app"
---

# genMarkdownTable

- en: data copy from sql、excel transfer Markdown table
- zh: 从mysql可视化工具、excel表格等复制的数据转化为标准的markdown表格
- 或者更多的数据来源，改变函数参数即可
    
    ## github
    
    [https://github.com/96bearli/genMarkdownTable](https://github.com/96bearli/genMarkdownTable)
    
    ## main
    

```python
def gen_md_table(cp: str, sp="\r\n", sp_="\t", header=None):
    """
    生成markdown表格
    :param cp: str 从sql复制的表格数据，特征是以\r\n分行，\t分隔同行数据
    :param sp: str 行末分隔符，默认"\r\n"
    :param sp_: str 同一行数据分隔符，默认"\t"
    :param header: list 表头列表，默认["字段名", "字段说明", "字段类型"]
    :return: str
    """
    if header is None:
        header = ["字段名", "字段说明", "字段类型"]
    str_return = "|" + "|".join(header) + "|" + "\n"
    str_return += "|  ---- " * header.__len__() + "|\n"
    sts = cp.split("\r\n")
    for s in sts:
        if not s:
            continue
        str_return += '|' + '|'.join([_ for _ in s.split("\t")]) + '|\n'
    return str_return
```

## example

```python
exp = """Id  station'id varchar(255)
CityId  city'id    varchar(255)
StationName name    varchar(255)"""

result = gen_md_table(cp=exp, sp="\n", sp_="\t", header=['name', 'description', 'type'])
print(result)

"""output
|name|description|type|
|  ---- |  ---- |  ---- |
|Id|station'id|varchar(255)|
|CityId|city'id|varchar(255)|
|StationName|name|varchar(255)|
"""
```
