# TailScale 实现远端访问整段局域网(ZeroTier另一选择)_其他网络设备_什么值得买
2021-10-07 15:30:30 34 点赞 166 收藏 31 评论

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/d4c37e7c-d941-430c-96f4-3db7fc63f607.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_2/)

> 之前的贴文分享了\[ZeroTier 实现远端访问局域网]([https://kingtam.win/archives/zerotier-route.html](https://kingtam.win/archives/zerotier-route.html)) ，最近发现一款软件\`TailScale\`(数码小树人的貼文 - 让你可以随时随地用 NAS 和家里的电脑) 感觉设定上更简单一点，仅仅透过帐号登入就可以接入\`TailScale\`网络内。
>
> \`TailScale\`是以\[Wireguard]([https://www.wireguard.com/](https://www.wireguard.com/)) 的协议加密，所以安全性更高，而且是 P2P 连线，流量不经伺服器，延迟更低、私密性更高。

1.  安装及加入到`TailScale`网络
2.  透过设定其中 1 部`TailScale`的[装置](https://www.smzdm.com/fenlei/zhuangzhi/)为网关「Subnet Router」，可以访问整个内网网段

如果是**Windows**用户可以到[官网](https://tailscale.com/download/windows)下载安装[Tailscale](https://pkgs.tailscale.com/stable/tailscale-ipn-setup-1.14.4.exe)，或者透过[Chocolatey 软件包](https://kingtam.win/archives/choco.html)安装。

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/fd5191b5-37de-4824-a664-d15959bff25f.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_3/)

choco install tailscale -y

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/0b998e53-5d37-44d9-8f65-3ba49bf13114.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_4/)

双击桌面右下角的图示

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/5c6662a1-91a2-4bc7-839d-2c830657bd0d.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_5/)

使用[谷歌](https://pinpai.smzdm.com/2047/)或者[微软](https://pinpai.smzdm.com/1461/)账号进行登录

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/aa4f9456-0bdb-4d6a-8407-e3277f6e86b8.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_6/)

成功授权后可以查看分配到的 IP 地址

**Linux**用户可以到[Download · Tailscale](https://tailscale.com/download/linux)找到相对应的[CPU](https://www.smzdm.com/fenlei/cpu/)平台及架构安装.

我以**Ubuntu 20.04**为范例

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/6c72eed9-2c52-4190-be5e-560888168c9e.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_7/)

-   添加 `Tailscale` 的金钥和软件仓库地址

`curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/focal.gpg | sudo apt-key add -`

`curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/focal.list | sudo tee /etc/apt/sources.list.d/tailscale.list`

-   安装`Tailscale`

`sudo apt update  
sudo apt install tailscale`

-   启用`Tailscale` 及透过网址验证登入到`Tailscale` 网络

sudo tailscale up

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/b9629977-e501-47d1-8eed-be550c6b0102.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_8/)

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/46f9dcaf-7fb9-4cf8-a8c1-da367e1558fd.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_9/)

此页面显示成功授权

返回到 Linux 终端查看分配到的 IP 地址

tailscale ip

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/cda5ddd8-a349-4cea-a9f0-06d80dd6f1a2.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_10/)

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/a0f74f96-6c0d-42c5-8004-37bc1b59e910.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_11/)

`Tailscale` 网络内的装置可以互通

**2. 透过设定其中 1 部「TailScale」的装置为网关「Subnet Router」，可以访问整个内网网段**

> 因为不是每一部设备都可以安装到 Tailscale ， 所以这样设定「Subnet Route」的好处是家里内网只要 1 部 Linux 装置接入到 Tailscale 网络，其他所有的外网的 Tailscale 装置都可以透过这部 Linux 装置作为网关访问家里内网所有的设备，如: 印表机，[路由器](https://www.smzdm.com/fenlei/luyouqi/)，伺服器等等。

-   启用 **Linux** 上的 **IP** 转发 (IP Forwarding)

因为「Subnet Route」的设定需要用到 IP 转发 (IP Forwarding) 特性

使用以下指命启用

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/fa0478f2-acb1-4c56-82a4-14ba143ba2ca.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_12/)

`echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.conf  
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.conf  
sudo sysctl -p /etc/sysctl.conf`

-   以 Linux 作为「Subnet Route」(子网路由网关) 连接到`Tailscale`

> `Tailscale` 不需要防火墙配置，会自动管理规则，以允许转发。设定过程也没涉及到`iptable`，一条指令可以完成。

sudo tailscale up --advertise-routes=20.13.3.0/24

> `20.13.3.0/24`替换为自己的网段，可以是 IPV4 或者 IPV6。

-   登入到网页控制台 > `Machines`

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/0faba825-cd01-4907-9002-00f79fcd7dab.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_13/)

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/c4c93550-f90a-4672-a24c-02b614d6747c.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_14/)

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/71124154-31b8-4c2d-9c52-bfebdecea1b3.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_15/)

`Enable All`允许转发所有

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/e45b0a62-4bdb-4682-b6cd-7cea9a73077e.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_16/)

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/7b66d14e-f6ad-494a-88a4-75d5d8a98dcb.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_17/)

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/7e9699f8-cde5-44b7-acc1-dae2b243b7cb.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_18/)

基于安全特性，`Tailscale`每隔 6 个月需要重新授权装置，作为「Subnet Router」(子网路由网关)，建议停用。

-   外网的`Tailscale`装置线路测试

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/72815851-c086-4f35-9ec7-a998a6ad1a9e.jpeg?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_19/)

可以看到是经节点「Subnet Router」IP `100.101.41.50` 接入到内网 `20.13.3.1`，延迟也很理想。

以下是使用内网 IP 连入家里的各服务及装置

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/12a5629a-7228-4032-b059-c4b24da6ce38.jpeg?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_20/)

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/9c3af098-fcf2-4650-8f5c-9f292cb58663.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_21/)

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/5ff30f40-59ef-4c04-b71a-70125aa4ec69.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_22/)

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/0f374de7-727f-4857-bc95-c1ee5bd50a65.jpeg?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_23/)

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/b6997700-b151-459e-b208-aff263e23e8c.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_24/)

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/0023a328-647a-47b5-9a2f-d9d2c50fa5cf.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_25/)

测速

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-04-06/3d1189f8-4436-4975-bbb3-25f0a354c251.png?raw=true)
](https://post.smzdm.com/p/ad2kw7xx/pic_26/)

> `TailSale`和`ZeroTier`的使用场景上非常相近，没有那个占优，根据自身网络环境的连线质量较佳去选取。
>
> 透过[ZeroTier 实现远端访问局域网](https://kingtam.win/archives/zerotier-route.html)的设定不是对所有自定网段有效，如今次的`20.13.3.0/24`的网段失效，`Zerotier`误判为外网网段而不能访问。而使用`TailSale`的设定可以实现远端访问网段为`20.13.3.0/24`的局域网。

-   [Download Install the app and sign in to get started Tailscale](https://tailscale.com/download/linux/ubuntu-2004)
-   [Enable IP forwarding on Linux · Tailscale](https://tailscale.com/kb/1104/enable-ip-forwarding/)
-   [Subnet routers and traffic relay nodes · Tailscale](https://tailscale.com/kb/1019/subnets/)

![](https://res.smzdm.com/pc/pc_shequ/dist/img/the-end.png) 
 [https://post.smzdm.com/p/ad2kw7xx/](https://post.smzdm.com/p/ad2kw7xx/)
