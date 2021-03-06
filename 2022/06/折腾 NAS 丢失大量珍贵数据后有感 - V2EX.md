# 折腾 NAS 丢失大量珍贵数据后有感 - V2EX
#### 背景

1.  第一版 NAS 已用 5 年，为占美无风扇工控机、一块 WD60EFRX 、优越者 Y-3359R 进行 DIY，稳定时千兆可跑满。
2.  为了顺便当做远程桌面，没有使用黑群晖或 Linux，使用了 Win7 系统。Samba 服务经常无故卡死，遇到网络无法连接、0x00000000XX 等类似错误或 “服务器存储空间不足无法处理此命令” 等错误无数次，每次基本都需要重启。花了无数时间来解决这些问题，无果。一直在忍受，后续甚至写了脚本做定时重启。
3.  第二版（在用）为群晖 DS920+、一块 WD140EMFZ，以及上面的 6T，买之前提前了解了很久 RAID5 是否安全，后选择 SHR 。

#### 灾难

1.  因计划把 6T 和 14T 硬盘都放入群晖，但硬盘插入群晖会被格式化，所以 14T 硬盘到货当天，用外接硬盘座接入 14T 硬盘，在 Win7 下进行 6T 到 14T 文件全量复制，期间报错无数，推测可能是 6T 磁盘长时间运行导致文件分配表错误。搜索相关资料提示 chkdsk 修复 NTFS 分区。
2.  \[重点] chkdsk 完毕后，6T 硬盘中大量资料丢失（超过 80%），其中包括各种重要文件，尝试 DiskGenius 修复未果（不光是无法找回文件分配表，而且连 RAW 数据都无法找回）。
3.  不幸中的万幸，其中少部分文件在各个网盘有备份，另一部分几年前有刻盘备份，但其中一些珍贵的资料再也无法找回（包括结婚视频、小孩照片、老光盘 ISO 等个人产生文件，与老游戏、各种收藏的视频等网络下载资源）。
4.  花费几天时间在各种平台搜索网络下载资源，因年份久远，几乎全部失效，近似于无法找回。

#### 现状与未来

1.  计划在过一段时间之后再买一块 14T，尽可能避开同批次，2 块盘开 SHR 。
2.  每隔一段时间继续买 14T，加到 4 块，开 SHR2 。

#### 教训与体会

1.  任何情况下大量数据复制尽可能先备份，或者使用专门的工具进行，谨慎进行 chkdsk 等硬盘修复措施。不要因为是程序员身份就各种放心大胆地操作，稍有不慎就会产生非常严重的后果。
2.  脑子要清楚，选择方案要合理可行。其实可以选择先把 6T 使用 Ghost 备份一份到 14T 硬盘，再进行数据复制，反正空间足够。
3.  重要资料放在 NAS 一定要开 RAID，硬盘少就用 RAID1，硬盘多可考虑 RAID5 、RAID6 或者群晖的 SHR 或 SHR2，其中尽可能选择允许 2 块盘故障时可恢复的方案。同时使用多份备份，网盘、冷备、刻盘等。
4.  网络下载资料可考虑使用脚本生成 ed2k 链接，文件丢失后可以重新下载，或使用网盘快速离线。
5.  从 2015 年第一版 NAS 到今年买群晖中间的 5 年里，硬盘柜里一直只有一块硬盘，最开始选择的 5 盘位硬盘柜产生了严重的浪费。感慨自己想得太多，有美好的愿望但却一直没有落实，又感叹运气太好，这 5 年中唯一的一块硬盘居然没坏。
6.  第一版 NAS 使用的这 5 年中，是人生大事接连不断的几年，根本存不下钱，更别提这种大额投入了。所以钱真的可以解决绝大部分问题。如果有钱，最开始我就买群晖并放满硬盘，开各种灾备，就可以避免这种大量数据搬运，数据丢失的概率会大大降低。
7.  只有栽跟头才能让人积累经验，只有疼痛才能让记忆更深。不舍得投入成本保护数据，还是因为数据对你不够重要。
8.  漏了一点，为避免突然断电损坏硬盘，尽可能上 UPS 。目前家中所有的有硬盘设备都接在 UPS 后面，包括群晖，而且 UPS 切换电池模式后会通知群晖转到安全模式，也算是进一步降低意外损坏的风险。 
    [https://www.v2ex.com/t/699891](https://www.v2ex.com/t/699891)
