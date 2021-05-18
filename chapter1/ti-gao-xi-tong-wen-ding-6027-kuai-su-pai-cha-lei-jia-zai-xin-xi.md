## 前言

## SC 命令学习

![](/assets/2021051800.png)

> **sc -d className**

![](/assets/2021051801.png)class-info：也像个类全路径名

code-source：类代码来源

name：类全路径

iisInterface：是否是接口                                                                                                                                                

 isAnnotation：是否是注解                                                                                                                                         

 isEnum：是否是枚举                                                                                                                                                

 isAnonymousClass：是否是匿名类                                                                                                                                        

 isArray：是否是数组                                                                                                                                            

 isLocalClass：是否是局部类（https://www.geeksforgeeks.org/class-islocalclass-method-in-java-with-examples/）                                                                                                                                     

 isMemberClass：是否是成员类（https://www.logicbig.com/how-to/code-snippets/jcode-reflection-class-ismemberclass.html）                                                                                                                                              

 isPrimitive：是否为原始类型（boolean，int，long。。。）                                                                                                                                             

 isSynthetic：是否是合成类（https://www.geeksforgeeks.org/method-class-issynthetic-method-in-java/）                                                                                                                                               

 simple-name：类简称                                                                                                                                     

 modifier：类的修饰符，比如 abstract,interface,public                                                                                                                            

 annotation：类上的注解                                                                                                                                                             

 interfaces：类实现的接口                                                                                                                                                            

 super-class：类继承的父类                                                                                                                                                            

 class-loader：类加载器                                                                                                  

 classLoaderHash：类加载器 hashcode



> **sc -d -f className**

![](/assets/2021051802.png)查看类的属性的列表

> **sc -d -f -x 1 calssName**

![](/assets/2021051805.png)-x 决定静态变量的遍历深度，不填写默认是 -x0，对比上面两个图，-x1 会把静态属性的第一层属性也给打印出来。

> **sc -E regex**

![](/assets/2021051806.png)-E 可以搜索相关的符合正则表达式的类









