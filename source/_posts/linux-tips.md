---
title: linux 学习笔记
date: 2018-01-09 15:39:18
tags: linux 命令
---
iftop: display bandwidth usage on an interface
http://www.ex-parrot.com/pdw/iftop/

### sysctl  命令
   sysctl命令用于运行时配置内核参数，这些参数位于/proc/sys目录下。sysctl配置与显示在/proc/sys目录中的内核参数．可以用sysctl来设置或重新设置联网功能，如IP转发、IP碎片去除以及源路由检查等。用户只需要编辑/etc/sysctl.conf文件，即可手工或自动执行由sysctl控制的功能。

常用参数的意义:
```
    -w   临时改变某个指定参数的值，如

        sysctl -w net.ipv4.ip_forward=1

    -a   显示所有的系统参数

    -p   从指定的文件加载系统参数，如不指定即从/etc/sysctl.conf中加载
```
如果仅仅是想临时改变某个系统参数的值，可以用两种方法来实现,例如想启用IP路由转发功能
```
 1) #echo 1 > /proc/sys/net/ipv4/ip_forward

 2) #sysctl -w net.ipv4.ip_forward=1
```
以上两种方法都可能立即开启路由功能，但如果系统重启，或执行了 service network restart命令，所设置的值即会丢失，
如果想永久保留配置，可以修改/etc/sysctl.conf文件
```
将 net.ipv4.ip_forward=0改为net.ipv4.ip_forward=1 
 #重新加载系统参数
 sysctl -p  
```




