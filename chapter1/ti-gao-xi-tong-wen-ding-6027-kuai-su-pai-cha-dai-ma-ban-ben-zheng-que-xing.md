## 前言

当自己找了半天终于找到了 bug 的原因, 开始了修改老 bug 制造新 bug 的旅程, 但是有时候当自己修改了代码的 bug 推送到远程分支开始编译部署了之后, 发现代码好像还是原来的 bug, 没有产生新的 bug, 感觉代码没推上去, 又不知道是不是真的没推上去还是就是原来的 bug 没有修复掉

## JAD 命令学习

![](/assets/2021041800.png)

jad + 类的全路径名 查看类的代码和类加载器\(启动类加载器会显示是空\):



![](/assets/2021041802.png)

jad -E + 正则表达式 找到符合该正则表达式的类的代码:



![](/assets/2021041804.png)

jad + 全路径类名 + 方法名 查看指定方法的代码:



![](/assets/2021041805.png)

还可以使用一些其他参数不打印行号和类加载器这样的:



![](/assets/2021041806.png)

还可以指定类加载器查看代码:

![](/assets/2021041807.png)



## JAD 使用例子:

当疑惑为什么修改了一个 bug 部署后没有引起更多的 bug 的时候使用

