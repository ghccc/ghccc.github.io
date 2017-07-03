---
layout: post
title:  在ubuntu16.04上安装genymotion安卓模拟器
date:   2017-07-03
tag: 安卓 玩机
---
### 一.起因

最近沉迷于王者农药，但是感觉在手机上玩着不爽快，于是想着可以电脑玩就好了。

### 二.经过

谷歌一番之后选定了名为genymotion的据说是性能卓越，是历史上最快的安卓模拟器，最重要的是居然支持linux系统啊有木有！！！

#### （一）注册下载并安装

   1.在[官网](https://www.genymotion.com/)注册，然后在点击**Trial**之后会自动跳出来一个界面，点击下方的**Get Genymotion personal version**选择个人版本，接着弹出下载界面，选择ubuntu 16.04版本，下载。
  
**ps：因为本人以前已经安装过virtualbox，所以此处可以直接安装genymotion。**

   2.下载后得到一个名为genymotion-2.9.0-linux_x64.bin的二进制文件，**Alt + T**打开终端，接着键入：

	chmod +x genymotion-2.9.0-linux_x64.bin "给文件添加执行权限"
	./genymotion-2.9.0-linux_x64.bin	“执行文件开始安装”
	
询问安装路径，默认安装在当前路径下，可以将安装文件复制到目的目录下再安装，安装完之后移动整个genymotion文件夹到目的目录下也行，y，回车。

#### （二）配置软件

   打开软件之后直接登录刚刚注册的账号，并勾选个人试用版本。登录之后点击上方菜单栏的**Add**添加虚拟机，选择版本，**Next**开始下载，下载完毕之后就可以打开模拟机了。

##### 1.安装ARM_Translation_Android和Google APPS

然而这时候并不能直接安装APP，因为genymotion是一款基于x86架构的安卓模拟器，而大部分安卓模拟器都是基于ARM架构的，所以我们还需要安装ARM_Translation_Android系列包来进行兼容。

ARM_Translation_Android系列包下载地址：[这里](https://mega.nz/#F!JhcFwKpC!yfhfeUzvIZoSdBgfdZ9Ygg)

安卓 4.4及以下： [ARM Translation Installer v1.1](http://www.mirrorcreator.com/files/0ZIO8PME/Genymotion-ARM-Translation_v1.1.zip_links) 

[gapps](https://www.androidfilehost.com/?fid=95832962473395379)

安卓 5.x ：ARM_Translation_Lollipop   
gapps-x86-5.0_20160402.zip/gapps-x86-5.1_20160402.zip


安卓 6.x： ARM_Translation_Marshmallow   
gapps-x86-6.0_20160402.zip

### 三.结果

终于可以好好的玩一下农药了,谁都别拦我，我要上王者!!!!!!!!!!!!








   






参考文章：

1. [官网](https://www.genymotion.com/help/desktop/faq/#google-play-services)

[Ubuntu 14.04 使用速度极快的Genymotion 取代蜗牛速度的原生AVD模拟器](http://blog.csdn.net/tecn14/article/details/27566599)

[github上的genymotion](https://gist.github.com/wbroek/9321145)

[genymotion参考配置指南](http://www.snowdream.tech/2016/10/17/android-genymotion-install-and-settings/)

[Genymotion Tool Collections](http://23pin.logdown.com/posts/697026)

