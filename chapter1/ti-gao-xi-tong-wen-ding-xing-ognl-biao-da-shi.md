

## 前言

妹妹: 姐姐平时都用这么多了命令么, 妹妹我平时啥也不会用

姐姐: 妹妹自称平时啥也不会用, OGNL 用的这么熟, 增删改查样样都不落



## OGNL 命令学习



![](/assets/2021061404.png)

OGNL 这个表达式可以获取相关的值, 方便排查问题

OGNL 一般需要指定类加载器, 可以先使用 SC 命令查询相关类加载器的 hash 如下:

![](/assets/2021061405.png)

1. 比如常见的使用 OGNL 通过调用静态方法获取 Spring 所加载的 bean 类:

ognl -c classLoaderHash -x  返回值的遍历的属性的层次  'OGNL表达式'

![](/assets/2021061406.png)

getBean 的代码实现:

![](/assets/2021061407.png)

2. 获取 bean 之后就可以调用非静态的方法

![](/assets/2021061408.png)

3. 调用构造函数

![](/assets/2021061409.png)

4. 还可以表达式先后赋值

![](/assets/2021061411.png)

5. 可以使用 this 代表当前对象

![](/assets/2012061413.png)



## 参考

https://commons.apache.org/proper/commons-ognl/language-guide.html

![](/assets/2021061401.png)

https://jueee.github.io/2020/08/2020-08-15-Ognl%E8%A1%A8%E8%BE%BE%E5%BC%8F%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95/

![](/assets/2021061402.png)

https://github.com/alibaba/arthas/issues/71

![](/assets/2021061403.png)

https://blog.csdn.net/u010634066/article/details/101013479

![](/assets/2021061415.png)

