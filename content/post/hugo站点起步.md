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

迁移wordpress博客

~~~
git clone https://github.com/lonekorean/wordpress-export-to-markdown
mv blog.WordPress.2023-08-31.xml wordpress-export-to-markdown/export.xml
cd wordpress-export-to-markdown
npm install 
node index.js
# 交互cli，自动生成dir、md，并下载img
# 查看输出目录
~~~
