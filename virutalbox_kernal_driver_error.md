---
title: MacOS Big Sur 上 VirtualBox 运行虚拟机报 Kernel driver not installed (rc=1908)
date: 2021-09-21
tag: [VirutalBox， MacOS]
category: 技术笔记
---
## MacOS Big Sur 上 VirtualBox 运行虚拟机报 Kernel driver not installed (rc=1908)

MBP 上打开VirutalBox，运行以前的或者新安装vm，都会报错： Kernel driver not installed (rc=1908)

原因是MacOS更新后导致的。

解决方法： 卸载VirutalBox，重启电脑，安装最新版本的VirtualBox，隐私设置中允许VirtualBox运行，重启电脑。

参考： https://www.cxyzjd.com/article/u011700186/109741194 

