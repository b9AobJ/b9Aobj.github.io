---
layout: post
title: "使用MWPhotoBrowser时出现的一些坑"
date: 2015-03-17 09:40:11 +0800
comments: true
thumbnail: /img/2015/06/2.jpg
categories: 
---
在使用MWPhotoBrowser时，公司项目需求要将这类型的图片浏览由横向拖动改为竖向拖动，我了个去，被玩坏了。。。。  <!--more-->
先说些错误吧，其实都是自己的问题，在没有搞清楚该项目架构的时候就在那里胡乱修改，导致后面直接重置后再修改，那工作量，不说了，都是自己惹的  

###改动1
&emsp;&emsp;&emsp;因为由横向改为竖向，即可以在MWPhotoBrowser.m文件中进行修改即可（至于这个开源代码怎么使用，就请自行查看作者的描述吧），主要是Frame Calculations这个代码块当中，具体的话是修改:
	