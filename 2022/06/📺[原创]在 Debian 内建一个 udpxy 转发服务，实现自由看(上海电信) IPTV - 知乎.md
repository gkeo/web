# ðº[åå]å¨ Debian åå»ºä¸ä¸ª udpxy è½¬åæå¡ï¼å®ç°èªç±ç(ä¸æµ·çµä¿¡) IPTV - ç¥ä¹
## æèªå·±å®¶éçç½ç»å¾

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-53-08/9ab6b9ec-6dcf-415a-b779-ee0603ac5e10.jpeg?raw=true)

## å¼å¤´ç¹å«è¯´æ

-   æ¬æä¸æ¯ step by step å°ç½æç¨, **ä»æä¾å¤§è´éç½®æµç¨åæ ¸å¿éç½®åæ°**, éè¦å¯¹å¦ä¸åå®¹æä¸å®çåºç¡: Linux, èææº, Docker, åºç¡é®é¢è¯·ä¸è¦çè¨æé®, èªè¡æç´¢
-   æ¬æ¹æ³ç®åéç¨äºä¸æµ·çµä¿¡ç IPTVï¼æªè³åææ¶ 2021-11ï¼ï¼å¶ä»å°åºä»ä¾åè
-   æ¬æåå®¹**ä¸**æ¶å **ç ´è§£ è¿è§è½¯ä»¶ ä½¿ç¨æ¼æ´** ç­ææåå®¹, ææè½¯ä»¶åéç½®æ¹æ³é½æ¯åè§åæ³, ææä¸è½½é¾æ¥, å¼ç¨æç¨é½æ¯å®ç½åå§å°å

## æ¦è¿°

-   å¨ Debian 10 åå»ºä¸ä¸ª udpxy æå¡, ç´æ¥ä¸çµä¿¡åç«ä¸ç IPTV ä¿¡å·å£å¯¹æ¥, æ éæ¨å·, æ éçµä¿¡ç IPTV çå­, å³å¯è½¬å IPTV ç»æ­, åç½ç´æ¥ç¨ .m3u è¡¨è§çï¼å¤ç½éè¿ç«¯å£è½¬åæè xteve + plex è½¬åè§ç, å®ç°å¨ä»»ä½å°æ¹èªç±å°çèªå·±ä¿¡å·æºç IPTV
-   å¤§è´æµç¨: PVE å®è£ Debian 10, å®è£ Docker, Docker Compose, è®¾ç½®ç½ç», å¨ docker é¨ç½²: udpxy(xteve, plex)

> udpxy (_'you-dee-pixie'_) is a data stream relay: it reads data streams from a multicast groups and forwards the data to the requesting clients (subscribers).  
> udpxy æ¯ä¸ä¸ªå° UDP ç»æ­æ°æ®æµåæ TCP åè®®åæ­æµçæµéä¸­ç»§ï¼å®å° UDP æµéä»ç»å®çå¤æ­è½¬åå°è¯·æ±ç HTTP å®¢æ·ç«¯ï¼è®¢éèï¼ã

### å®è£å®¿ä¸»æº PVE

-   `https://www.proxmox.com/downloads`, å·ä½ç¥

### å®è£ Debian Official Cloud Image

-   `https://cloud.debian.org/images/cloud/` ç¨ 10(buster) æ 11(bullseye)
-   ä¸è½½ debian-1x-generic-amd64.qcow2 æè debian-1x-genericcloud-amd64.qcow2 éå
-   å¤§è´æµç¨ï¼å»ºç« VM æº, å é¤ç¡¬ç, ä¸ä¼ éåå°å®¿ä¸»æº /root/ï¼ç¨è¿æ¡æä»¤ `qm importdisk xxx debian-10-genericcloud-amd64.qcow2 local-lvm` (XXX æ¯ VM ç ID) åå»ºç³»ç»ç¡¬çï¼åç¹ç¼è¾ -- æ·»å ç¡¬çï¼åç¹`è°æ´ç£çå¤§å°`ï¼å¢å å¤§å° (å° 10 G å·¦å³å¯)ï¼ç¶å reboot å³å¯ï¼Debian ç³»ç»åä¸ç¨åä»»ä½æä½, èªå¨æ©å®¹
-   å¦æç¨ \*-genericcloud-amd64.qcow2 çéåè¦æ·»å  CloudInit è®¾å¤ï¼åéç½®

### å®è£ Docker, Docker Compose

-   Docker Engine å®æ¹æç¨: `https://docs.docker.com/engine/install/debian/`
-   Docker Compose V2 å®æ¹æç¨: `https://docs.docker.com/compose/cli-command/`, è¿éæ³¨æ: å¨ Debian 10 ä¸ï¼å¨ç¨æ·ç cli-plugins çè·¯å¾æ¯: `/usr/libexec/docker/cli-plugins`, èä¸æ¯è¿ç¯æç¨éç `/usr/local/lib/docker/cli-plugins`, å·ä½å¯ä»¥å»ç Docker Compose ç Github

### åéç½®ä¸ç½ç»

-   æ¬æ¹æ³çç¡¬ä»¶è¿æ¥ï¼ä½ ç Debian 10 èææºè³å°è¦ 2 ä¸ªç½å£ï¼å¶ä¸­ä¸ä¸ªå£ (ens19) ç´æ¥æ¥çµä¿¡åç«åé¢çæ IPTV ä¿¡å·çå£ï¼ç¨äºè·å IPTV çæºä¿¡å·ï¼å¦ä¸ä¸ªå£ (eht0) æ¥äº¤æ¢æºï¼ç¨äºè½¬å udpxy çç»æ­ã
-   ä¸æµ·çµä¿¡æ­æ¾ IPTV åªéè¦è¿å° 23 å¼å¤´ççµä¿¡ä¸ç½å³å¯, æ éæ¨å·, æ ä»»ä½è®¤è¯, æ éçµä¿¡ç IPTV çå­, è¿å»çæ¹æ³å°±æ¯ç½å£ tag ä¸ VLAN 85 å³å¯ï¼è§ä¸å¾ï¼

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-53-08/3c04bce2-d0d9-4617-807c-b04f85d04083.jpeg?raw=true)

-   å¨ Debian 10 åè¾å¥ `ifconfig`, æ¥çä½ ç ens19 ç½å£ (å³ä¸å¾ç vmbr3 èæç½å£) æ¯ä¸æ¯æ¿å°äº 23 å¼å¤´çå°å (ä½ çè¿ä¸ªç½å£åå¯è½ä¸å« ens19, å¯è½å« eth1)

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-53-08/b23db0d4-971c-43c6-9ef4-4d72c7f3195b.jpeg?raw=true)

-   **éè¦ï¼ç½ç»è®¾ç½®ï¼ä¸ç¶ igmp çæµéèµ°ä¸é**

è¾å¥ (æ³¨æï¼æä»¤ä¸­ç ens19 è¯·èªè¡æ¿æ¢ä½ ç IPTV ç½å£å)

```text
sysctl -w net.ipv4.conf.ens19.force_igmp_version=2
sysctl -w net.ipv4.conf.ens19.rp_filter=0
sysctl -w net.ipv4.conf.all.rp_filter=0
```

åè¾å¥, ä¿å­çæ

```text
/sbin/sysctl -p
/sbin/sysctl -w net.ipv4.route.flush=1
```

å é¤è¿åæ¹æ³

```text
sysctl -w net.ipv4.conf.ens19.force_igmp_version=0
sysctl -w net.ipv4.conf.ens19.rp_filter=1
sysctl -w net.ipv4.conf.all.rp_filter=1
/sbin/sysctl -p
/sbin/sysctl -w net.ipv4.route.flush=1
```

### Docker Compose å®è£ udpxy

docker-compose.yml éç½®ï¼

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

å®è£æä»¤: `docker compose up -d`

æ³¨æ `eth0` æ¯æ¥äº¤æ¢æºçå£ï¼æ¯ `ens19` æ¥çµä¿¡åç« IPTV çå£, æ ¹æ®èªå·±å®éæåµä¿®æ¹

udpxyd ç web è®¿é®å°å (`/status/` è¦æå¨): `http://192.168.1.172:4022/status/`

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-53-08/44822f06-444d-480e-9afc-b0f7fb53ae70.jpeg?raw=true)

æ¬å° PC ä¸ç¨ PotPlayer æ­æ¾æ¶ udpxy status æ¾ç¤ºæªå¾

### å°æ­¤å°±å»ºå®äº

è®¿é®å°åæ ¼å¼ï¼`http://192.168.1.172:4022/udp/239.45.3.146:5140`ï¼ç´æ¥å¤å¶è¿ PotPlayer å°±è½ç

> ä¸æµ·çµä¿¡ 239 å¼å¤´ç IPVT åç½å°å, è¯·èªè¡æ¥æ¾ï¼æèèªå·±æå, è¿éä¸æä¾, ä¹ä¸è¦çè¨ç´¢è¦

### å¦éä¸ xteve å plex ç docker-compose.yml

xteve (å½åå¤§ä½¬èªå»ºç XMLTV æå¡å¨: `http://epg.51zmt.top:8000/e.xml`)

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

plex (PLEX_CLAIM è¦èªå·±è·å)

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

### å¦åéä¸ portainer ç docker-compose.yml

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
