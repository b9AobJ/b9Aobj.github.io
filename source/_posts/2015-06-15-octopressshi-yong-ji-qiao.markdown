---
layout: post
title: "octopress使用技巧"
date: 2015-06-15 16:18:57 +0800
datetime: 2015-06-15 16:18:57 +0800
comments: true
thumbnail: /img/2015/06/1.jpg
description: "octopress使用强技能"
categories: 
---
为自己的博客网站添加想要的功能和样式<!--more-->
http://youess.github.io/blog/octopress-blog-records.html 
##octopress基本操作
* 新建博客文章，`rake new_post['blog_name']`
* 产生HTML文件， `rake generate`
* 预览博客效果， `rake preview`
* 推送内容到github， `rake deploy` 或者 `rake gen_deploy`
* 保存到github项目中  
    `git add .`  
	`git commit -m "modified note"`  
	`git push [for first time](origin source)`  


##一些配置
###更换主题
	git clone https://github.com/jeremyrea/slimpress .theme/slimpress  
	rake install['slimpress']  

###修改`_config.yml`
*日期格式： <code>date-formate: %Y年-%m月-%d日</code>  
*分类前缀显示为中文：<code>category_title_prefix: "分类"</code>
*继续阅读/阅读全文：<code>excerpt_link: "阅读全文 &arr;"</code>