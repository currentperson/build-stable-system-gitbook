## 前言

系统的稳定性还在于线上问题的快速排查, 比如线上 oom, 线程死锁, 连接池耗尽等等都可能会造成严重的生产事故.为此我打算系统学习一下 Arthas 和相关的知识

## 安装

```bash
curl -O https://arthas.aliyun.com/arthas-boot.jar
java -jar arthas-boot.jar
```

## Thread 命令学习:

### 选择好 attach 的进程, 进入 arthas 命令界面, 执行 help thread 展示了详细的命令.![](/assets/2021032000.png)thread --all \(展示全部线程\) \|\| thread \(展示第一页\)![](/assets/2021032001.png)

字段解释:

Internal threads : 代表 JVM 内部线程

ID: 线程id, 可以使用 thread ID 查看线程执行栈      

NAME: 线程名称                                          

GROUP: 线程组                  

PRIORITY: 优先级       

STATE: 线程状态,http://mcace.me/java%E5%B9%B6%E5%8F%91/2018/08/24/java-thread-states.html          

%CPU: cpu 使用率   

DELTA\_TIME:  为采样间隔时间内线程的增量 CPU 时间，小于 1ms 时被取整显示为 0ms。

TIME: 线程运行总 CPU 时间。   

INTERRUPTED: 是否被中断\(https://blog.csdn.net/CringKong/article/details/80526996\)    

DAEMON: 是否是守护线程\(User threads是高优先级的thread，JVM将会等待所有的User Threads运行完毕之后才会结束运行。daemon threads是低优先级的thread，它的作用是为User Thread提供服务。 因为daemon threads的低优先级，并且仅为user thread提供服务，所以当所有的user thread都结束之后，JVM会自动退出，不管是否还有daemon threads在运行中。https://zhuanlan.zhihu.com/p/130289531\) ![](/assets/2021032002.png)



### thread PID![](/assets/2021032004.png)

可以查看指定线程\(比如 CPU 利用率极高\)的线程堆栈, 从而定位到代码的问题.

其他的命令感觉可能不那么常用, 感兴趣的可以自己试用下

## Thread 使用例子:

比如一个线程 hang 住了, 这个时候就能用上面的两个命令排查 hang 住的代码, 还是挺实用的, 线程 hang 住的情况比如翻页接口忘记请求下一页造成死循环, 再比如下面这段代码:

```java
public static boolean isEmail(String string) {
    String regEx1 = "^([a-z0-9A-Z]+[-|\\.]?)+[a-z0-9A-Z]@([a-z0-9A-Z]+(-[a-z0-9A-Z]+)?\\.)+[a-zA-Z]{2,}$";
    Pattern p;
    Matcher m;
    p = Pattern.compile(regEx1);
    m = p.matcher(string);
    if (m.matches())
        return true;
    else
        return false;
}
```

你能看出为什么线程会 hang 住么

