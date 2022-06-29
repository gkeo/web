# 黑群晖硬件选用与兼容列表 - Tank 米多贝克
阅读次数: 4,446

部分品牌机电脑或服务器可能会出现各种定制主板的不兼容毛病，建议采用正规 DIY 渠道主板。尽量不使用品牌机安装黑群晖。理论上只建议 Intel 上黑群晖 AMD 由于条件有限无法测试。cpu 型号请自行百度相关参数。群晖只支持软 RAID，服务器改群晖需要注意群晖只支持小部分硬件 RAID 卡，请自行咨询服务器销售商确认 RAID 卡是否支持 IT 直通，群晖系统能否使用。 如果不知道自己 cpu 的年代可以点下方连接查看（只有零售桌面型号，J N D 开头的工控低功耗 CUP）  [http://www.mydrivers.com/zhuanti/tianti/cpu/ ](http://www.mydrivers.com/zhuanti/tianti/cpu/ "http&#x3A;//www.mydrivers.com/zhuanti/tianti/cpu/")

### 引导型号：DS918/3617/3615/3622    系统版本 7.x

CPU / 硬件需求与建议 / AHCI 要求：Intel 3455 系列或 4 代或以上版本才支持 GPU 硬件转码压制视频与照片 必需 64 位 CPU，9 代或以上除 i3 910x 之外其它型号全部需要单独打补丁才可以实现硬件解码但是不一定都生效，918 最多支持双网卡加网卡请注意检测主板网卡顺序第三张网卡不被兼容（有补丁可以开启），DS918 的系统只支持 INTEL 的万兆网卡和一般的千兆网卡 8125B 之类的网卡，3617/3615/3622 支持常见服务器与家用网卡。  主板必需支持 AHCI，支持 NVME/  PCIE（ M.2/U.2）SSD 硬盘缓存（需要破解 3615 不支持 NVME）

运行情况：非主板南桥原生 6SATA 的主板安装时会报端口错误。j3455/4125/4005/5xxx 系列部分主板可能有无法软关机 bug。 NVME 硬盘缓存需要手动破解，INTEL 6-9 代用起来兼容性无敌，10-11 代可能 GPU 无法解码只能软解码，AMD 基本也是这个情况，

### 引导型号：DS3617 / 系统版本 6.17/1.02B

CPU / 硬件需求与建议 / AHCI 要求：Intel 4 代或以上版本（AMD 参考同时代产品）必需 64 位 CPU，支持 4 个或以上的网卡，兼容大部分万光网卡，主板必需支持 AHCI

运行情况：完美，可以打 up3 补丁，暂时不支持 Drive 2.x，没有按需要同步功能（注目前只支持 win10 1809 或以上版本 ）

### 群晖型号 / 系统版本：DS3617 / 系统版本 6.22|6.23/1.03B

CPU / 硬件需求与建议 / AHCI 要求： Intel 4 代或以上版本（AMD 参考同时代产品）必需 64 位 CPU / 硬盘接口必需支持 AHCI

运行情况：不支持 UEFI / 不支持 UEFI 启动，必需开启 VT VD 功能否则 VMM 无法使用（虚拟机）Intel 9 代以后部分主板可能不支持核显使用传统引导启动，会无法正常使用

### 引导型号：DS3615 / 系统版本 6.17/1.02B

CPU / 硬件需求与建议 / AHCI 要求：Intel 3 代或以下版本（AMD 同代产品功耗太高不推荐）必需 64 位 CPU，支持 4 个或以上的网卡，兼容大部分万光网卡，不需要主 支板持 AHCI

运行情况：完美，可以打 up3 补丁。暂时不支持 Drive 2.x，没有按需要同步功能（注目前只支持 win10 1809 或以上版本 ）

### 引导型号：DS3615 / 系统版本 6.2|6.22/1.03B

CPU / 硬件需求与建议 / AHCI 要求：Intel 3 代或以下版本（AMD 同代产品功耗太高不推荐） 必需 64 位 CPU，支持 4 个或以上的网卡，兼容大部分万光网卡，主板要支持 AHCI

运行情况：不支持 UEFI / 不支持 UEFI 启动，必需开启 VT VD 功能否则 VMM 无法使用（虚拟机）Intel 9 代以后部分主板可能不支持核显使用传统引导启动，会无法正常使用

### 引导型号：DS3615 / 系统版本 5.2

CPU / 硬件需求与建议 / AHCI 要求：兼容老机器尤其是 32 位 CUP，不支持 x64 指令 / 硬盘接口不用支持 AHCI

运行情况：不支持 UEFI / 系统内的 VMM 虚拟机大部分系统不可引导 / 其它 BUG 未知 / 而且老 CPU 功耗太高建议购买我们店铺的成品 NAS 一年就可以省出一到两台 NAS 的电费

部分工控制主板 / 旧 CPU 主板实测情况

| cpu intel G645 + 主板 H61 | 3615/3617 6.17 系统正常，1.03b 6.26.22 用 BIOS 引导也无法搜索出机器 |
| D2550 双千兆网卡主板 | 主板屏蔽了 x64 指令集，部分破\*解了 bios 的可以安装 6.1x，普通此 cpu 主板只能 5.2 |

常见硬件实测情况

| 华擎 J3455itx | 所有系统都可以正常引导使用，1.2 的 BIOS 版本有 BUG BIOS 设置无法保存问题建议升级后再安装。升级到 6.2.3 引导之后部分主板无法软关机 7.1 也能正常安装 |
| 星际蜗牛 J1900 | 所有系统都能正常引导使用 |
| 梅捷 N3160 | 所有系统都能正常引导使用 |
| i3 9100F/i5 9400f | 所有系统都能正常引导使用，无显卡可以运行 3617/15 |
| 不知名 AMD 880 | 只能 6.17  BIOS 设置无效 |
| 英特尔 4-9 代 i3 i5 | 所有系统都能正常引导使用 |
| 技嘉 9 代以上英特尔主板 | 10/11 代技嘉固定有 BUG（12 代未测试）每次开机都必需重置主板设置才能搜索到 ip，请不要购买 |
| 华擎 10/11 英特尔代主板 | 由于华擎 BIOS 锁定采用核心显卡引导，无法使用传统引导。所以 3615/3617   6.2.x 版本无法正常使用，需要加装独立显卡或者亮机卡才能使用。请慎重或使用 ds918 引导。 |
| 英特尔 9 代部分 i5/i7 及 10/11 代 | DS918 + 系统因为核心显卡（GPU）更换马甲 ID 不能正常驱动需要替换驱动中的 id 字段文件，部分 10 代 11 代替换了也不能驱动硬件但是可以软解码，目前已经有 9-12 代的全部 GPU 驱动补丁 |
| 微星大部分主板 PCIE 兼容性有问题对部分扩展卡兼容性比较差 | 可能会出现板载网卡不认或者插了 NVME 硬盘 SATA 口不可用、板载网口无法使用等问题。 |

**硬件兼容列表，列表只供参考只针对大众化品牌，冷门品牌山寨货不一定能用。** 

**网卡：** 

AMD

amd8111e : AMD 8111 (new PCI LANCE)  
pcnet32 : AMD PCnet32 PCI

Asix  
asix : ASIX AX88xxx Based USB 2.0 EthernetAdapters  
ax88179_178a : ASIX AX88179/178A USB3.0/2.0 to Gigabit Ethernet

Atheros / Qualcomm  
atl1 : Atheros/Attansic L1 Gigabit Ethernet  
atl1c : Atheros L1C Gigabit Ethernet  
atl1e : Atheros L1E Gigabit Ethernet  
atl2 : Atheros L2 Fast Ethernet  
alx : Qualcomm Atheros AR816x/AR817x

Broadcom  
b44 : Broadcom 440x/47xx  
bnx2 : Broadcom NetXtremeIIBCM5706/BCM5708/5709/5716  
bnx2x : Broadcom NetXtremeII 10GbBCM57710/BCM57711/BCM57711E/BCM57712  
tg3 : Broadcom Tigon3BCM5705/BCM5703/BCM5702/BCM5701/BCM5700/BCM5721/BCM5751/BCM5788/BCM5704/BCM5752/BCM5789  
tg3 :BCM5723/BCM5761/BCM5787/BCM5755/BCM5722/BCM5754/BCM57781/BCM57785/BCM5718BCM57765/BCM57761  
tg3 :BCM5719/BCM5725/BCM5762/BCM5720/BCM57790/BCM57795/BCM57766/BCM57780

Chelsio  
cxgb : Chelsio 10Gb Ethernet  
cxgb3 : Chelsio Communications T3 10Gb  
cxgb4 : Chelsio Communications T4 Ethernet

Cisco  
enic : Cisco VIC Ethernet NIC

D-Link  
dl2k : D-Link DL2000-based Gigabit EthernetAdapter

HP  
hp100 : HP 10/100VG PCLAN (ISA, EISA, PCI)

Intel

i219V：9 代以后的主板可能认不出来，但是可以通过修改型号解决问题。群晖 unraid esxi pve mov 不认 intel i219v 网卡解决方法[https://b23.tv/wmW3HN](https://b23.tv/wmW3HN)

e100 : Intel PRO/100+  
e1000 : Intel PRO/1000CT/F/GT/MF/MT/T/XF/XT82543GC/82546EB/82545EM/82540EM/82540EP/82544GC/82544EI/82547EI/82541EI  
e1000 : 82545GM/82546GB/82541ER/82547GI/82541GI/82541PI/82566MM/82566MC/82566DM/82566DC/82563EB/82564EB/82562  
e1000e :82573L/82572EI/82571EB/82573E/82573V/82567/82574L/82566MM/82566MC  
e1000e :82566DM/82566DC/82563EB/82574IT/82583V/82579LM/82579V/82577LC/82577LM  
e1000e : 82578DC/82578DM/Gigabit CT DesktopAdapter/PRO/1000 PT/PF/I217-LM/V/I218-V/LM/I219 LM/V  
igb :82576EB/82576NS/82580EB/82575EB/Gigabit ET/ET2/EF/VT  
igb :I350-AM4/I350-BT2/I350-AM2/I350-T2/I350-T4/I350-F2/I350-F4  
igb : I340-F4I340-T4/I210-AT/I210-IS/I210-IT/I210-T1/I210-AS/I210-CS  
ixgbe : Intel 10 Gigabit AFDA/AT/AT2/CX4/XF LR/XR/82599EB/82598EB/82599ES/82599EN  
ixgbe :X540-T1/X540-T2/540-AT2/X520-SR1/X520-SR2/X520-DA2/X520-LR1/X520-DA1/X520-DA4/X520-QDA1/X520-T2X550-BT2/AT/AT2 X550-T1/T2  
i40e : Intel 40 GigabitX710-AM2/XL710-AM1/XL710-AM2/X710-DA2/X710-DA4/XL710-QDA1/XL710-QDA2

JMicron  
jme : JMicron(R) PCI-Express Gigabit

Marvell  
skge : Marvell Yukon Gigabit Ethernet  
sky2 : Marvell Yukon 288E8021/88E8022/88E8035/88E8036/88E8038/88E8050/88E8052/88E8053/88E8055/88E8061/88E8062,SysKonnect SK-9E21D/SK-9S21

Mellanox  
mlx4_en : Mellanox Technologies 10GbitEthernet

NVIDIA  
forcedeth : nForce Ethernet

Brocade/QLogic  
bna : Brocade 1010/1020 10Gb Ethernet  
qla3xxx : QLogic QLA3XXX Network Driver  
qlcnic : QLOGIC QLCNIC 1/10Gb ConvergedEthernet  
qlge : QLogic QLGE 10Gb Ethernet

pegasus : Pegasus/Pegasus II USB Ethernetdriver

pch_gbe : EG20T PCH Gigabit ethernet Driver

Realtek  
r8101 :RTL8100E/RTL8101E/RTL8102E-GR/RTL8103E(L) RTL8102E(L)/RTL8101E/RTL8103TRTL8401/RTL8401P/RTL8105E RTL8402/RTL8106E/RTL8106EUS  
r8168 : RTL8111B/RTL8168B/RTL8111/RTL8168RTL8111C/RTL8111CP/RTL8111D(L) RTL8168C/RTL8111DP/RTL8111E  
r8168 : RTL8168E/RTL8111F/RTL8411RTL8111G/RTL8111GUS/RTL8411B(N) RTL8118AS  
r8169 : RTL8110SC(L)RTL8110S/RTL8110SB(L)/RTL8169SB(L)/RTL8169S(L)/RTL8169  
r8150 : USB RTL8150 based Ethernet device  
r8152 : RTL8152/RTL8153 Based USB 2.0/3.0Ethernet Adapters  
rtl8150 : USB RTL8150 Based USB EthernetAdapter

SiS  
sis900 : SiS 900/7016  
sis190 : SiS190/SiS191

VIA  
via-rhine : VIA Rhine  
via-velocity : VIA Velocity

Virtio  
virtio_net : Virtio network driver

VMware  
vmxnet3 : VMware VMXNET3 ethernet driver

sc92031 : Silan SC92031 PCI Fast EthernetAdapter driver

**扩展卡：** 

3ware  
3w-xxxx: 3ware 5/6/7/8xxx ATA-RAID support  
3w-9xxx : 3ware 9xxx SATA-RAID support  
3w-sas: 3ware 97xx SAS/SATA-RAID support

Adaptec  
aacraid : Adaptec AACRAID support (DellPERC2, 2/Si, 3/Si, 3/Di, HP NetRAID-4M, IBM ServeRAID & ICP SCSI), HBA 1000series  
aic94xx : Adaptec AIC94xx SAS/SATA support

HighPoint  
hptiop : HighPoint RocketRAID 3xxx/4xxxController support

HP  
hpsa : Smart Array P212 P410 P410i P411P812 P712m P711m ; (Gen6/7 Controllers)  
hpsa : Smart Array P222 P420 P420i P421P822 P220i P721 ; (Gen8 Controllers)  
hpsa : Smart Array P430 P430i P431 P830P830i P831 P731m P230i P530 P531 ; (Gen8.5 Controllers)  
hpsa : Smart Array P440 P441 P840 P440arP244br H240 H241 H240ar H244br ; (Gen9 Controllers)  
hpsa : HP StorageWorks 1210m P1224 P1228P1228m P1224e P1228e P1228em

IBM  
ips : IBM ServeRAID support

Intel  
gdth : Intel/ICO RAID controller support  
isci : Intel C600 Series Chipset SASController

Initio  
Initio 9100U(W) support  
a100u2w Initio INI-A100U2W support

stex : Promise SuperTrak EX Series support

SYM53C8XX Version 2 SCSI Support  
IBM Power Linux RAID adapter support

QLogic  
QLogic QLA 1240/1x80/1x160 SCSI support

LSI  
megaraid_sas : MegaRAID SAS9361-8i/9361-4i9341-8i/9341-4i/9380-8e/9380-4i4e/9270-8i9271-4i/9271-8i/9271-8iCC  
megaraid_sas : 9286-8e/9286CV-8e/9286CV-8eCC/9265-8i/9285-8e/9240-4i/9240-8i/9260-4i/9260CV-4i/9260-8i/9260CV-8i  
megaraid_sas :9260DE-8i/9261-8i/9280-4i4e/9280-8e/9280DE-8e/9280-24i4e/9280-16i4e/9260-16i/9266-4i/9266-8i/9285CV-8e  
mpt2sas : LSI SAS 6Gb/s Host AdaptersSAS2004, SAS2008, SAS2108, SAS2116, SAS2208, SAS2308 and SSS6200  
mpt3sas : LSI SAS 12Gb/s Host AdaptersSAS3004, SAS3008, SAS3108

Marvell  
mvsas : Marvell 88SE64XX/88SE94XX SAS/SATAsupport (SAS/SATA 3Gb/s PCI-E 88SE64XX and 6Gb/s PCI-E 88SE94XX) 
 [https://www.mi-d.cn/1338](https://www.mi-d.cn/1338)
