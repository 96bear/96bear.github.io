---
title: "python装饰器decorator的简单计时示例"
date: "2022-03-17"
categories: 
  - "python"
  - "zatan"
---

```python
#python3
import time

def timeit(iteration):
    def inner(f):
        #*args,**kwargs 
        def wrapper(*args, **kwargs):
            start = time.time()
            # _为丢弃变量
            for _ in range(iteration):
                ret = f(*args,**kwargs)
            print("Time used(s):",time.time()-start)
            return ret
        return wrapper
    return inner

@timeit(2)
def double(n):
    time.sleep(1)
    #return 2*n

double(2)
```
