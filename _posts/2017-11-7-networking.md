---
layout: post
title:  linux主机双网卡（内外网）同时使用路由设置
date:   2017-11-7
tag: linux 网络
---

###  一.背景

在我帮客户做项目时，需要在用网线在内网连接客户服务器，同时需要连接外网查资料。我用的是ubuntu16.04系统，双网卡（一个有线网卡连内网和一个无线网卡连WiFi通过路由连外网）在同时设置内外网时，发现内网无法联通，用route命令查看时，发现有两个默认网关，其中外网的默认网关的跃点是600，内网的是100。



###  二.网卡配置

####  内网网卡 
  
 ip地址：10.100.216.55     

子网掩码：255.255.255.0    

网关：10.100.216.1    

设备：enp4s0f2

####  外网网卡

ip地址：10.100.113.44    

子网掩码：255.255.255.0  

网关：10.100.113.1  

dns服务器：114.114.114.114    

设备：wlp3s0

### 三.设置步骤

首先删除内网的默认网关：

	sudo route del default gw 10.100.216.55


然后设置内网转发，所有10.107.1开头的请求全部走enp4s0f2网卡

	sudo route add -net 10.107.1.0 netmask 255.255.255.0 dev enp4s0f2


备注：以上全部是临时设置，重启设备之后就会还原

参考文章：[Linux 双网卡（内外网） 同时使用路由设置]<http://m.itboth.com/d/3eiu6b/linux>

	


 
	









