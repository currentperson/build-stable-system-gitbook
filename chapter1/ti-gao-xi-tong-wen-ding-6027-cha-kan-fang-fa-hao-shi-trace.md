## 前言

```java
public static int wuyifantime() {
    final int sleepTime = 1000 * 60 * new Random().nextInt(3);
    try {
        // 0 分钟到 3 分钟
        Thread.sleep(sleepTime);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    changeobject();
    return sleepTime;
}

public static void changeobject() {
    try {
        Thread.sleep(200);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    testgt18();
}

public static void testgt18() {
    try {
        Thread.sleep(200);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

## TRACE 命令学习

```bash
 USAGE:                                                                                                                                                                                                   
   trace [--exclude-class-pattern <value>] [-h] [-n <value>] [--listenerId <value>] [-p <value>] [-E] [--skipJDKMethod <value>] [-v] class-pattern method-pattern [condition-express]                     

 SUMMARY:                                                                                                                                                                                                 
   Trace the execution time of specified method invocation.                                                                                                                                               
   The express may be one of the following expression (evaluated dynamically):                                                                                                                            
           target : the object                                                                                                                                                                            
            clazz : the object's class                                                                                                                                                                    
           method : the constructor or method                                                                                                                                                             
           params : the parameters array of method                                                                                                                                                        
     params[0..n] : the element of parameters array                                                                                                                                                       
        returnObj : the returned object of method                                                                                                                                                         
         throwExp : the throw exception of method                                                                                                                                                         
         isReturn : the method ended by return                                                                                                                                                            
          isThrow : the method ended by throwing exception                                                                                                                                                
            #cost : the execution time in ms of method invocation                                                                                                                                         
 EXAMPLES:                                                                                                                                                                                                
   trace org.apache.commons.lang.StringUtils isBlank                                                                                                                                                      
   trace *StringUtils isBlank                                                                                                                                                                             
   trace *StringUtils isBlank params[0].length==1                                                                                                                                                         
   trace *StringUtils isBlank '#cost>100'                                                                                                                                                                 
   trace -E org\\.apache\\.commons\\.lang\\.StringUtils isBlank                                                                                                                                           
   trace -E com.test.ClassA|org.test.ClassB method1|method2|method3                                                                                                                                       
   trace demo.MathGame run -n 5                                                                                                                                                                           
   trace demo.MathGame run --skipJDKMethod false                                                                                                                                                          
   trace javax.servlet.Filter * --exclude-class-pattern com.demo.TestFilter                                                                                                                               

 WIKI:                                                                                                                                                                                                    
   https://arthas.aliyun.com/doc/trace                                                                                                                                                                    

 OPTIONS:                                                                                                                                                                                                 
     --exclude-class-pattern <value>                                exclude class name pattern, use either '.' or '/' as separator                                                                        
 -h, --help                                                         this help                                                                                                                             
 -n, --limits <value>                                               Threshold of execution times                                                                                                          
     --listenerId <value>                                           The special listenerId                                                                                                                
 -p, --path <value>                                                 path tracing pattern                                                                                                                  
 -E, --regex                                                        Enable regular expression to match (wildcard matching by default)                                                                     
     --skipJDKMethod <value>                                        skip jdk method trace, default value true.                                                                                            
 -v, --verbose                                                      Enables print verbose information, default value false.                                                                               
 <class-pattern>                                                    Class name pattern, use either '.' or '/' as separator                                                                                
 <method-pattern>                                                   Method name pattern                                                                                                                   
 <condition-express>                                                Conditional expression in ognl style, for example:                                                                                    
                                                                      TRUE  : 1==1                                                                                                                        
                                                                      TRUE  : true                                                                                                                        
                                                                      FALSE : false                                                                                                                       
                                                                      TRUE  : 'params.length>=0'                                                                                                          
                                                                      FALSE : 1==2                                                                                                                        
                                                                      '#cost>100'
```

![](/assets/2021072401.png)

使用:

```bash
--exclude-class-pattern <value> 想要排除的类                                                
-n, --limits <value> 观察几次
--listenerId <value> 指定的 listenerId
-p, --path <value> 这个没看懂啥作用
-E, --regex 正则表达式
--skipJDKMethod <value> 是否跳过 jdk 的方法, 默认是 true
-v, --verbose 是否多输出一些信息, 默认是 false
<class-pattern> 类名
<method-pattern> 方法名
<condition-express> ognl 表达式
```

## **这个方法主要用于查看方法的耗时, 主要有两点常见的使用办法: **

#### **比如查看耗时在 1 s 以上的方法:**

```bash
[arthas@63687]$ trace com.codog.demo.Main wuyifantime '#cost > 1000'
Press Q or Ctrl+C to abort.
Affect(class count: 1 , method count: 1) cost in 26 ms, listenerId: 2
`---ts=2021-07-24 22:37:54;thread_name=main;id=1;is_daemon=false;priority=5;TCCL=jdk.internal.loader.ClassLoaders$AppClassLoader@3d4eac69
    `---[406.151996ms] com.codog.demo.Main:changeobject() #29

```

![](/assets/2021072405.png)

#### 这个方式只能计算一层的耗时, 想看到下面几层的耗时:

#### 

```bash
➜  demotemp telnet localhost 3658
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
  ,---.  ,------. ,--------.,--.  ,--.  ,---.   ,---.                           
 /  O  \ |  .--. ''--.  .--'|  '--'  | /  O  \ '   .-'                          
|  .-.  ||  '--'.'   |  |   |  .--.  ||  .-.  |`.  `-.                          
|  | |  ||  |\  \    |  |   |  |  |  ||  | |  |.-'    |                         
`--' `--'`--' '--'   `--'   `--'  `--'`--' `--'`-----'                          
                                                                                

wiki       https://arthas.aliyun.com/doc                                        
tutorials  https://arthas.aliyun.com/doc/arthas-tutorials.html                  
version    3.5.2                                                                
main_class com.codog.demo.Main                                                  
pid        63687                                                                
time       2021-07-24 22:11:33                                                  

[arthas@63687]$ 
[arthas@63687]$ 
[arthas@63687]$ 
[arthas@63687]$ trace com.codog.demo.Main changeobject --listenerId 1
Press Q or Ctrl+C to abort.
Affect(class count: 1 , method count: 1) cost in 21 ms, listenerId: 1
```

![](/assets/2021072404.png)

```bash
[arthas@63687]$ trace com.codog.demo.Main wuyifantime
Press Q or Ctrl+C to abort.
Affect(class count: 1 , method count: 1) cost in 113 ms, listenerId: 1
`---ts=2021-07-24 22:12:14;thread_name=main;id=1;is_daemon=false;priority=5;TCCL=jdk.internal.loader.ClassLoaders$AppClassLoader@3d4eac69
    `---[405.964495ms] com.codog.demo.Main:wuyifantime()
        `---[405.313685ms] com.codog.demo.Main:changeobject() #29

`---ts=2021-07-24 22:12:15;thread_name=main;id=1;is_daemon=false;priority=5;TCCL=jdk.internal.loader.ClassLoaders$AppClassLoader@3d4eac69
    `---[120406.120851ms] com.codog.demo.Main:wuyifantime()
        `---[405.479587ms] com.codog.demo.Main:changeobject() #29

`---ts=2021-07-24 22:14:15;thread_name=main;id=1;is_daemon=false;priority=5;TCCL=jdk.internal.loader.ClassLoaders$AppClassLoader@3d4eac69
    `---[120407.586592ms] com.codog.demo.Main:wuyifantime()
        `---[405.349313ms] com.codog.demo.Main:changeobject() #29

`---ts=2021-07-24 22:23:50;thread_name=main;id=1;is_daemon=false;priority=5;TCCL=jdk.internal.loader.ClassLoaders$AppClassLoader@3d4eac69
    `---[60407.737325ms] com.codog.demo.Main:wuyifantime()
        `---[402.376084ms] com.codog.demo.Main:changeobject() #29
            `---[402.127891ms] com.codog.demo.Main:changeobject()
                `---[200.970532ms] com.codog.demo.Main:testgt18() #39

`---ts=2021-07-24 22:24:50;thread_name=main;id=1;is_daemon=false;priority=5;TCCL=jdk.internal.loader.ClassLoaders$AppClassLoader@3d4eac69
    `---[60406.014846ms] com.codog.demo.Main:wuyifantime()
        `---[401.328146ms] com.codog.demo.Main:changeobject() #29
            `---[401.128872ms] com.codog.demo.Main:changeobject()
                `---[200.614497ms] com.codog.demo.Main:testgt18() #39
```

![](/assets/2021072402.png)





