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