---
layout: post
title: "利用live555在Mac端搭建rtsp推流"
date: 2017-10-20 10:44:12 +0800
comments: true
thunbnail: /img/2017/10/1.jpg
cover: /img/2017/1.jpg
categories: 
---

利用live555在自己的Mac电脑上搭建一个rtsp推流；<!--more-->

最近需要探查SDK是否支持RTP协议，需要一个rtsp流地址播放，但是网络上给的测试地址只有一个大白熊的流可以用，但是这个流非常卡，SDK播放非常有问题，说明了SDK对于rtsp支持是有问题的，但是不清楚是否是因为这个流也有问题呢。所以就自己就想办法自己推流试试是否可以播放。后续再进行做相关的SDK优化。

老规矩，google.com，找到live555.com。自己下载源码编译，http://www.live555.com/liveMedia/#config-unix在此处找到Mac编译。

1. 下载tar.gz包，解压并cd live
2. 终端至live文件夹，运行`./genMakefiles <os-platform>`，注意：这里的os-platform是mac平台，至于其他平台可以查看live文件夹下的config.<os-platform>文件，Mac对应macosx。所以是`./genMakefiles macosx`
3. 然后运行`make`。如果你想打包live555库，可以运行`make install`。解决端口占用，最好运行这个命令`make CPPFLAGS=-DALLOW_RTSP_SERVER_PORT_REUSE=1 install`


编译完成之后就会生成mediaServer文件夹，会有一个live555MediaServer可执行文件。
```  
cd mediaServer
./live555MediaServer   
```

这样就启动了一个 rtsp server，根据提示当前只支持部分视频格式，并不支持.mp4后缀的文件。

可以看到支持格式有：h264、h265、.aac、.ac3、.amr、.dv、.m4e、.mkv、.mp3、.mpg、.ogg、.ts、.vod、.wav、.webm

推流步骤：  
将NARUTO.mkv文件复制到和上面live555MediaServer可执行文件的同一个目录，
可用vlc在打开网络中输入地址 rtsp://your_ip:port/file.mkv 观看视频了。
还可以生成 m3u8文件在手机上访问，http://your_ip:8000/file.mkv。

手机端观看：同一个网络环境下，输入上面vlc地址既可以播放   
rtsp://192.168.1.106:8554/NARUTO.mkv