---
title: 为什么说python的多线程是鸡肋
date: 2019-04-15
tag: [python, 多线程]
category: 技术笔记
---

# 为什么说python的多线程是鸡肋

1. 因为GIL也就是全局解释器锁的存在，限制线程对共享内存资源的访问，直到python解释器遇到I/O操作或者操作次数达到一定数目时才会释放GIL
2. python多线程不适合用在计算密集型（CPU bound）代码中，适合用在IO密集型（IO bound）代码中，python3引入的协程asyncio，能更好的处理IO bound问题，python2中可以使用python多线程处理IO bound问题；计算密集型（CPU bound）可以试试python的多进程