---
title: 进程和线程
date: 2019-04-18
tag: [进程, 线程]
category: 技术笔记
---

# 进程、线程

让我们使用常规的日常对象 - 房子来对流程和线程进行类比。

房子实际上是一个容器，具有某些属性。如果以这种方式看待它，房子本身并没有主动做任何事情 - 这是一个被动的对象。进程也是这样。

线程就像住在房子里的人。

### 单个线程

如果你曾经独自生活，那么你就知道这是什么 - 你知道你可以随时在家里做任何你想做的事，因为房子里没有其他人。

### 多个线程

当你把另一个人加入房子时，事情发生了巨大的变化。假设你结婚了，所以现在你有一个配偶住在那里。你不能在任何一个点上进入洗手间;你需要先检查一下，确保你的配偶不在那里！

如果你有两个负责任的成年人住在一个房子里，一般来说你可以合理地松懈“安全” - 你知道另一个成年人会尊重你的空间，不会试图让厨房着火（故意！），等等。

现在，把几个孩子扔进去，突然间事情变得更加有趣。

### 回到进程和线程

就像房子占据房地产区域一样，进程占用内存。就像一个房子的居住者可以自由进入他们想要的任何房间一样，进程的线程都可以访问该内存。如果一个线程分配一些东西（比如：妈妈出去买一个游戏），所有其他线程立即可以访问它（因为它存在于公共地址空间 - 它在房子里）。同样，如果进程分配内存，那么这个新内存也可用于所有线程。这里的技巧是识别内存是否应该对进程中的所有线程可用。如果是，则需要让所有线程同步对它的访问权限。如果不是，那么我们将假设它特定于特定线程。因为只有那个线程可以访问它，我们可以假设不需要同步 - 线程不会自行启动！

进程是线程的容器，一个进程包含多个线程，至少有一个线程。

## 互斥锁、信号灯

### 互斥锁

为了理解互斥锁，使用淋浴间进行类比，一个人进去后从里面把门锁上，外面的人就进不去了，需要等待里面的人使用完后释放锁，购买的人才能继续使用，每次只能有一个线程使用。

优先级、等待时长，两个因素决定了互斥锁解除后等待的线程，哪一个接着使用资源。

### 信号灯

理解信号灯，拿厨房的使用作为类比，同一时间可以有多个人使用，但同一时间不能让太多人使用，需要限制同一时间使用的人数。

#### 数量为1时的信号灯

等同于互斥锁。淋浴间的状态仅且只有两种，没有其他可能性：

1. the door is unlocked and nobody is in the room
2. the door is locked and one person is in the room

#### 数量大于1时的信号灯

假设我们在厨房安装了传统的钥匙锁。这种锁的工作方式是你如果有一把钥匙，就可以打开锁进来。有钥匙的人都遵守打开锁之后立刻从里面锁上门，没有钥匙的人从外面进不来。

现在的问题变成同一时间控制多少人在厨房。挂两把钥匙在厨房门外。厨房总是锁着，有人想进入厨房，看见门上挂着钥匙，于是取下钥匙开开厨房的门，进门后立刻用钥匙从里面锁上门。

因为进入厨房必须有钥匙，可以通过控制钥匙的数目来控制进入厨房的人。

对于线程，这是通过信号灯来完成的。一个信号灯与互斥锁一样，你拥有互斥锁时就有权利访问资源，否则无法访问资源。多个信号灯，根据线程数目来确定信号灯的数目。（With threads, this is accomplished via a *semaphore*. A “plain” semaphore works just like a mutex — you either own the mutex, in which case you have access to the resource, or you don't, in which case you don't have access. The semaphore we just described with the kitchen is a *counting semaphore* — it keeps track of the count (by the number of keys available to the threads).）

能将互斥锁当信号灯使用吗？答案是不能。

能将信号灯当互斥锁使用吗？答案是可以。

实际上，在一些操作系统，没有互斥锁，只有信号灯。那为什么还需要互斥锁呢？

就像家里的淋浴室，互斥锁是特殊的信号灯，如果您希望在特定代码段中运行一个线程，则互斥锁是迄今为止最有效的实现。（If you want one thread running in a particular section of code, a mutex is by far the most efficient implementation.）



### 参考：

[Processes and Threads](<http://www.qnx.com/developers/docs/6.4.1/neutrino/getting_started/s1_procs.html>)

[阮一峰的《进程与线程的一个简单解释》](<http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html>)

