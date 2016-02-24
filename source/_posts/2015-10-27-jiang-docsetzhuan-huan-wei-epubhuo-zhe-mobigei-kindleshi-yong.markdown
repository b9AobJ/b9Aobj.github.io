---
layout: post
title: "将docset转换为epub或者mobi给kindle使用"
date: 2015-10-27 13:52:53 +0800
comments: true
thumbnail: /img/2015/06/10271352.jpg
description: "将docset转换为epub或者mobi给kindle使用"
cover: /img/2015/10271352b.jpg
bootstrap: true
categories: 
---
手机看书逼格不够的，还是拿本kindle耍耍吧<!--more-->  

　　偶然间在github大洋中遨游的时候发现了有人将iOS.docset文档转换成一个个mobi文件可以供kindle使用。自己兴趣一下子提升了起来，由于该脚本原作者已将源码删除，因此找了将近一天时间终于在另一位github贡献者那里找到一份[源码](https://github.com/darvin/docset2ebook),二话不说先[fork](https://github.com/langyapojun/docset2ebook)过来先.  
　　
##使用方法
使用方法已经在里面有了，这里粗略讲解一下:  
1. 先去下载**kindlegen**;  
2. 将**kindlegen**的脚本路径放入$PATH中，这里我将其加入/Users/用户名/.bash_profile(注意，这是一个隐藏文件，请显示隐藏文件)中,  
　　<pre>`export PATH=$PATH:/Users/用户名/Desktop/KindleGen_Mac_i386_v2_9`  </pre>  
3. 下载docset2ebook的源码，解压出来后打开fetch_and_convert.sh,可以看到：  
    <pre><code>##!/bin/sh
FORMAT=epub
OUTPUT_DIR=~/Documents/ADCBooks
COMMAND="python docset2kindle.py"
mkdir $OUTPUT_DIR

$COMMAND /Developer/Documentation/DocSets -o $OUTPUT_DIR/XCode -f $FORMAT
$COMMAND /Library/Developer/Shared/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset -o $OUTPUT_DIR/OSX  -f $FORMAT

wget http://devimages.apple.com/docsets/20110720/com.apple.adc.documentation.AppleiOS4_3.iOSLibrary.xar
xar -xf com.apple.adc.documentation.AppleiOS4_3.iOSLibrary.xar
$COMMAND com.apple.adc.documentation.AppleiOS4_3.iOSLibrary.docset -o $OUTPUT_DIR/iOS -f $FORMAT</code></pre>  
	这里可以看到这是转换epub格式的，如果你想换成mobi，只需修改**FORMAT=mobi**,然后是找到iOS.docset的位置，在Mac OS 10.11当中，iOS.docset位置已不是这个位置，在这里我只需要iOS的文档，所以Mac OS的文档转换我全部注释了：  
	<pre><code>##!/bin/sh
FORMAT=mobi
OUTPUT_DIR=~/Documents/ADCBooks
COMMAND="python docset2kindle.py"
mkdir $OUTPUT_DIR

$COMMAND /Users/Sidney/Library/Developer/Shared/Documentation/DocSets/ -o $OUTPUT_DIR/XCode -f $FORMAT</code></pre>
4.打开终端，运行sh脚本,静待佳音。  
![](/img/2015/10/terminal.png)

![](/img/2015/10/directory.png)

![](/img/2015/10/kindle.png)

