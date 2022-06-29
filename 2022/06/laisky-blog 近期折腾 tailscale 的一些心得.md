# laisky-blog: 近期折腾 tailscale 的一些心得
> Changelog:
>
> -   2021/11/17: 改进 tailscale 的介绍
> -   2022/3/10: 增加 ACL tags 的介绍

![](https://s3.laisky.com/uploads/2021/10/Tailscale-Logo-Black.jpg)

## 一、我的需求

最近自己装了一台高配台式机，打算当作自己的主力机器使用，笔记本电脑就作为一个移动 UI，工作都远程连到家里台式机使用。

为了满足这个需求，就需要 NAT 穿透，正好最近 mesh vpn 的概念很火，又看到 tailscale 正好对个人用户免费，于是就想试试看了。

## 二、TailScale 是什么

VPN 是什么？

虽然国人看到 VPN 第一反应应该是 “翻墙”，但 VPN 最初应该也是最普遍的用途应该是用来做内网打通， 这也是其名字虚拟私有网络的用意，VPN 让你可以在公开的网络线路上建立一个私有的子网， 然后将所有接入的机器都分配一个私有的内网地址，让他们可以通过 VPN 的私有网络互联。

比如最常见的需求就是，公司有一个内网办公环境，当你外出办公时，也希望你的电脑能够接入办公网络。 因为外网的机器和内网的机器不能互联，所以一般会有一个中心服务器， 所有的子节点都和中心服务器相连，然后中心服务器转发所有的流量。

![](https://s3.laisky.com/uploads/2021/11/hub.png)

这样做的缺点显而易见，首先是中心服务器（hub）会成为瓶颈。 其次，某种极端情况下，如果节点 A 和 节点 B 距离非常近，但是都离 hub 很远， 这样就会导致非常高的延迟。

![](https://s3.laisky.com/uploads/2021/11/hub-2.png)

那么我们就会想，能不能让节点间直接互联呢？ 这就是 mesh VPN，其实现就是 wireguard。

![](https://s3.laisky.com/uploads/2021/10/scaleway-wireguard_mesh.png)

wireguard 的每一个节点都会存储其他所有节点的信息，并且和其他所有的节点都建立 tls 连接。 如果涉及到内网穿透的话，那么你需要找到一台处于网关位置的节点（内外网都可达），将其设置为 coordinator， 扮演类似于 hub 的角色， 分发穿透内外网的流量。

wireguard 的缺点在于：

-   配置比较繁琐
-   维护也比较困难，增删节点都需要改动所有节点的配置

基于上述这些痛点，tailscale 做了一些改进：

1.  在原有的 ICE、STUN 等 UDP 协议外，实现了 DERP TCP 协议来实现 NAT 穿透
2.  基于公网的 coordinator 服务器下发 ACL 和配置，实现节点动态更新
3.  通过第三方（如 Google） SSO 服务生成用户和私钥，实现身份认证

简而言之，我们可以认为 tailscale 是更为易用版的、功能封装更为完善的 wireguard。

## 三、TailScale 能做到什么

只要你的机器可以连到公网，tailscale 可以让所有的机器连接到同一个私有子网内。 你可以像在同一个局域网里那样，随时随地的连接你的任意设备。

比如现在我的台式机和我的笔记本都登录了同一个 tailscale 账号， 它们都共享同一个 `100.64/10` 的子网，也就是可以随时随地的互联。 即使我的笔记本在公司内网之中，无法和家里台式机直连，但是依靠 relay 转发流量， 两者依然可以畅通无阻的直接连接。

### 1、传输文件

tailscale 内置 taildrop，可以在设备间传输文件，因为 tailscale 支持 android/ios/mac/windows/linux，所以实际上这也是一个很好用的全平台文件传输工具。 而且如果设备处在同一个局域网的话，传输速度也会非常快。

### 2、远程开发

我家台式机安装的是 windows 系统，我启用了 wsl2，然后安装了 systemd、sshd。 笔记本上通过 vscode remote ssh 随时随地打开台式机上的 vscode server 进行远程开发。

这样做的一大优点是，台式机硬件的售价非常便宜，比如金士顿 32G 内存只要 880， 金士顿 1TB M2 SSD 也只要 800。WD 4TB 蓝盘不到 1 千，18 TB 黑盘也只要 2k。 也就是说，你可以用很低的成本组装一台超强配置的机器。 而同样的价钱去选购笔记本的话，只能买到很差的配置。

我认为移动办公的精髓不是你抱着一台笔记本到处移动， 而应该是无论你在哪里，只要有网，就可以用任何设备接入一个统一的办公环境。

### 3、代理

tailscale 节点间是点对点 tls 连接，所以实际上也可以用来做网络代理。 比如我在境外服务器上安装一个 tailscale 的节点，然后再在境外服务器上安装一个 [cow](https://github.com/cyfdecyf/cow)（用来做 http proxy server）。 那我就可以在任意一台机器上，访问境外服务器的 tailscale 子网 IP + cow 端口的形式实现 HTTP/SOCKS 代理。

cow 的安装很简单：

```null
curl -L git.io/cow | bash
```

我选择将其安装到 `/usr/local/bin/cow`，然后配置文件 `/home/laisky/.cow/rc` 只需要一行：

```null
listen = http://100.118.165.101:17777
```

可以将其添加进 systemd：

```null
sudo vi /etc/systemd/system/cow.service

[Unit]
Description=cow service
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=laisky
ExecStart=/usr/local/bin/cow -c /home/laisky/.cow/rc

[Install]
WantedBy=multi-user.target
```

用 cow 当代理服务器是因为安装最方便，再说我此前本来也在用 cow，就顺便了。

## 四、安装、部署

参照这个官方页面安装，然后登录即可：[https://tailscale.com/download](https://tailscale.com/download)

### 1、自建中继

tailscale 提供的 relays 数量有限，而且全部在国外。 你也可以自建中继。

#### 安装 derper

自建中继服务器被称为 derper，是用 go 开发的，建议先安装 go 环境。

go 是预编译的，安装起来很简单，下载、解压即可。

拿 AMD64 linux 举例：

```null
wget https://golang.org/dl/go1.17.2.linux-amd64.tar.gz
tar -xzf go1.17.2.linux-amd64.tar.gz
sudo mv go /opt/go
sudo ln -s /opt/go/bin/* /usr/local/bin/

export GOROOT=/opt/go
export GOPATH=/home/laisky/.go
go version
```

其他安装方式参考：[https://golang.org/doc/install](https://golang.org/doc/install)

derper 的安装方式为：

```null
# 安装
go install tailscale.com/cmd/derper@main

# 启动
sudo derper -c=/root/derper.conf -hostname=xx.xx.xx -a=:443 -stun
```

启动 derper 的参数：

-   `-hostname`：有效的公网域名，derper 会自动为这个域名申请 letsencrypt tls 证书。
-   `-a`：指定 derper 监听的 tcp 端口，默认为 443，修改为其他端口的话似乎转发流量会有问题。
-   `-stun`：stun 协议的 udp 端口，health check 的时候会用到。

需要注意的是，因为 derper 会申请公网 TLS 证书，如果你的服务器在国内，那么域名必须要备案。 而且由于中国封锁了 letsencrypt，所以你的 derper 在启动一会儿后，很可能会报这样的错：

```null
http: TLS handshake error from client-a-ip:34379: acme/autocert: missing certificate
```

上述错误表示证书申请失败，如果你有自己的证书的话，可以用 `-certmode=manual -certdir=xxx` 的形式指定证书文件夹。

顺带一提，如腾讯云这样的国内云服务器提供商，会审查 TLS handshake 里的 SNI 信息， 如果发现 SNI 域名未备案，会阻断 TLS 握手。 所以，我曾经也尝试过用一个境外服务器的域名，然后指向到境内服务器 IP 的形式尝试绕过备案， 然而短暂的用了几分钟后，就遇到了 tls handshake reset 的问题。 目前看来，只要你想用境内服务器，那么备案就是绕不过的问题。

Ps. 如果你有备案域名，那么可以用 nginx stream 反向代理的形式做 443 端口 4 层转发。 nginx stream 可以在四层探测 SNI 信息，然后分发到不同的后端，这样你 derper 的 443 和其他域名的 443 就可以共存在同一个服务器上了。

Ps2. 实际用下来之后，发现建设国内 derper 实际上也有问题。虽然因为境内延迟超低，在国内做穿透的时候会非常得爽。 但是如果要做跨境穿透，因为境内的 derper 和境外的 relays 通信也有干扰，很可能导致境内外的两个节点始终无法协商到同一个 relay 上。 综合而言，如果你仅有境内穿透的需求，那么搭一个境内 derper 会很爽；但如果你要跨境，那么在境外 HK、SG 等地搭一个 derper 可能更好。

因为 tailscale 的协议时常会更新，官方建议时不时重新执行一下 `go install tailscale.com/cmd/derper@main` 以更新 derper server。

derper 也可以加到 systemd 里自启动：

```null
➜  ~ sudo vi /etc/systemd/system/derper.service

[Unit]
Description=derper service
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=root
ExecStart=/home/laisky/.go/bin/derper -c=/root/derper.conf -hostname=x.x.x.x -a :443 -stun

[Install]
WantedBy=multi-user.target
```

你也可以在 go dockerfile 里执行上述步骤，然后以容器的形式运行 derper。 不过需要注意的是，因为 derper 会在后台申请 letsencrypt，你不要太频繁地对同一个域名创建运行容器， 否则很容易触发 letsencrypt 的频率限制（168hr 内申请 5 次。）

#### 配置 ACL

安装好 derper 后，需要在网页上配置服务器信息，地址在 [https://login.tailscale.com/admin/acls](https://login.tailscale.com/admin/acls)

![](https://s3.laisky.com/uploads/2021/10/acl.png)

在其中加上一段 `derpMap`：

```null
"derpMap": {
    "Regions": {
        "900": {
            "RegionID": 900,
            "RegionCode": "REGION_NAME",
            "Nodes": [
                {
                    "Name": "NODE_NAME",
                    "RegionID": 900,
                    "HostName": "xxx.xxx.com",
                    // IPv4 可以不填，填的话可以绕过对域名的 DNS 解析
                    "IPv4": "x.x.x.x",
                }
            ]
        }
    }
}

```

其中 RegionID 必须是数字，而且必须是 9xx。

为了调试自己的 derper 是否可用，你可以先把官方的所有中继都禁用了：

```null
"derpMap": {
    "OmitDefaultRegions": true,
}
```

在节点服务器上，需要重启 tailscaled 才能生效，win/mac 的话可以直接退出 tailscale 程序再重新打开。

linux 上，可以用：

```null
sudo systemctl restart tailscaled
```

重新进程后，可以查看 tailscale 网络的状态：

```null
# 查看 relays 节点的列表和延迟，应该能看到你自己安装的 derper 节点
sudo tailscale netcheck

# 查看当前 derper 和其他 derper 节点的连接状态
sudo tailscale status
```

顺带一提，tailscale netcheck 实际上只 check 3478/udp 的端口， 就算 netcheck 显示能连，也不一定代表 443 端口可以转发流量。 最简单的办法是直接打开 [https://x.x.x.x](https://x.x.x.x/) 的域名，如果看到如下页面，且地址栏的 SSL 证书标签显示正常可用，那才是真没问题了。

![](https://s3.laisky.com/uploads/twitter/FCM_azhVcAc5Oxy.jpg)

## 五、ACL

### 1、Tags

-   [Network access controls (ACLs)](https://tailscale.com/kb/1018/acls/)
-   [Server role accounts using ACL tags](https://tailscale.com/kb/1068/acl-tags/)

最近学习了一下 ACL tags，它可以为机器标记 tag，然后为 tag 配置访问策略。 这可以解决 tailscale 节点一旦登录，就可以访问用户名下所有节点所有服务所导致的安全隐患。

想象有这么一个场景，我系统通过 tailscale 方便的连接一台不完全属于我的设备， 这台设备可能还有其他人也在使用。如果我仅仅是安装一个 tailscale， 那么所有能登录这台设备的人都可以通过 tailscale 连接我所有的设备。

所以我希望实现这么一个需求：我可以连接这台节点，但是这台节点不能连接我的其他节点。

```null
// 需要先在 ACL 里预声明好 tags，然后才能在 machines 页面为主机设置 tag
"tagOwners": {
    // trusted 是我自己的机器，可以连所有服务
    "tag:trusted": [
        "abc@gmail.com"
    ],
    // work 是公司配发的机器，可能装有企业监控，只允许连 work 和 insecure
    "tag:work": [
        "abc@gmail.com"
    ],
    // insecure 是最低安全级别，除 tcp proxy 外不允许连任何服务
    "tag:insecure": [
        "abc@gmail.com"
    ],
},
// 为主机设置别名
"hosts": {
    "proxy1": "1.2.3.4",
    "proxy2": "1.2.3.4",
    "home": "1.2.3.4",
},
// Access control lists.
"acls": [
    // 允许所有登录的机器，连接代理和我工作机的 SSH
    {
        "action": "accept",
        "src": [
            "abc@gmail.com"
        ],
        "dst": [
            "proxy1:17777",
            "proxy2:17777",
            "home:22"
        ]
    },
    // 允许 trusted 的机器连接所有服务
    {
        "action": "accept",
        "src": [
            "tag:trusted"
        ],
        "dst": [
            "*:*"
        ]
    },
    // 公司的电脑，只允许连公司的电脑
    {
        "action": "accept",
        "src": [
            "tag:work"
        ],
        "dst": [
            "proxy1:17777",
            "proxy2:17777",
            "home:22",
            "tag:work:*"
        ]
    }
]
```

 [https://blog.laisky.com/p/tailscale/](https://blog.laisky.com/p/tailscale/)
