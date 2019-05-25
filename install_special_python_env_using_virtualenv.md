---
title: windows下使用virtualenv如何安装指定版本python
date: 2018-12-25
tag: [2018年, python]
category: 技术笔记
---

如何使用virtualenv创建两个环境，一个环境是python3，一个环境是python2，需要在windows上同时安装python3和python2

## 步骤1： windows系统安装python2和python3

最新版本的python2是python 2.7.15


## 步骤2：安装virtualenv

pip list 查看是否安装virtualenv，如果没有安装执行pip install virtualenv安装

## 步骤3：新建目录创建指定版本python的环境

这里创建的测试目录是E:\python_projects\pachong_test

创建环境的命令,参数是两个横杠开头：

<pre>
cd E:\python_projects\pachong_test;virtualenv --python=C:\Python27\python.exe --no-site-packages  venv
</pre>

命令解释： 

<pre>
--no-site-packages
</pre>

添加该参数将不会安装全局的包到virtual环境中；

<pre>
--python=C:\Python27\python.exe
</pre>

指定python版本的绝对路径

安装截图如下：

![](https://thinknotes-1256255945.cos.ap-chengdu.myqcloud.com/thinknotes/20181225-01.png)

## 激活venv环境

在测试目录E:\python_projects\pachong_test下执行venv\Scripts\activate激活venv环境，使用pip list查看当前安装的包都有哪些，
可以看到没有系统环境的包，非常干净，python版本也是2.7.15,使用deactive命令退出venv环境

具体过程如下图：

![](https://thinknotes-1256255945.cos.ap-chengdu.myqcloud.com/thinknotes/20181225-02.png)

