---
layout: post
title: "Xcode_7_beta模拟器iOS9无法连接网络"
date: 2015-06-25 10:38:33 +0800
comments: true
thumbnail: /img/2015/06/11.jpg
cover: /img/2015/11.jpg
categories: 
---
he resource could not be loaded because the App Transport Security policy requires the use of a secure connection. <!--more-->  


&emsp;&emsp;在iOS9的iPhone模拟器中来调试的时候出现“<code>The resource could not be loaded because the App Transport Security policy requires the use of a secure connection.</code>” 
&emsp;&emsp;一开始以为是Xcode的beta版本的问题，后来发现不对，肯定是自己配置的问题，goggle一番后在github找到一篇番文，从而尝试之，奇效显著。[原文](https://github.com/meteor/meteor/issues/4560)   
&emsp;&emsp;解决方法：在App中的<code>info.plist</code>中添加<code>NSAppTransportSecurity</code>(Dictionary)，在其中添加一个key,<code>NSAllowsArbitraryLoads</code>(Boolean)并设置为<code>YES</code>.  
希望可以帮助到大家。
