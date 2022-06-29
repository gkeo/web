# 蜗牛星际折腾黑群晖（完善） – Isrey
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-10-42/15703b51-f509-4d4f-9f90-56b514b715ff.jpeg?raw=true)

自从给蜗牛装好了群晖以后，感觉腰也不痛了，腿也不酸了，一生气就能直接上顶楼了。以前没用过 NAS 这类私有云的产品，用了之后确实感觉非常方便，再也不需要在家拿个 U 盘到处跑。用三块硬盘组了 RAID5 以后存放资料在黑群晖里面也还是比较放心的。

但是黑群晖的终究是黑的，为了更加完美折腾是必不可少的。但是好在 DSM 的通用型很强，毕竟是 LINUX 的底子，装好以后什么不修改都基本能正常使用，本着不折腾不舒服，越折腾越不舒服的理念，还是要把这台机器折腾成理想的状态才罢休。网络的力量是无穷的，正因如此黑群晖的群众基础也特别强大，所以很多小问题早就已经发现并解决了。

在爬了很多论坛的楼，翻阅了很多资料以后，经过一番研究，现在装的这台蜗牛群晖也算是基本满意了。站在巨人的肩膀上，解决了以下几个问题，顺便记录下完善的过程，安装的过程可以看前面的[【这篇文章】](https://isrey.com/archives/112/)。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-10-42/749a7404-e017-4eda-82c5-0458290369a7.png?raw=true)

_已经装好的是 DS918+（6.2.0）的系统，U 盘引导用的是 Jun1.04b；_  
_目前 DDSM 半洗白，可以转码显示视频封面，硬盘休眠与网络唤醒均可，硬盘位顺序正常，处理器显示为 J1900 信息；_

### 一、硬盘位的顺序

首先说一说这 A 款主板的 SATA 控制器：  
这款 A 款蜗牛有两个 SATA 控制器，有 6 个 SATA 接口（包含一个 mSATA 接口）。处理器控制 2 个能引导的接口（内存旁边的一个和 mSATA ），板载控制器控制 4 个硬盘架的接口但不能引导。

装好 DSM 后硬盘顺序应该是处理器控制的两个接口在前（假设为 1、2），控制硬盘架上的四个接口在后 (假设为 3、4、5、6)。所以只要是放在硬盘架上的硬盘在 DSM 都会标识在 3 号到 6 号盘之间。

若需要将硬盘架上的顺序改为 1、2、3、4 号标识，可以修改引导盘里的`grub.conf`配置文件来实现。  
修改盘序号需要在`extra_args_918`变量里增加两个值`SataPortMap=24`和`DiskIdxMap=0400`。  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-10-42/3e2ae845-12ea-4701-938d-a02e37c7f58a.png?raw=true)

即：

修改完成后保存重启，我的硬盘是自上而下放在下面两个盘位中的所以是 3 号和 4 号位。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-10-42/e679a994-6a0f-4aa1-989d-3dd1f2f8f13c.png?raw=true)

如果盘位顺序还是有误，需要把主板连接的 SATA 物理更换一下，我直接是主板接口上下交换位置就正常了。  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-10-42/4ab7be05-ffab-4727-98cd-4f11f19e7b8f.png?raw=true)

简单解释下这两个值：  
具体的含义可以[~【参考此处】~](https://github.com/evolver56k/xpenology/blob/master/synoconfigs/Kconfig.devices#L229)的第 229 行和 249 行：

> `SataPortMap=24`  
> 大意是配置系统有两个 SATA 控制器，第一个控制器有 2 个接口，第二个控制器有 4 个接口；
>
> `DiskIdxMap=0400`  
> 大意是将第一个 SATA 控制器的接口序号设置为从 5 开始，第二个 SATA 控制器的接口号从 1 开始（04 和 00 都为 16 进制）；

### 二、用 SSD 引导后隐藏启动盘

直接把启动镜像写入到 mSATA 盘里面，存储空间管理员里面会有一个 14G 左右的盘始于未使用状态，就是 mSATA 盘里除开启动分区后的剩余空间，像下面一样。  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-10-42/cd95c3ff-c690-42ea-8cfa-e25d9923579a.png?raw=true)

可以将其初始化并利用起来，但 14G 的空间利用起来也没什么价值，且本来自带的 SSD 就很弱，用来存资料也有一定崩盘的风险。为了防止看着碍眼，可以用上面的方法把这个盘隐藏掉。

还是需要通过修改引导盘里的`grub.conf`配置文件来实现。  
需要在`sata_args`变量里增加`DiskIdxMap=1000`这个值，且在启动时选择第三项启动项（VMware/ESXI）启动。  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-10-42/0b72c3d8-a674-4a73-9fb1-821297d501dc.png?raw=true)

这样修改的目的是将处理器的 SATA 控制器的接口号改为从 16 开始，第二个控制器的接口号改为从 0 开始。这样刚好可以把 SSD 排除在可以显示的范围外。

即：

本来 918 的盘位是 4 个，将接口号改成 5 开始就可以隐藏，但改成 16 而不是 4 的原因是 Xpenology 论坛里的老外说法是 Jun1.04b 的引导将 918 的盘位修改为了 16 个貌似是可以让启动速度更快什么的。

用 mSATA 盘引导在启动时最好选择第三项启动项（VMware/ESXI）启动，看了这位值友的[【文章】](https://post.smzdm.com/p/alpzllno/)后参考了`grub.conf`文件，用下面的代码比较好解释：  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-10-42/143ce185-ace0-4896-870d-221fad9e636c.png?raw=true)

大概的理解为当选择第三项（VMWare/ESXI）传给`loadlinux`函数的`bootdev`的参数是`sata`，从参数命名上来看更加适合用 mSATA 盘来启动，如果用 U 盘启动则选择第一个启动项更合适，因为第一项的值是`usb`。只是从参数命名上来看，并没有更深的探究；  
实测如果使用 U 盘引导选择第三项启动项之后，在 DSM 中在信息中心的外置设备中会显示 U 盘信息。

### 三、信息中心显示的处理器的型号

装好 DSM 系统以后，信息中心显示的是白群晖机器的处理器信息，比如 DS3617 系统就显示的是 Xeon D 处理器的信息，很明显是直接写死的，看上去很不爽。  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-10-42/30b7d74e-28c2-4b50-95a8-30fb98ef0187.png?raw=true)

蜗牛用的是 J1900 的处理器就老老实实显示 J1900 的信息就好了。翻了下 xpenolgy 论坛，[这位韩国小哥](https://github.com/FOXBI)已经给出解决方案了，这里搬运一下，原文可以[看这里](https://xpenology.com/forum/topic/13030-dsm-5x6x-cpu-name-cores-infomation-change-tool/)。

1.  下载`ch_cpuinfo_en.tar`在电脑上，[【这里下载】](https://github.com/FOXBI/ch_cpuinfo)；
2.  通过 FileStation 将下载好的文件上传到 DSM 上；
3.  用 Putty 或者其他 SSH 工具连接上 DSM；
4.  在 SSH 工具中操作；

最后信息中心显示如下信息：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-10-42/da501cfb-e7b2-4256-8ee5-7d8231131fec.png?raw=true)

折腾完毕！

`* 图片：凤翔湖` 
 [https://isrey.com/archives/129/](https://isrey.com/archives/129/)
