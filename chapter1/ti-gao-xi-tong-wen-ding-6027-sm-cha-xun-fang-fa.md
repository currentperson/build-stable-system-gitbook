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

![](/assets/2021061901.png)

```bash
sm [-c <value>] [--classLoaderClass <value>] [-d] [-h] [-n <value>] [-E] class-pattern [method-pattern]
```

使用:

```bash
sm -d com.codog.demo.Main generateWord
```



![](/assets/2021061902.png)



```
jad com.codog.demo.Main generateWord
```

![](/assets/2021061904.png)







