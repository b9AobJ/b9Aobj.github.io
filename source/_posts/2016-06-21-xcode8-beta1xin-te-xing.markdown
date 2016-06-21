---
layout: post
title: "Xcode8 beta1新特性"
date: 2016-06-21 10:01:36 +0800
comments: true
thumbnail: /img/2016/06/1s.jpg
cover: /img/2016/1.jpg
categories: 
---
   WWDC 2016已经结束，伴随着iOS10的发布，新的Xcode8beta1也出来了，虽然是beta，以前也被beta版本各种蹂躏，但还是需要亲自下载来体验一波的。 <!--more-->   
###Swift 3  
   Swift 3 是第一个主要的release版本。这个release版本统一核心API命名规则,,旨在增强代码的一致性和清晰。流行的系统API，如Core Graphics和 Grand Central Dispatch 与Swfit有着更丰富的表现力和协调力。
###Interface Builder
Interface Builder 设计画布已经彻底再造工程，让你更快地工作并且提供更大的控制。在任何充满活力的苹果设备上看到一个完全实时的应用程序预览。你可以在不同的设备之间快速切换，看到不同设备中的界面显示效果。平移和缩放非常快，甚至你可以缩小故事板鸟瞰图时编辑你的界面。总体来说，Xcode8的interface builder胜过之前任何的版本，遥想当年使用sb，整台macBook卡死在那里的日子😂😂😂😂  
![](/img/2016/06/interfacebuilder.png)
###运行时问题 Runtime Issues

这Xcode新特性,自动识别跟踪找到漏洞并且报告问题, 有些很难跟踪的bug，直到您的应用程序到了用户手中,也可能没有被发现。

######Thread Sanitizer spots:新的线程污点清理器，解决多线程情况下的资源竞争条件,数据的变化和其它相关线程的bug  
######View Debugger:使用更新的带有更大的保真度和视觉精度检查UI约束问题的视图调试器  
######Memory Debugger:可以用新的内存调试跟踪器跟踪发出的内存泄漏警报  
![](/img/2016/06/viewmemory.png)
###签名变的简单而强大 Signing Made Easy and Powerful  
设备设置和代码签名有了极大的简化，新的自动化代码管理系统可以帮开发者自动签署他们需要的资源，从而让代码在你苹果设备的应用程序中正确地签名、调用和执行。开发者也可以选择手动配置文件，并且设置每个构建配置的签名程序，如果遇到任何问题，报告导航器会用需要改进的错误消息和日志提醒你。当你有个多个Mac的时候,Xcode会在每个Mac中自动生成对应的开发者证书。

在你的苹果设备上开发和运行您的应用程序和进入Xcode的偏好设置输入Apple ID一样容易。苹果开发者账号不是必需的。

Provisioning Profile 文件选取，已经从Buiid Settings移动到了General中,Buiid Settings中已经标识了 Deprecated。
![](/img/2016/06/sign.png)

