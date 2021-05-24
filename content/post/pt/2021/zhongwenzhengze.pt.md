---
title: "中文匹配" # Title of the blog post.
date: 2021-04-30T14:55:12+08:00 # Date of post creation.
description: "Article description." # Description used for search engine.
featured: true # Sets if post is a featured post, making it appear on the sidebar. A featured post won't be listed on the sidebar if it's the current page
draft: false # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
#featureImage: "/images/path/file.jpg" # Sets featured image on blog post.
#thumbnail: "/images/path/thumbnail.png" # Sets thumbnail image appearing inside card on homepage.
#shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - JavaScript
tags:
  - Reg
  - 中文
---

\w匹配的仅仅是中文，数字，字母，对于国人来讲，仅匹配中文时常会用到，见下

```
匹配中文字符的正则表达式： [\u4e00-\u9fa5]
```
或许你也需要匹配双字节字符，中文也是双字节的字符<!--more-->
```
匹配双字节字符(包括汉字在内)：[^\x00-\xff]
```
注：可以用来计算字符串的长度（一个双字节字符长度计2，ASCII字符计1）
更多常用正则表达式匹配规则：
```
英文字母:[a-zA-Z]

数字:[0-9]
```
匹配中文，英文字母和数字及_:
```
^[\u4e00-\u9fa5_a-zA-Z0-9]+$
```
同时判断输入长度：
```
[\u4e00-\u9fa5_a-zA-Z0-9_]{4,10}
^[\w\u4E00-\u9FA5\uF900-\uFA2D]*$
```
1、一个正则表达式，只含有汉字、数字、字母、下划线不能以下划线开头和结尾：
```
^(?!_)(?!.*?_$)[a-zA-Z0-9_\u4e00-\u9fa5]+$
```
其中：

^ 与字符串开始的地方匹配
```
(?!_)　　不能以_开头

(?!.*?_$)　　不能以_结尾

[a-zA-Z0-9_\u4e00-\u9fa5]+　　至少一个汉字、数字、字母、下划线
```
$　　与字符串结束的地方匹配
```
放在程序里前面加@，否则需要\\进行转义 @"^(?!_)(?!.*?_$)[a-zA-Z0-9_\u4e00-\u9fa5]+$"

（或者：@"^(?!_)\w*(?<!_)$" 或者 @" ^[\u4E00-\u9FA50-9a-zA-Z_]+$ " )
```
2、只含有汉字、数字、字母、下划线，下划线位置不限：
```
^[a-zA-Z0-9_\u4e00-\u9fa5]+$
````
3、由数字、26个英文字母或者下划线组成的字符串
```
^\w+$
```
4、2~4个汉字
```
@"^[\u4E00-\u9FA5]{2,4}$";
```
5、
```
^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$
```
用：(Abc)+ 来分析： XYZAbcAbcAbcXYZAbcAb

作者：每一天为明天168  
链接：https://www.jianshu.com/p/8695c2ba8ace  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。