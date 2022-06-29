# 📺[原创]在 Debian 内建一个 udpxy 转发服务，实现自由看(上海电信) IPTV - 知乎
## 我自己家里的网络图

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-53-08/9ab6b9ec-6dcf-415a-b779-ee0603ac5e10.jpeg?raw=true)

## 开头特别说明

-   本文不是 step by step 小白教程, **仅提供大致配置流程和核心配置参数**, 需要对如下内容有一定的基础: Linux, 虚拟机, Docker, 基础问题请不要留言提问, 自行搜索
-   本方法目前适用于上海电信的 IPTV（截至发文时 2021-11），其他地区仅供参考
-   本文内容**不**涉及 **破解 违规软件 使用漏洞** 等敏感内容, 所有软件和配置方法都是合规合法, 所有下载链接, 引用教程都是官网原始地址

## 概述

-   在 Debian 10 内建一个 udpxy 服务, 直接与电信光猫上的 IPTV 信号口对接, 无需拨号, 无需电信的 IPTV 盒子, 即可转发 IPTV 组播, 内网直接用 .m3u 表观看，外网通过端口转发或者 xteve + plex 转发观看, 实现在任何地方自由地看自己信号源的 IPTV
-   大致流程: PVE 安装 Debian 10, 安装 Docker, Docker Compose, 设置网络, 全 docker 部署: udpxy(xteve, plex)

> udpxy (_'you-dee-pixie'_) is a data stream relay: it reads data streams from a multicast groups and forwards the data to the requesting clients (subscribers).  
> udpxy 是一个将 UDP 组播数据流变成 TCP 协议单播流的流量中继，它将 UDP 流量从给定的多播转发到请求的 HTTP 客户端（订阅者）。

### 安装宿主机 PVE

-   `https://www.proxmox.com/downloads`, 具体略

### 安装 Debian Official Cloud Image

-   `https://cloud.debian.org/images/cloud/` 用 10(buster) 或 11(bullseye)
-   下载 debian-1x-generic-amd64.qcow2 或者 debian-1x-genericcloud-amd64.qcow2 镜像
-   大致流程：建立 VM 机, 删除硬盘, 上传镜像到宿主机 /root/，用这条指令 `qm importdisk xxx debian-10-genericcloud-amd64.qcow2 local-lvm` (XXX 是 VM 的 ID) 创建系统硬盘，再点编辑 -- 添加硬盘，再点`调整磁盘大小`，增加大小 (到 10 G 左右可)，然后 reboot 即可，Debian 系统内不用做任何操作, 自动扩容
-   如果用 \*-genericcloud-amd64.qcow2 版镜像要添加 CloudInit 设备，再配置

### 安装 Docker, Docker Compose

-   Docker Engine 官方教程: `https://docs.docker.com/engine/install/debian/`
-   Docker Compose V2 官方教程: `https://docs.docker.com/compose/cli-command/`, 这里注意: 在 Debian 10 下，全用户的 cli-plugins 的路径是: `/usr/libexec/docker/cli-plugins`, 而不是这篇教程里的 `/usr/local/lib/docker/cli-plugins`, 具体可以去看 Docker Compose 的 Github

### 先配置下网络

-   本方法的硬件连接：你的 Debian 10 虚拟机至少要 2 个网口，其中一个口 (ens19) 直接接电信光猫后面的有 IPTV 信号的口，用于获取 IPTV 的源信号，另一个口 (eht0) 接交换机，用于转发 udpxy 的组播。
-   上海电信播放 IPTV 只需要进到 23 开头的电信专网即可, 无需拨号, 无任何认证, 无需电信的 IPTV 盒子, 进去的方法就是网口 tag 上 VLAN 85 即可（见下图）

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-53-08/3c04bce2-d0d9-4617-807c-b04f85d04083.jpeg?raw=true)

-   在 Debian 10 内输入 `ifconfig`, 查看你的 ens19 网口 (即上图的 vmbr3 虚拟网口) 是不是拿到了 23 开头的地址 (你的这个网口名可能不叫 ens19, 可能叫 eth1)

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-53-08/b23db0d4-971c-43c6-9ef4-4d72c7f3195b.jpeg?raw=true)

-   **重要：网络设置，不然 igmp 的流量走不通**

输入 (注意：指令中的 ens19 请自行替换你的 IPTV 网口名)

```text
sysctl -w net.ipv4.conf.ens19.force_igmp_version=2
sysctl -w net.ipv4.conf.ens19.rp_filter=0
sysctl -w net.ipv4.conf.all.rp_filter=0
```

再输入, 保存生效

```text
/sbin/sysctl -p
/sbin/sysctl -w net.ipv4.route.flush=1
```

删除还原方法

```text
sysctl -w net.ipv4.conf.ens19.force_igmp_version=0
sysctl -w net.ipv4.conf.ens19.rp_filter=1
sysctl -w net.ipv4.conf.all.rp_filter=1
/sbin/sysctl -p
/sbin/sysctl -w net.ipv4.route.flush=1
```

### Docker Compose 安装 udpxy

docker-compose.yml 配置：

```text
version: "3"
services:
  udpxy:
    container_name: udpxy
    image: agrrh/udpxy:latest
    network_mode: host
    restart: always
    command: -T -a eth0 -p 4022 -m ens19
```

安装指令: `docker compose up -d`

注意 `eth0` 是接交换机的口，是 `ens19` 接电信光猫 IPTV 的口, 根据自己实际情况修改

udpxyd 的 web 访问地址 (`/status/` 要打全): `http://192.168.1.172:4022/status/`

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-53-08/44822f06-444d-480e-9afc-b0f7fb53ae70.jpeg?raw=true)

本地 PC 上用 PotPlayer 播放时 udpxy status 显示截图

### 到此就建完了

访问地址格式：`http://192.168.1.172:4022/udp/239.45.3.146:5140`，直接复制进 PotPlayer 就能看

> 上海电信 239 开头的 IPVT 内网地址, 请自行查找，或者自己抓包, 这里不提供, 也不要留言索要

### 另附上 xteve 和 plex 的 docker-compose.yml

xteve (国内大佬自建的 XMLTV 服务器: `http://epg.51zmt.top:8000/e.xml`)

```text
version: "3"
services:
  xteve:
    container_name: xteve
    environment:
      - TZ=Asia/Shanghai
    image: alturismo/xteve:latest
    logging:
      driver: json-file
      options:
        max-file: 3
        max-size: 10m
    network_mode: host
    restart: always
    volumes:
      - ./xteve/_config:/config:rw
      - ./tmp/xteve/:/tmp/xteve:rw
      - ./tvheadend/data/:/TVH
      - ./xteve/:/root/.xteve:rw
```

plex (PLEX_CLAIM 要自己获取)

```text
version: "3"
services:
  plex:
    container_name: plex
    environment:
      - PLEX_CLAIM=claim-xxxxxxxxxxxxxxxxxx
      - TZ=Asia/Shanghai
    image: plexinc/pms-docker:latest
    network_mode: host
    restart: always
    volumes:
      - ./config:/config
      - ./data:/data
      - ./transcode:/transcode
```

### 另再附上 portainer 的 docker-compose.yml

```text
version: "3"
services:
 portainer:
  image: portainer/portainer-ce:latest
  container_name: portainer
  restart: always
  volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./data:/data
  ports:
    - 8000:8000
    - 9000:9000
  network_mode: bridge
```

 [https://zhuanlan.zhihu.com/p/433373652](https://zhuanlan.zhihu.com/p/433373652)
