---
title: "win10 powershell切换conda虚拟环境无效"
date: "2022-04-27"
categories: 
  - "windows"
---

```powershell
#初始化powershell
conda init powershell
#更改powershell的执行策略
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

重新打开powershell就可以激活了
