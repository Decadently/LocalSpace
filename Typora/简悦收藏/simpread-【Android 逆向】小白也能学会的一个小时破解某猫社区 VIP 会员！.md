> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.cnblogs.com](https://www.cnblogs.com/android-alvin/articles/13820978.html)

前言
--

想要成为一名优秀的 Android 开发，你需要一份完备的 [知识体系](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2FAndroid-Alvin%2FAndroid-P7%2Fblob%2Fmaster%2FAndroid%25E5%25BC%2580%25E5%258F%2591%25E8%25BF%2598%25E4%25B8%258D%25E4%25BC%259A%25E8%25BF%2599%25E4%25BA%259B%25EF%25BC%259F%25E5%25A6%2582%25E4%25BD%2595%25E9%259D%25A2%25E8%25AF%2595%25E6%258B%25BF%25E9%25AB%2598%25E8%2596%25AA%25EF%25BC%2581.md)，在这里，让我们一起成长为自己所想的那样~。

不记得是哪一天，忽忽悠悠地就进入了某猫社区（你懂的），从此，每天早上一瓶营养快线。庆幸的是，该社区为了盈利，开启了 VIP 通道和播放次数限制，不然可以直接喝蛋白质了。不过正值青春、精力旺盛的我们，怎么能让理智控制欲望？那就成为高大上的会员，开启 VIP 加速通道，无限观看！充钱？充钱是不可能充钱的，这辈子都不可能充钱。作为 Android 开发者，应该用特殊的手段来搞定特殊的事情，带着我们目标，那就来一次 Android 的逆向之旅吧！

发布此文的目的，除了分享整个破解过程外，还希望可以帮助你打开 Android 逆向的大门，体验不一样的 Android 世界。

准备工作
----

玩逆向的基本工具，这里需要准备 Android 逆向三件套 [apktool](https://links.jianshu.com/go?to=https%3A%2F%2Flink.juejin.im%3Ftarget%3Dhttps%253A%252F%252Fibotpeaches.github.io%252FApktool%252F)、[dex2jar](https://links.jianshu.com/go?to=https%3A%2F%2Flink.juejin.im%3Ftarget%3Dhttps%253A%252F%252Fsourceforge.net%252Fprojects%252Fdex2jar%252F)、[jd-gui](https://links.jianshu.com/go?to=https%3A%2F%2Flink.juejin.im%3Ftarget%3Dhttp%253A%252F%252Fjava-decompiler.github.io%252F)

apktool：反编译 apk、重新打包新 apk。你可以得到 smali、res、AndroidManifest.xml 等文件；

dex2jar：把 Android 执行的 dex 文件转成 jar 文件；

jd-gui：一款可以方便阅读 jar 文件的代码工具。

他们之间的关系，可以参考下图：

![](https://img-blog.csdnimg.cn/img_convert/b4ee8e955b7c3f0ced88ec173b11596b.png)

[工具我已经传到 GitHub，但不保证以后是最新版本](https://links.jianshu.com/go?to=https%3A%2F%2Flink.juejin.im%3Ftarget%3Dhttps%253A%252F%252Fgithub.com%252Fgoldze%252FAnti-Android-KM)

> 下载好上面三个工具，就可以开启你的逆向之旅了！

逆向开始
----

> 提到逆向，可能很多朋友会想到 Xposed，用 Xposed 去 Hook 参数，绕过 if 判断。没错，用 Xposed 确实很容易就能达到目的，但是它的限制比较大，手机需要 root，并且安装 Xposed 模块，或者需要跑在 VirtualXposed 虚拟环境下，增加了使用者的上手成本。

我们这里的破解方式，会直接输出一个破解版 Apk，使用者不需要进行任何多余的操作，安装即可使用。

### 第一步：将原应用 apk 后缀改成 zip，解压出 classes.dex 文件

![](https://img-blog.csdnimg.cn/img_convert/c0491f1d39f403b46930a24d2b5a12d6.png)

![](https://img-blog.csdnimg.cn/img_convert/42a1ddc2ef4579e52cd0d6ee554b6b8a.png)

> 其实这一步是最难的，逆向最难的在于脱壳，脱壳分两种，手动脱壳 (手脱) 和机器脱壳(机脱)。什么是脱壳呢？就是很多 App 在发布到应用市场之前，会进行加固，即加壳，它会把真正的 dex 文件给 "藏" 起来，我们就需要通过脱壳的方式去找到应用里真正的 dex，才能拿到里面的源码。只有拿到源码并读懂源码(混淆后连蒙带猜)，才能找到爆破点，才能修改代码重新编译。

> 我们这里破解的某猫 App，因为它的特殊性质，它上不了市场，也没有加固，最可气的是，它居然不混淆，这让我们破解的难度直线下降。

### 第二步：使用 dex2jar 将 classes.dex 转成 jar 文件

cmd 到 dex2jar 文件夹目录，执行

```
    d2j-dex2jar D://xxx/xxx/classes.dex 


```

*   1

得到 jar 文件

![](https://img-blog.csdnimg.cn/img_convert/3a6342f8f6f96be680d5da6c88b56a5a.png)

静态分析
----

拿到源码后，首先我们需要找到应用的限制点，绕过 App 里面的判断。

![](https://img-blog.csdnimg.cn/img_convert/510692bc11e41f237af637584524358a.png)

然后分析源码，该从哪里开始入手呢？

我们都知道，一个完整 Android 应用，可能会存在各种第三方，各种依赖库，这些依赖都会被编译到 dex 里面，所以这个 Jar 包里面会存在很多不同包名的类文件，为了方便找到破解应用的包名，我们可以借助 adb 打印栈顶 activity 的类全路径：

```
    adb shell dumpsys activity | findstr "mFocusedActivity" 


```

*   1

![](https://img-blog.csdnimg.cn/img_convert/c69a7fe1b475de3e6082e7f838dd0dab.png)

activity 的包路径已经打印出来了，接下来在 jar 文件里面找到 PlayLineActivity.java 的相关代码。

根据页面 Toast 提示，很轻松就能定位到爆破点。

![](https://img-blog.csdnimg.cn/img_convert/abb36ff82f3f74dc547d5c6c9ad0188a.png)

```
    UserUtils.getUserInfo().getIs_vip().equals("1") 


```

*   1

可以看出，当会员字段为 1 时，说明是会员用户，就会切换至线路 2。

```
    Hawk.put("line", "2"); 


```

*   1

那接下来只需要修改用户实体类 UserModel 的 getIs_vip() 方法，让它永远返回 1 就行了。

破解
--

dex2jar、jd-gui 都只是分析工具，下面才是真正破解的开始。

### Smali 简介

> Dalvik 虚拟机和 Jvm 一样，也有自己的一套指令集，类似汇编语言，但是比汇编简单许多。我们编写的 Java 类，最后都会通过虚拟机转化成 Android 系统可以解读的 smali 指令，生成后缀为 .smali 的文件，与 Java 文件一一对应 （也可能会比 Java 文件多，典型的比如实现某个接口的匿名内部类），这些 smali 文件就是 Dalvik 的寄存器语言。 只要你会 java，了解 android 的相关知识，就能轻松的阅读它，

所以，我们真正需要修改的东西，是 java 代码对应的 smali 指令。

### 反编译

我们利用 apktool 工具，来提取 apk 里面的 smali 文件。

cmd 到 apktool 文件夹下面，执行 （你也可以配置环境变量，这样会方便一些）

```
    apktool.bat d -f [apk输入路径] [文件夹输出路径] 


```

*   1

![](https://img-blog.csdnimg.cn/img_convert/97f272cd09e7a17ca3523a37a3ca8042.png)

反编译成功后，打开 smali 文件夹，找到 UserModel.java 对应包名下的 UserModel.smali 文件。

### 爆破

找到了爆破文件，找到了爆破点，接下来就可以对 UserModel.smali 文件进行爆破了（为什么叫爆破，我也不知道，行内都是这样叫的，感觉高大上，其实就是修改文件）。

用编辑器打开 UserModel.smali ，找到 getIs_vip 方法

![](https://img-blog.csdnimg.cn/img_convert/ccb2b688f5df9c943b2135326ce0031c.png)

可以看到，它返回了成员变量 is_vip 的值，我们只需要把它的返回值修改成 1 就行了。

> 如果对 smali 指令不熟悉，你可以花 10 分钟去了解一下 smali 的基本语法。

![](https://img-blog.csdnimg.cn/img_convert/29d072a1cf1db4e0738a4fc09ff9879f.png)

定义一个 string 类型的常量 v1，赋值为 1，并将它返回出去。

### 动态调试

破解的这个好像太简单了，都省掉了调试步骤，那就直接

保存，搞定！

回编
--

接下来把反编译生产的文件夹又重新回编成 apk。

### 重新打包

cmd 到 apktool 文件夹下面，执行

```
    apktool b [文件夹输入路径] -o [apk输出路径] 


```

*   1

如果修改 smali 文件没有问题的话，就可以正常生成一个新的 apk 文件。

![](https://img-blog.csdnimg.cn/img_convert/01116462479a9338c99498dde0f9b4ee.png)

这时候直接将重新打包的 apk 文件拿去安装是不行的，因为之前 zip 解压的目录中，META-INF 文件夹就是存放签名信息，为了防止恶意串改。

所以我们需要对重新打包的 apk 重新签名。

### 重新签名

首先准备一个 .jks 的签名文件，这个开发 android 的同学应该很熟悉了。

配置了 JDK 环境变量，直接执行：

```
jarsigner -verbose -keystore [签名文件路径] -storepass [签名文件密码] -signedjar [新apk输出路径] -digestalg SHA1 -sigalg MD5withRSA [旧apk输入路径] [签名文件别名] 


```

*   1

![](https://img-blog.csdnimg.cn/img_convert/6060d77d541e4d51836dcee3fa170a77.png)

最后在你的文件夹下面，就可以看到一个 某猫 VIP 破解版. apk。

安装并验证功能

![](https://img-blog.csdnimg.cn/img_convert/b4106245161b1dc35225d5eb82866710.png)

![](https://img-blog.csdnimg.cn/img_convert/f900f228cf48bcc15f6c95f994c8c9de.png)

总结
--

最后来梳理一下破解流程：

1、将原应用 apk 后缀改成 zip，解压出 classes.dex 文件

2、使用 dex2jar 将 classes.dex 转成 jar 文件

3、将 jar 文件用 jd-gui 打开，查看源代码

4、adb 定位到类名包路径，找到相关代码

5、apktool 反编译 apk，找到 smali 对应的爆破点

6、修改 smali 文件，调试程序

7、重新打包，重新签名

以上是我对这次破解流程的一个总结，如果有不对或者遗漏的地方，还请各位大佬指正。

最近有点感冒，干咳一个多月了还不好起来，想到小菊花妈妈课堂那句话：孩子咳嗽老不好，多半是废了。我也就放弃治疗，待在空荡的房间，干着喜欢干的事。我也是刚接触 Android 逆向没多久，一开始以为很复杂，很麻烦，当时只是抱着无聊想试试的心态，反正都放弃治疗了，没想到只花了一个多小时，竟然就成功了，并没有想象中的那么难。

如果你对逆向也感兴趣的话，并且和我一样是初学者，我觉得这个实战过程非常适合你。一是能让你感受到破解的整个过程；二是难度不大，不会打击到你的兴趣，同时还能得到一定的成就感。

原文地址：[github.com/goldze/Anti…](https://links.jianshu.com/go?to=https%3A%2F%2Flink.juejin.im%2F%3Ftarget%3Dhttps%253A%252F%252Fgithub.com%252Fgoldze%252FAnti-Android-KM)

文章写到这里就结束了，如果你觉得文章写得不错就给个赞呗？你的支持是我最大的动力！

![](https://img-blog.csdnimg.cn/img_convert/2e0e0ea2ee61973e031db6aefcf7da6f.png)

最后，熟悉的阅读分享环节
------------

*   [阿里腾讯 Android 开发十年，到中年危机就只剩下这套移动架构体系了！](https://blog.csdn.net/weixin_43901866/article/details/88322941)
*   [“我在阿里做了 5 年招聘，给求职者 10 条建议”](https://blog.csdn.net/weixin_43901866/article/details/88253709)

> 本文在开源项目：[https://github.com/Android-Alvin/Android-LearningNotes](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2FAndroid-Alvin%2FAndroid-P7%2Fblob%2Fmaster%2FAndroid%25E5%25BC%2580%25E5%258F%2591%25E8%25BF%2598%25E4%25B8%258D%25E4%25BC%259A%25E8%25BF%2599%25E4%25BA%259B%25EF%25BC%259F%25E5%25A6%2582%25E4%25BD%2595%25E9%259D%25A2%25E8%25AF%2595%25E6%258B%25BF%25E9%25AB%2598%25E8%2596%25AA%25EF%25BC%2581.md) 中已收录，里面包含不同方向的自学编程路线、面试题集合 / 面经、及系列技术文章等，资源持续更新中。
> 【Android 逆向】小白也能学会的一个小时破解某猫社区 VIP 会员