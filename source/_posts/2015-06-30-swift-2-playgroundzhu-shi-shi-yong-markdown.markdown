---
layout: post
title: "Swift 2 Playground注释使用markdown"
date: 2015-06-30 11:40:29 +0800
comments: true
thumbnail: /img/2015/06/12.jpg
cover: /img/2015/12.jpg
bootstrap: true
categories: 
---
　　在Xcode 7和Swift 2 playground当中引用了markdown语法来写注释。<!--more-->  
标准代码注释可以是单行注释、多行注释：  
`//A Single line comment`   
`/*`  
`A block comment`  
`over two lines`  
`*/`  


　　而现在Xcode 7我们可以丰富我们的注释文本了，只需在后面添加一个`:`即可：  
`//: _Rich text_ single line *comment* thanks to __markdown__`  
![](/img/2015/06/playground/playground1.png)  
　　可能你加上去之后没有系那是出效果，如果要看文本效果的话，可以在菜单中选择`Editor`--`Show Rendered Markup`，然后就可以看到效果了。  

![](/img/2015/06/playground/playground2.png)  

![](/img/2015/06/playground/playground3.png)
