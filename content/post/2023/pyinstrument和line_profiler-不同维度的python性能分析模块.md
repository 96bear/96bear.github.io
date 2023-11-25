---
title: "pyinstrument和line_profiler——不同维度的python性能分析模块"
date: "2023-06-30"
categories: 
  - "python"
---

## 函数维度：pyinstrument

`pip install pyinstrument` `pyinstrument your-script.py`

```shell
Usage: pyinstrument [options] scriptfile [arg] ...

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  --load-prev=ID        instead of running a script, load a previous report
  -m MODULE_NAME        run library module as a script, like 'python -m
                        module'
  --from-path           (POSIX only) instead of the working directory, look
                        for scriptfile in the PATH environment variable
  -o OUTFILE, --outfile=OUTFILE
                        save to <outfile>
  -r RENDERER, --renderer=RENDERER
                        how the report should be rendered. One of: 'text',
                        'html', 'json', 'speedscope', or python import path
                        to a renderer class
  -t, --timeline        render as a timeline - preserve ordering and don't
                        condense repeated calls
  --hide=EXPR           glob-style pattern matching the file paths whose
                        frames to hide. Defaults to '*/lib/*'.
  --hide-regex=REGEX    regex matching the file paths whose frames to hide.
                        Useful if --hide doesn't give enough control.
  --show=EXPR           glob-style pattern matching the file paths whose
                        frames to show, regardless of --hide or --hide-regex.
                        For example, use --show '*/<library>/*' to show frames
                        within a library that would otherwise be hidden.
  --show-regex=REGEX    regex matching the file paths whose frames to always
                        show. Useful if --show doesn't give enough control.
  --show-all            show everything
  --unicode             (text renderer only) force unicode text output
  --no-unicode          (text renderer only) force ascii text output
  --color               (text renderer only) force ansi color text output
  --no-color            (text renderer only) force no color text output
```

给出的结果以web页面做友好地展示 ![来自官网的示例图](images/screenshot.jpg)

## 代码维度：line\_profiler

`pip install line_profiler`

```python
#使用@profile 装饰要执行的函数
@profile
def fun(n):
    pass
fun(100)
"""

Wrote profile results to primes.py.lprof
Timer unit: 1e-06 s

File: primes.py
Function: primes at line 2
Total time: 0.00019 s

Line #      Hits         Time  Per Hit   % Time  Line Contents
==============================================================
     2                                           @profile
     3                                           def primes(n):
     4         1            2      2.0      1.1      if n==2:
     5                                                   return [2]
     6         1            1      1.0      0.5      elif n<2:
     7                                                   return []
     8         1            4      4.0      2.1      s=range(3,n+1,2)

"""
```
