## 前言

```java
public class Main {

    public static String generateWord(String type) {
        switch (type) {
            case "宝,我今天输液了; 输的什么液?":return "想你的夜";
            case "宝，我今天喝酒了; 喝的什么酒":return "和你的天长地久";
            case "宝，我今天种地了; 种的什么地":return "对你的死心塌地";
            default:return "";
        }
    }
}
```

## SM 命令学习



```bash
[arthas@91969]$ help sm
 USAGE:                                                                                                                                                    
   sm [-c <value>] [--classLoaderClass <value>] [-d] [-h] [-n <value>] [-E] class-pattern [method-pattern]                                                 
                                                                                                                                                           
 SUMMARY:                                                                                                                                                  
   Search the method of classes loaded by JVM                                                                                                              
                                                                                                                                                           
 EXAMPLES:                                                                                                                                                 
   sm java.lang.String                                                                                                                                     
   sm -d org.apache.commons.lang.StringUtils                                                                                                               
   sm -d org/apache/commons/lang/StringUtils                                                                                                               
   sm *StringUtils *                                                                                                                                       
   sm -Ed org\\.apache\\.commons\\.lang\.StringUtils .*                                                                                                    
                                                                                                                                                           
 WIKI:                                                                                                                                                     
   https://arthas.aliyun.com/doc/sm                                                                                                                        
                                                                                                                                                           
 OPTIONS:                                                                                                                                                  
 -c, --classloader <value>                           The hash code of the special class's classLoader                                                      
     --classLoaderClass <value>                      The class name of the special class's classLoader.                                                    
 -d, --details                                       Display the details of method                                                                         
 -h, --help                                          this help                                                                                             
 -n, --limits <value>                                Maximum number of matching classes (100 by default)                                                   
 -E, --regex                                         Enable regular expression to match (wildcard matching by default)                                     
 <class-pattern>                                     Class name pattern, use either '.' or '/' as separator                                                
 <method-pattern>                                    Method name pattern   
```

![](/assets/2021061901.png)

```bash
sm [-c <value>] [--classLoaderClass <value>] [-d] [-h] [-n <value>] [-E] class-pattern [method-pattern]
```

使用:

```bash
sm -d com.codog.demo.Main generateWord

[arthas@91969]$ sm -d com.codog.demo.Main generateWord
 declaring-class  com.codog.demo.Main                                                                                                                      
 method-name      generateWord                                                                                                                             
 modifier         public,static                                                                                                                            
 annotation                                                                                                                                                
 parameters       java.lang.String                                                                                                                         
 return           java.lang.String                                                                                                                         
 exceptions                                                                                                                                                
 classLoaderHash  3d4eac69                                                                                                                                 

Affect(row-cnt:1) cost in 24 ms.
```

![](/assets/2021061902.png)

```
jad com.codog.demo.Main generateWord

[arthas@91969]$ jad com.codog.demo.Main generateWord

ClassLoader:                                                                                                                                               
+-jdk.internal.loader.ClassLoaders$AppClassLoader@3d4eac69                                                                                                 
  +-jdk.internal.loader.ClassLoaders$PlatformClassLoader@761bf405                                                                                          

Location:                                                                                                                                                  
/Users/bytedance/IdeaProjects/demotemp/target/classes/                                                                                                     

       public static String generateWord(String type) {
/*10*/     switch (type) {
               case "宝,我今天输液了; 输的什么液?": {
/*11*/             return "想你的夜";
               }
               case "宝，我今天喝酒了; 喝的什么酒": {
/*12*/             return "和你的天长地久";
               }
               case "宝，我今天种地了; 种的什么地": {
/*13*/             return "对你的死心塌地";
               }
           }
/*14*/     return "";
       }

Affect(row-cnt:1) cost in 393 ms.
```

![](/assets/2021061904.png)

