+++
title = 'Hugo站点起步'
date = 2023-11-25T17:07:27+08:00
draft = true
+++

# 发布博客流程

作为备忘

~~~sh
hugo new post/hugo站点起步.md
~~~

在content/post下生成一份文档，完成编辑

~~~sh
hugo -d docs --buildDrafts
~~~

生成静态页面文件到docs目录

~~~sh
git add .
git commit -m "msg"
git push
~~~

