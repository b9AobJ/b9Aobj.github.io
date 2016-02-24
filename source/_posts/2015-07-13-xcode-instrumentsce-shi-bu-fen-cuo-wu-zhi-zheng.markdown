---
layout: post
title: "Xcode Instruments测试部分错误指正"
date: 2015-07-13 09:42:46 +0800
comments: true
thumbnail: /img/2015/06/07130942.jpg
cover: /img/2015/07130942b.jpg
bootstrap: true
categories: 
---
###Automation Error： ScriptAgent signaled.
当自动script数值不能在真机跑并出现以上错误的时候，<!--more-->我们需要在真机`设置`-`开发者`-`Enable UI Automation`打开即可。并且你还需要打开`logging`-`start recording`选项。  

###Script threw an uncaught JavaScript error: target.frontMostApp().alert().buttons()["取消"] could not be tapped  
