---
layout: post
title: 在Proteus上使用中断函数
tag: 2020
---
在Proteus上进行仿真实验时，有时会出现不能编译的问题。例如，在对80C51进行仿真时，如果在代码里面使用中断函数，则会出现如下的错误：
>ERROR L121: IMPROPER FIXUP

这并不是源代码本身有什么错误，只是Proteus没有进行正确的配置。只需要转到Source Code的选项卡，在菜单栏里面选中“构建(Build)”，然后进入“工程设置”。

接下来可以看到工程选项，将ROM的值更改为Large。
![屏幕截图 2020-10-30 212615.png](https://i.loli.net/2020/10/30/IYOPZsXwMWGpRkU.png)

然后重新编译，就可以正常通过了。
