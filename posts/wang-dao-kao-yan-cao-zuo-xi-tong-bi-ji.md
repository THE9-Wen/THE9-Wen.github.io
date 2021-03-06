---
title: '王道考研操作系统笔记'
date: 2021-07-03 22:06:49
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
# 调度算法

适用于批处理系统：

先来先服务算法

短作业优先算法

适用于交互式系统：

时间片轮转调度算法

非抢占式优先级调度算法

抢占式优先级调度算法

多级反馈队列调度算法

补充：有的操作系统会根据优先级调整就绪队列



通常：

系统进程优先级高于用户进程

前台进程优先级高于用户进程

操作系统会优先处理I/O繁忙型的进程（可以尽早地让I/O设备与CPU并行）



进程同步与进程互斥

异步：各并发执行的进程以格子独立的、不可预知的速度向前推进

同步：必须按照一定次序执行 进程之间的之间制约关系

进程同步的机制

进程互斥：互斥共享的资源 当一个进程需要访问临界资源时，其他的资源必须等待 进程之间的间接互斥关系

进入区 // 上锁

临界区 // 访问临界资源

退出区 // 解锁

剩余区 // 做其他事情



进程互斥的四个原则

1. 空闲让进
2. 忙则等待
3. 有限等待
4. 让权等待



进程互斥的软件实现方法

- 单标志法
  - 每个进程进入临界区的权限只能被另一个进程赋予
- 双标志先检查
- 双标志后检查
- Peterson算法

通过两个进程并发进行分析



进程互斥的硬件实现方法

- 中断屏蔽方法
  - 关中断
  - 临界区
  - 开中断
- TestAndSet指令 TSL指令
- Swap指令



信号量机制





吸烟者问题





读者写者问题

共享文件

读者可以同时读文件

写进程和其他所有的进程同时访问文件时可能发生数据不一致的问题

P是枷锁 V是解锁

使用count计数

第一个读进程负责加锁，否则直接进入

最后一个读完的进程负责解锁 注意加锁解锁的过程需要一个互斥信号量保证原子性

问题：如果有源源不断的读进程，可能写进程会出现饥饿

多层的互斥变量 P V 操作 

读写公平法



哲学家进餐问题

哲学家： 思考 进餐

避免死锁

定义互斥信号量数组

需要添加条件：

- 方案一：至少需要有一个筷子留在桌子上
- 方案二：哲学家需要在两边都有筷子的时候才能拿起筷子



管程

1. 为什么引入管程

   管程出现之前使用信号量机制实现同步和互斥

   管程是一种高级的同步与互斥机制

2. 管程的定义和基本特征

   定义：一种同属的软件模块，组成：

   - 共享数据结构（类似于属性）
   - 一组过程（类似于方法）
   - 初始化的语句（类似于构造器）
   - 一个名字（类似于类名）

   类似于一个类

   基本特征：

   1. 局部管程的数据只能由局部管程的过程访问
   2. 调用管程内部的过程访问管程共享数据
   3. 每次仅允许一个进程在管程内执行某一个内部过程（同一时刻只有一个进程在使用管程中的某一个函数）

   程序员不需要关心是如何实现同步的，只需要调用管程的过程就可以实现同步和互斥

   java里面的管程：`synchronized`关键字

   练习：在java里面实现生产者消费者问题



死锁的概念

例子：哲学家问题（每一个哲学家都持有左手的筷子并且在等待右手的筷子）

每个人都持有一个资源并且在等待其他的资源

区别：死锁、饥饿、死循环 都是程序发生了一些同属状况无法继续推进

死锁：至少要有两个进程，进程一定处于阻塞态

饥饿：可能只有一个进程发生饥饿，可能处于就绪（长期等待CPU）或阻塞（长期等待I/O）态

死循环：可能只有一个进程，可能处于运行态

必要条件：

1. 互斥条件
2. 不可剥夺条件（得到之前除非自己放弃不可被别人抢夺）
3. 请求和保持条件（进程持有至少一个资源，又提出了新的请求，但又对自己的资源不放）
4. 循环等待条件（发生了循环等待不一定发生了死锁，但是发生了死锁一定会出现循环等待）

合适会发生死锁：对不可剥夺的资源分配不合理时

1. 对系统资源的竞争
2. 不合理的进程推进顺序
3. P V操作设计不合理



预防死锁

避免死锁

死锁的检测和解除



预防死锁

1. 破坏互斥条件

   SPOOLing技术

   举例：打印机是独占设备，添加一个输出进程托管打印机

2. 破坏不剥夺条件

   方案一：当某个进程请求的资源的不到满足时，释放所有的资源

   方案二：由操作系统协助，根据优先级剥夺资源

3. 破坏请求和保持条件

   方案一：采用静态分配方法，即一次申请全部的资源，在没有满足之前不投入运行

4. 破坏循环等待条件

   顺序资源分配法：给系统的资源编号，规定必须按照编号递增的顺序请求资源

   必定有一个进程获得资源的编号是最大的，他申请资源的过程一定是畅通无阻的

这些策略或多或少都有一些缺陷

前后联系



避免死锁（非常重要）

安全序列：

如果系统按照安全序列分配资源，则每一个进程都可以顺利完成

系统中找不出一个安全序列，那么系统处于不安全状态

系统处于安全状态，那么一定不会发生死锁；如果处于不安全状态，那么可能对发生死锁

银行家算法：

核心思想：如果一次资源分配会使得系统处于不安全状态，那么可以进行这一次的资源分配



算法：

系统有n个进程 m种资源

最大需求矩阵Max

分配矩阵Allocation

Need矩阵



死锁的检测与解除

1. 利用某一种数据结构来保存资源的请求和分配信息
2. 提供一种算法，利用上述信息来检测系统是否进入死锁状态

数据结构：图

算法：如果可以消除所有的边那么没有发生死锁，如果不能消除所有的边，那么已经发生了死锁

​	依次消除与不阻塞（系统可以提供进程前申请的资源）进程相连的边，直到无边可消

在一个系统中不一定所有的进程都发生了死锁

解除死锁的方法：

1. 资源剥夺法
2. 撤销进程法：代价比较大
3. 进程回退法：操作系统需要记录进程的历史信息，不太容易实现

如何决定对谁动手？

- 进程的优先级
- 已执行的时间
- 还需要多久完成
- 进程已经使用了多少资源
- 优先牺牲批处理的





# 内存管理

## 什么是内存

程序在执行之前需要先放到内存中才能被CPU处理

如何区分各个程序的信息储存在了内存的哪个地方？

内存地址

每一个内存地址对应一个存储单元 各个计算机可能不同 按字节编址 每一个储存单元1B 按字编址



进程的运行原理：指令

寄存器



物理地址：起始地址+相对地址

逻辑地址（相对地址）：



写程序到程序运行

编译：把高级语言翻译为机器语言

链接：产生完整的逻辑地址

装入：



绝对装入：在编译的时候就知道程序会存放在内存的哪一个位置

静态重定位：装入的时候由装入程序计算地址，装入之后就不可以移动了

动态重定位：逻辑地址到物理地址的转换在程序运行时才会进行



### 内存管理的概念

内存管理要做些什么？

1. 内存空间的分配与回收
2. 虚拟技术（操作系统的虚拟性）
3. 地址转换（地址重定位由操作系统进行）
4. 内存保护（各个进程在各自的内存空间内运行）
   - 两种方式：1. 设置上下限寄存器 2. 利用重定位寄存器、界地址寄存器进行判断



覆盖与交换

内存空间的扩充：

**覆盖技术**

解决程序大小超过物理内存总和的问题

常用的段放在固定区

不常用的段放在覆盖区

让不可能被同时访问的程序段共享一个覆盖区



**交换技术**

中级调度

低级调度

高级调度

挂起

进程的七状态模型



PCB会常驻内存



应该在外存的什么位置保存被换出的进程？

对换区 连续分配的方式 I/O速度比文件区更快



什么时候进行交换？

内存紧张的时候



应该换出什么进程？

阻塞态的进程

优先级比较低的进程

考虑进程在内存中驻留的时间



覆盖与交换的区别：

覆盖：在同一个进程中

交换：不同的进程



内存空间的分配与回收：

1. 连续分配管理方式
2. 离散分配方式

单一连续分配：

内存被分为系统区和用户区



固定分区分配方式

用户分区被分为若干个固定大小的分区，每个分区只装入一个作业

- 分区大小相等
- 分区大小不相等



### 连续分配管理方式

单一连续分配

固定分区分配

动态分区分配



1. 单一连续分配（有内部碎片）

   系统区

   用户区

   某一个作业独占用户区

2. 固定分区分配（有内部碎片）

   用户区被划分为若干个固定大小的分区

   每一个分区可以装入一道作业

   分区大小相等

   分区大小不相等

3. 动态分区分配（有外部碎片）

   在进程装入内存的时候才会动态地建立分区

   - 系统应该用什么样的数据结构记录内存的使用情况呢？

     - 空闲分区表	分区号 分区大小 起始地址
     - 空闲分区链

   - 当内存中有很多个空闲分区可以满足需求的时候应该选择那个分区进行分配呢？

     - 动态分区分配算法（对性能的影响很大）

   - 如何进行分区的分配与回收操作

     - 相邻的空闲分区合并
     - 如果前后都没有空闲分区，则在空闲分区表添加一个分区

   - 动态分区分配

     - 内部碎片：分配给某进程的内存区域中，没有被用上的部分
     - 外部碎片：内存中的部分空闲分区太小以至于难以利用

     紧凑技术



### 动态分区分配算法

**首次适应算法**

每次从低地址开始查找，找到第一个能满足大小的空闲分区

**最佳适应算法**

空闲分区按照容量递增的次序链接

每一次都选择最小的分区进行适配，会留下来越来越多的小碎片

**最坏适应算法**

空闲分区按照递减的顺序排列

可能缺少大的连续的内存

**临近适应算法**

从上一次查找结束的位置开始查找空闲分区链

高地址地区更有可能会被划分成更小的分区



### 基本分页存储

让一个进程分散地存到内存的不同地方

### 非连续分配的方式

基本分页存储





