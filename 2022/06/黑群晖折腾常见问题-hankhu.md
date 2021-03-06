# 黑群晖折腾常见问题-hankhu
**1**、**问：黑群晖是否需要一个 SSD 当做系统盘？**

答：群晖系统跟 Windows 不同，Windows 有个盘要当成系统盘，而群晖会在每个硬盘上自动安装系统。每个硬盘？对，没错，就是每个硬盘。比如你是 6 盘位，接了 6 个硬盘，这 6 个硬盘初始化以后，每个硬盘都有系统了。所以拿一个 SSD 来做系统盘的这个做法没必要。

**2\*\***、问：\*\*   **我安装黑群晖到这一步就卡住不动了，怎么办？**

[![](https://www.hankhu.com/wp-content/uploads/2020/11/word-image-1.jpeg)
](https://www.hankhu.com/wp-content/uploads/2020/11/word-image-1.jpeg)

答：6.0 以上的黑群安装，做好引导盘启动后，屏幕上显示这个界面就不用管它了，到电脑上用群晖助手搜索后进行下一步的安装操作。

**3\*\***、问：安装过程出错（比如错误 \***\*13/21/35/38\*\***），怎么办？\*\*

答：群晖在安装过程中出错，有时屏幕提示的信息与实际出错的原因并不符，比如错误 13 实际是因为没修改镜像文件的 U 盘 vid 和 pid 信息导致的。总结一下需要注意的地方：

（1）用 U 盘做为引导盘的，需要用 ChipGenius（芯片无忧）读取出 PID 和 VID，然后写到引导文件，需要注意的是：ChipGenius 读取出 PID 和 VID 是 4 位数值，引导文件里面的 0x 需要保留不能删除，只需要修改 0x 后面的 4 位数值就行了。否则就会报错 13。如果用 SSD 作为引导，则不需要修改 PID 和 VID。

（2）正式安装之前，需要检查引导版本和系统安装包的版本是否匹配，不要刷了 3617 的 1.02b 引导去安装 6.2 版本或者刷了 3615 的 1.03b 的引导去安装 3617 的安装包，否则有可能报错 21。正确的做法是：3615 和 3617 的 1.02b 引导安装对应是 6.17 的安装包，3615 和 3617 的 1.03b 引导安装对应是 6.2 或者 6.21 的安装包，918 的 1.03a2 引导只能安装 918 的 6.2 版本，918 的 1.04b 引导可以安装 918 的 6.2 和 6.21 版本。

（3）群晖的引导盘和数据存储盘是分开的，所以安装之前要把硬盘挂上，否则安装过程找不到硬盘就会报错 38。

（4）群晖的数据存储硬盘，不需要事先进行分区与格式化，如果你的硬盘已经分区和格式化，安装过程中就会报错 35：磁盘无法格式化。用 DiskGenius 把每个硬盘的分区全部删除即可。

**4\*\***、问：群晖装了 \***\*8G\*\***内存，可用内存还有 \***\*4G\*\***多，想分配 \***\*2G\*\***安装个 \***\*Win7\*\***虚拟机，为什么群晖的 \***\*VMM\*\***虚拟机提示内存不足？\*\*

答：出现这个提示，并不是你的内存真的不足，真正的原因是：你的 CPU 和主板不支持虚拟化，或者是支持虚拟化的硬件，但是 BIOS 没打开虚拟化相应的选项。

**5\*\***、问：为什么用 \***\*hyper-v\*\***安装群晖任意一个型号的 \***\*6.2\*\***都不成功？\*\*

答：因为 hyper-v 虚拟机系统只能安装群晖的 6.0 以下版本。

**6\*\***、问：为什么安装 \***\*918\*\***都找不到 \***\*IP\*\***？\*\*

答：不是所有的硬件都可以安装 DS918+，主要是网卡不支持。

**7\*\***、问：群晖 \***\*3615\*\***和 \***\*3617\*\***为什么升级安装到 \***\*6.2\*\***以上的版本就找不到 \***\*IP\*\***了？\*\*

答：因为目前 3615 和 3617 的 6.2 以上版本，不能用 UEFI 方式引导，需要用 legacy 方式（即：传统 BIOS 方式）引导，进到 BIOS 去改引导方式就可以了。如果是从 6.17 升级到 6.2 以上版本，需要更换 1.03b 的引导并改用传统方式启动，否则也是找不到 IP 的。

**8\*\***、问：\*\* **DS3615xs\*\***、\***\*DS916+\*\***、\***\*DS3617xs\*\***、\***\*DS918+\*\***这几个型号有什么区别？\*\*

答：白群 DS3615xs、DS916+、DS3617xs、DS918 + 这四个型号后两位数字代表产品上市年代（分别是 2015 年、2016 年、2017 年和 2018 年上市），前面的数字代表最多允许接入的数据存储盘位（分别是：36 盘、9 盘、36 盘和 9 盘）。其区别在于：DS916+/DS918 + 是定位家用（有硬件解码功能，硬盘组合默认用 SHR 方式），DS3615xs/DS3617xs 的定位是企业用（没有硬解功能只能软解，硬盘组合默认用 RAID 方式），DS3617xs 是 DS3615xs 代替产品、DS918 + 是 DS916 + 代替产品。但是黑群安装用哪个其实区别不大，都差不多。根据硬件支持安装对应的型号就好了。

**9\*\***、问：黑群和白群有什么区别？\*\*

答：先讲一下黑群没洗白、洗半白、洗全白的区别  
（1）黑群没洗白：不能注册和登录 QuickConnect（简称 “QC”，就是没有公网的用户利用群晖服务器进行内网穿透）；DS Video/DS Photo/Moments 这三个套件的预览图是黑的；DS Video 看片时视频质量的高、中、低码率不能自由切换只能选择原码；DS Video 不能离线转码（群晖默认不能播放 RM/RMVB 等文件格式，以前网上下载的影片多为此类格式，所以有时需要转换成能播放的 MP4 文件格式）。  
（2）洗半白：DS Video/DS Photo/Moments 预览图正常；DS Video 的视频质量高、中、低码率可以自由切换；DS Video 可以离线转码。但是不能注册和登录 QuickConnect。  
（3）全洗白：以上的限制全部都没有啦~

在引导文件输入正确的 SN 和对应的 MAC 并且安装与 SN 对应的系统后，即全白；如果只有正确的 SN，MAC 对应不上或者安装的系统与 SN 不对应，这个只能算白半。

**10\*\***、问：右上角老是提示 \***\*”\*\***编解码器未成功激活 \***\*”\*\***，怎么办？\*\*

答：到套件中心安装多媒体服务器和 FFMPEG，重启群晖后就正常了。

**11\*\***、问：群晖怎样升级到 \***\*6.2\*\***以上版本？\*\*

答：目前 DS918 + 用 1.04b 的引导可以升级到最新版本，DS3615xs 和 DS3617xs 用 1.03b 的引导可以升级到最新版本。能升级的前提是你的网卡支持最新版本。

**12\*\***、问：你们都装的群晖的哪个版本？\*\*

有用 5.0 的吗，使用体验怎么样，稳定吗？  
有用 5.2 的吗，使用体验怎么样，稳定吗？  
有用 6.0 的吗，使用体验怎么样，稳定吗？  
有用 6.1 的吗，使用体验怎么样，稳定吗？  
有用 6.2 的吗，使用体验怎么样，稳定吗？  
有用 6.21 的吗，使用体验怎么样，稳定吗？

答：经常会有人问这个[问题](https://www.hankhu.com/tag/%e9%97%ae%e9%a2%98/ "查看更多关于 问题 的文章")。官方出的每一个版本，分为正式版（安装文件链接在：[https://archive.synology.com/download/DSM/release/](https://www.tenlonstudio.com/go/445k)）和测试版（安装文件在：[https://archive.synology.com/download/DSM/beta/](https://www.tenlonstudio.com/go/67rk)），一般来讲，正式版都是稳定的。对于黑群来说，不折腾都是很稳定的，系统安装好了以后丢到角落，长期运行不需要理它，一般都不会有问题。如果你喜欢折腾，比如经常安装这个、卸载这个、升级那个等等，这个就不好说了，也许一个不经意的动作之后，系统就崩了。

**13\*\***、问：群晖的 \***\*Download Station/aria2/uTorrent/rTorrent/Transmission/qBittorrent\*\***用来下载 \***\*BT\*\***种子为什么没有速度，换成迅雷就可以满速，或者换成百度网盘就可以秒下？\*\*

答：我们首先了解一下 BT 下载的原理：BT 是仅仅通过 P2P（点对点）的方式进行下载，参与上传（俗称做种）的人数越多，下载速度越快。群晖的 Download Station/aria2/uTorrent/rTorrent/Transmission/qBittorrent 只是一个下载下载工具，下载速度快不快，取决于几点：（1）你的网络是否是公网；（2）是否在光猫 / 路由器里面设置了端口映射（有些路由叫做端口转发）。如果你的宽带是大内网，那么你只能连接到跟你同样在大内网的用户。只有你有公网并且开了端口，其他人才可以通过 P2P 找到你，你也可以找到更多的人。另外，迅雷和百度都有自己的文件缓存服务器，只要你想下载的文件，在服务器是存在的，除了 p2p 的下载方式以外，开了他们家的会员，还可以享受服务器的下载加速。因此，为什么你会觉得迅雷和百度可以秒下的原因了。

**14\*\***、问：我现在安装好了群晖系统，以后万一主板坏了换一个主板或者换了内存，是否要重装系统？\*\*

答：正常来讲，群晖安装好以后更换了硬件（包括：CPU，主板，内存这三大件），是不需要重装系统的。当然可能也会有点存在特殊情况，比如你本来在 intel 的 CPU 安装的是 DS916+/DS918 + 正常使用，换到某些 AMD 平台的 CPU，有可能就不认了，引导找不到 IP 启动不成功。

**15\*\***、问：原来安装的版本是 \***\*6.2\*\***，不小心点了升级，结果进不去了怎么办？\*\*

答：由于群晖不能直接降级安装，所以不可以直接安装回原来的 6.2 版本。解决方法：（1）物理机：用一个新的 U 盘刷一个 “PE” 系统，用刷了 PE 的 U 盘去引导黑群，然后进到 DG，把黑群上每个硬盘的第 1 个分区格式化一次。然后把黑群晖引导 U 盘放到电脑上，用 “U 盘还原工具” 先还原空间，重新刷写 6.2 的引导文件。刷好以后就可以去按照全新安装的方法安装回 6.2 版本了。原来硬盘的数据都在。（2）虚拟机：挂载一个 “PE” 的 ISO 文件，从 ISO 引导进到 PE，把每个硬盘的第 1 个分区格式化一次然后退出。把原来的引导文件删掉，同时新建一个引导文件，刷回 6.2 版本的引导，然后按照全新安装的方法安装 6.2 版本的系统。

**16\*\***、问：听说用 \***\*U\*\***盘引导不能休眠，能否把引导写到硬盘，做成引导和数据同在一个盘上？\*\*

答：从技术上来讲，把引导和数据做到一个盘上是没有问题的。但不建议这样做，因为这样做的后果：（1）以后不支持升级；（2）万一引导坏了，需要重新引导，操作起来比较麻烦，而且风险还很大：比如一个不小心的操作，把引导文件写到了存入数据的分区中，全部数据就没了。而用 U 盘做引导的话，引导坏了直接重新刷 U 盘，或者换个新的 U 盘，对数据一点都不会影响。其实 “U 盘不能休眠” 的说法是不准确的，能否休眠，要看你的硬件是否支持，另外还要看你是否安装了影响休眠的套件。

**17\*\***、问：黑群设置了休眠是不是比较省电？\*\*

答：理论上休眠是比不休眠省一点电，其实硬盘还是很智能的，你如果不设置休眠，它不读写数据的时候，磁头是远离磁盘的，反而你开启了休眠，硬盘会频繁的起停，很伤硬盘。因此，建议大家不要在休眠的问题上太纠结。

**18\*\***、问：黑群可以远程访问吗？\*\*

答：可以远程访问，但是需要一定的条件（满足任意条件均可）：

（1）有公网 IP，设置了 DDNS，并且设置相应的端口映射（有些路由里面称为 “端口转发”）；

（2）没有公网 IP，开通某些内网穿透的服务（比如：FRP/Ngrok / 花生壳 / 内网盒子 /...... 等等）；

（3）已经洗成全白，通过 QuickConnect 地址访问。

**19\*\***、问：黑群可以远程唤醒吗？\*\*

答：可以远程唤醒，但是需要一定的条件（必须同时满足以下所有条件，缺一不可）：

（1）硬件要支持唤醒，BIOS 已经打开相应的设置；

（2）获取到网卡真实的 MAC 地址，并且引导文件的 MAC 地址与网卡真实的 MAC 地址一致；

（3）允许群晖使用网络唤醒功能（群晖控制面板，电源设置，网络唤醒打勾）；

（4）设置群晖网络唤醒（用群晖助手设置 WOL）；

（5）给群晖在路由器里面设置固定 IP 并开启 ARP 绑定，有些路由器可能只有设置固定 IP 功能，但是没有 ARP 绑定的功能，就没办法了（不要以为设置固定 IP 就是 ARP 绑定，两者有什么区别可以百度 ）；

（6）路由器设置端口映射，把 UDP-9 端口映射。

**20\*\***、问：我的群晖进入套件中心看不到内容了，也无法下载。\*\* 

答：首先要保证群晖的网络是畅通的，然后进入控制面板 - 区域选项 - 时间 - 与 NTP 服务器同步，选择一个服务器点 “立即更新”，当时间成功同步以后，重新进入套件中心，一切正常。

**21\*\***、问：明明我的 \***\*CPU\*\***用的是 \***\*i7\*\***，为什么群晖控制面板 \***\*-\*\***信息中心显示的 \***\*CPU\*\***信息不对？\*\*

答：因为群晖控制面板 - 信息中心显示的 CPU 信息，是按照官方白群的硬件信息显示的，并非实时显示你实际的硬件信息。

**22\*\***、问：怎样直接用域名免端口访问？\*\*

答：要想直接用域名免端口访问，需要满足一定的条件（满足任意一个即可）：

（1）运营商没封你的 80 端口，把 80 端口设置端口映射，可以用 “[http:// 域名” 访问；](http://域名”访问；)

（1）运营商没封你的 443 端口，把 443 端口设置端口映射，可以用 “[https:// 域名” 访问。](https://域名”访问。)

**23\*\***、问：我的黑群想远程访问，怎么知道自己是否有公网 \***\*IP\*\***？\*\*

答：首先你要确定是光猫拨号还是用路由器拨号，然后进到负责拨号的设备查看 PPPoE 拨号后获取到的 IP 地址，如果显示的 IP 地址是在 10.0.0.0～10.255.255.255、172.16.0.0～172.31.255.255、192.168.0.0～192.168.255.255 这三种地址范围的，99% 可以判断为私有 ip（俗称大内网）。如果是在这三种范围以外的，恭喜你了，99% 的机率为公网 IP。

**24\*\***、问：我有公网 \***\*IP\*\***，为什么光猫 \***\*/\*\***路由器重启后 \***\*IP\*\***会变？\*\*

答：由于 IPv4 地址紧缺，运营商为了节省公网 ip 地址，往往会给普通家庭宽带用户动态分配 ip 地址，也就是说你的路由器获取到的 ip 地址会在一定的时间内变化，即使你的路由器没有断电也会重新分配。有些可能每天变 1 次，有些可能每天要变好几次。

**25\*\***、问：我没有公网 \***\*IP\*\***，有什么影响吗？\*\*

答：没有公网 IP，数据流就只能出不能进，意思是只能从内网访问公网资源而不能公网访问内网资源。举个简单的例子，假如你在家里的群晖架设了一个导航网站 web.nas.com，你只能连到家里的网络，才访问这个导航网站，离开家里的网络就无法访问了。

**26\*\***、问：我现在用 \***\*3615-5.2\*\***，是否无损升级到最新版本，数据会不会丢失？\*\*

答：能否升级的关键在于你用什么硬件配置。最简单的方法就是：先不接上原来的硬盘，拿一个新的优盘写入你想要升级那个版本对应的引导，然后开机后用群晖助手搜索，如果能能顺利搜索到 IP 的，基本上都是可以升级的，升级过程跟全新安装的过程差不多，只是多了一个数据转移的步骤，并且系统会提示 “保留原有的数据和设置” 和“保留原有数据，全新安装”这两个选择，如果你想用原来的系统设置就选 “保留原有的数据和设置”，否则选择“保留原有数据，全新安装” 的话，你的系统是全新的（原来的数据都在，比如：电影，相片，音乐等等）。

**27\*\***、问：安装套件的时候，系统提示需要开启家目录，在哪开启？\*\*

答：在群晖控制面板 - 用户账号 - 高级设置，向下拉到最下面，在 “启用家目录服务” 上打勾，选择所在 “家目录” 存储位置，点应用后立即生效。

**28\*\***、问：外网用域名访问我的群晖，登录后老是提示没有权限就自动返回登录，怎么回事？\*\*

答：在群晖控制面板 - 安全性 - 安全性，“忽略 IP 检查来加强浏览器的兼容性” 打勾，点应用后再次访问就不会出现这个提示了。

**29\*\***、问：群晖 \***\*6.2\*\***有什么新的功能？\*\*

答：群晖 6.2 有以下新功能：

DSM 为使管理更加简易，密钥管理器现可选择储存在本地 Synology NAS 上。加密共享文件夹无需通过 USB 设备便可自动挂载。(建议您将密钥管理器储存至 USB 外接设备以达到较好的资料保护效果。)  
一般用户现可在 File Station 右键查看共享文件夹內容。  
新增支持 IBM WebSphere SSO。  
加强密码强度机制以确保账户安全性。  
新增短信服务供应商 SendinBlue 以及 Clickatell API (RESTful)。  
新增泰语支持。  
更新隐私权声明 以及安裝流程中的相关设定。  
iSCSI Manager  
为 IT 管理者打造的全新使用者管理界面，全面提升 iSCSI 的管理与监控体验。  
高级 LUN 具备闪电般的秒级快照、还原和复制技术，且通过 VAAI / ODX 的整合加速虚拟机 (VM) 的整体效能。因高级 LUN 已可利用文件系统缓存提高效率，DSM 6.2 及以后的版本将不再支持段落分块 LUN，但 DSM 6.1 或较旧版本的段落分块 LUN 支持将不受影响。  
支持跨存储空间的 LUN 复制。  
新增支持每个 iSCSI target 连接网络界面的网络入口设定。  
用户可以选择将设定 Thin provisioning 的 LUN 上的空间回收选项关闭以提升 I/O 效能。  
存储空间管理员  
全新设计的系统概况提供所有存储元件的健康资讯，让您轻易掌握系统状态。  
调整原有的存储架构，以存储池取代原有 Disk Group 和 RAID Group，让使用者对存储元件的操作更加清晰。  
Smart Data Scrubbing 针对磁盘群组侦测支持的文件系统格式及 RAID 类型自动执行 data scrubbing，并提升其易用性。支持任务计划执行。通过少数的设定即可提升文件正确性与一致性。  
使用者将可以依据 IT 管理需求调整 RAID resync 速度。  
用户可通过存储空间管理员远程移除硬盘。  
为新硬盘及先前未设定过的现有硬盘预设每月 S.M.A.R.T 快速检测。  
在每次 DSM 升级后，未设定坏扇区或硬盘寿命警告的用户，DSM 将会提供通知。  
整合硬盘、磁盘群组和储存空间的健康状态使其一致。  
日志中心现已包含硬盘日志。  
当存储池的 RAID 类型为 RAID 5、RAID 6、RAID F1 或 SHR (三颗硬盘或以上) 时，提供可以调整 stripe cache size（条带缓存大小）的选项。  
Synology High Availability Manager 2.0  
High Availability Manager 已独立为套件以提供更佳的系统维护并提升更新的弹性。  
新增效能页面将显示更多主服务器和副服务器的信息，提供您更详细的效能状况。  
全新设计的系统更新将排除重要更新中不必要的重启，同时确保集群的安全并且服务不中断。  
您可以在首次建立 SHA 集群时选择只复制系统设定以加速集群的设定过程。  
重新修改关于集群和服务器的控制页面，提供更直观的引导步骤帮助您升级内存和更换风扇等操作。  
首次建立 SHA 集群时，新的 SHA 使用者可以选择只复制系统设定以缩短建立时间。  
Btrfs 文件系统支持范围  
更多使用 ARM 系列平台的 Synology NAS 现在可支持 Btrfs 文件系统，让您享受新一代文件系统所带来的众多强大功能。  
适用于下列机种:  
18 - 系列: DS218、DS418 以及即将上市的更多 x18 系列  
支持 Azure AD  
将现有 Azure AD 加入 SSO 客户端，单一登入功能将省去重新验证身份的时间，提升工作效率。  
安全咨询中心  
使用智能分析技术分析使用者登入信息，回报异常登入的地理位置并以 DSM 通知您。  
每日及每月安全性报表提供更全方位、更完整的扫描信息，IT 管理者可以检查 DSM 上的异常活动。  
TLS/SSL 安全性等级  
提供更高级的方式，让您根据不同的服务选择适合的连线安全性等级以及更弹性的设定以符合当下的网络需求。  
两步骤验证  
当两步验证功能启动时，Synology NAS 管理者必须进行电子邮件通知设定。  
域 / LDAP 管理  
可弹性地指定特定域群组拥有管理员权限。  
FTP  
新增 FTPS 连接支持 ECDSA 凭证。  
NFS  
为负载平衡及网络后备新增 NFS v4.1 多重路径设定，完整支持 VMware vSphere® 6.5。  
适用于下列机种：  
18 - 系列: DS3018xs, DS418play, DS918+, DS418j, DS418, DS718+, DS218+  
17 - 系列: FS3017, FS2017, RS4017xs+, RS3617xs, RS3617RPxs, DS3617xs, RS3617xs+, RS18017xs+, DS1817+, DS1817, DS1517+, DS1517, RS217  
16 - 系列: RS2416RP+, RS2416+, RS18016xs+, DS416play, RS816, DS916+, DS416slim, DS416j, DS416, DS216play, DS716+II, DS716+, DS216j, DS216+II, DS216+, DS216, DS116  
套件中心  
全新设计的使用者介面将带来更直观的操作经验，让您快速找到一切所需。  
IHM tool  
新增支持 DS118, DS218play, DS418j 及 DS418 的 Seagate IronWolf 健康管理 (IHM)。

已知问题及限制

DSM 6.2 是最后在网络界面中支持 IPv6 隧道的版本。  
DSM 6.2 后 Synology 桌面工具将不支持 Fedora。  
DSM 6.2 及之后的版本中，将不再更新打印机、云端打印机、喇叭、Wi-Fi 网卡、数字电视棒、LTE 以及蓝牙设备等 USB 设备的驱动程式。  
无线网卡设为桥接模式时，不支持家长监控及设备清单  
升级到此版本后，将不再支持与过去的 DSM 版本建立 Virtual Machine Manager 集群，需将其他主机升级至相同或之后的版本，VMM 集群方可正常运作。  
基于安全因素考量，通过 SSH 连线进 DSM 时，将无法以 DSA 类型的公开秘钥登入。  
以 VPN 或 Proxy 服务器登入时，有些功能可能有验证问题。您可以前往控制台 > 安全性，然后点选信任的代理服务器按钮以新增信任服务器至清单上。  
Smart Data Scrubbing 计划会延续之前已设定的 RAID scrubbing 任务计划，若当任务计划正在执行时 DSM 升级，升级完成后会立即重新执行任务。  
Office 2.x 及以下版本不兼容于 DSM 6.2。

**30\*\***、问：\*\* **video\*\***套件和 \***\*moments\*\***套件为什么视频看不到缩略图？\*\*

答：视频看不到缩略图的原因可能有两种情况：（1）黑群没有洗白，黑群在使用上受到官方限制，默认是看不见缩略图的。（2）白群看不到或者已经洗白的黑群看不到缩略图，基本上是设置不对。需要在电脑上打开 video 网页端进行设置，其中：电影和电视节目用的是刮削器，是不具备缩略图的功能的，只有设置成 “其他” 才能生成缩略图（设置完成以后系统会重新索引，需要等待索引完成才会显示出来）。

**31\*\***、问：刚刚用 \***\*drive\*\***套件跟电脑同步的文件，为什么群晖要多占用一倍的空间？\*\*

答：由于 drive 套件同步默认开启版本，因此会消耗磁盘空间存放不同的文件版本。如果不需要此项功能，可以在群晖左上角菜单，打开 drive 管理控制台，找到你同步的文件夹，打开 “版本控制”，在“启用版本控制” 上取消勾选，确定。如果被多余占用的空间还没恢复，在取消版本后把套件卸载再重新安装一次，空间就恢复出来了。

**32\*\***、问：用 \***\*download\*\***套件下载的文件，为什么群晖要多占用一倍的空间？\*\*

答：由于 download 套件在下载时会生成临时文件，随着下载进度占用空间会逐渐增多，并且下载完成后由于设置了做种时间，在做种期间临时文件不会自动清除，所以就会占用磁盘空间。如果你想立即恢复那些被占用的空间，可以先把做种的文件手动点一次 “停止”，这样被占用的空间就可以恢复出来，需要做种的话可以重新点 “开始”。

**33\*\***、问：每次登录群晖，系统都会提示 \***\*IP\*\***地址由于 \***\*SSH\*\***登录错误被锁定，是怎么回事？\*\*

答：这是因为你的群晖开启 SSH 服务的时候，用默认的 22 端口，并且通过端口映射到了外网，然后被一些居心不良的人恶意的用扫描工具群扫。为了避免这个问题，加强群晖的安全管理。

**34\*\***、问：磁盘 \***\*1\*\***还有 \***\*20%\*\***左右的剩余空间，群晖就开始警告空间不足了，很烦，怎么办？\*\*

答：在控制面板，通知设置，高级设置，可以更改警告的比例（比如可以设置成不足 1% 警告），点 “应用” 后生效。

**35\*\***、问：忘记 \***\*admin\*\***的密码了，怎么办？\*\*

答：目前有 2 个解决方法：（1）重装系统：找个 PE 引导盘启动到 PE 下，打开 diskgenius，把所有硬盘的第 1 个分区格式化一次，然后重启从群晖引导盘启动进行全新安装系统。（2）拆下硬盘挂到 PE 或者其他的 Linux 下，打开 xshell 或者 winscp 之类的软件，用 root 用户登录，找到硬盘上 / etc/shadow 文件打开，每一行代表一个账户信息，找到 “admin：开头、:7:::” 结尾的这行，前面的 “admin：” 是用户名 admin，后面这串字符就是已经加密过的密码了。你可以用虚拟机安装一个群晖，设置 admin 账号密码为 123456，同样用 xshell 或者 winscp 之类的软件，用 root 用户登录，找到硬盘上 / etc/shadow 文件打开，照葫芦画瓢把加密过的密码复制出来，覆盖你原来忘记密码的 shadow 文件，保存后放回群晖机器上，开机输入密码 123456 即可登录。

**36\*\***、问：已经安装了黑群，如何获取的真实 \***\*MAC\*\***地址？\*\*

答：Windows 下运行 xshell/putty 等 SSH 软件，用 root 用户登录（系统未获取 root 权限的，需要先获取 root），输入这行命令：dmesg | grep eth0 即可显示真实 MAC 地址。

**37\*\***、问：如何获取 \***\*root\*\***权限？\*\*

答：系统版本为 6.17 以及 6.17 以下的，请搜索《群晖 DSM6.17 及以下版本开启 root》；系统版本为 6.2 以及 6.2 以上的，请搜索《群晖 6.2 版本开启 root 的方法》。

**38\*\***、问：\*\* **video\*\***默认的刮削器不好用，能否改成豆瓣的刮削器？\*\*

答：可以用豆瓣。安装方法：以 root 用户登录 SSH 安装：sudo wget -N --no-check-certificate [https://sh.9hut.cn/dsvp.sh](https://sh.9hut.cn/dsvp.sh) && sudo bash dsvp.sh install。如果不需要了也可以卸载：sudo wget -N --no-check-certificate [https://sh.9hut.cn/dsvp.sh](https://sh.9hut.cn/dsvp.sh) && sudo bash dsvp.sh uninstall。

**39\*\***、问：群晖登录时提示 \***\*“\*\***抱歉，您所指定的页面不存在 \***\*”\*\***怎么办？\*\*

答：找同版本、同型号的群晖系统安装包，把. pat 后缀改成. 7z，然后用 7-zip 进行解压，找到 libsynoshare.so.6 和 libsynopkg.so.1 这两个文件，用 root 用户登录 SSH，把群晖的 / lib/libsynoshare.so.6 和 / lib/libsynopkg.so.1 这两个文件替换成刚刚解压出来的 2 个文件，然后重启系统就可以正常登录了。

**40\*\***、问：\*\* **DS918\*\***系统中硬盘模式 \***\*SHR\*\***，在 \***\*DS3615/3617\*\***里面怎么没有了？\*\*

答：默认在 DS3615/3617 里面是没有 SHR 的，想让 3615、3617 也开启 SHR 也是可以的，用 root 登录 SSH 编辑文件 / etc.defaults/synoinfo.conf，找到 supportraidgroup="yes"，在这一行前面加 #或者删掉这行，添加一行内容：support_syno_hybrid_raid="yes"，按 ESC，输入: wq 保存退出，重启 NAS 后生效。

**41\*\***、问：为什么设置休眠，并且没有套件影响休眠，可是系统还是无法进入休眠？\*\*

答：群晖不休眠的原因之一是因为群晖 syslog 每隔一段时间就检查设备情况而且一直在写日志，所以硬盘没办法休眠。解决方案：用 root 用户登陆 SSH，打开 / etc.defaults/syslog-ng/patterndb.d/scemd.conf 文件编辑，找到

destination d_scemd {file("/var/log/scemd.log"); };  
改成：

destination d_scemd {file("/dev/shm/scemd.log"); };  
重启系统即可完美休眠；

**42\*\***、问：已经安装好的系统，想修改 \***\*SN MAC\*\***，怎么改？\*\*

答：使用 DiskGenius 进行修改，或在 PE 中修改。

**43\*\***、问：多盘位的群晖，套件安装在哪个盘，能不能改到其他盘？\*\*

答：打开套件中心，点右上角的设置，就可以看到套件安装在哪个存储空间了，可以选保存到其他的盘（下次安装后生效）。

**44\*\***、问：用群晖 \***\*web station** **架设了一个网站，但是由于只有 \*\***443\***\* 端口，没有 \*\***80\***\* 端口，怎么设置用 \*\***https\***\* 访问？**

答：群晖 “控制面板 - 网络 - DSM 设置”，把“将 http 连接自动重新导向到 https” 这个打勾，点应用。为了使 https 访问不会报错，还需要在 “控制面板 - 安全性 - 证书” 那里添加域名的证书，并设置该域名证书为默认证书，相应的服务配置好相应的证书，即可生效。

**45\*\***、问：群晖登录的时候提示 \***\*“\*\***请输入 \***\*6\*\***位数代码 \***\*”\*\***，怎么回事？\*\*

答：这是因为你开启了 Google 的二次验证的原因。在 “控制面板 - 用户账号 - 高级设置” 做相应的设置。

**46\*\***、问：群晖建立存储空间的时候，文件系统应该选 \***\*btrfs\*\***还是 \***\*ext4\*\***？\*\*

答：DSM 6.0 及 6.0 以上版本支持 Btrfs 和 ext4 文件系统，这两个系统各有特点，请根据自己的实际情况选择合适自己的系统。

### ext4 文件系统

ext4 还有一些明显的限制。最大文件大小是 16 tebibytes（大概是 17.6 terabytes），这比普通用户当前能买到的硬盘还要大的多。使用 ext4 能创建的最大卷 / 分区是 1 exbibyte（大概是 1,152,921.5 terabytes）。通过使用多种技巧， ext4 比 ext3 有很大的速度提升。类似一些最先进的文件系统，它是一个日志文件系统，意味着它会对文件在磁盘中的位置以及任何其它对磁盘的更改做记录。纵观它的所有功能，它还不支持透明压缩、重复数据删除或者透明加密。技术上支持了快照，但该功能还处于实验性阶段。

### Btrfs 文件系统

Btrfs（B-tree file system,B-tree 文件系统）是针对 Linux 开发的一个新的 CoW（copy-on-write, 写时复制 ）文件系统。它最初是由甲骨文公司在 2007 年着手开始开发的，并在 2014 年 8 月正式发布其稳定版。开发 Btrfs 的目的在于解决 Linux 文件系统中缺少池、快照、校验和以及集成的跨多设备访问等问题，目标在于实现 Linux 的规模化存储。规模化不仅仅是指解决存储问题，也意味着通过简洁的界面提供对存储的管控和管理能力，让大家能看到已使用的内容并使它更可靠。

建议选择 Btrfs 文件系统

**47\*\***、问：内网用 \***\*IP\*\***登录群晖时，为什么浏览器总是提示证书错误不安全？\*\*

答：这是正常的现象。由于你设置了启用 https，但是由于 SSL 证书是只针对域名，所以用 IP 访问会提示证书。

**48\*\***、问：局域网内有 \***\*2\*\***台群晖，有公网 \***\*IP\*\***，有域名，如何实现外网可以同时访问这 \***\*2\*\***台群晖？\*\*

答：可以在路由器上设置不同的端口转发，不同端口对应不同的群晖 ip。例如：群晖 A 的内网访问地址是 192.168.1.100:5000，群晖 B 的内网访问地址是 192.168.1.101:5000，在路由器上设置外网端口 100 对应内网 IP 地址 192.168.1.100 内网端口 5000，设置外网端口 101 对应内网 IP 地址 192.168.1.101 内网端口 5000。外网打开[http:// 域名: 100 则访问 A 群晖，打开 http:// 域名: 101 则访问 A 群晖。](http://域名:100则访问A群晖，打开http://域名:101则访问A群晖。)

**49\*\***、问：群晖设置了谷歌二次验证，但是手机丢失了无法进入怎么办？\*\*

答：Windows 下运行 Winscp 软件，以 root 用户登录群晖，找到 / usr/syno/etc/preference/sheldon/google_authenticator 文件并删除。

**50\*\***、问：主板上 \***\*SATA\*\***接的硬盘，位置能否调换？\*\*

答：群晖是根据硬盘的 SN 来辨认，而不是通过 SATA 接口的位置来分辨，所以位置可以调换。即使是组了 RAID 的，也可以调换 SATA 位置。

**51\*\***、问：群晖 \***\*video\*\***套件想用时光网的刮削器，怎样安装？\*\*

答：以 root 用户登录 SSH，输入：sudo wget -N --no-check-certificate [https://sh.9hut.cn/dsvpmt.sh](https://sh.9hut.cn/dsvpmt.sh) && sudo bash dsvpmt.sh install 进行安装；如需要卸载，则输入：sudo wget -N --no-check-certificate [https://sh.9hut.cn/dsvpmt.sh](https://sh.9hut.cn/dsvpmt.sh) && sudo bash dsvpmt.sh uninstall。

**52\*\***、问：群晖 \***\*video\*\***套件的刮削器，为什么有时能搜到海报有时又不行，特别是电视剧很多都不搜索不出来？\*\*

答：根据 Synology 官方的说明，需要按照影片和剧集所规定的命名规则，刮削才可以正确辨识。电影命名格式为：电影名称 (发行年份). 扩展名，例如电影《流浪地球 》的文件名应命名为：流浪地球 (2019).mkv；剧集命名格式：电视影集名称. SXX.EYY. 扩展名 (「S」为「季数」；「E」为「集数」)，例如《权利的游戏》第八季第 4 集应命名为： 权利的游戏. S08.E04.mkv。

**53\*\***、问：\*\* **ESXI\*\***虚拟机安装的群晖，并且使用 \***\*SATA\*\***控制器直通，为什么主板接的 \***\*4\*\***个 \***\*SATA\*\***硬盘，在群晖里面显示不全？\*\*

答：由于虚拟机系统的引导也采用 SATA 格式，群晖系统会 “认为”SATA 被占用，所以才没有完全显示出来。解决办法：虚拟机开机后，引导菜单选择第三项带有“VMware/ESXI” 的这个引导方式，就可以避免此问题出现。

**54\*\***、问：我想把群晖 \***\*video\*\***里面的某个电影分享给朋友看，怎样操作？\*\*

答：首先你需要有公网 IP，然后用域名（或者 IP 地址）登录自己的 video 网页版，找到你想分享的影片，点右下解的 “…”，在菜单中点一下“公开共享” 进入，在弹出的菜单左上角 “公开共享” 打勾，并复制链接，把这个链接分享给你的朋友即可。需要注意的是：video 默认是设置是共享的时候禁止转码，如果你的影片需要转码才能观看的话，你的朋友是无法观看的哦，如果你是白群或者黑的已经洗白，可以根据自己的情况，设置共享的时候选择是否开启转码。

**55\*\***、问：我想把群晖里面的某个文件分享给朋友下载，怎样操作？\*\*

答：首先你需要有公网 IP，然后用域名登录自己的 DSM 网页，打开桌面的 File station，找到你需要分享的文件，点右键 “共享”，点左上角的“有效期” 进入设置（可以根据自己的情况设置选择开始时间、停止时间和访问次数，可设置单项或者多项。），确认无误后点 “保存”，然后在“共享链接” 一栏中复制链接，点 “保存” 后生效。

**56\*\***、问：我群晖系统显示的都是 \***\*001132\*\***开头的假 \***\*MAC\*\***地址，路由器也是显示这个，但是主板标签明明不是这个地址的，怎么回事？\*\*

答：黑群的系统是引导盘和系统盘分开的，物理机的引导盘一般都用 U 盘或者小的固态硬盘来做，虚拟机的引导盘用虚拟的磁盘。引导盘包含了 DSM 系统的 SN 和 MAC 等信息，而群晖系统的 MAC 都是 001132 开头的，所以如果你没有去更改引导盘的 MAC 地址，那么默认显示的就是 001132 开头的 MAC。你也可以编辑打开引导盘的 grub.cfg 文件，修改 MAC 地址为你主板标签上的 MAC 地址。

**57\*\***、问：我的群晖不想升级，但是看到小红点又不爽，怎样办？\*\*

答：以 root 用户登录 SSH，编辑 / etc/host 文件，在最下面一行写入 127.0.0.1 update.synology.com，然后保存退出即可（无需重启）。

**58\*\***、问：物理机安装群晖，常见的千兆 \***\*PCIE\*\***网卡最高能支持到什么版本？\*\*

答：由于网卡芯片众多，未标注的型号并不是不支持，请自行测试：

（1）Intel 211/210/82576/82580 物理机安装 DS3615xs/DS3617xs/DS918 + 均可以支持最新的 6.22-24922UP3；

（2）Intel 82583 物理机安装 DS3615xs 最高支持最新的 6.22-24922UP3，物理机安装 DS3617xs/DS918 + 最高支持 6.2-23739；

（3）Intel 82574L/82579LM 物理机安装 DS3615xs/DS3617xs 最高支持 6.2-23739，不支持 DS918+；

（4）RTL 8168/8111 物理机安装 DS3615xs/DS3617xs 最高支持 6.2-23739，物理机安装 DS918 + 最高支持 6.21-23824；

（5）Intel 219 暂不支持物理机安装群晖 DS918+，可以安装 DS3615xs/DS3617xs。

**59\*\***、问：我用 \***\*ESXI** **安装的群晖分配了 \*\***16\***\* 个核心，怎么看群晖使用几个核心？**

答：由于群晖系统信息显示的 CPU 并非真实检测的信息，因此我们需要用 root 用户登录 SSH，输入以下命令回车后后显示的数字就是群晖系统实际工作的核心数。

cat /proc/cpuinfo |grep "processor"|wc -l

**60\*\***、问：群晖的引导菜单默认是自动进入第一个菜单，我想把它改成自动进入第三个菜单，可以吗？\*\*

答：可以的。把引导盘的 grub.cfg 文件用工具打开编辑，找到 set default='0'这行，改成 set default='2'，保存退出，重启群晖后生效。

**61\*\***、问：群晖在路由器映射了端口，外网可以用 \***\*[http://\*\*](http://**)**域名 \***\*:5000\*\***访问群晖，为什么在局域网 \***\*[http://\*\*](http://**)**域名 \***\*:5000\*\***就不能访问？\*\*

答：这是因为你的路由不支持端口回流造成的。

**62\*\***、问：暴风播库云二期的机器安装的群晖系统，为什么接上 \***\*SSD\*\***可以认，接了 \***\*10T\*\***硬盘就认不出来？\*\*

答：有可能是因为 BIOS 版本太低的原因，建议升级到最新版本。暴风播库云二期用的是华擎 J3455-itx 主板，所以可以在华擎官网下载：[https://www.asrockchina.com.cn/MB/Intel/J3455-ITX/index.cn.asp#BIOS](https://www.tenlonstudio.com/go/bmou)

**63\*\***、问：为什么已经洗白的黑群，在 \***\*Moments\*\***里面重新索引完成以后，视频还是看不到缩略图？\*\*

答：有可能是因为没有洗白之前的索引生产的临时文件，在洗白之后没有被清理而被保存下来，所以看到的并不是重新索引后的缩略图。解决方法：以 root 用户登录 SSH，进入 @eaDir 文件夹，把洗白之前生成缩略图时产生的. fail 文件全部删掉，然后在 Moments 里面重新索引，等索引完成就可以正常显示了。

**64\*\***、问：想在黑群里面安装个 \***\*emby\*\***，套件中心怎么找不到？\*\*

答：由于 emby 是第三方的套件，所以需要在套件中心 - 设置 - 套件来源，添加一下：[https://synology.emby.media/?package\\\_repository=360efc6e-de72-4073-b603-2bfbd7001586](https://synology.emby.media/?package\_repository=360efc6e-de72-4073-b603-2bfbd7001586)

**65\*\***、问：我想让黑群的信息显示我的真实 \***\*CPU\*\***信息，怎么做？\*\*

答：需要下载补丁并安装到黑群晖里面，就可以显示真实的 CPU 信息。

**66\*\***、问：用群晖 \***\*Drive\*\***与电脑同步的时候，提示文件正在使用或者没有访问权限，有几个文件同步了好几天了还是失败，怎么办？\*\*

答：这是由于 Win 的权限机制造成的。解决方法：先把同步停止，下载（管理员取得所有权），解压到电脑上，双击运行，在弹出的菜单中点 “是”，然后到你同步失败的文件夹上点右键，你会发现菜单多了一个 “管理员取得所有权”，点它会出现一个执行的窗口，然后等窗口消失以后，重新打开同步，此时一般都可以继续同步了。如果还不行，则再次停止同步任务，把 NAS 或者电脑端的不能同步的文件，先剪切出来放到其他文件夹，重新打开同步，等同步任务完成以后，重新把之前剪切的文件再次放回 NAS 或者电脑，此时同步任务就可以继续同步了。

**67\*\***、问：群晖密码输错几次后，再次登录，它提示：此 \***\*IP\*\***地址已被封锁，因为它已达到特定时间允许的登录尝试失败最大数。请联系系统管理员。如何解决？\*\*

答：如果你是在内网操作导致被封锁的，并且登录密码还记得，那么你可以把当前电脑的内网 IP 地址换一个（如果是外网操作导致被封锁的，可以把光猫重启一下，它会自动换另外一个公网 IP 地址），然后再次管理用户登录群晖，输入正确的密码后，打开控制面板 - 安全性 - 账户 - 允许 / 封锁名单 - 封锁名单，找到你误操作导致被封锁的 IP 地址，点删除，关闭。

**68、问：传说 Intel 低功耗 cpu 即将变砖，J3455 安装的群晖还能用吗？**

答：目前尚未有变砖的事实。所以大家还请稍安勿躁，静观其变，等待最终的解决方案。

**69\*\***、问：物理机安装群晖，常见的万兆 \***\*PCIE\*\***网卡最高能支持到什么版本？\*\*

答：由于网卡芯片众多，未标注的型号并不是不支持，请自行测试。以下网卡均支持物理机安装 DS3615xs/DS3617xs/DS918 + 均可以支持最新的 6.22-24922UP3：

（1）Intel 82598/82599；

（2）Intel X520/X540/X550。

**70\*\***、问：物理机安装群晖，常见的 \***\*4\*\***万兆 \***\*PCIE\*\***网卡最高能支持到什么版本？\*\*

答：由于网卡芯片众多，请自行测试，据说基本上均可以支持最新的 6.22-24922UP3。本人了解不详。 
 [https://www.hankhu.com/168/](https://www.hankhu.com/168/)
