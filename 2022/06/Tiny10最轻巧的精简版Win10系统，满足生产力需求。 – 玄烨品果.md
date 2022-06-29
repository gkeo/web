# Tiny10最轻巧的精简版Win10系统，满足生产力需求。 – 玄烨品果
NTDEV 发布了一个精简版的 Win10 系统，并且取名为 Tiny10。具体有多精简呢？这么说吧，全部安装完毕，C 盘只占用不到 5GB 空间。

Tiny10 被精简了大部分的常用的功能，只保留的基本的运行功能。当然正常的上网、驱动安装这些都没有问题的。系统语言也默认只保留的英文，如果需要中文语言显示，还需要安装字体。

系统被精简的很彻底，只保留的一些常用的系统组件，比如计算器、CMD、记事本、运行、画图     ![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-00-27/1d20a309-ae79-4d96-8084-c62e2091b013.png?raw=true)
基于 Win10 的 1809 版本制作，安装完成只用了 4.43GB。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-00-27/68ccbc4e-404a-49c7-9d5e-a9d164e66a67.png?raw=true)

整个运行速度超级快，玄烨尝试在 09 款的古董笔记本上进行测试，使用的是奔腾双核 T9600，2G 内存，32G 固态。正常安装过程非常之快，大约只用了 7-8 分钟就按照完毕。运行的速度，感觉电脑年轻了十岁一样。

这样的精简系统，适合于对中高端玩家，对系统操作熟悉，也适合安装到旧电脑或者是 HTPC 主机上、软路由、NAS，只适合做一般生产力使用，不适合普通人娱乐办公用。

玄烨用来安装在虚拟机上面，做 iOS 刷机越狱非常方便，运行也超级快。

下面是简单的安装教程，如果你感兴趣，也可以试试。

## Tiny10 下载：

官方：

[Tiny10 21H2 x64 ISO](https://t.co/tTgoDlIdNJ)
[Tiny10 21H2 x86 ISO](https://t.co/7sasf7dBxG)

#### 安装教程：

准备 U 盘一个，电脑一台（虚拟机也可以）

1，网上搜索下载大白菜 / 老毛桃等 U 盘 WinPE 工具，安装好之后，制作一个 U 盘版 WinPE 工具。拷贝 Tiny64 系统到 U 盘。

2，启动电脑，选择 U 盘启动，选择 WinPE 系统启动。

3，启动后，找到 Tiny64 系统镜像，用虚拟软件加载到磁盘，找到 window 系统安装工具，选择 Tiny64 系统镜像的 Souces 的 install.esd，安装磁盘、引导磁盘默认选择。

4，全自动安装完成后，重启电脑后继续安装，安装完毕就可以了。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-00-27/d7b94372-e1fc-45f8-a248-ef381ca9897a.png?raw=true)
 ![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-00-27/8a8f2c4c-5862-411c-85b3-71a254bf12da.png?raw=true)

FAQ:

1, 没有中文显示

a, 找一台正常的 Win10 系统，在 C 盘 / Windows/Fonts，拷贝整个 Fonts 文件夹到 Tiny64 系统，重启。

b, 点开始系统设置，Time/Language，在 Language 里面，Add a Language 点击左边＋号，选择 Chinses (Simplified,China) , 然后重启电脑即可。 
 [https://dkxuanye.cn/?p=1817](https://dkxuanye.cn/?p=1817)
