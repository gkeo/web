# WOL 网络唤醒避坑指南：常见问题的分析与内容补充_软件应用_什么值得买
2021-12-04 21:15:09 54 点赞 258 收藏 26 评论

社区里面已经有很多关于 WOL 的优质文章，写本文的目的是为了丰富社区中关于 WOL 话题的相关内容，UP 会以问题为导向尝试解释一些使用过程中的问题，如果你在折腾 WOL 的过程中遇到了一些困难没有找到答案，那么或许本文能够帮到你。

本文内容较长，没耐心的可以跳到最后一章看总结。如有错误，请不吝惜指正，谢谢！

WOL 的设置过程 UP 就不赘述了，请参考下面其他博主的文章：

少数派文章：《[不用开机键，你的 Windows 也能随时就绪：WoL 网络唤醒入门](https://sspai.com/post/67003)》

## 一、为什么我在 BIOS 找不到 WOL 的设置？

网络唤醒是需要硬件支持的，需要满足以下两点（绝大多数现今的电脑都能满足，除非你的电脑真的很老旧）：

-   **使用支持 ATX 2.01 标准的电源**；
-   **[主板](https://www.smzdm.com/fenlei/zhuban/)需要支持 PCIE 2.2 标准**：

在 PCIE 2.2 标准之前，那些旧的主板上会有一个用于网络唤醒的接口（ WAKEUP-LINK header ）需要使用一个特殊的电缆线连接到网卡上才能实现网络唤醒。但是在 PCIE 2.2 标准开始，那些支持此项标准的主板和网卡就不再需要前面所说的网络唤醒电缆线了，因为待机时的电源是通过 PCIE 总线转发的。（参考自 Wiki：[Wake-on-LAN](https://en.wikipedia.org/wiki/Wake-on-LAN#Hardware_requirements)）

也就是说，如果你设置了网络唤醒，你的电脑在**睡眠**（S3）、**休眠**（S4）或**关机**（S5）时，你的网卡还是会处于待机状态，那么此时网卡待机时所需要的电源就是由 PCIE 总线转发的。

> 不同的主板对网卡待机的电源策略不同，并不是所有主板都支持网卡在睡眠（S3）、休眠（S4）或关机（S5）时处于待机（Standby）状态。

因此我们在 BIOS 中查找 WOL 有关的设置时，往往需要到**电源**的相关选项中去寻找关于 WOL 的选项，其中的关键词有（参考自[少数派文章](https://sspai.com/post/67003)）：

-   ACPI
-   PCIE 设备开机
-   Automatic Power On
-   Wake on LAN/WLAN
-   Power Management
-   Power On by Onboard LAN
-   Power On by PCI-E Devices
-   ......

以 Up 的[华擎](https://pinpai.smzdm.com/2621/) Z370M Pro4 主板 BIOS 为例，这块主板的 BIOS 中没有很直观的 “wake”、“WOL”等关键字，网络唤醒的开启设置放到了 “ACPI 配置 --> PCIE 设备开机” 中：

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/75443884-c9ea-4eda-bed2-f6a359ab311d.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_2/)

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/008d4fb3-7d7c-414e-8c5f-eef70410df28.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_3/)

另一个例子是 UP 的公司电脑：

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/7be7cb47-d9c0-4585-a374-2b606384705e.jpeg?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_4/)

二、为什么电源关闭后无法唤醒电脑： Windows 电源管理策略和其他设置  

* * *

### （一）Windows 电源管理策略

电源关闭后无法唤醒电脑的原因有很多，这一章我们从操作系统层面 —— **Windows 的电源管理策略**开始讲起。

首先，Windows 官方文档《 [System Power States](https://go.smzdm.com/3ecd4f1e758f18ee/ca_bb_yc_163_86162909_16293_2621_1641_0) 》中讲述到，**Windows 系统自身（抛开 BIOS 不谈）是不支持 “快速启动” 和“关机”状态下的网络唤醒，只支持 “睡眠”、“休眠” 状态下的网络唤醒**：

-   睡眠（Sleep），属于 S3 电源状态；
-   快速启动（Fast Startup），属于 S4 电源状态；
-   休眠（Hibernate），属于 S4 电源状态；
-   关机（Soft off），属于 S5 电源状态

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/6f3a7202-6ee9-43d8-905f-a892ad2c88f9.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_5/)

> 对于我们普通用户，电脑的电源状态在我们的感知上无非就 “开” 和“关”两种，但是对于系统和硬件来说，除了 “开” 和“关”之外还有很多种状态——ACPI 标准定义了包括 S0~S5 在内的多种电源状态（ACPI Power State），比如我们上面写到的睡眠（Sleep, S3）、休眠（Hibernate, S4）或关机（Soft Off, S5）等，Windows 官方文档《 [System Power States](https://go.smzdm.com/3ecd4f1e758f18ee/ca_bb_yc_163_86162909_16293_2621_1641_0) 》中就列出了 ACPI 规范中的多种电源状态（Power State） 。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/aa77da72-b976-4029-acdb-573fe5c29af8.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_6/)

-   对于**“快速启动（fast startup, S4）”：** 从 Windows 8.1 开始到 Windows 11，“快速启动” 作为系统默认的 “**关机” 方式**，是不支持网络唤醒的，**这也是为什么在 WOL 的教程中会让你关闭 “快速启动”**：

> “快速启动”：快速启动是一种关机类型，它使用休眠文件来加快后续的启动速度，在这种关机状态下，Windows 系统不支持网络唤醒。“快速启动”与 “休眠” 同属于 S4 电源状态。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/61e5c90c-b087-4723-be5e-7045cf77ef14.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_7/)

“快速启动” 属于 “**hybrid shutdown（混合关机）**”，是关机的一种类型：

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/5606e78b-4d0f-4736-9b03-358c08382853.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_8/)《System Power States》

虽然 “快速启动” 与“休眠（Hibernate）”同属于电源状态 S4，但 Windows 10 中系统只会对前者禁用网络唤醒；当用户明确指示 Windows 系统进入 “休眠” 模式时，Windows 是支持网络唤醒的（《[Wake on LAN (WOL) behavior in Windows 10](https://go.smzdm.com/05026e968fc4cad7/ca_bb_yc_163_86162909_16293_2621_1641_0)》）：

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/701e7976-0d24-4fe6-9cdf-f90eec113ad3.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_9/)官方文档解释了快速启动和休眠这两种电源状态在 WOL 下的区别

有些教程会让你在 Windows 中关闭 “休眠” 功能来解决网络唤醒无法生效的问题，但很明显不会产生作用，因为 Windows 是支持 “休眠” 状态下的网络唤醒，造成无法唤醒的原因并不在 “休眠” 模式上。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/e4d25f55-4800-46b9-81af-2f4efa8c98f9.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_10/)

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/900a8ed3-816e-49c1-860c-e82e655c467c.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_11/)

-   对于**“关机（soft off, S5）”**，Windows 也是不支持网络唤醒的，那么这里就很矛盾了，既然 Windows 不支持关机后网络唤醒，那么网络唤醒这个东西怎么还会存在？

    实际这时候产生作用的就是主板了：

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/670bfb1c-3af7-46af-8d53-b7034957cc3c.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_12/)微软官方文档解释了 BIOS 的作用

主板的硬件支持能让电脑能从 “关机（soft off，S5）” 中实现网络唤醒，而 Windows 系统自身是不支持这一特性的。

> 另外，Windows 7 和 Windows 10 在 WOL 的实现上也是略有区别的，**主要的区别就在于 Windows 7 没有 “快速启动”。** 所以就会有人遇到从 Windows 7 升级到 Windows 10 之后网络唤醒失效的情况——其实就是因为 Windows 10 把 “快速启动” 作为系统默认的一种 “关机” 方式，导致网络唤醒失效，而由于 Windows 7 没有 “快速启动” 所以不存在这个问题（具体请参考《[Wake on LAN (WOL) behavior in Windows 10](https://go.smzdm.com/05026e968fc4cad7/ca_bb_yc_163_86162909_16293_2621_1641_0)》）。要解决此问题，你可能需要在 Windows 10 / 11 中关闭快速启动。

你可以在 CMD 或 Powershell 中使用 **powercfg /a** 命令查询目前系统上所有可用的、硬件可支持的 “睡眠” 策略：

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/0543e39b-e285-443b-8ae6-5dfc784fd534.jpeg?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_13/)

之所以强调关于 “睡眠” 模式，是因为不同的硬件对 “睡眠” 模式的支持可能会存在较大的差异，这直接影响了 WOL 实现的与否。

关于 “睡眠” 模式下软硬件环境的状态，可以参考[微软](https://pinpai.smzdm.com/1461/)的 《[System Sleeping States](https://go.smzdm.com/86b8a50bcda12480/ca_bb_yc_163_86162909_16293_2621_1641_0)》。

> “**混合睡眠**”（更多请参考：[HP 知识库](https://go.smzdm.com/1a1a7ac2d1ae974a/ca_bb_yc_163_86162909_16293_2621_1641_0)）：混合睡眠是睡眠和休眠的组合 - 它将所有打开的文档和程序保存到内存和硬盘上，然后让计算机进入低耗能状态，以便可以快速恢复工作。如果发生电源故障，Windows 可从硬盘中恢复您的工作。如果打开了混合睡眠，让计算机进入睡眠状态的同时，计算机也自动进入了混合睡眠状态。

### （二）其他需要注意的 Windows 设置说明

-   为什么我关闭了 “快速启动” 选项依然无法实现唤醒？

在网卡设备的 “高级” 选项卡中，尝试关闭 “节能以太网”、“绿色以太网”、“节能模式” 等相关的设置，可参考《[Wake on LAN doesn't work when power down from Windows](https://superuser.com/questions/1513614/wake-on-lan-doesnt-work-when-power-down-from-windows)》。

-   “**只允许幻数据包唤醒计算机**” 建议勾上。

不然会存在电脑被 “误” 唤醒的情况（虽然几率不大）。因为不是只有这一种数据包能够唤醒电脑，也就是说，你的网卡在待机的过程中，可能会被内部网络上传输的数据包 “误导” 从而唤醒你的主机。普通家庭网络还好，但如果是企业网络的话，网络环境相对比较 “吵杂”，电脑被误唤醒的几率会大一些，因此对于我们来说勾上此选项是不会错的（不勾也是可以的，不勾选是为了利用这一特性来实现网络唤醒，但那种场景绝大多数人接触不到，也没必要。关于“误” 唤醒的更多内容，请参考微软官方文章《[Unwanted wake-up events occur when you enable the Wake On LAN feature](https://go.smzdm.com/7b5b095509b34007/ca_bb_yc_163_86162909_16293_2621_1641_0)》）。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/7fbf6739-7e2e-4700-91cd-a627a0ddb001.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_14/)

-   **请安装完整的网卡驱动，否则可能无法实现网络唤醒（**参考自 [wiki](https://en.wikipedia.org/wiki/Wake-on-LAN#Microsoft_Windows)）：  

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/5d0bdea6-2642-4fc7-a13e-a716a6c6919a.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_15/)单靠 Windows 自身提供的驱动可能无法实现网络唤醒

三、为什么停电后无法实现网络唤醒：BIOS 的电源管理策略  

* * *

> 关键字：来电开机、深度睡眠  

大家看完上面 Windows 下的电源策略后，我们再去看 BIOS 的相关内容就会好理解一些。

这里我们以一个场景来展开：**停电了，恢复供电之后网络唤醒却不起作用了，这是为什么？**

> 这种情形等同于把主机的电源线拔掉再插进去。

我们在上文讲过：  

> 如果你设置了网络唤醒，你的电脑在睡眠（S3）、休眠（S4）或关机（S5）时，你的网卡还是会处于待机状态......

再具体点说，电源会向网卡提供 5V 的电流以保证网卡处在**待机（Standby）**状态，只要网卡保持在待机状态，那么在其他条件都满足的情况下，网卡就可以接收到网络唤醒的信号以实现网络开机。那么当停电后，恢复供电时，**由于有些主板不支持恢复到 “先前” 的状态，对于网卡来说就是无法恢复到 “待机” 状态，因此这时候网络唤醒就不起作用了。** 

所以当你在设置 BIOS 时，你需要检查一下你的 BIOS 是否支持 “**来电开机**” 或者 “**断电恢复**”：

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/585cb44e-00d2-46e3-abd9-6465da36f6cc.jpeg?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_16/)

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/5568fe70-2d43-47a1-b60e-e712362635ef.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_17/)

有些主板的来电恢复功能，能够支持恢复到 “上一个状态”，比如说恢复到 S4 或者 S5 状态：

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/951ff900-874c-4aca-a357-dd6a9f411149.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_18/)断电后恢复到 “上一个状态（Last State）”

关于 BIOS 的设置，**另一方面还需要禁用主板的 “深度睡眠” 功能，因为在这种状态下是无法实现网络唤醒的。** 

> 关于 “深度睡眠”，不同的 BIOS 可能叫法不同，有的叫 “**深度节能模式**”，有的叫 “**超低电量模式**” 等，但本质上是一样的。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/4765095f-0033-4c2c-a52a-f893636320aa.jpeg?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_19/)

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/c5b22b35-5d7b-49f7-b8a1-c6180dcdbfba.jpeg?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_20/)“Deep Power Off Mode（超低电量模式）”

## 四、为什么关机一段时候后外网无法网络唤醒：Magic Packet 与外网设置

场景：**关电脑一段时候后，外网的网络唤醒失效**。  

原因是因为长时间的离线，导致了[路由器](https://www.smzdm.com/fenlei/luyouqi/) ARP 表中电脑的 MAC 信息被删除，因此外网发过来的数据包无法正确传递到电脑的网卡，从而无法唤醒。

社区里有博主详细讲解过这种情况：

解决办法是在路由器中使用 ARP 绑定功能，将 IP 与 网卡的 MAC 地址进行绑定（UP 这里使用的是 Pandava，ARP 绑定功能在 “内网 LAN -> DHCP [服务器](https://www.smzdm.com/fenlei/fuwuqi/) -> 静态 IP 设置” 中）：

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/bc156f94-b4ef-46a2-b6d7-79eaf4451d26.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_21/)

在路由器的端口转发中也要设置好相应的规则：

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/fc4420f3-5143-44ac-8f3b-093708f5fff8.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_22/)

解决了 ARP 的问题后，那么在这一章，UP 要补充的是关于外网唤醒的一些信息补充。

### （一）唤醒魔包（Magic psacket）与端口转发

需要了解的是，唤醒魔包是通过 OSI 网络模型中的 “数据链路层”（二层）传播的，因此无关 IP、无关传输协议（TCP、UDP），网卡只要识别到特定广播帧就可以唤醒电脑。

但是由于路由器是工作在网络层（第三层），想要通过外网唤醒电脑，那么就要在路由器的端口转发中，向外网暴露相应的端口来接收唤醒魔包，并指定所要唤醒电脑的 IP 地址。

以 Up 来说，通过暴露到外网的 19999 端口来接收唤醒数据包，并转发到对应的 IP 地址：

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/7cf31af9-4972-4197-865c-ddf2fd1949b4.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_23/)

**在这个场景下，不需要强调传输协议和内网端口**。我们前面讲过，唤醒魔包工作在第二层，无关 IP、无关传输协议，因此只要告诉路由器要转发到的 IP 地址，那么在 ARP 绑定的作用下，路由器就知道 IP 地址背后网卡的 MAC 地址，因此网卡就能够接收到数据包并唤醒电脑。因此，不管上面我们用什么端口号（在不影响其他端口的情况下）或者协议（ TCP 或者 UDP ），都不影响我们唤醒电脑。

> 关于上述的原理，可参考：《[Can I send a Wake On LAN packet without broadcasting?](https://superuser.com/questions/104755/can-i-send-a-wake-on-lan-packet-without-broadcasting)》

但是，如果你在开机的情况下，想要在 Windows 中（或其他系统）测试外网唤醒，那么这种时候你就需要在路由器的端口转发规则中指定对应的端口号和协议，以便让路由器把数据包转发到 Windows 系统上。那么在这种情形下，网络唤醒则采用 UDP 协议传输。这里有篇文章说明了网络唤醒所使用的端口是否有讲究，并且也说明了网络唤醒数据包的工作原理：《[Does it matter what UDP port a WOL signal is sent to?](https://superuser.com/questions/295325/does-it-matter-what-udp-port-a-wol-signal-is-sent-to)》。

### （二）外网唤醒的最佳实践：通过内网唤醒

我们前面知道，如果要实现外网唤醒，需要通过 ARP 绑定来让路由器知道网卡的 MAC 地址，这种情况就需要把 IP 地址固定下来。但如果说你不知道电脑的 IP 地址，只知道网卡的 MAC 地址，那么这种时就没办法来使用 ARP 绑定。  

在这种情况下，我们就可以利用内网的相关特性来实现网络唤醒。

首先我们要理解，“唤醒魔包（Magic Packet）” 是一种广播帧，而**路由器是无法转发广播帧的**，这也就是为什么我们需要使用路由器的 ARP 绑定功能，来让路由器把外网传过来的唤醒魔包精确地转发到对应电脑的网卡上。

但是在内网下，则可以在子网内广播唤醒魔包，让内网下所有电脑的网卡都接收到数据包，当对应的网卡发现唤醒魔包内的 MAC 地址与自己 MAC 地址对应上时就会唤醒电脑，而发起唤醒数据包的一方就不需要知道想要唤醒的那一台电脑的 IP 地址。

举个例子，Up 的 NAS 虚拟机上安装了一个 Debian 系统，通过 “wakeonlan” 命令即可以广播唤醒魔包：  

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/87c69564-9764-4f2f-8a97-458b1fc78981.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_24/)wakeonlan 命令说明

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/5da03e26-9ff3-443f-ae55-713c5e2eeb6b.png?raw=true)
](https://post.smzdm.com/p/amx025p4/pic_25/)此命令还可以实现批量唤醒多个网卡

除了 “wakeonlan” 工具之外，还有 “eth-wake”、“wol” 工具等，可以参考社区里其他博主的文章：

回过头来，对于外网唤醒来说，比较好的一种方式就是通过外网下达指令，来让内网的机器来发起唤醒魔包。就好比上面《使用快捷指令实现 WOL 开启电脑的》中利用路由器来发起广播从而唤醒电脑。

五、总结  

* * *

前面我们讲了很多内容，这里我们做个总结，没有耐心的朋友可以直接看着一部分。  

关于**在 BIOS 中找不到网络唤醒的选项**：

-   如果在 BIOS 设置中找不到比较明显的 “wake”、“WOL” 等相关的网络唤醒设置选项，那么请去到电源的相关设置中去查找，关键词包括 ACPI、PCIE 等；

从**硬件层面**上：

只要能够让电脑的网卡进入待机模式，或者说恢复到待机模式，那么网卡就能接收唤醒指令来执行电脑唤醒，从这一点出发：

-   Windows 的 “睡眠” 和“休眠”模式能够使网卡进入待机状态，而 “快速启动” 这种关机形式则不行，可以利用 powercfg /a 命令查看本机所支持的相关睡眠模式，了解硬件支持的程度；
-   BIOS 的 “深度睡眠” 模式（或者其他名字）无法让网卡处在待机模式，需要在 BIOS 中关闭；  
-   BIOS 如果不支持断电恢复的相关功能（“断电恢复”、“来电开机”、“Restore AC Power Loss” 等），网卡也就无法恢复到原来的待机模式，这也是为什么会出现停电恢复后无法网络唤醒的情况。

从 **Windows 系统层面**上：

在设置上，需要排除会影响到网卡待机，或者会让电脑被 “误” 开机的因素：

-   关闭 “快速启动” 设置，这一模式是造成 Windows 7 升级到 Windows 10 后无法唤醒的原因之一；  
-   需要开启 “只允许幻数据包唤醒计算机”，避免电脑被“误” 唤醒；
-   如果其他条件都满足下依然无法实现网络唤醒，那么请尝试在网卡设备的 “高级” 选项卡中关闭 “节能以太网”、“绿色以太网”、“节能模式” 等相关的 “节能” 设置；
-   请安装完整的网卡驱动；

从**网络层面**上：

-   关机一段时候后无法外网唤醒电脑，是因为电脑长时间的离线导致了路由器 ARP 表中电脑的 MAC 信息被删除，解决办法是使用路由器的 ARP 绑定功能将 IP 和 MAC 地址进行绑定；  
-   “唤醒魔包” 工作在 OSI 模型的第二层，无关 IP 地址、传输协议（TCP 或 UDP）和端口，所以我们在设置路由器的端口转发时，只要保证在从外网发送过来的唤醒数据包转发到正确的 IP 地址即可，端口号和协议无需太过于关注（只要不影响其他端口转发规则即可）；
-   但如果你想要在 Windows 中测试外网的网络唤醒功能，那么在路由器的端口转发中就需要明确指定 UDP 协议，并且在 Windows 中开放相应的端口来接受转发过来的数据；

**个人认为的最佳实践**：外网通过 SSH 来命令内网的设备（路由器或主机）发起广播来实现网络唤醒。

好处在于：

-   无需 ARP 绑定，无需被唤醒的电脑有固定的 IP；  
-   诸如 “wakeonlan” 等命令可以批量唤醒电脑；
-   通过 SSH 更安全；
-   配合相关的软件（如 IOS 的捷径），可以实现方便快捷的一键唤醒（如语音命令 Siri 执行快捷指令）；  

（完）

![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-20%2012-09-26/85f15280-e0d2-4c07-834e-ccef6cd7dca4.png?raw=true) 
 [https://post.smzdm.com/p/amx025p4/](https://post.smzdm.com/p/amx025p4/)
