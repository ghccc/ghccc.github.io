---
layout: post
title:  Windows10免安装oracle客户端通过plsql连接远端数据库
date:   2017-11-23 
tag: linux oracle
---

### 一.配置环境

宿主机：ubuntu 16.04

虚拟机：Windows 10家庭中文版64位

软件：PL/SQL Developer 12.0.6

PL/SQL Developer - 中文语言包

Windows32位的oracle instant client 






官方网址：

[plsql和汉化包下载](https://www.allroundautomations.com/bodyplsqldevreg.html)

[oracle instant client下载](http://www.oracle.com/technetwork/cn/topics/winsoft-085727.html)

备注：下载之前记得勾选许可协议并且登录oracle账号

### 二.配置流程

1.下载instantclient包，解压存放在本地，我的本地目录是D:\instantclient_11_2

2.在D:\instantclient_11_2目录下新建两级目录\network\admin，然后在admin文件夹下面新建文件tnsnames.ora，编辑此文件，向其中添加如下内容：


	数据库别名/数据库ip = 
      		(DESCRIPTION =
                   (ADDRESS_LIST =
           	     (ADDRESS = (PROTOCOL = TCP)(HOST = 数据库IP)(PORT = 端口号))
         	)
         	(CONNECT_DATA =
            	   (SERVICE_NAME = 要连接的数据库名称)
         	)	
       		)

3.设置操作系统环境变量（网上建议设置，但是我没有设置也能用），因为我没有设置也能用，所以就没有配置了。

4.安装PL/SQL Developer，安装中文汉化包，运行后出现的登录窗体不能进行登录，点击取消按钮，这时会在无登录状态下进入。点击工具栏配置-->首选项，配置连接的oracle主目录名（第一步的解压包目录）和OCI library（OCI库），我的是D:\instantclient_11_2，oci.dll就在主目录下面，所以在两个方框中输入D:\instantclient_11_2和D:\instantclient_11_2\oci.dll,配置输入完毕之后点击确定，然后重新打开plsql，输入用户名和口令，数据库一栏的格式为：数据库ip地址/数据库实例名，连接一般为normal。


参考文章：

1.[win7 64位不安装Oracle客户端配置PLSQL](http://www.jianshu.com/p/b4830bcc4555)

2.[不安装oracle客户端 PLSQL11 64位 连接 ORACLE11g](http://blog.csdn.net/jojoy_828/article/details/74330627)











	










