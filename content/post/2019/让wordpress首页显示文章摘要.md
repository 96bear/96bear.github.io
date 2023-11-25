---
title: "让WordPress首页显示文章摘要"
date: "2019-09-13"
categories: 
  - "wordpress"
tags: 
  - "twenty-fifteen"
  - "twenty-twelve"
  - "wordpress"
  - "wordpress显示文章摘要"
---

wordpress首页显示全文不但拖慢了加载速度，还覆盖了好的文章，浪费页面空间。之前为了这个解决问题查了不少资料。终于在[过往记忆该篇文章](https://www.iteblog.com/archives/21.html)（此处致谢！）的帮助下完成了这一操作，但部分内容因为**主题不同略有区别。**

原教程使用的 主题Twenty Twelve ，我是用的Twenty Fifteen。

下面贴出原教程和对应的改动。

* * *

**_原教程：_**

到wordpress后台，依次选择 **外观-->编辑-->选择右边的index.php文件**，在里面可以看到语句

<table class="wp-block-table"><tbody><tr><td><code>&lt;?php </code><code>while</code> <code>( have_posts() ) : the_post(); ?&gt;</code><code>&lt;?php get_template_part( </code><code>'content'</code><code>, get_post_format() ); ?&gt;</code><code>&lt;?php </code><code>endwhile</code><code>; ?&gt;</code></td></tr></tbody></table>

可以看出，index.php是嵌套一个 content.php 的文件用于专门显示文章的内容，这就是为什么在首页老是显示文章全文。那么，打开content.php文件找到

<table class="wp-block-table"><tbody><tr><td><code>&lt;?php </code><code>the_content( __( </code><code>'Continue reading &lt;span&gt;&amp;rarr;&lt;/span&gt;'</code><code>, </code><code>'twentyeleven'</code> <code>) ); </code><code>?&gt;</code></td></tr></tbody></table>

将它修改为

<table class="wp-block-table"><tbody><tr><td><code>&lt;?php if(!is_single()) {the_excerpt();} else</code> <code>{the_content(__('(more…)'));} ?&gt;</code></td></tr></tbody></table>

**改动**：

可以看到原教程分成三步

第一步经验证完全符合，

第二步第三步：

找到line32--line37

<table class="wp-block-table"><tbody><tr><td>the_content(<br>sprintf(<br>__( 'Continue reading %s', 'twentyfifteen' ),<br>the_title( '&lt;span class="screen-reader-text"&gt;', '&lt;/span&gt;', false )<br>)<br>);</td></tr></tbody></table>

替换为：

<table class="wp-block-table"><tbody><tr><td><code>if(!is_single()) {the_excerpt();} else</code> <code>{the_content(__('(more…)'));}</code></td></tr></tbody></table>

* * *

OK，over。其他主题照情况来改就行

友情提示，修改前最好做个备份避免网站崩溃。

![](images/get.jpg)

http://bear962464.cn/2019/09/07/370/
