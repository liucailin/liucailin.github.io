---
layout: post
title: Linux Note
---

## 网络设置
- ip addr 显示IP地址
- ifup xxx ifdown xxx 激活
- vi /etc/sysconfig/network-scripts/ifcfg-xxx ONBOOT=yes 开机启动
- systemctl restart network 重启网络服务

## ssh连接
- vi /etc/ssh/sshd_config PermitRootLogin yes 允许root登陆
- systemctl restart sshd
- ~/.ssh/authorized_keys 密钥登陆

## yum
- yum list xxx 查找是否可以安装xxx
- yum search xxx 搜索xxx安装包
- yum list installed 列出已安装的

## 文本内容查阅
- cat - View the whole file
- nl - 和cat类型，带行数输出
- head - View the top few lines of a file
- tail - View the bottom few lines of a file
- more - View the whole file, one screenful at a time
- less - 和more类似，功能更强大

## VirtualBox CentOS7
- 使用默认的网络连接，网络桥接需要配置，否则上不了网
- ifup激活网卡
- https://www.codetd.com/article/8792817
- shutdown -h now 马上关机