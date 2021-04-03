## 前言

线上 jvm 时不时也会遇到一些问题, 或者怀疑是问题的问题, 这个怎么排查下找下原因呢

## JVM 命令学习

![](/assets/2021040300.png)

执行 help jvm 先看下 jvm 命令的说明, 很好, 啥参数也没有, 直接执行一下\(以下说明是我找了一些资料整理的, 有不对的地方欢迎指正\)

**RUNTIME 相关:**

INPUT-ARGUMENTS: JVM 启动参数

CLASS-PATH: 类加载路径

BOOT-CLASS-PATH: 扩展类加载路径

LIBRARY-PATH: 第三方类库地址

**CLASS-LOADING 相关:**

![](/assets/2021040311.png)

LOADED-CLASS-COUNT: 当前加载的类的数量

TOTAL-LOADED-CLASS-COUNT: 总共加载的类的数量

UNLOADED-CLASS-COUNT: 卸载的类的数量

IS-VERBOSE: 是否打开类加载的 trace 功能

**COMPILATION 相关:**

![](/assets/2021040310.png)

编译器名称和编译所花的时间

**GARBAGE-COLLECTORS 相关:**

![](/assets/2021040309.png)

各个垃圾回收器的回收次数和时间

**MEMORY-MANAGERS 相关:**

![](/assets/2021040307.png)

CodeCacheManager: 不知道干啥的

Metaspace Manager: 元空间管理器

PS Scavenge: 这种垃圾回收机制应用的堆空间

PS MarkSweep: 这种垃圾回收机制应用的堆空间

其他的垃圾回收组合可以再看下 [https://www.jdon.com/idea/jvm-gc.html](https://www.jdon.com/idea/jvm-gc.html)

**MEMORY 相关:**

![](/assets/2021040306.png)

HEAP-MEMORY-USAGE: 堆内存使用情况

NO-HEAP-MEMORY-USAGE:  堆外内存使用情况

PENDING-FINALIZE-COUNT: 等待被回收内存数量

关于 used 和 committed 的区别是 used 是实际使用的内存, committed 是系统许诺 jvm 能使用的内存\([https://docs.oracle.com/javase/8/docs/api/java/lang/management/MemoryUsage.html\](https://docs.oracle.com/javase/8/docs/api/java/lang/management/MemoryUsage.html\)\)

**OPERATING-SYSTEM 相关:**

![](/assets/2021040305.png)

OS: 操作系统

ARCH: CPU 架构指令集

PROCESSORS-COUNT: CPU 核数

LOAD-AVERAGE:  CPU 负载

VERSION: 略

**THREAD 相关:**

![](/assets/2021040304.png)

COUNT: JVM当前活跃的线程数

DAEMON-COUNT: JVM当前活跃的守护线程数

PEAK-COUNT: 从JVM启动开始曾经活着的最大线程数

STARTED-COUNT: 从JVM启动开始总共启动过的线程次数

DEADLOCK-COUNT: JVM当前死锁的线程数

**FILE-DESCRIPTOR 相关:**

![](/assets/2021040301.png)

MAX-FILE-DESCRIPTOR-COUNT：JVM进程最大可以打开的文件描述符数

OPEN-FILE-DESCRIPTOR-COUNT：JVM当前打开的文件描述符数

## Thread 使用例子:

当线上内存报警, 可以查看内存的整体使用情况, 可以快速定位fullgc的次数是不是很多很频繁, 或者接口偶发耗时长是不是垃圾回收器的耗时太长等可以进行参数优化和回收器的替换

