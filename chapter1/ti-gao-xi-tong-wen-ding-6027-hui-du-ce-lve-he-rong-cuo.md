## 前言

构建稳定的系统, 特别在流量大的系统中, 一定的灰度策略和容错性是很保证系统正常运行的必要条件

## 打印日志被开除的这几次

### 1. 影响监控指标和报警

之前别人已经打印了一行日志:

```java
log.info("info1 - a:{},b:{}", a, b);
```

我想打个 c

```java
log.info("info1 - a:{},c:{},b:{},", a, c, b);
```

结果有 b 这个输出到日志参与了监控和报警, 配置的是包含 info1 关键词, 第一个逗号和第二个逗号中的最大值大于 100 的时候报警, 我们的 c 最小值是 101, 导致发布的时候出现了大量的报警

### 2. 占位符格式与传参数量不对

我就想打印一下 a 和 b, 结果写成了 

```java
log.info("info1 - a:{},b:{}", a);
```

导致打印的时候报错了, 整个接口都挂掉了

### 3. 参数异常导致程序崩掉

空指针:

```java
log.info("info1 - a:{}", a.getAttribute1().getInnerAttribute2());
```

a.getAttribute1\(\) 是空的时候就会这个空指针的错了

io异常等:

```java
log.info("info1 - a:{}", queryAFromRemote());
```

请问的时候报错也会导致正常的代码终止

踩了 bugjson 的内存泄漏等:

```java
log.info("info1 - a:{}", FastJson.toJsonString(a));
```

### 4. 打印日志导致接口超时或系统过载

打印日志本身分为同步和异步, 但不管什么配置都会有响应的时间消耗, 本身比如网关, 其他依赖自身系统设置的客户端超时时间呀都是在长期磨合中配置的, 一行日志可能会成为压倒骆驼的最后一根稻草, 触发服务之间连锁重试等

## 灰度策略和容错

我们发布的时候可以采用灰度策略, 一般灰度策略是这样的: 根据用户 id 的后 x 位来做入参, 在微服务中的远程配置中心配置灰度比例, 比如 1, 如果这 x 位小于灰度比例, 就走新的代码, 否则就走老的代码, 这大家有的时候发现微信有些新特性其他小伙伴有, 你却没有, 可能就是你的工号尾数比较靠后.

容错呢, 其实就是加上 try-catch,,,

![](/assets/2020121200.png)

灰度和 try-catch 不仅仅是在这种新加的代码里, 一些比较边缘的功能\(比如说一些营销信息或者标签信息等非必要信息的查询和展示\)也可以在流量变大的时候用来降级或者在报错的时候不影响主要功能, 在大流量的系统里确实是很好用的功能

## 代码实现

先定义一个灰度弱依赖方法

```java
public interface GrayWeakFunction<T, R> {

    default boolean isBusinessOpen() {
        return false;
    }

    R executeNew(T request);

    R executeOld(T request);
}
```

实现根据用户工号灰度的抽象类:

```java
public abstract class BaseUserIdGrayWeakFunction<T, R> implements GrayWeakFunction<T, R> {
    @Override
    public boolean isBusinessOpen() {
        return Constant.REQUEST_USER_ID.get() < queryFromConfigCenter();
    }

    protected abstract Integer queryFromConfigCenter();
}
```

编写模板:

```java
public class GrayWeakTemplate {
    private GrayWeakTemplate() {
    }

    public static <T, R> R doWeakFunction(
        GrayWeakFunction<T, R> weakFunction, T request, String tip) {
        try {
            return weakFunction.isBusinessOpen() ? weakFunction.executeNew(request) :
                weakFunction.executeOld(request);
        } catch (Exception e) {
            System.out.println(tip + e.getMessage());
            return null;
        }
    }
}
```



