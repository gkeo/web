# 高性能Type-C移动硬盘挑选指南 - 知乎
_2020/12/30_

_修正汇总图表并新增 Jmicorn 和 VIA 相关产品_

_别了，2020_

_2020/12/4_

_新增 RTL9210b NVMe/SATA to USB3.2 gen2 兼容型方案_

_新增 JHL7440 NVMe to Thunderbolt 3/USB3.2 gen2 兼容型方案_

_为 USB3.2 Gen2x2 增加更多资料_

_后续估计不会再更新了_

_2020/4/4_

_新增 USB 3.2 Gen2x2 部分，修正所有 USB 命名，修正接口描述错误和部分措辞；更新图表。_

_新增 ROG Strix Arion 篇目；新增大量成品案例；移除部分” 推荐 “含义字眼_

_注意本文中_

_USB 3.2 Gen1=USB3.1 Gen1_

_USB 3.2 Gen2=USB3.1 Gen2_

## 前言

这片文章最早是在 17 年发在百度 Macbook 吧的，到如今的版本应该算是第四版了

16 年 MBP 全 Type-C 模具推出，Mac 用户陷入了与花样繁多转接设备的斗智斗勇之中

但三年以来 Type-C 产品随着接口进一步泛用化，产品格局已经和以前有了很大的不同，ThunderBolt 3 产品相较于先前有了飞跃式的增加，在笔记本市场，Type-C 也已经被广泛应用于各种新锐机型上

所以本篇文章的主要目的是帮助读者在便携储存领域向 Type-C 口以及更高速的接口协议完成外设迭代

* * *

## 目录 / 结构

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/835396c9-0fe2-4c80-a58e-6cb8efd0b7d7.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/308bbeb0-59fe-42a1-82d7-603afed6a462.png?raw=true)

超长预警

只准备买品牌产品的朋友请直接看结论就行了

想买硬盘盒 DIY 的请直接跳转到三，四，五，六按类推荐部分

想详细了解的可以全文通读

* * *

## **结论**

我知道很多读者对其他内容不感兴趣，所以先把总结放上来，想了解详细内容以及 DIY 选项的朋友可以跳过总结往下。

本文针对 USB 3.2 Gen 2 & Gen 2x2 与 雷电三 协议下的高性能（外接固态）方案进行推荐，至于机械硬盘，由于其自身性能的限制，并不需要对接口与主控有多考究，购买时只需要挑选外观讨喜做工牢靠，注意不要购买 USB2.0 外部接口的即可；

本文以桥接主控为单位，协议分组为大类进行推荐和排雷（这样组织文章的理由我会在最后的后记中阐述）。

各大厂商的常见桥接主控一览：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/351818d0-664f-4172-8747-3a82fa9ead93.jpeg?raw=true)

强调一下，我个人不会再提供购买咨询

目前（2020 年 12 月）

USB 外接储存最强方案是 USB 3.2 Gen2x2 转 NVMe，即以 ASM2364 为主控的固态移动硬盘，极速可达 20Gbps，成品有 西数 P50，希捷 FireCuda Gaming SSD；盒子有麦沃，乐扩等厂商的产品。

缺点是 USB 3.2 Gen2x2 并没有登陆笔记本平台，在桌面平台也没有普及，但可以通过 PCIe 扩展卡提供支持，而且也向下兼容 USB。

Thunderbolt 3 的最理想方案是以 Titan Ridge JHL7440 为核心的移动硬盘， 雷电 3 下极速可达 22Gbps，USB 下可达 10Gbps，相比过往的雷电外设胜在兼容 USB 接口。

成品有 Lacie Rugged SSD Pro（移动硬盘，兼容 USB Type-C 但不兼容 Type-A）

有支持 DIY 的方案但只有 PCB 售卖，没有带壳成品，支持 Type-A 和 Type-C 连接

Dock / 硬盘盒一体方案 比如佳翼雷电 3 mini dock 看起来很好，但要求雷电 3 链接且 必须外接供电，因此本文不纳入

看不懂主控的朋友请往下看

* * *

## 前置科普：

首先明确本文的硬盘盒定义：

_不需要外接电源的硬盘转接设备_

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/8fe726d7-575d-4bca-80d5-5e611c26805f.jpeg?raw=true)

因此这类外接坞尽管性能强劲，但由于便携问题直接 Pass 了

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/e8a17de8-6625-4d14-b65c-f635ba35ceac.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/ddb07266-9a1f-488e-851d-4474a79d407c.jpeg?raw=true)

而保留下来的是这类 _**外部接口→桥接主控（主控电路板）→内部接口**_ 这种比较简单直接的方案；尽管我们这里舍弃额外的供电，但外置电源有其价值，详情请见后记。

Q：为什么不选择 U 盘而是死磕移动硬盘  
A：由于 U 盘主控大部分不上心，导致 U 盘性能普遍不佳，而换做硬盘则可以有灵活的高性能的选择，从而改善体验。但在后续的帖子里你其实可以看到像 ChipFancier 这样的 U 盘外形，但内里还是转接 SDD 的方案

Q：为什么需要转接才能使用 SSD，SSD 不能直接连接 USB 吗  
A：硬盘主控并不提供 USB 协议的接口，因此需要桥接主控（Bridge Controller 下文简称主控）来将硬盘接口转接为 USB

Q：为什么帖子推荐的主控所连接的外部接口都是 USB Type-C  
A：两方面原因。

一方面，USB type-C 代表着更高性能的接口协议，比如 雷电三 ，以及 USB 3.2 Gen2X2 （USB3.2 Gen2 不是，它仍然支持 Type-A 口）

另一方面，更强大的供电。

USB Type-A 在 Battery Charge 1.2 规范下能够提供最大 5V 1.5A 的电力，而 Type-C 则能翻倍到 5V 3A；这是不挂载额外的电源管理方案前提下能提供的最大电力了。

供电能力即使对于固态也十分重要，这个问题我会在后记中补充

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/19b6ac43-0ef8-4d63-a738-738afcccb224.jpeg?raw=true)

图源百度百科，有一定偏颇，看看就好；常规硬盘盒能取到的最大功率是 15W（Battery Charge 规范）；于借口协议无关

* * *

**接口 / 协议科普：** 

传输数据本质上来说仍然是一种通信

我们所使用的 Type-C 接口，在通信活动中所扮演的角色远不止一个正反插的 USB 口那么简单，在接口之下，隐藏着规范与指导通信活动的更深层存在，我们称之为通信协议（以下简称协议）

引用一下该文章中关于接口与协议的比喻

接口：

> 人在用语言交谈时，需要用嘴说话，用耳朵听，通过空气来传播。信息的传递依赖嘴巴和耳朵来进行发送和接收。微机系统的通信，则利用电、光等媒介。最常用的是电，表现在数字电路中，就是高低电平的变化。单片机的 IO 口能实现高低电平的收发，认为它是一种通信接口。接口是通信所依赖的实体。

协议：

> 人在说话时，通过声带振动、口型的变化发出不同的声音。这些声音按照一定的规则，承载了我们所要表达的思想和信息，这套规则称为语言。两个人对话，需要使用两个人都能理解的语言进行，一个只懂中文和另一个只懂英文的人，根本没法用语言交流（当然可以用其他方式，比如面部表情、肢体语言等）。同样，微机系统通信时，也要有这样一套双方都遵从的规定，而这个规定被称为协议。通信协议和接口都可以有多种，并且两者之间存在一定的关联。

本文中，我们会将物理接口与通信协议 / 接口协议加以区分，我们所可以直观看到的接口，在这里我们称为面子，而规范传输与通信活动的协议，我们会称为里子

第一点我们所需要认识到的是，面子和里子（物理接口与接口协议）并不是绑定的

举个栗子

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/9384127d-c733-4518-8be1-03be1f4ba1d6.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/64fcb98b-f49e-453f-ad8f-efccfa7c6215.jpeg?raw=true)

你可以看到这款主板带有六个面子上是 USB 3.0 Type-A 外形的接口，但接口协议，也就是里子是 PCIE X1

它只能配合专用配件使用，如果你把 U 盘插上去是无法使用的，面子不决定里子

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/2ca5afe1-1aac-434a-996e-4845da67543e.jpeg?raw=true)

这种情况在协议复杂的 Type-C 接口下尤其需要注意，比如请勿将 USB Type-C 与 雷电 3（TB3）混淆，这事儿卖硬盘盒、转接器和线缆的 JS 可喜欢干了

* * *

下面正事

硬盘盒常规都会带有两个传输接口，内部的一个或多个用于连接硬盘，外部的一个则用于连接主机。

目前的消费级硬盘，绝大多数都归于两种接口协议（里子）下

一个是 SATA III （带宽上限 6Gbps）

一个是 Nvme (带宽上限 16/32/64Gbps)

企业 SAS 和远古 IDE 之类的这里不提

SATA 目前主要以 SATA III（6Gbps） 活跃于各类硬盘上，它的面子主要有三种形式

一个是 与自身同名的 SATA 接口，也最为广泛

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/b8fb83a6-2429-47b4-bbdb-03b951a62849.jpeg?raw=true)

一个是将自身小型化后广泛用于早期笔记本的 MSATA 接口

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/f702445f-900d-4240-a32c-83ffa08110b3.jpeg?raw=true)

最后一个则是适应接口发展和统一，从而发展成的 M.2 B&M key 接口（对应母口为 B Key）

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/9905629b-51a3-4fed-adf9-b0562e6fa011.jpeg?raw=true)

（大部分 B key 母口在储存类场景下仅支持 SATA 协议，而 M key 一般同时支持 NVMe 与 SATA，M key 与 B key 相对于边缘的距离是不同的，因此这个反过来插是要怼爆接口的）

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/f637fade-5827-433e-bb8e-6478a9559854.jpeg?raw=true)

豆知识：

SATA 全名 Serial ATA ，这个系列其实还有一位新锐大哥 SATA Express（简称 SATA-E），在 9 系主板大量出现过，曾被寄予厚望

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/52eec68c-4087-4170-b98c-25dfb64f1c27.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/43e7332d-60c4-466a-9686-e56216736290.jpeg?raw=true)

这个接口看起来是两个 SATA 的基础上再加点东西，似乎仍然是走 SATA 的老路，但其实是走的 PCIE 协议，理论带宽 PCIe 3.0 X2 （16Gbps），想不到吧（手动滑稽）

甚至，对于这个接口还有扩展为 PCIE 口的外设样品

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/66a32996-cf6f-490d-b2c2-1beb377dca6b.jpeg?raw=true)

可惜的是，由于其愚蠢的外形和其他的诸多原因，SATA-E 不出所料地被 M.2 干死了

豆知识结束，回归到 Nvme 部分，Nvme 的里子还是 PCIE 协议的变形与优化，目前市面上的 NVMe 硬盘能支持到 PCIE 3.0 X4 的带宽（上限 32Gbps，PCIe 4.0 为 64Gbps）尽管基本上跑不到带宽上限，但是传输速度是实打实的惊人；Nvme 的面子形式也主要有三种

一个是目前相对最普遍的形式，M.2 M key 接口

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/ae461fce-2395-4720-a0b2-32f2d630ed07.jpeg?raw=true)

一个是本家，PCIE3.0 X4 接口

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/19d293c0-4279-4dd8-ab74-eefe3c642e55.jpeg?raw=true)

最后一个多用于服务器，U.2 接口

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/5e66e75d-170b-4e1d-9b6c-2da4e6d834e1.jpeg?raw=true)

盘体尺寸为 15mm 厚 2.5 英寸对角

U.2 的另一个名称是 SFF-8639，它是最新的连接器设计，用于连接 MultiLink SAS 驱动器或 PCIe 驱动器（包括硬盘驱动器和 SSD 驱动器）。 SFF-8639 是 SFF-8680 的修订版，它是一个 29 针 2 通道 SAS 驱动器接口。SFF-8639 U.2 是一款 68 针驱动器接口连接器，具有更高的信号质量，可支持 12Gbps SAS 和 Gen 3 x4 PCIe 或 PCI Express NVMe。

引用自[https://www.mr-mao.cn/archives/mini-sas-introduction.html](https://link.zhihu.com/?target=https%3A//www.mr-mao.cn/archives/mini-sas-introduction.html)

可以看的出 U.2 规格的应用是针对企业市场的攻城略地

感谢 对 SATA Express 和 U.2 错误的指正

由于 PCIE 盘体积较大，U.2 盘较为少见，因此市面上的 Nvme 硬盘盒仅有搭载的 M.2 M key 的对内接口（并且盘体尺寸一般限制在 2280 以内）

M.2 接口虽然不是本篇的重点，但是这里推荐一篇有关 M.2 接口入门文章，感兴趣的朋友可以通过这个来了解，2280 之类的尺寸含义这里也有介绍

接下来是对外接口篇，会相对简略许多

eSATA，老雷电这种注定被干死的就不浪费篇幅了，FireWire 这种已经凉凉的也不谈了。

这里对外接口按里子分类仅有两类

一种是 USB3.2 (Gen1 带宽 5Gbps，Gen2 带宽 10Gbps，Gen2x2 带宽 20Gbps)

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/d47973ed-b33d-498d-a704-52b3dfad7bd4.jpeg?raw=true)

一种是 Thunderbolt （Thunderbolt 3 理论 40Gbps，实际数据带宽上限 22Gbps，Thunderbolt 4 实际带宽为 32Gbps）

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/4e30c686-0b51-409f-a152-4394f78010f7.png?raw=true)

USB 3.2 面子上有 USB Type-A，Type-B，Type-C，Micro USB 等形式，本篇以 Gen2 为主，因此出现最多的会是 Type-C，Gen2x2 仅支持 Type-C。各协议都可向下兼容

TB3 的标准面子有且仅有 Type-C

_上面的对内对外接口可以一对一任意组合，市面上基本都有产品_

* * *

**下面按照里子组合分为四大类推荐**

PS. 由于 TB3 的接口带宽远高于单个 SATA3 的带宽，因此市面上的对内 SATA 对外 TB3 的产品基本是磁盘阵列，体积大且要求额外供电，因此这里不对这个组合作介绍

## **对内 SATA 对外 USB3.2 gen2：**

代表：三星 T5（ASM235CM），ChipFancier（ASM1351） ，HP P600 等；

三星 T5（ASM235CM）：

三星 T5 算是一个经典的 Type-C 高性能移动固态方案；但该产品线已经更新到 T7，更快更薄更安全；  
三星 T5 在 Windows，Mac OS，以及 Android 平台都有软件支持，支持 OTG，支持 Trim

T5 其实也是很老套的硬盘加盒子的形式，这里附一篇 T5 的第三方评测：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/25ef8fd0-f10a-42f4-b635-ae2caf8dd4ea.jpeg?raw=true)

通过拆解可以看出，T5 其实也跑不出硬盘 + 盒子转接的结构模式

T5 是一个典型的 USB3.2 Gen 2 转 mSATA 方案  
硬盘采用的是和 850EVO 同款的主控 (S4LN062X01)，但缓存和颗粒都是升级过的。  
桥接（转接）主控为 祥硕电子 ASM235CM ，与推荐的 ASM1351 同为祥硕电子的第二代 USB3.2 Gen2 桥接主控产品。  
目前市场搭载 ASM235CM 的第三方转接盒子有但不多，比如高联的产品，但不能由三星的软件支持，下面 DIY 部分会提到。

详细评测请点链接，这里不作赘述

ADATA SE730H（VL716）：

同样小巧的 SE730H 方案，配备了由 VL716 提供的 USB3.2 Gen2 高速接口，内部同样是盘加盒的形式，与 T5 不同的是内部不是 mSATA 接口而是 M.2 B key 2242 规格。

SSD 方面主控为 慧荣 SM2285，颗粒是来自镁光的 TLC

详情见下方链接，不作赘述

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/ed3a3f32-e743-464a-ab18-ea58224c3c9a.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/e731eb5d-4171-4f88-91b9-8239aaddd728.jpeg?raw=true)

ChipFancier 的 Type-C 紧凑固态 U 盘方案（ASM1351）：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/2158b105-9b29-419f-a08a-a2fa4475ea78.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/07f44eb5-2527-4e5c-b407-783ec9253612.jpeg?raw=true)

较为老牌的外置固态方案（仔细看能够看到 ASM1351 桥接主控，那个最小的方形 IC）  
优点明显：结构非常紧凑，体积小性能强，无需线缆  
缺点也是：溢价，选择空间小

**DIY 选择：被广泛使用—— VL716 和 ASM1351**

**相对更为可靠一点但也更贵——ASM235CM**

盒子价格视做工和转接接口外形（面子）的不同有所浮动，一般都在 100 以内，ASM235CM 较贵，性能上没有很明显的提升

各品牌如佳翼，Orico，飚王，绿联等的 Type-C 盒子大多都基于这两款主控，购买时找客服确认一下即可；此外，主控固件版本也需要注意，搭载较老版本的固件可能会导致不支持 Trim 或其他使用问题  
实际速度取决于所安装的 SATA 协议固态，极速可超过 500MB/s

系列测评：

**VL716：** 

CHH 测评：[VL716 终于剁到\~\~~ 在 NVMe 普及的年代，淘汰 SATA 的好去处！！ - 电脑讨论 - Chiphell - 分享与交流用户体验](https://link.zhihu.com/?target=http%3A//jump.bdimg.com/safecheck/index%3Furl%3DrN3wPs8te/pL4AOY0zAwh2pvYgGHx5FtuUoJffNHpPjLhqcKnS33Fd6rQCSOXyNUpCt0T8Jxj3vreQz83O3gMifuBgolyc8Kzw1eauD7PIfpePPD1VPtMGykdtJZRSh8nFmVtNs9G8LVuAnaENGTDEBYcrz7ahgZF20VdLWm1ch2Pah4egTNMg%3D%3D)

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/8b73b507-24d8-433d-9d59-c877522257e7.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/f06d6a38-8fef-4623-a81b-092ee8d264a7.jpeg?raw=true)

**ASM1351：** 

ASM1351 的 DIY 产品较为稀少，往往是制成 U 盘成品（比如上面的 ChipFancier）后进行售卖，评测请参考其产品评价，这里不啰嗦了

**ASM235CM：** 

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/84acc1bf-9779-494b-8e59-342b91171e8c.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/d49b72da-e3ae-4d23-9c77-93cfe2ad1c83.jpeg?raw=true)

**JMS580：** 

目前该主控产品主要有 蓝硕 U23QC 和 佳翼 Q5CW 两款产品（感谢评论区 @一条咸鱼 补充）

我目前没有该主控的相关产品，也搜索不到评测，如果有玩过的朋友欢迎来分享体验

**这里需要额外插入一段排雷 ASM1352R：** 

装备写轮眼的朋友在总结的表内能注意到一款特殊标注的主控：ASM1352R

这是一款双 SATA III 协议口转 USB 3.2 Gen 2 的主控

支持独立双盘，RAID0，RAID1，JOBD 模式

然而实际在 RAID0 下的测试性能可能还会低于单盘

CHH 的一篇测试：[USB 3.1 SSD RAID0 小烟盒开箱退货记\~~](https://link.zhihu.com/?target=http%3A//jump.bdimg.com/safecheck/index%3Furl%3DrN3wPs8te/pL4AOY0zAwh2pvYgGHx5FtuUoJffNHpPjLhqcKnS33FYWD0y7uqKObyUwEWNqimdDreQz83O3gMifuBgolyc8Kzw1eauD7PIfpePPD1VPtMGykdtJZRSh8nFmVtNs9G8LVuAnaENGTDEBYcrz7ahgZF20VdLWm1ch2Pah4egTNMg%3D%3D)

我自己的简测：[【测评】ASM1352R USB3.1 双盘 RAID 方案开盒](https://link.zhihu.com/?target=http%3A//jump.bdimg.com/safecheck/index%3Furl%3Dx%2BZ5mMbGPAs95UolpNuq%2BCwM2BpViN6QF0BYrUCVRu9Im%2BN8qdQtn9vMxcX5g2/%2BJ%2B4GCiXJzwrPDV5q4Ps8h%2Bl488PVU%2B0wbKR20llFKHycWZW02z0bwtW4CdoQ0ZMMQFhyvPtqGBkXbRV0tabVyHY9qHh6BM0y)

积极评价的也有：[这硬盘盒居然能玩阵列！ORICO 双硬盘硬盘盒评测](https://link.zhihu.com/?target=http%3A//jump.bdimg.com/safecheck/index%3Furl%3Dx%2BZ5mMbGPAvCYqy0hOjXz/h2B8klIxU%2B/k8X9E5zfXMJA1iUnm/EJgkkSnRLVCReuixOaq7dNH8qhwm4WnvPP6Z7oG/S9Ri0J1VAfJ%2BMK42i%2BGTu0jeX9eIFhSN0NM2VjQnW1d0dPk/H3I7eyat8gi6j/LsAyfwgm5DJ7YwbhtYwPGbuJnYGNA%3D%3D) 关于 Orico 的 ASM1352R 双盘阵列测评

不过这玩意体积较大，不便携。感兴趣的朋友可以买一个玩玩。

问题可能在于文件系统，没有后续确认。

因为实际体验很差，虽然双 SATA 双盘十分吸引人，但是很不推荐

* * *

## **对内 NVMe 对外 USB 3.2 Gen2：**

**Caution：使用时请注意内置硬盘功耗并使用 Type-C 口以获得充足的供电**

上面的方案由于对内接口 SATA3 的带宽仅有 6Gbps，因此最高速度也就局限于此，希望更进一步开发 USB3.2 协议潜能的就可以看这个了

目前支持 Nvme 转 USB 的桥接主控有 **JMS583**、**RTL9210**和**ASM2362**，由于拆解很难收集到所以直接将三个合并起来介绍

此外，Realtek 还推出了 **RTL9210b** 主控，可以支持 SATA/NVMe 双协议（同时只支持其中之一）

额外提一嘴 JMS583，该主控下的 NVMe 盘主控似乎可以直接跟计算机通信，因此该主控备受 DIY 固态开卡人士的青睐，希望保留厂商原生软件支持的 用户也可以采用这个方案

先是成品部分：

**三星 T7（ASM2362）：** 

三星 T5 的后继者，三星 T7。在原有 T5 的基础上提升了接口速度，削减厚度，同时增加了指纹模块，对商务人士而言确实是比较诱人的升级

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/576ae013-9c2a-4415-bf05-2540600a153a.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/6db3d36b-28ff-449c-a44c-cd69a1dc8b85.jpeg?raw=true)

根据内构概念图可以看的出来，不同于 T5 的很大一点是颗粒板载，不再有动手改造的可能，T7 的三维确实不足以塞下一块 2280 盘，这点也在意料之中；所用桥接主控为 ASM2362，拆解可见 的 T7 拆解

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/77874f89-acd9-41e6-b3d5-4aad29bbbab3.jpeg?raw=true)

**闪迪 Extreme Pro：** 

注意是 Extreme Pro，不要跟 Extreme 那个搞混

虽然能找到拆解，但是仅指出了该产品是盘盒分离结构并且盘是 SN750 方案，并没有告知桥接主控型号，可惜，但是按照闪迪和西数的作风，应该也是 ASM2362。详情请阅读下方链接

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/cf5334bd-51cb-4e0a-b8b8-2db868579a68.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/2d12e02e-65cd-45a1-a80d-6060c25bac76.jpeg?raw=true)

新款的 Extreme Pro 已经支持 USB3.2 Gen2x2 20G 接口，内置硬盘 SN730

**Crucial X8（ASM2362）：** 

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/e17ee1a3-8082-4d29-bce7-dc8afe1b7ac8.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/62505389-d2d7-4c4f-a27e-a5fc58d99b26.jpeg?raw=true)

虽然 Crucial 现在在固态市场不算响亮，但比较早期的固态玩家应该知道这个品牌基本就是镁光亲儿子

根据拆解后的观察，可以看到也是盘盒结构，搭载硬盘基本确定是 Crucial P1 ；

SSD 主控是慧荣 SM2263ENG，缓存是 1G 镁光 DDR3，而颗粒是镁光的 QLC （wtf？）

桥接主控是 ASM2362，详见下方链接

成品还有其他的一些，比如联想拯救者之类的，但大多都是东拼西凑的组装货，这里不提了

下面直接 DIY 部分：

**ROG Strix Arion（ASM2362）：** 

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/d1f4953a-8749-4ecc-a4ee-957a1ed6f7bd.jpeg?raw=true)

这个产品目前在海外还是挺火的，我不确定是不是 ROG 大力跟各媒体人 PY 过；

产品价格目前大约 400RMB，跟常规的 JMS583 以及 ASM2362 盒子的 200 左右的价位有不小的差距，但也确实有独到之处

Advantage：

1.  无螺丝拆装，玩多了盒子就会理解这玩意对外部观感确实影响很大
2.  R G B
3.  这么便宜的 ROG 配件确实很适合充值信仰

Disadvantage：

1.  相对贵
2.  成也 RGB 败也 RGB，据说光效控制软件很烂；另外我这种人比较喜欢黑白色调，没有白色设定就比较劝退
3.  19 年 11 月才上市，太晚，WD P50 基本同期上市

随便贴一个 B 站的测评

NVMe 桥接方案其实出现最早的是 JMS583，后来 ASM2362 以及 RTL9210 才推出

目前各个牌子比如 ORico，绿联，飚王等都有产品

国内最早做这个产品的应该是佳翼

所以这里拿佳翼 JMS583 主控的转 Nvme X2 盒子做个典型

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/2e1bfe21-74c9-4b74-a997-36aca7bc0924.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/e21d84f5-d99f-40ba-ad5f-bd89babdfb89.jpeg?raw=true)

ASM2362 主控则是后来才推出的，因此产品较少

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/85f8398a-9d36-4487-8c17-b5298fa5bab3.jpeg?raw=true)

这两款主控都可跑到 900MB/s 以上的传输速度，但需自行挑选合适的 Nvme 固态

* * *

## **对内 Nvme 对外 USB 3.2 Gen2x2：**

**Caution：使用时请注意内置硬盘功耗并使用 Type-C 口以获得充足的供电**

Gen2x2 接口理论带宽 20Gbps ，追上了早期的半速雷电 3 是目前 USB 阵营最强的传输协议了

目前市面上仅有 ASM2364 主控支持这个方案的硬盘桥接

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/cc7c1bc0-646b-4e5d-8c86-a988e8c760d8.jpeg?raw=true)

而产品似乎也仅有 西部数据 P50 和 希捷 Firecuda Gaming SSD，以及闪迪新款 Extreme Pro，使用和 P50 相同的 PCB

DIY 方案和配套的相对便宜的 Gen2x2 母口扩展卡（ASM3242）都已经陆续出现不少，不过似乎由于价格原因并没有像我预期的那样很快把 Gen2 口的方案淘汰掉。

西数 P50 拆解和评测信息请见我自己的文章

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/ae47cb87-06bf-42db-acb7-ac55812fbcd9.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/774c3211-19a0-4253-abb2-50c22cd0ed02.jpeg?raw=true)

DIY 方案：

硬盘盒有麦沃，Orico 等品牌，价格都在 200 元左右

PCB 设计都要比 Gen2 的几个方案肥硕一些，因此 DIY 盒子的体积都要比先前的大

没有 USB 3.2 Gen2x2 的计算机也可以通过购买 PCIe 扩展卡来提供支持，早期的技嘉扩展卡价格昂贵且渠道稀少，目前国内卖该接口扩展卡的主要也还是麦沃和 Orico，都是 ASM3242 方案。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/a23d392e-b336-4084-be4c-2ae15e898e89.jpeg?raw=true)

* * *

## 对内 NVMe 对外 Thunderbolt 方案（新兼容型）：

目前新的兼容型雷电方案有且仅有**JHL7440** 一种

不同于上文介绍的 USB to NVMe 只能通过 USB 协议链接

也不同于下文 旧 thunderbolt 3 to NVMe 只能通过 Thunderbolt 协议链接

JHL440 在雷电接口上可以 以雷电 3 协议进行链接，在 USB 接口上可以以最高 USB3.2 Gen2 协议进行链接，为同时追求强 IO 性能和兼容性的用户提供了十分理想的选择，可惜的是它同时也继承了雷电方案的部分缺点

Advantage：

-   强兼容性（雷电 3 & USB3.2 Gen2）
-   强性能（雷电 3 最高 22Gbps 数据传输带宽，高 4k 性能，低延迟）

Disadvantage：

-   较昂贵（雷电设备一贯的特点）
-   体积较大（器件较多导致成品体积相对 USB 方案较大）
-   非一致性（不像 USB 那样的单 IC 解决问题，雷电口需要多 IC 共同构建，不同厂商使用的不同设计和固件会导致兼容性和性能的差异）
-   出生太晚（雷电 4 32Gbps 的数据带宽支持近在眼前，雷电 3 已进入淘汰倒计时）

成品：

**LaCie Rugged SSD pro：** 

Caution：该产品只支持 Type-C 接口的 USB 连接，Type A 母口不支持

评测和拆解链接见我的测评：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/81526e81-f43c-4cf7-b162-cfa684793cab.jpeg?raw=true)

DIY 方案：

JHL7440 硬盘盒配件

支持 Type A 和 Type C 的 USB 连接

目前仅有 PCB 售卖，并没有其他品牌包装成成品

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/de477b15-b6a8-444b-ab3b-1b18c68a8489.jpeg?raw=true)

## **对内 NVMe 对外 Thunderbolt 方案（旧）：**

_Caution：不支持 USB 主机口_

_Caution：不支持 USB 主机口_

_Caution：不支持 USB 主机口_

Thunderbolt 3 的主控有且仅有 Intel 自己在研发生产，而且由于这里只着眼于在硬盘盒上的应用，因此尽管型号不少，但基本没啥区别，而且都不支持在 USB 协议上运行（Titan Ridge 系列存疑）  
所以这里不纠结主控型号直接进入产品环节了（反正都是牙膏厂的芯）

典型产品：三星 X5（主控不明），HP P800（JHL6340）

**三星 X5：** 

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/e24fffb2-59de-45e1-96ce-6eb5bd953065.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/84f5f369-0e42-4395-852e-bbefbe33bacc.jpeg?raw=true)

三星 X5 是今年（2018 年）八月底发布的产品，算是很年轻的产品了

感谢评论区 提供的 X5 拆解评测

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/2e57df60-d896-4aa0-82dc-c8353e10ff10.jpeg?raw=true)

具体参数和评测这里不多介绍，请参阅三星官网和各评测媒体

**HP P800（JHL6340）：** 

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/a81e5927-90ec-4aba-bc00-8cca6357c272.jpeg?raw=true)

我个人的评测可以翻我的的文章列表

这里直接贴一个 Chiphell 的评测：

[提示信息 - Chiphell - 分享与交流用户体验](https://link.zhihu.com/?target=http%3A//jump.bdimg.com/safecheck/index%3Furl%3DrN3wPs8te/pL4AOY0zAwh2pvYgGHx5FtuUoJffNHpPjLhqcKnS33FYVcHbZIuGtcljo7oysIWibreQz83O3gMifuBgolyc8Kzw1eauD7PIePVh3pd1btBTfnMy395mKr5qn/Aj2Opic%2B%2B10%2BBh3SpX%2BN8RX/2RTBejUblHq5typ2Pah4egTNMg%3D%3D)

免驱支持 Mac OS 以及 Windows  
这款移动硬盘可更换内置硬盘，且内置硬盘性能不佳，所以比较推荐自己买最低配的后自行更换高性能大容量盘

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/5fd67d59-8493-421e-a7ca-9765593ad99a.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/35d82d35-68c5-4165-a6b5-23eb9e8a5def.jpeg?raw=true)

（可以看到雷电三线缆和内置硬盘都是可更换的）

其他 DELL，LaCie 之类的产品就不多作介绍了，评测一搜就有

随后是例行的 DIY 产品环节：

这里就偷点懒直接找马云找答案了，海外玩家基本没有其他的方案

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/f3de746d-f82d-4b85-908d-90e496d6d162.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/f1c6a9b0-2c44-481c-bf67-687fa645528b.jpeg?raw=true)

排除掉磁盘阵列等搜索结果，TB3 转 Nvme 的只剩下佳翼的与一个高频出现的盒子，两个方案

关于比较多见的貌似是 LT-Link 推出的 TB3 方案：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/5f92d05d-a4d9-462c-9d56-441d3d23e608.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/4ab332dd-6faa-406a-8452-b01dc23f33c0.jpeg?raw=true)

你可以将它与上方的 P800 进行比较，会发现 PCB 的走线和器件位置是完全一样的，甚至连外形都是一样的，啥意思我不多说啦。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/fb766f8a-7c8d-43ba-a723-ee2921b8b365.jpeg?raw=true)

而佳翼的方案是在 18 年 11 月 1 日上架的，方案总体上也和 P800 类似，但部分布局和走线上确实有所不同

所以买 DIY 产品还是买品牌产品比如 P800 是一个仁者见仁智者见智的问题，LT-Link 盒子价格 1000 出头，跟 HP P800 拉不开差距，而佳翼的目前也涨到 1000 出头的价位了，看起来 DIY 产品似乎反而缺少性价比（笑）

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/58caf443-03d9-4bf4-a4e1-7a1d00c935a2.gif?raw=true)

顺带一提，雷电三硬盘盒的一个有趣玩法：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/010aa0a8-4d45-403b-9559-93ba4d25c056.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/2ca831c4-9f6c-44d5-8497-27924777c6e5.jpeg?raw=true)

目前有雷电三硬盘盒配合 M.2 外接显卡部件进行外接 PCIE 设备的玩法，我个人已经测试过使用 P800 外接显卡在 Windows 和 Mac OS 下都是可行的，外接英特尔 750 也可行，但需要换成一般的转接设备；这两个配件的价格加起来相比入门级雷电盒子比如 Akitio Node 没有优势，功能上除了可以拆分使用外也没突出之处，有兴趣的朋友可以弄一套把玩把玩

这套设备在 [egpu.io](https://link.zhihu.com/?target=http%3A//egpu.io/) 也有自己的词条，只不过估计少有像我这样替换 TB3 转 M.2 配件的

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/1d8bc402-9571-4b8c-a68d-29ceb14f86bf.jpeg?raw=true)

* * *

**主控整理：** 

这次新增了 Realtek 和 Intel 部分的主控，USB 桥接主控龙头仍然是先前的三家厂商的产品，分别为 VIA（威盛电子），JMIcorn（智微电子），以及 ASMedia（祥硕电子）  
很凑巧，都是台湾厂商。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/b6674c95-8bad-422f-bf3c-fc37417f2d22.jpeg?raw=true)

VIA 和 ASMedia 是早在 USB3.0 混战时期就开始为计算机主板提供 USB 桥接主控的老牌厂商，后来则是英特尔一统天下；JMIcorn 也是老牌厂商之一，不过此前重心应该不在桥接主控  
有趣的是如今英特尔强推 TB3，只生产兼容 USB3.2 的 TB3 主控而不推出独立的 USB 3.2 主控，目前的 USB3.2 设备主控市场仍像是 3.0 混战时期由第三方所占据，然而主机控制器基本完全被 ASMedia 把持（USB 3.2 Gen2 主机控制器 ASM1142，Gen 2x2 ASM3242）

三家基本上是目前盒子主控的龙头，其他也还有几个不错的方案，主要是 Realtek，在网卡领域压倒性的那个小螃蟹

（浅绿色为支持，淡黄色为不支持）

很遗憾 VIA 官网没有一毛钱信息是关于桥接主控的，这里只放一个 PPT 了；VL720 至今仍未面世。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/73a31d8d-9cbb-42fb-8428-1a0936aad26a.jpeg?raw=true)

以上，就是本文的主要内容了

* * *

## **补充 / 后记：**

USB 插旧雷电三设备（JHL6340）我已经替各位测试过了，连 VCC 指示灯都不亮 ，系统内无反应

主板 Z270G，自带 C 口 USB 3.2 gen2

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/f5a49cd0-f0fe-4c89-ac9a-f26ec067bbe0.jpeg?raw=true)

**USB 接口的供电对于固态硬盘重要吗？**

由于 USB 进入 3.0 时代后供电能力持续增强，以及人们对于固态硬盘低功耗的固有印象，在固态移动硬盘这个场景下很多人（包括我）都会不把功耗当回事，但其实是会埋下隐患的。

我们挑个案例

以该文章对 3.5 英寸机械硬盘的测试（读写功耗稳定在 7W 左右）为参照

同为 SATA 3 接口的英特尔固态 S3710 的活动功耗高达 6.9W

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/5edfcb4a-e08a-42ef-aee5-bf908e8a6121.jpeg?raw=true)

而搭载 MLC 颗粒的 NVMe 盘功耗更为爆炸，1TB 版 970Pro 的平均功耗可达 5.7W，峰值达到 8.5W

英特尔的 3D XPoint 颗粒在功耗上更是是不甘示弱

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/b65729cb-412e-4198-a510-e1dbbf7dfa6f.jpeg?raw=true)

这时回头看看接口供电，这谁顶得住啊

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/12c6b359-06ef-4e7b-ba44-41c9704511f6.jpeg?raw=true)

不过也没必要过于担忧，一众 TLC 盘则在功耗上好看的多，以 760P 为例：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/fd302092-83bf-471b-9b98-1c238f253f5c.jpeg?raw=true)

所以，越是在高性能的应用场景下，供电越是需要纳入参考，不想折腾这些的拿 TLC 或者 QLC 盘装上就是了

此外，MLC 颗粒也偏向于耗电大户，我手上信息有限，只能找到一个 Tom's hardware 很老的测试供大家参考

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/a56766d3-0832-4c3a-ac2e-d331e7182c13.jpeg?raw=true)

**再往下则是关于本文组织形式：** 

Q：为什么帖子要以主控为单位而非以实际产品为单位进行介绍与推荐？

A： 原因之一是产品众多，核心部件制造门槛低；我个人没有足够的精力和时间进行逐一评测，测出来也过于庞杂

原因之二是因为产品同质化严重

以我手头的 M.2 盒子与 ORICO 的 SATA 盒子为例  
二者都使用的是同款主控 VL716

M.2 盒子：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/3f9a637c-d5a9-4dd3-8ba3-ee13891f3e71.jpeg?raw=true)

ORICO SATA 盒子：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/953b14c1-b279-4181-99de-8a74fb5fe09f.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/b3dd9a46-2fac-4c44-9126-5db84dcf58fc.jpeg?raw=true)

二者结构都很简单  
除了主控以外的功能 IC 都仅有一颗贴片 FLASH，用于储存信息  
其余的包括晶振电路和供电回路只要照抄厂商的手册就可以，绝大部份的功能都已经由厂商集成到主控中去了  
能够由下游制造商所影响的电气性能实在有限  
因此，在主控相同的前提下死磕不同品牌的产品性能差异完全没有必要，着眼于外形设计还有价值一些。  
而外形这种主观判断也不需要我说三道四，因此最后选择以主控为中心进行介绍，列举部分典型产品作为代表的组织方式

Q：为什么分类时以协议（里子）组合进行分类，而非按接口（面子）组合分类？

A：与上面的缘由类似，一方面接口组合过多，很多特性重合  
另一方面同协议下的不同接口差异很小，转接成本普遍较低

以 NVME（PCIE）为例  
PCIE 转 M.2 M Key 结构简单且十分便宜（你的手要是拿得住的话，说不定可以徒手持线转接）

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/a66d8978-b4fd-4f47-989d-828056ecbd69.jpeg?raw=true)

转回去的也是同理

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-06-59/018846a0-3a9d-4691-9cb0-0211ed1e760f.jpeg?raw=true)

尽管有部分例外，比如 U.2 转接设备稀少且昂贵；只支持卡片式固态的 MSATA，M.2 B Key 由于不包含完整的 SATA 供电定义，仅能转接成 SATA 数据口而非包含供电的完整接口  
但是在硬盘领域核心功能是相同的，因此本帖中合并并简化到接口协议进行讲解 
 [https://zhuanlan.zhihu.com/p/58257546](https://zhuanlan.zhihu.com/p/58257546)
