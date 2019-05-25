---
title: python3 urllib 获取服务器http 5xx状态码
date: 2019-01-05
tag: [2019年, python3网络爬虫]
category: 技术笔记
---

当服务器状态码为5xx时，设置重试机制


urllib.error可以接收有urllib.request产生的异常。urllib.error有两个方法，URLError和HTTPError

![](https://thinknotes-1256255945.cos.ap-chengdu.myqcloud.com/thinknotes/2019-01-05_0.jpg)

使用urllib.error.HTTPError捕获异常获取http status:

![](https://thinknotes-1256255945.cos.ap-chengdu.myqcloud.com/thinknotes/2019-01-05_01.jpg)

使用urllib.error.URLError捕获异常,通过hasattr判断是否有code属性，如果有说明可以获取http status:

![](https://thinknotes-1256255945.cos.ap-chengdu.myqcloud.com/thinknotes/2019-01-05_02.jpg)

和上图相同，vscode插件pylint提示有问题，但结果显示不影响执行：

![](https://thinknotes-1256255945.cos.ap-chengdu.myqcloud.com/thinknotes/2019-01-05_3.jpg)

脚本运行结果，可以获取到500状态码：

![](https://thinknotes-1256255945.cos.ap-chengdu.myqcloud.com/thinknotes/2019-01-05_4.jpg)