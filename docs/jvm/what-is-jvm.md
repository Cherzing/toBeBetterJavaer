---
title: 大白话+手绘图带你认识 JVM，JVM到底是什么？
shortTitle: 大白话带你认识JVM
category:
  - Java核心
tag:
  - Java虚拟机
description: JVM是Java程序执行的环境，它隐藏了底层操作系统和硬件的复杂性，提供了一个统一、稳定和安全的运行平台。
head:
  - - meta
    - name: keywords
      content: Java,JavaSE,教程,二哥的Java进阶之路,jvm,Java虚拟机
---

# 第一节：大白话带你认识 JVM

“二哥，之前的文章里提到了 JVM，说实在的，我还不知道它到底是干嘛的，你能给我普及一下吗？”三妹咪了一口麦香可可奶茶后对我说。

“三妹，不要担心，这篇内容来带你认识一下什么是 JVM，这也是 Java 中非常重要的一块知识，每个程序员都应该了解的。尤其是大中厂面试和 Java 性能优化时，必须要掌握的。”我回答。

看过《[Java 发展简史](https://javabetter.cn/overview/what-is-java.html)》的小伙伴应该知道，Sun 在 1991 年成立了一个由詹姆斯·高斯林（James Gosling）领导的，名为“Green”的项目组，目的是开发一种能够在各种消费性电子产品上运行的程序架构。

一开始，项目组打算使用 C++，但 C++ 无法达到跨平台的要求，比如在 Windows 系统下编译的 Hello.exe 无法直接拿到 Linux 环境下执行。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/overview/seven-01.png)

在当时，C++ 已经非常流行了，但无法跨平台，只能忍痛割爱了。

怎么办呢？

三妹不知道有没有听过直译器（解释器）这玩意？（估计你没听过）就是每跑一行代码就生成机器码，然后执行，比如说 Python 和 Ruby 用的就是直译器。在每个操作系统上装一个直译器就好了，跨平台的目的就达到了。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/overview/seven-02.png)

但直译器有个缺点，就是没法像编译器那样对一些热点代码进行优化，从而让机器码跑得更快一些。

怎么办呢？

来个结合体呗，编译器和直译器一块上！

![](https://cdn.tobebetterjavaer.com/stutymore/what-is-jvm-20231019153456.png)

编译器负责把 Java 源代码编译成字节码（[字节码会在后面细讲](https://javabetter.cn/jvm/bytecode.html)），Java 虚拟机（Java Virtual Machine，简称 JVM） 负责把字节码转换成机器码。转换的时候，可以做一些压缩或者优化（[JIT](https://javabetter.cn/jvm/jit.html)，后面会讲），这样的机器码跑起来就快多了。

不仅跨平台的目的达到了，而且性能得到了优化，两全其美！

“为什么 Java 虚拟机会叫 Java 虚拟机呢？”三妹问了一个很古怪的问题。

虚拟机，顾名思义，就是虚拟的机器（多苍白的解释），反正就是看不见摸不着的机器，把它想象成一个会执行字节码的怪兽吧。

记得上大学那会，由于没有 Linux 环境，但又需要在上面玩一些命令，于是就在 Windows 上装 Linux 的虚拟机，这个 JVM 就类似这种东西。

说白了，就是我们编写 Java 代码，编译 Java 代码，目的不是让它在 Linux、Windows 或者 MacOS 上跑，而是在 JVM 上跑。

## JVM 家族

说到这，三妹是不是想问，“都有哪些 Java 虚拟机呢？”来看下面这张思维导图：

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/overview/seven-04.png)

除了我们经常看到，经常听到的 Hotspot VM，还有很多，下面我来简单介绍一下。

- Sun Classic：世界上第一款商用 Java 虚拟机，但执行效率低下，导致 Java 程序的性能和 C/C++ 存在很大差距，因此给后来者留下了“Java 语言很慢”的刻板印象。

- Exact VM：为了提升 Classic 的效率，Sun 的虚拟机团队曾在 Solaris（Sun 研发的一款类似 Unix 的操作系统）上发布过这款虚拟机，它的执行系统里包含有热点探测、即时编译等，但不是很成熟。

Sun Classic 在 JDK 1.4 的时候被彻底抛弃，而 Exact VM 被抛弃得更早，取代它的正是 HotSpot VM——时也命也。

- HotSpot VM：OracleJDK（商用）和 OpenJDK（开源）的默认虚拟机，也是目前使用最广泛的 Java 虚拟机。

HotSpot 的技术优势就在于热点代码探测技术（名字就从这来）和准确式内存管理技术，但其实这两个技术在 Exact VM 中都有体现，因此你看起个好的名字多重要（开玩笑了，这就是命）。

热点代码探测，指的是，通过执行计数器找出最具有编译价值的代码，然后通知即时编译器以方法为单位进行编译，解释器就可以不再逐行的将字节码翻译成机器码，而是将一整个方法的所有字节码翻译成机器码再执行。

这样的话，效率就提高了很多，对吧？

- Mobile VM：Java 在移动手机端（被 Android 和 IOS 二分天下）的发展并没有那么成功，因此 Mobile VM 的声望值比较低。

- Embedded VM：嵌入式设备上的虚拟机。

- BEA JRockit：曾经号称是“世界上最快的 Java 虚拟机”，后来被 Oracle 收购后就没有声音了。

- IBM J9 VM：提起 IBM，基本上所有程序员都知道了，也是个巨头，所以他家的虚拟机也很强，在职责分离和模块化上做得比 HotSpot 更好。目前已经开源给 Eclipse 基金会。

- BEA Liquid VM：是 BEA 公司开发的可以直接运行在自家系统上的虚拟机，可以越过操作系统直接和硬件打交道，因此可以更大程度上的发挥硬件的能力。不过核心用的还是 JRockit，所以伴随着 JRockit 的消失，Liquid VM 也退出历史舞台了。

- Azul VM：是 Azul 公司在 HotSpot 基础上进行大量改进后的，可以运行在 Azul 公司专有硬件上的虚拟机。2010 年起，Azul 公司的重心从硬件转移到软件上，并发布了 Zing 虚拟机，性能方面很强大。

- Apache Harmony 和 Google Android Dalvik VM 并不是 严格意义上的 Java 虚拟机，但对 Java 虚拟机的发展起到了很大的刺激作用。但它们终究没有熬过时间。

- Microsoft JVM：在早期的 Java Applets 年代，微软为了在 IE 中支持 Applets 开发了自己的 Java 虚拟机。你敢相信？Microsoft JVM 只有 Windows 版本，它与 JVM 实现的“一次编译，到处运行”的理念完全沾不上边。

关键是，1997 年 10 月，Sun 公司因为这事把微软告了，最后微软赔给了 Sun 公司 2000 万美金，并且终止了在 Java 虚拟机方面的发展。如果，我是说如果，如果微软保持着对 Java 的热情，后面还有 .Net 什么事？

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/overview/seven-05.png)

## JVM 内部结构

“Java 虚拟机长什么样子呢？”了解了这么多 Java 虚拟机后，三妹继续追问到。

Java 虚拟机虽然是虚拟的，但它的内部是可以划分为：

- 类加载器（Class Loader）
- 运行时数据区（Runtime Data Areas）
- 执行引擎（Excution Engine）

这三部分的，见下图。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/overview/seven-06.png)

好，我们这三个结构再细化一下，如下图所示。

![](https://cdn.tobebetterjavaer.com/stutymore/what-is-jvm-20231030185742.png)

### 1）类加载器

类加载器是 Java 虚拟机的一个子系统，用于加载类文件。每当我们运行一个 Java 程序，它都会由类加载器首先加载。

类加载器负责将字节码文件加载到内存中，主要经历加载-》连接-》实例化三个阶段，以完成类加载操作。

[戳链接了解类加载机制和类加载器](https://javabetter.cn/jvm/class-load.html)

### 2）运行时数据区

JVM 定义了在 Java 程序运行期间需要使用到的内存区域，简单来说这块内存区域存放了字节码信息以及程序执行过程数据。来看下面这张图：

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/overview/seven-07.png)

来一一解释下。

- PC 寄存器（PC Register），也叫程序计数器（Program Counter Register），是一块较小的内存空间，它的作用可以看做是当前线程所执行的字节码的信号指示器。

- JVM 栈（Java Virtual Machine Stack），与 PC 寄存器一样，JVM 栈也是线程私有的。每一个 JVM 线程都有自己的 JVM 栈（也叫方法栈），这个栈与线程同时创建，它的生命周期与线程相同。

- 本地方法栈（Native Method Stack），JVM 可能会使用到传统的栈来支持 [Native 方法](https://javabetter.cn/oo/native-method.html)（前面讲过）的执行，这个栈就是本地方法栈。

- 堆（Heap），在 JVM 中，堆是可供各条线程共享的运行时内存区域，也是供所有类实例和数据对象分配内存的区域。

- 方法区（Method area），在 JVM 中，被加载类型的信息都保存在方法区中。包括类型信息（Type Information）和方法列表（Method Tables）。方法区是所有线程共享的，所以访问方法区信息的方法必须是线程安全的。

- [运行时常量池](https://javabetter.cn/string/constant-pool.html)（前面讲过字符串常量池），运行时常量池是每一个类或接口的常量池在运行时的表现形式，它包括了编译器可知的数值字面量，以及运行期解析后才能获得的方法或字段的引用。简而言之，当一个方法或者变量被引用时，JVM 通过运行时常量区来查找方法或者变量在内存里的实际地址。

不过需要说明的是在 JDK 1.8 及以后的版本中，方法区被移除了，取而代之的是元空间（Metaspace）。元空间与方法区的作用相似，都是存储类的结构信息，包括类的定义、方法的定义、字段的定义以及字节码指令。不同的是，元空间不再是 JVM 内存的一部分，而是通过本地内存（Native Memory）来实现的。在 JVM 启动时，元空间的大小由 MaxMetaspaceSize 参数指定，JVM 在运行时会自动调整元空间的大小，以适应不同的程序需求。

![](https://cdn.tobebetterjavaer.com/stutymore/what-is-jvm-20231030191213.png)

### 3）执行引擎

执行引擎包含了：

- 解释器：读取字节码流，然后执行指令。因为它是一行一行地解释和执行指令，所以它可以很快地解释字节码，但是执行起来会比较慢（毕竟要一行执行完再执行下一行）。
- 即时（Just-In-Time，[JIT](https://javabetter.cn/jvm/jit.html)）编译器：即时编译器用来弥补解释器的缺点，提高性能。执行引擎首先按照解释执行的方式来执行，然后在合适的时候，即时编译器把整段字节码编译成本地代码。然后，执行引擎就没有必要再去解释执行方法了，它可以直接通过本地代码去执行。执行本地代码比一条一条进行解释执行的速度快很多。编译后的代码可以执行的很快，因为本地代码是保存在缓存里的。
- [垃圾回收器](https://javabetter.cn/jvm/garbage-collector.html)，用来回收堆内存中的垃圾对象。

字节码执行引擎从元空间获取字节码指令进行执行（[后面会细讲](https://javabetter.cn/jvm/how-run-java-code.html)）。当 Java 程序调用一个方法时，JVM 会根据方法的描述符和方法所在的类在元空间中查找对应的字节码指令。字节码执行引擎从元空间获取字节码指令，然后执行这些指令。

## 小结

“三妹，关于 Java 虚拟机，今天我们就学到这吧，后面再展开讲，怎么样？”转动了一下僵硬的脖子后，我对三妹说，“Java 虚拟机是一块很大很深的内容，如果一上来学太多的话，我怕难倒你。”

“好的，二哥，我也觉得今天的知识量够了，我要好好消化几天。我会加油的！”三妹似乎对未来充满了希望，这正是我想看到的。

总的来说，JVM 是 Java 程序执行的环境，它隐藏了底层操作系统和硬件的复杂性，提供了一个统一、稳定和安全的运行平台。

你把 Java 源代码编译后的字节码文件扔给它，它就可以在 JVM 中执行，不管你是在 Windows、Linux 还是 MacOS 环境下编译的，它都可以跑，屏蔽了底层操作系统的差异。

---

一个人可以走得很快，但一群人才能走得更远。[二哥的编程星球](https://javabetter.cn/zhishixingqiu/)已经有 **3700 多名** 球友加入了，如果你也需要一个良好的学习环境，扫描下方的优惠券加入我们吧。新人可免费体验 3 天，不满意可全额退款（星球官方机制 😄）。

<img src="https://cdn.tobebetterjavaer.com/paicoding/7c0ac1af155d3c1a1df268a17813ca45.png" title="二哥的编程星球" width="400" />

这是一个**编程学习指南 + Java 项目实战 + LeetCode 刷题的私密圈子**，你可以阅读星球专栏、向二哥提问、帮你制定学习计划、和球友一起打卡成长。

![](https://cdn.tobebetterjavaer.com/stutymore/what-is-jvm-20231019155412.png)

两个置顶帖「球友必看」和「知识图谱」里已经沉淀了非常多优质的内容，**相信能帮助你走的更快、更稳、更远**。

- [二哥的 Java 面试指南专栏发布了 ✌️](https://javabetter.cn/zhishixingqiu/mianshi.html)
- [二哥的原创实战项目技术派上线了 ✌️](https://mp.weixin.qq.com/s/AyRK5OWGYugS6s2vn2CPuA)
- [2000+薪资待遇还不错的公司名单 ✌️](https://mp.weixin.qq.com/s/7JIKnYCYtYrpAXRTjU0LLw)
- [二哥的并发编程小册发布了 ✌️](https://javabetter.cn/thread/)

最后，把二哥的座右铭送给大家：**没有什么使我停留——除了目的，纵然岸旁有玫瑰、有绿荫、有宁静的港湾，我是不系之舟**。
