---
title: "wordpress之额外css"
date: "2019-09-06"
categories: 
  - "wordpress"
tags: 
  - "css"
  - "wordpress"
  - "优化"
  - "透明"
---

本文记录本站使用过的，正在使用的css

```


/*全站半透明化*/
.hentry,.post-navigation,.comments-area,.site-footer,.page-header {
    background-color: rgba(255, 255, 255, 0.7);
}
button, input[type="button"], input[type="reset"], input[type="submit"]{
    background-color:rgba(51, 51, 51, 0.8);
}
.entry-footer,button, input, select, textarea{
    background-color:rgba(247, 247, 247, 0.6);
}
input:focus,textarea:focus{
    background-color:rgba(247, 247, 247, 0.8);
}
@media screen and (min-width: 59.6875em) {
    body:before {
        background-color: rgba(255, 255, 255, 0.6);
    }
}
```

```
/*字体设置*/
*:not([class*="icon"]):not(i) {font-family: Segoe UI, "Microsoft Yahei" !important;}
```

长期更新
