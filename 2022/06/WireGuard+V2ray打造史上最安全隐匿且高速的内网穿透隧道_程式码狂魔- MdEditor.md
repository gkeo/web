# WireGuard+V2ray打造史上最安全隐匿且高速的内网穿透隧道_程式码狂魔- MdEditor
## **WireGuard+V2ray 打造史上最安全隐匿且高速的内网穿透隧道**

![](https://mdimg.wxwenku.com/getimg/ccdf080c7af7e8a10e9b88444af98393f01efff0f1441c287d1f4084847d45f89a9178bc53772eb4b82868f486bb4cc8.jpg)

首先讲一下需求，其实需求简单无比，**那就是无论身处何时何地，只要有网际网路，就可以访问内网资源**，说得具体点，比如出差在外想访问家里 NAS 储存的档案，通过直接访问 NAS 的内网 IP（比如 192.168.2.10）就可以实现访问，就好像自己坐在家里一样

## **时代背景**

传统的内网穿透方案只能实现某个埠的穿透，想换个埠又需要重新配置，而在本方案中，整个内网资源都是共享的，只需配置一次即可。缺点是需要一个 **大频宽公网伺服器** 做转发（如果流量使用不大也可使用小频宽公网伺服器），不做转发也需要一个公网伺服器作为 **信令伺服器** 交换各自 IP 做 NAT 打洞操作，但 TCP 打洞难度太大（需要 **三次握手**），一般是 UDP 打洞，但是 UDP 又会被**QoS**，而且打洞成功率还需要受 NAT 型别影响（Full Cone NAT 打洞成功率高，且需要 NAT 两边都是该型别，简而言之该型别 NAT 会 **记住内网主机出去的埠**，如果 NAT 不记住，则埠全靠猜），所以话题绕回来了，想要稳定还是需要一个大频宽公网伺服器做转发。

该方案简单来讲就是**WireGuard over WebSocket over HTTPS**，也是一种**UDP over TCP**的方案。那为什么这么简单的需求需要这么复杂的实现呢？因为需要大频宽公网伺服器做转发，就牵扯出一些列问题，这里简单说一下时代背景：

1.  **国内商用频宽贵**：因为国家政策要求民用频宽量大价低，所以运营商普遍用高价商宽补贴家庭频宽。首先，宽频是运营商管的，就是那三家垄断，国内网路基础设施只有三大运营商有资质建设，阿里云、腾讯云、华为云等资料中心都必须要跟运营商买频宽，价格没法谈，偏贵。所以想想为什么百度云要限速了吧，你下载 1G 的小电影所支援给运营商的频宽费用和百度支付的根本不在一个数量级。其次 IP 资源受美国控制，国内独立 IP 价格更贵。而海外的商用频宽却很便宜，因此大频宽公网伺服器一般选择海外伺服器，既然出国了，那出国流量都会受到婶查的影响。所以这是第二点背景
2.  **网 X 络 X 婶 X 茶**：富强、民主、文明、和谐！@#￥#￥%...，这就需要我们满足 **隐匿** 的需求，隐匿其实是在安全层面之上的，不安全谈何隐匿。
3.  **运营商对 UDP 实施 QoS**：因为 WireGuard 使用的是 UDP，作者也说过使用 UDP 的一大原因是因为 TCP over TCP 效能太糟糕，所以作者其实也考虑了 UDP over TCP 的场景，其次运营商为什么要对 UDP 实施 QoS，在之前的文章**《2021-11-21_5 分钟了解游戏加速器的原理与搭建》**中有详细阐述，总之就是 **控制 UDP 的成本高的离谱**，那还不如一刀切，直接 QoS 掉。

所以这么简单的需求需要这么复杂的实现，背景都是一环套一环的。

## **前置条件**

为便于理解本方案，请按顺序阅读之前的文章

-   **《2021-12-18\_被 Linux 创始人称做艺术品的组网神器——WireGuard》**
-   **《2021-12-24\_一群匿名者打造的强大基础通讯网路构建工具——V2ray》**

## **开局一张图**

![](https://mdimg.wxwenku.com/getimg/6b990ce30fa9193e296dd37902816f4b127383eccdc4358b753ac1669e58343fad3f440380da88d2d785a542b34a9275.jpg)

如上图所示，peer1 和 peer3 为位于 NAT 后的主机，peer2 为位于公网的主机，peer1 和 peer3 部署 wireguard 和 v2ray，peer2 部署 wireguard、v2ray 和一个常规 HTTPS 网站（使用 nginx 作为 web 伺服器），整个服务对外只暴露 443 埠，非常安全。

名词约束：

-   peer1 中的 wireguard 称为 w1，v2ray 称 v1，同理 w2，w3 分别是 peer1 和 peer3 中的 wireguard，v2，v3 分别是 peer1 和 peer3 中的 v2ray

本方案整体流程为

1.  w3 将 udp 资料传送给监听在本地某埠的 v3，此处为内网环境
2.  v3 作为客户端请求 nginx，并携带约定好的 path，**此处为公网环境，对外看来就是普通 HTTPS 流量**
3.  nginx 接收到 v3 的请求后，发现其携带约定好的 path，通过 location 配置，将该请求升级为 Websocket 长连线，并且将请求转发给 v2。
4.  v2 接收到请求后发现是 v3 传送，并且是 dokodemo-door 任意门协议，于是将流量传送到指定埠，被 w2 接收

通过上面 4 步，建立起了 peer3 到 peer2 的连线，同理重复上面 4 步，建立起 peer1 到 peer2 的连线，于是组网成功。

所以它是如何满足安全隐匿且高速的需求？

-   安全：安全自不用说，如今网际网路上 TLS 已成为主流，是整个网际网路的安全基石。
-   隐匿：在安全的基础之上，在 HTTPS 流量中包含 WireGuard 流量，流量经过外层 HTTPS 加密后在经过 WireGuard 加密，自然是隐匿的
-   高速：理论上 UDP 是快于 TCP 的，但是由于 QoS 的存在，高速从何谈起？但是将 UDP 封装在 TCP 中，相较于被 QoS 的 UDP，那自然是高速的。
-   此方案还有一个好处是伺服器故障可自动切换，假设另外一台和 peer2 配置相同，当 peer2 故障时，在 DNS 控制台处将域名指向的 IP 切换到新 IP 即可实现故障自愈（自愈时间视 DNS 更新时间而定）

## **环境准备**

    3 .68 .156 .128 
    fakesite .com

假设三台主机均为 CentOS 作业系统

## **peer2 配置**

### **HTTPS 站点搭建**

首先 peer2 上需要有一个正常访问的 HTTPS 网站，如果已经有一台伺服器并且配置了 HTTPS 网站，直接使用即可

1.  在域名控制台将域名 DNS 新增 A 记录，指向 peer2 的公网 IP
2.  在 peer2 安装 nginx


    yum -y install epel- release yum -y install nginx

1.  配置网站，写入如下配置


    cat > /etc/nginx /conf.d/fakesite .com.conf <<EOF server { # 和申请的域名一致 server\_name fakesite.com;
        access\_log /etc/nginx/fakesite.com.log main; ​ location / { root /usr/share/nginx/html; index index.html index.htm; }
        error\_page 500 502 503 504 /50x.html; location = /50x.html { root /usr/share/nginx/html; } ​ listen 80; } EOF

1.  启动`nginx`（输入 `nginx` 命令即可）并访问，`curl http://fakesite.com`有返回说明成功
2.  配置 HTTPS，使用 certbot 配置 HTTPS，配置好之后 `curl https://fakesite.com` 有返回说明成功


     yum -y install epel-release yum -y install certbot-nginx ​
     certbot --nginx \-d fakesite.com \-d www.fakesite.com

1.  配置 v2ray 访问 path，在 HTTPS 配置好之后，配置档案会自动加上证书相关配置


    server {  server\_name fakesite.com;
        access\_log /etc/nginx/fakesite.com.log main; ​ location / { root /usr/share/nginx/html; index index.html index.htm; }
        error\_page 500 502 503 504 /50x.html; location = /50x.html { root /usr/share/nginx/html; } ​ listen 443 ssl;  ssl\_certificate /etc/letsencrypt/live/fakesite.com/fullchain.pem; ssl\_certificate\_key /etc/letsencrypt/live/fakesite.com/privkey.pem;
        include /etc/letsencrypt/options-ssl-nginx.conf; ssl\_dhparam /etc/letsencrypt/ssl-dhparams.pem; ​ }

在此基础之上，需要配置 v2ray 访问的特定 path，该 path 越复杂越好，此处以 `someRandomPathHere` 为例，如下所示：

-   配置后端 v2ray 的 path
-   配置后端 v2ray 的埠
-   配置后端 v2ray 监听在**127.0.0.1**，因为不提供外部访问，只供 nginx 访问


    server { ... 

         location /someRandomPathHere { proxy\_redirect off;
             proxy\_pass http: proxy\_http\_version 1.1 ;  proxy\_set\_header Upgrade \\$http\_upgrade; proxy\_set\_header Connection "upgrade" ; proxy\_set\_header Host \\$http\_host; }
     ... }

### **配置 v2ray**

v2ray 配置如下，几点需要注意

-   `"listen": "127.0.0.1"`：监听在`127.0.0.1`
-   `"port": 1090`：监听在 1090 埠，和 nginx`proxy_pass`配置一致
-   `"path": "/someRandomPathHere"`：path 和 nginx`location`配置一致


    mkdir -p ~ /v2ray-wg cat > ~/v 2ray-wg/config.json <<EOF {
        "log" : { "loglevel" : "debug" },
        "dns" : { "servers" : \[ "1.1.1.1" , "8.8.8.8" , "8.8.4.4" \] },
        "inbounds" : \[ {
                "listen" : "127.0.0.1" , "port" : 1090 , "protocol" : "vless" , "settings" : { "clients" : \[ {
                            "id" : "c71c5890-56dd-4f32-bb99-3070ec2f20fa" } \],
                    "decryption" : "none" },
                "streamSettings" : { "network" : "ws" , "wsSettings" : { "path" : "/someRandomPathHere" } } } \],
        "outbounds" : \[ {
                "protocol" : "freedom" , "settings" : { "domainStrategy" : "UseIP" } } \] } EOF ​ ​
     v2ray -c ~ /v2ray-wg/config .json ​

启动 v2ray 之后，访问该 path，如果得到一个**Bad Request**说明在 nginx 对接 v2ray 成功

    $ curl https: / /fakesite.com/some RandomPathHere Bad Request

### **配置 wireguard**

wireguard 配置如下，有几点需要注意

-   `Address = 5.5.5.1/24`：指 peer2 的 wireguard 自用内网 IP
-   `ListenPort = 10000`：wireguard 监听埠，很遗憾目前 wireguard 只能监听在 `0.0.0.0` 即所有网路装置介面上，不能监听在 `127.0.0.1` 上，但是预设情况下即使监听在了 eth0 上也需要配合 iptables 开放埠，所以也是安全的
-   iptables 规则中的 eth0 网络卡根据实际名称修改


    cat > /etc/wireguard/wg.conf<<EOF \[Interface\] Address = 5.5.5.1/24 PostUp = iptables -I FORWARD -i %i -j ACCEPT PostUp = iptables -I FORWARD -o %i -j ACCEPT PostUp = iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE PostDown = iptables -D FORWARD -i %i -j ACCEPT PostDown = iptables -D FORWARD -o %i -j ACCEPT PostDown = iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE ListenPort = 10000 PrivateKey = aHWomHYVWebT+lAPyZcofEfdQYXdFOXpVWRKD91OyXA= ​
     \[Peer\] PublicKey = 451o6In0DqSTyg1GE4WzrK4Z0BLuFXTrjdqjBJ/RLwc= AllowedIPs = 5.5.5.2/32,192.168.2.0/24 ​ ​
     \[Peer\] PublicKey = QOY2VaTZLtof4rtSrTS42d2Ld8vOc8hJwNcJu9DA8h8= AllowedIPs = 5.5.5.3/32 EOF

启动 wireguard

    wg-quick up wg

检视 wg 资讯，此时还没有 peer 连线上来：

    $ wg interface: wg public key: 1yrnzlpVNhpQyyBj0oehVqCaL/06RjK/tcd3icMuZ0Q=
      private key: (hidden) listening port: 10000 ​
    peer: 451o6In0DqSTyg1GE4WzrK4Z0BLuFXTrjdqjBJ/RLwc= allowed ips: 5.5.5.2/32, 192.168.2.0/24 ​
    peer: QOY2VaTZLtof4rtSrTS42d2Ld8vOc8hJwNcJu9DA8h8= allowed ips: 5.5.5.3/32 ​

## **peer1 配置**

### **配置 v2ray**

peer1 上 dev2ray 配置有以下注意点

-   在入站中配置任意门协议，且远端主机（即 peer2）埠为 10000，和 peer2 中的 wireguard 监听埠一致，这样才会往 wireguard 传送资料；且协议一定要是 udp，因为 wireguard 只支援 udp 协议
-   `"address": "fakesite.com"`：指向 peer2 的网站，埠为 443
-   `security":"tls"`：**需要启用 tls**，因为是作为客户端去访问 https 站点！上面 peer2 中不需要启用 tls 是因为流量已经在**nginx 处解密** 了，到了 v2ray 就已是明文流量。


    mkdir -p ~/v2ray-wg \# 建立配置档案 cat > ~/v2ray-wg/config.json <<EOF {
        "log" : { "loglevel" : "debug" },
        "inbounds" : \[ { 
                
                
                "listen" : "127.0.0.1" , 
                "port" : 10000 , 
                "protocol" : "dokodemo-door" , "settings" : { 
                    "address" : "127.0.0.1" , 
                    "port" : 10000 , 
                    "network" : "udp" } } \],
        "outbounds" : \[ {
                "protocol" : "vless" , "settings" : { "vnext" : \[ {
                            
                            "address" : "fakesite.com" , "port" : 443 , "users" : \[ {
                                    "id" : "c71c5890-56dd-4f32-bb99-3070ec2f20fa" , "encryption" : "none" } \] } \] },
                "streamSettings" : { "network" : "ws" , 
                    "security" : "tls" , "wsSettings" : { "path" : "/someRandomPathHere" } } } \] } EOF ​
    \# 启动v2ray v2ray -c ~/v2ray-wg/config.json

### **配置 wireguard**

peer1 上的 wireguard 配置如下，有几点需要注意

-   `Address = 5.5.5.2/24`：指 peer1 的 wireguard 自用内网 IP
-   iptables 规则中的 eth0 网络卡根据实际名称修改
-   `Endpoint = 127.0.0.1:10000`：配置 peer2 的连线为`127.0.0.1:10000`，因为此时流量已经被本地的 v2ray 接管了，传送给 peer1`127.0.0.1:10000`的流量都将从 v2ray 的任意门协议到达 peer2 上的`127.0.0.1:10000`，peer2 上已经配置了 wireguard 去接收该流量。
-   `AllowedIPs = 5.5.5.0/24`：整个内网流量全部转发给 peer2
-   `PersistentKeepalive = 15`：每隔 15s 就给 peer2 传送心跳包，保持连线（因为 NAT 下，peer1 的公网 IP 和埠不定时在变化）


    cat > /etc/wireguard/wg.conf<<EOF \[Interface\] Address = 5.5.5.2/24 PostUp = iptables -I FORWARD -i %i -j ACCEPT PostUp = iptables -I FORWARD -o %i -j ACCEPT PostUp = iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE PostDown = iptables -D FORWARD -i %i -j ACCEPT PostDown = iptables -D FORWARD -o %i -j ACCEPT PostDown = iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE PrivateKey = oAaT5OjURGvVqs/pbMa2HAsZXpbwNQCEzW0MZBmGJ1Y= ​
     \[Peer\] PublicKey = 1yrnzlpVNhpQyyBj0oehVqCaL/06RjK/tcd3icMuZ0Q= AllowedIPs = 5.5.5.0/24 Endpoint = 127.0.0.1:10000 PersistentKeepalive = 15 EOF

启动 wireguard

    wg-quick up wg

### **测试**

检视 peer1 上 wg 资讯，发现已经连上 peer2 了

    $ wg interface: wg public key: 451o6In0DqSTyg1GE4WzrK4Z0BLuFXTrjdqjBJ/RLwc=
      private key: (hidden) listening port: 37301 ​
    peer: 1yrnzlpVNhpQyyBj0oehVqCaL/06RjK/tcd3icMuZ0Q= endpoint: 127.0.0.1:10000 allowed ips: 5.5.5.0/24 latest handshake: 28 seconds ago transfer: 92 B received, 212 B sent persistent keepalive: every 15 seconds ​

再去 peer2 上检视 wg 资讯，发现 peer1 也已经连上了

    $ wg interface: wg public key: 1yrnzlpVNhpQyyBj0oehVqCaL/06RjK/tcd3icMuZ0Q=
      private key: (hidden) listening port: 10000 ​
    peer: 451o6In0DqSTyg1GE4WzrK4Z0BLuFXTrjdqjBJ/RLwc= endpoint: 127.0.0.1:48832 allowed ips: 5.5.5.2/32, 192.168.2.0/24 latest handshake: Now transfer: 616 B received, 184 B sent ​
    peer: QOY2VaTZLtof4rtSrTS42d2Ld8vOc8hJwNcJu9DA8h8= allowed ips: 5.5.5.3/32

在 peer1 上 ping peer2 的内网 IP，发现可以 ping 通

    $ ping 5.5 .5 .1 PING 5.5 .5 .1 ( 5.5 .5 .1 ) 56 ( 84 ) bytes of data. 64 bytes from  5.5 .5 .1 : icmp\_seq= 1 ttl= 64 time= 220 ms 64 bytes from  5.5 .5 .1 : icmp\_seq= 2 ttl= 64 time= 71.5 ms 64 bytes from  5.5 .5 .1: icmp\_seq= 3 ttl= 64 time= 70.4 ms

## **peer3 配置**

### **配置 v2ray**

peer3 和 pee1 v2ray 的配置是一模一样的，实际上，除了 peer2，也就是位于公网的主机以外，其他 peer 的 v2ray 配置都是相同的，目的都是为了给 wireguard 搭建好底层隧道，暴露 `127.0.0.1:10000` 给 wireguard，此处就不再赘述，配置好后启动即可。

### **配置 wireguard**

peer3 上的 wireguard 配置如下，有几点需要注意

-   `Address = 5.5.5.3/24`：指 peer3 的 wireguard 自用内网 IP
-   因为 peer3 在该场景下指客户端，需要访问 peer1 的内网资源，所以不需要配置 iptables 转发规则
-   `AllowedIPs = 5.5.5.0/24,192.168.2.0/24`：整个内网流量全部转发给 peer2，并且包含 `192.168.2.0/24` 网段，该网段转发给 peer2 后 peer2 再转发给 peer1，peer1 在转发给和 peer1 同区域网下的主机，不清除可参考之前的文章：**《2021-12-18\_被 Linux 创始人称做艺术品的组网神器——WireGuard》**
-   `PersistentKeepalive = 15`：每隔 15s 就给 peer2 传送心跳包，保持连线（因为 NAT 下，peer3 的公网 IP 和埠不定时在变化）


    cat > /etc/wireguard/wg.conf<<EOF \[Interface\] Address = 5.5.5.3/24 PrivateKey = AA7GomiYl60DW5ZAGgn7VlwGWX8/Jw74qiYWPpknGWQ= ​ ​ \[Peer\] PublicKey = 1yrnzlpVNhpQyyBj0oehVqCaL/06RjK/tcd3icMuZ0Q= AllowedIPs = 5.5.5.0/24,192.168.2.0/24 Endpoint = 127.0.0.1:10000 PersistentKeepalive = 15 EOF

启动 wireguard

    wg-quick up wg

### **测试**

首先在 peer2 上看 peer3 是否连线上了，如下所示为连线上的情况

    $ wg interface: wg public key: 1yrnzlpVNhpQyyBj0oehVqCaL/06RjK/tcd3icMuZ0Q=
      private key: (hidden) listening port: 10000 ​
    peer: 451o6In0DqSTyg1GE4WzrK4Z0BLuFXTrjdqjBJ/RLwc= endpoint: 127.0.0.1:59366 allowed ips: 5.5.5.2/32, 192.168.2.0/24 latest handshake: 2 seconds ago transfer: 4.27 KiB received, 1.61 KiB sent ​
    peer: QOY2VaTZLtof4rtSrTS42d2Ld8vOc8hJwNcJu9DA8h8= endpoint: 127.0.0.1:37201 allowed ips: 5.5.5.3/32 latest handshake: 18 seconds ago transfer: 424 B received, 184 B sent ​

此时在 peer3 上分别 ping peer1（5.5.5.1）和 peer2（5.5.5.2），能 ping 通说明至少 wireguard 的内网通了，然后在 ping 需要 peer1 转发的内网 ip，如 192.168.2.5，如果能通说明 peer1 的转发是没问题的

在保险一点可以 telnet 埠，在主机 192.168.2.5 上监听一个 tcp 埠 1024

    nc -lvp 1024

然后在 peer3 上 telnet，能通说明隧道通讯成功，如下为成功的输出：

    $ telnet 192.168 .2 .5  1024 Trying 192.168 .2 .5 ... Connected to 192.168 .2 .5 . Escape character is  '^\]' .

## **埠梳理**

整个连线流程一共涉及到 4 个埠，如下图所示，**其中暴露在公网的只有 443 埠！**, 虽然 wireguard 也监听在了`0.0.0.0:10000`，但是一般需要设定 iptables 规则才能暴露在公网，后续也会介绍到，可以将 peer2 中执行的 v2ray 和 wireguard 关进 docker 里面，隔离网路。

    0 .0 .0 .0 :10000

![](https://mdimg.wxwenku.com/getimg/ccdf080c7af7e8a10e9b88444af983930997167033df082d9cd392ac590a01a0675645962d08b86ed961c4d6ad3caa70.jpg)

## **结合 docker**

其中 peer2 可以将 v2ray 和 wireguard 关进 docker 中，因为 peer2 纯做流量转发，不需要共享 peer2 上的内网资源，而 peer1 和 peer3 可将 v2ray 执行在 docker 中，但是不建议将 wireguard 执行在 docker 中，因为 docker 实际上有一层网路隔离，如果 wireguard 执行在 docker 中，只有该容器本身会接入 wireguard 的内网中，但是宿主机的网路并没不会接入，但实际场景中 peer1 一般为家庭内网中的一台 linux 节点（如路由器等）也可以将 wireguard 执行在 docker 中。

总之一句话，需要宿主机加入 wireguard 网路，则在宿主机上执行，如果只是纯粹做流量转发，则可以在 docker 中执行 wireguard。

将 wireguard 执行在 docker 中还有一个好处，正是由于 docker 的网路隔离，所以 wireguard 监听在 0.0.0.0 上而不用担心会不小心暴露在公网！

**下面的例是将 peer2 中的 wireguard 和 v2ray 执行在 docker 中**

### **建立网桥**

建立网桥用于自定义容器 IP

    docker network create --subnet= 172.18.0.0 / 16 mynet

### **执行 v2ray**

容器中 v2ray 的配置只有监听在 `"listen": "0.0.0.0"` 这个区别，且一定要监听在`0.0.0.0`，监听在 `127.0.0.1` 只能在容器内部访问，**容器之间无法访问**。

配置 v2ray，需要注意

-   `"listen": "0.0.0.0"`：docker 内部网路是隔离的，需要监听在 0.0.0.0


    mkdir -p ~/v2ray-wg cat > ~/v2ray-wg/config.json <<EOF {
        "log" : { "loglevel" : "debug" },
        "dns" : { "servers" : \[ "1.1.1.1" , "8.8.8.8" , "8.8.4.4" \] },
        "inbounds" : \[ {
                
                "listen" : "0.0.0.0" , "port" : 1090 , "protocol" : "vless" , "settings" : { "clients" : \[ {
                            "id" : "c71c5890-56dd-4f32-bb99-3070ec2f20fa" } \],
                    "decryption" : "none" },
                "streamSettings" : { "network" : "ws" , "wsSettings" : { "path" : "/someRandomPathHere" } } } \],
        "outbounds" : \[ {
                "protocol" : "freedom" , "settings" : { "domainStrategy" : "UseIP" } } \] } EOF

执行 v2ray，指定固定为`172.18.0.10`，此处很重要

-   意味着 nginx`location`处配置也需要指定该 IP


    docker run -d \\ \--name v2ray-wg \\ 
    \--network mynet \\ 
    \--ip 172.18.0.10 \\ 
    \--restart=always \\ 
    \-v ~/v2ray-wg/config.json:/etc/v2ray/config.json \\ v2fly/v2fly-core ​ # 如需删除，使用下面命令 # docker stop v2ray-wg && docker rm v2ray-wg

检视日志，有类似于 `creating stream worker on 0.0.0.0:1090` 的输出说明成功。

    $ docker logs -f v2ray-wg ​ app/dns: DNS: created UDP client initialized for  1.1 .1 .1 : 53 
    2021 / 12 / 26  06 : 03 : 37 \[Info\] app/dns: DNS: created UDP client initialized for  8.8 .8 .8 : 53 
    2021 / 12 / 26  06 : 03 : 37 \[Info\] app/dns: DNS: created UDP client initialized for  8.8 .4 .4 : 53 
    2021 / 12 / 26  06: 03 : 37 \[Debug\] app/proxyman/inbound: creating stream worker on  0.0 .0 .0 : 1090 
    2021 / 12 / 26  06 : 03 : 37 \[Info\] transport/internet/websocket: listening TCP( for WS) on  0.0 .0 .0 : 1090

nginx 处 location 配置修改，`proxy_pass`需修改为`http://172.18.0.10:1090`

    server { ... 

         location /someRandomPathHere { proxy\_redirect off;
             proxy\_pass http: proxy\_http\_version 1.1 ;  proxy\_set\_header Upgrade \\$http\_upgrade; proxy\_set\_header Connection "upgrade" ; proxy\_set\_header Host \\$http\_host; }
     ... }

### **执行 wireguard**

wireguard 配置和 docker 外一致

     mkdir -p ~/wg
     cat > ~/wg/wg.conf<<EOF \[Interface\] Address = 5.5.5.1/24 PostUp = iptables -I FORWARD -i %i -j ACCEPT PostUp = iptables -I FORWARD -o %i -j ACCEPT PostUp = iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE PostDown = iptables -D FORWARD -i %i -j ACCEPT PostDown = iptables -D FORWARD -o %i -j ACCEPT PostDown = iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE ListenPort = 10000 PrivateKey = aHWomHYVWebT+lAPyZcofEfdQYXdFOXpVWRKD91OyXA= ​
     \[Peer\] PublicKey = 451o6In0DqSTyg1GE4WzrK4Z0BLuFXTrjdqjBJ/RLwc= AllowedIPs = 5.5.5.2/32,192.168.2.0/24 ​ ​
     \[Peer\] PublicKey = QOY2VaTZLtof4rtSrTS42d2Ld8vOc8hJwNcJu9DA8h8= AllowedIPs = 5.5.5.3/32 EOF

执行 wireguard，指定固定 IP 为`172.18.0.11`，此处很重要

-   意味着客户端（即 peer1 或 peer3）v2ray 的任意门协议远端 IP 指定该 IP


    docker run -d \\ \--name=wg \\ 
    \--cap-add=NET\_ADMIN \\ 
    \--cap-add=SYS\_MODULE \\ 
    \-e PUID=1000 \\ 
    \-e PGID=1000 \\ 
    \-v ~/wg/wg.conf:/config/wg0.conf \\ 
    \-v /lib/modules:/lib/modules \\ 
    \--network mynet --ip 172.18.0.11 \\ 
    \--sysctl="net.ipv4.conf.all.src\_valid\_mark=1" \\ 
    \--restart always \\ linuxserver/wireguard ​ # 如需删除，使用下面命令 # docker stop wg && docker rm wg

检视日志输出，如下输出即为成功。

    $ docker logs \-f wg .... Warning: \`/config/wg0.conf' is world accessible
    \[# \] ip link add wg0 type wireguard 
    \[# \] wg setconf wg0 /dev/fd/63 
    \[# \] ip -4 address add 5.5.5.1/24 dev wg0 
    \[# \] ip link set mtu 1420 up dev wg0 
    \[# \] ip -4 route add 192.168.2.0/24 dev wg0 
    \[# \] iptables -I FORWARD -i wg0 -j ACCEPT 
    \[# \] iptables -I FORWARD -o wg0 -j ACCEPT 
    \[# \] iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE

### **客户端 v2ray 任意门协议修改**

因为 peer2 中 wireguard 固定 IP 有所变化，所以客户端（peer1 或 peer3）中 v2ray 的任意门协议需要的远端主机 IP 需要与之一致，即需设定为`172.18.0.11`

以 peer1 中为例

-   `"address": "172.18.0.11"`：传送远端主机（即 peer2）的 IP，此 IP 为 peer2 容器中 wireguard 的 IP


    mkdir -p ~/v2ray-wg \# 建立配置档案 cat > ~/v2ray-wg/config.json <<EOF {
        "log" : { "loglevel" : "debug" },
        "inbounds" : \[ { 
                
                
                "listen" : "127.0.0.1" , 
                "port" : 10000 , 
                "protocol" : "dokodemo-door" , "settings" : { 
                    "address" : "172.18.0.11" , 
                    "port" : 10000 , 
                    "network" : "udp" } } \],
        "outbounds" : \[ {
                "protocol" : "vless" , "settings" : { "vnext" : \[ {
                            
                            "address" : "fakesite.com" , "port" : 443 , "users" : \[ {
                                    "id" : "c71c5890-56dd-4f32-bb99-3070ec2f20fa" , "encryption" : "none" } \] } \] },
                "streamSettings" : { "network" : "ws" , 
                    "security" : "tls" , "wsSettings" : { "path" : "/someRandomPathHere" } } } \] } EOF ​
    \# 启动v2ray v2ray -c ~/v2ray-wg/config.json

修改后重启 v2ray 即可。

### **埠和 IP 梳理**

红色部分即为 IP 指定变化的部分

![](https://mdimg.wxwenku.com/getimg/6b990ce30fa9193e296dd37902816f4b707d9cdeabbf64e26b4d20b38561d753af59823e959a7c111fb6de03204d65af.jpg)

## **注意事项**

-   文中 v2ray 配置中的 UUID 记得更改，请勿照抄
-   文中 v2ray 配置中的 Path 记得更改，请勿照抄
-   文中 wireguard 公钥私钥记得更改，请勿照抄

如果上面列出的都照抄，那么攻击者只需要知道你的网站域名，就可以访问你的内网资源，精心搭建的 HTTPS 隧道安全性不攻自破，切记切记。

-   伺服器域名切换故障自愈

peer2 可配置多个相同节点，当其中一个故障后，在 DNS 控制台切换到其他节点，但此时需要注意有一个域名请求死回圈，这种情况发生在 peer3 在 wireguard 中配置了 DNS 伺服器的情况下，如下

    \[Interface\] Address = 5.5.5.3/24 PrivateKey = AA7GomiYl60DW5ZAGgn7VlwGWX8/Jw74qiYWPpknGWQ=
     DNS = 192.168.2.1 ...

当 peer2 故障域名切换到另一台相同节点后，v2ray 会去请求域名指向的新 IP，此时会将域名请求传送给`192.168.2.1`，很明显此时 `192.168.2.1` 是不通的，因为 v2ray 连上并且 wireguard 工作正常后此内网 ip 才通，所以请求直接超时，解决办法有两种

-   暂时遮蔽掉 wireguard 的 DNS 配置，让 DNS 请求走本来的通道，当和 peer2 连线上之后再放开该配置
-   每次 v2ray 启动后主动去请求一下，迫使 v2ray 先请求域名，然后在启动 wireguard

## **参考**

-   [https:// hub.docker.com/r/v2fly/ v2fly-core](https://www-gushiciku-cn.translate.goog/jump/aHR0cHM6Ly9saW5rLnpoaWh1LmNvbS8/dGFyZ2V0PWh0dHBzJTNBLy9odWIuZG9ja2VyLmNvbS9yL3YyZmx5L3YyZmx5LWNvcmU=?_x_tr_sl=zh-TW&_x_tr_tl=zh-CN&_x_tr_hl=zh-CN&_x_tr_pto=sc)
-   [https:// hub.docker.com/r/linuxs erver/wireguard](https://www-gushiciku-cn.translate.goog/jump/aHR0cHM6Ly9saW5rLnpoaWh1LmNvbS8/dGFyZ2V0PWh0dHBzJTNBLy9odWIuZG9ja2VyLmNvbS9yL2xpbnV4c2VydmVyL3dpcmVndWFyZA==?_x_tr_sl=zh-TW&_x_tr_tl=zh-CN&_x_tr_hl=zh-CN&_x_tr_pto=sc) 
    [https://www-gushiciku-cn.translate.goog/pl/assy/zh-tw?\_x_tr_sl=zh-TW&\_x_tr_tl=zh-CN&\_x_tr_hl=zh-CN&\_x_tr_pto=sc](https://www-gushiciku-cn.translate.goog/pl/assy/zh-tw?_x_tr_sl=zh-TW&_x_tr_tl=zh-CN&_x_tr_hl=zh-CN&_x_tr_pto=sc)
