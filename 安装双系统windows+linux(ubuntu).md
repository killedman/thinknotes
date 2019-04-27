---
title: 安装双系统windows+linux(ubuntu)
date: 2019-04-27
tag: [2019年, Ubuntu, win10]
category: 技术笔记
---




## 安装次序很重要

先安装windows再安装linux

## 选择操作系统

Windows 10 + Ubuntu LTS 16.04.6

## 准备工作

1. 规划两个系统磁盘分区，给ubuntu系统腾出几百G的空间

2. 下载EasyBCD，在windows 10上安装修改启动项信息，使选择操作系统时更好识别 参考：[www.cnblogs.com/Duane/p/6776302.html](http://www.cnblogs.com/Duane/p/6776302.html)

## 安装windows 10

有系统情况下建议使用ghost安装器安装windows系统，特别方便，不需要制作u盘启动盘

参考： <https://www.jb51.net/os/580468.html>

##  安装Ubuntu

1. 网易镜像下载Ubuntu16.04 64位镜像

2. 下载制作启动盘软件，制作启动u盘，参考：<http://www.ubuntu.com.cn/download/desktop/create-a-usb-stick-on-windows>

## 解决安装完ubuntu后无windows启动菜单的问题

打开终端中输入：sudo gedit /etc/default/grub

其中gedit是文本编辑器，然后输入密码，密码就是开机密码，注意这里输入密码后不会显示出来，直接输完密码回车就行。

输入完回车后会弹出一个grub文件，将文本”GRUB_DEFAULT=**0**“中的0改成Windows 10系统的序号**4**，还可以修改”GRUB_TIMEOUT=10“中的10，修改默认的等待时间。改完后点击”保存“然后关闭。

修改完成以后，还有最重要的一步，上面文本编辑器的保存只是将内容修改了，但并没有载入更新配置，所以你还需要在终端输入：sudo update-grub

这步完成以后默认的Ubuntu+Windows双系统启动项就修改完了，现在可以重启电脑再看看效果。

##  参考链接

<https://www.libinx.com/2017/five-steps-win10-ubuntu-dual-boot/>

<http://www.ubuntu.com.cn/download/desktop/create-a-usb-stick-on-windows>

[www.cnblogs.com/Duane/p/6776302.html](http://www.cnblogs.com/Duane/p/6776302.html)



