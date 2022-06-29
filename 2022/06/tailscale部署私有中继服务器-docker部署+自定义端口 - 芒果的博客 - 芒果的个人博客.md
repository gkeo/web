# tailscale部署私有中继服务器-docker部署+自定义端口 - 芒果的博客 - 芒果的个人博客
本文在以下环境安装测试通过

-   腾讯云轻量应用服务器 ubuntu-20.04 lts
-   Let's Encrypt 证书
-   zouyq/derper 镜像
-   部署在国内服务器域名需备案

[![](https://mango-blog-1255355814.cos.ap-guangzhou.myqcloud.com/mango-bogad-tencentcloud.jpg)
](https://mangoroom.cn/go/tencentcloud1/)图 1：【腾讯云】云产品限时秒杀，爆款 1 核 2G 云服务器，首年 74 元

## 安装 docker

ssh 远程登陆到服务器, 推荐使用一键安装命令

```null
curl -sSL https://get.daocloud.io/docker | sh
```

## 申请证书

芒果这里是申请免费的 Let's Encrypt 证书，理论上使用其它的也是 ok 的。Let's Encrypt 证书的申请方法一可以自己到官方网站去申请，而是可以用宝塔面板自建一个静态页面网站，使用宝塔工具申请。芒果这里是使用宝塔申请。申请完毕后得到一个证书文件与密钥文件

```null

fullchain.pem 
privkey.pem 
```

然后将其拷贝只根目录下

```null
certpath
```

目录中, 并且按照以下格式重命名证书和密钥文件

```null
yourhostname.crt
yourhostname.key
```

## 安装镜像

ssh 远程登陆到服务器

```null
sudo docker pull zouyq/derper
sudo docker run -it -d -p 8082:8082 -p 3478:3478/udp --name derper  -v /certpath:/cert zouyq/derper /derper -hostname your-hostname -stun -a :8082 -certmode manual -certdir /cert 
```

### 检查启动状态

```null
sudo docker logs 036401e7240b
```

如果有以下提示，无报错则说明 deper 服务已经正常启动

```null
2022/03/05 02:23:04 no config path specified; using /var/lib/derper/derper.key
2022/03/05 02:23:04 derper: serving on :8082 with TLS
2022/03/05 02:23:04 running STUN server on [::]:3478
```

### 防火墙放行

-   在服务器运行商控制台面板安全组放行 8082tcp, 3478udp
-   在 linux 系统防火墙放行 8082tcp, 3478udp，可以使用宝塔面板设置

## 修改 tailscale 配置

tailscale 的节点设置与之前类似，但使用了非 443 端口，需要在配置里面指定说明, 如

```null

{
  
  "Groups": {
    "group:example": [ "user1@example.com", "user2@example.com" ],
  },
  
  "Hosts": {
     "example-host-1": "100.100.100.100",
  },
  "ACLs": [
    
    
    { "Action": "accept", "Users": ["*"], "Ports": ["*:*"] },
  ],
  "derpMap": {
    "Regions": { "900": {
      "RegionID": 900,
      "RegionCode": "mangoderp",
      "Nodes": [{
          "Name": "1",
          "RegionID": 900,
          
          "HostName":"tailscalederper.mangoroom.cn",
          "DERPPort":8082
      }]
    }}
  }
}
```

## 测试

部署完后可以进行以下测试，检验是否可以正常使用

-   1 将 tailscale 改为仅链接自定义节点，如

```null
 "derpMap": {
    "OmitDefaultRegions": true,
    "Regions": { "900": {
      "RegionID": 900,
      "RegionCode": "myderp",
      "Nodes": [{
          "Name": "1",
          "RegionID": 900,
          "HostName":"tailscalederper.mangoroom.cn",
          "DERPPort":8082
      }]
    }}
  }
```

-   2 重启各个客户端
-   3 各个客户之间相互 ping 或者发送文件

如果以上测试没有问题，那么整个部署就 OK 了。

 [https://mangoroom.cn/tools/tailscale-custom-derper-servers-custom-derpport-base-docker.html](https://mangoroom.cn/tools/tailscale-custom-derper-servers-custom-derpport-base-docker.html)
