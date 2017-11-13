---
layout: post
title:  CentOS利用iso镜像配置本地yum源
date:   2017-10-11 
tag: linux localyum
---

### 一.背景

在为客户的服务器搭建测试环境时，发现客户的服务器是内外网隔离的，因此无法使用在线yum源更新与安装软件，但是，搭建测试环境时，提示服务器缺少某些包，这些包不安装好，测试环境就无法搭建完成，于是就萌生了自己搭个本地yun源的念头。

### 二.搭建步骤

#### （一）上传iso镜像文件

服务器的系统版本是CentOS6.6

创建iso存放目录和挂载目录：

	mkdir /mnt/iso
	mkdir /mnt/cdrom

从本地目录上传iso文件到服务器的/mnt/iso目录下：

	scp /home/下载/CentOS-6.6-x86_64-bin-DVD1.iso  /mnt/iso/CentOS-6.6-x86_64-bin-DVD1.iso

上传文件需要时间，稍等一会儿。

#### （二）挂载镜像文件

将/mnt/iso目录下的镜像文件挂载到/mnt/cdrom目录下：

	mount -o loop /mnt/iso/CentOS-6.6-x86_64-bin-DVD1.iso /mnt/cdrom 

查看挂载是否成功：

	df -h

如果看到/mnt/cdrom即为挂载成功

#### （三）备份修改yum配置文件

yum源的配置文件位置是**/etc/yum.repos/**，在此目录下有四个配置文件:

**CentOS-Base.repo**是yum**网络源**的配置文件;

**CentOS-Media.repo**是yum**本地源**的配置文件;

至于其它两个配置文件是啥我也不是很清楚



创建一个目录把原有的配置文件放进去当做备份：

	mkdir /etc/yum.repos.d/bak
	cd /etc/yum.repos.d/
	mv *.repo /etc/yum.repos.d/bak

创建本地源的配置文件：

	vim loaclyum.repo

文件配置说明：

	[c6-media]
	name=CentOS-$releasever - Media
	#注：baseurl是iso文件的挂载目录，此处伟/mnt/cdrom
	baseurl=file:///mnt/cdrom/  
	#注：此处gpgcheck的值改为1
        gpgcheck=1
	#注：此处的enabled也改为1
	enabled=1
	#注：此处的值在你的/mnt/cdrom目录下可以看到，与其值一致即可
	gpgkey=file:///mnt/cdrom/RPM-GPG-KEY-CentOS-6

#### 清空缓存

	yum clean

#### 检查是否搭建完成

	yum install vim*

















