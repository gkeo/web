# 如何优雅地搭建自己的发卡站 - DOV
本文主要介绍独角数卡的搭建和记录遇到的一些问题，持续更新。

> 🦄独角数卡 (自动售货系统)- 开源式站长自动化售货解决方案、高效、稳定、快速！🚀🚀🎉🎉

写本文的原因主要是目前网上关于独角数卡的教程实在是太少，而且都是复制来复制去，基本没有任何参考价值。所以在该篇文章中介绍我写的关于独角数卡的 `Docker` 镜像，以及其一键搭建过程和 Nginx 的一些设置。

该 `Docker` 会随独角数卡仓库自动更新，可以配合 [`WatchTower`](https://github.com/containrrr/watchtower) 食用，喜欢的点点 star 吧～

> 独角数卡官方出了 `Docker` 镜像，但是写的甚是简陋，目前还是推荐我这个。

这里提醒以下，本镜像默认使用原版仓库无任何修改。

## \[](#Docker 安装)\[](#Docker 安装 "Docker 安装")Docker 安装

参考 [该教程](https://yeasy.gitbook.io/docker_practice/install) ，安装好`Docker` 和`docker-compose`。

## [](#独角数卡搭建)\[](# 独角数卡搭建 "独角数卡搭建") 独角数卡搭建

### [](#预创建文件夹)\[](# 预创建文件夹 "预创建文件夹") 预创建文件夹

```
`mkdir Shop && cd Shop
mkdir storage uploads
chmod 777 storage uploads`

BASH


```

❗注意此处文件夹权限一定要给！

### \[](# 编辑 docker-compose-yaml)\[](# 编辑 docker-compose-yaml "编辑 docker-compose.yaml") 编辑`docker-compose.yaml`

```
`version: "3"

services:
  faka:
    image: ghcr.io/apocalypsor/dujiaoka:latest
    
    container_name: faka
    environment:
        
        - INSTALL=true
        
    volumes:
      - ./env.conf:/dujiaoka/.env
      - ./uploads:/dujiaoka/public/uploads
      - ./storage:/dujiaoka/storage
    ports:
      - 127.0.0.1:56789:80
    restart: always
 
  db:
    image: mariadb:focal
    container_name: faka-data
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=<ROOT_PASSWORD>
      - MYSQL_DATABASE=dujiaoka
      - MYSQL_USER=dujiaoka
      - MYSQL_PASSWORD=<DB_PASSWORD>
    volumes:
      - ./data:/var/lib/mysql

  redis:
    image: redis:alpine
    container_name: faka-redis
    restart: always
    volumes:
      - ./redis:/data`

YAML


```

❗注意：

-   自行将形如 `<foobar>` 的变量替换为自己的信息，以下的替换要与 `docker-compose.yaml` 文件中相同

如果需要每次启动容器都运行某些命令，例如修改某个文件，则 `faka` 需进行如下映射：

```
`- ./start-hook.sh:/dujiaoka/start-hook.sh`

YAML


```

`start-hook.sh`需要提前创建并写好，例如：

```
`#!/bin/sh

echo "Executing start-hook ..."

cp -f /dujiaoka/resources/views/luna/layouts/_notice_xs.blade.php /dujiaoka/resources/views/luna/layouts/_notice.blade.php`

BASH


```

以及 TG 网友 Sir Wang 提供了一些其他的自定义修改：

```
 `- ./favicon.ico:/dujiaoka/public/favicon.ico:ro
- ./favicon.ico:/dujiaoka/public/assets/style/favicon.ico:ro

- ./default.png:/dujiaoka/public/assets/common/images/default.jpg:ro

- ./background.png:/dujiaoka/public/assets/luna/img/background.png:ro`

YAML


```

### \[](# 编辑 -env 文件)\[](# 编辑 -env 文件 "编辑. env 文件") 编辑 `.env` 文件

我这里把它创建为`env.conf`：

```
`APP_NAME=<YOUR_APP_NAME>
APP_ENV=local
APP_KEY=<YOUR_APP_KEY>
APP_DEBUG=false
APP_URL=<YOUR_APP_URL>

LOG_CHANNEL=stack

DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=dujiaoka
DB_USERNAME=dujiaoka
DB_PASSWORD=<DB_PASSWORD>

REDIS_HOST=redis
REDIS_PASSWORD=
REDIS_PORT=6379

BROADCAST_DRIVER=log
SESSION_DRIVER=file
SESSION_LIFETIME=120

CACHE_DRIVER=redis

QUEUE_CONNECTION=redis

DUJIAO_ADMIN_LANGUAGE=zh_CN

ADMIN_ROUTE_PREFIX=/admin`

INI


```

如果没有特殊需求可以直接用我上面给的文件，并替换形如 `<foobar>` 的变量即可。有其他问题可以参考[`dujiaoka/.env.example`](https://github.com/assimon/dujiaoka/blob/master/.env.example)。

❗注意环境变量如果写的不对可能导致 500，可以开 `APP_DEBUG=true` 看看具体是哪里写错了。

### [](#Epusdt)[](#Epusdt "Epusdt")Epusdt

[Epusdt](https://github.com/assimon/epusdt) (Easy Payment Usdt) 是独角数卡官方的开源 USDT 支付中间件 (TRC20 网络)，实测下来 **非常好用**， 推荐！

如果要添加 USDT 收款，需要在 `docker-compose.yaml` 中添加以下项：

```
`usdt:
  image: ghcr.io/apocalypsor/dujiaoka:usdt
  
  container_name: faka-usdt
  restart: always
  volumes:
    - ./usdt.conf:/usdt/.env
  ports:
    - 127.0.0.1:51293:8000`

YAML


```

同时要在目录下提前编辑好 `usdt.conf` 配置文件，参考 [文档](https://github.com/assimon/epusdt/blob/master/wiki/manual_RUN.md) 和[参考配置](https://github.com/assimon/epusdt/blob/master/src/.env.example)。

其中 `51293` 端口也最好反代，建议 Epusdt 用单独的域名。

### [](#启动服务)\[](# 启动服务 "启动服务") 启动服务

最后就是愉快的一键运行啦：

```
`docker-compose up -d`

BASH


```

这样独角数卡就会在本地运行起来。

**如果仅需要 HTTP 的话** 可以将 `docker-compose.yaml` 的`127.0.0.1:56789`改成 `80` 即可，不需要再用 Nginx 反代！

否则建议在宿主端用 Nginx 反代来实现 HTTPS。

反代时建议使用以下配置：

```
 `location ^~ /
{
    proxy_pass http://127.0.0.1:56789;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    proxy_set_header X-Forwarded-Proto  $scheme;

    add_header X-Cache $upstream_cache_status;

    proxy_set_header Accept-Encoding "";
    sub_filter "http://" "https://";
    sub_filter_once off;
}` 

NGINX


```

其中以下几行尤为重要，否则会在 HTTPS 中遇到许多问题！

```
`proxy_set_header X-Forwarded-Proto $scheme;
sub_filter "http://" "https://";
sub_filter_once off;`

NGINX


```

**对于使用 NPM 的用户，实测下来直接创建网站反代就可以了，不用进行额外的修改！**

**NPM 的安装和使用可以参考我的这两篇文章：** 

-   [如何优雅的替换掉宝塔面板](https://blog.dov.moe/posts/37571/)
-   [如何优雅地使用 NPM 进阶篇](https://blog.dov.moe/posts/56114/)

## [](#网页端安装)\[](# 网页端安装 "网页端安装") 网页端安装

网页端安装时数据库的 `host` 填 `db`，端口保持默认。

还需要注意的是，首次进入安装并完成后，需要将 `docker-compose.yaml` 环境变量中的 `INSTALL=true` 改为`INSTALL=false`，然后运行以下命令使其生效：

```
`docker-compose down && docker-compose up -d`

BASH


```

对于 Epusdt 支付方式的添加可以参考我的配置以及 [文档](https://github.com/assimon/epusdt/tree/master/plugins/dujiaoka)：

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-22%2016-53-07/bb49c8f3-a845-47e6-9116-8977cc27334f.webp?raw=true)
](https://blog.dov.moe/img/dujiaoka-epusdt.webp "dujiaoka-epusdt")

### \[](#1- 支付接口)\[](#1- 支付接口 "1. 支付接口")1. 支付接口

门槛最低的接口应该是 V 免签了，由于长期不更新我就没写 `Docker` 镜像，不过现在还能用，具体参考：

-   [GitHub - szvone/vmqphp: V 免签 PHP 版 完全开源免费的个人免签约解决方案](https://github.com/szvone/vmqphp)
-   [V 免签 PC 监控更新地址](https://pmhapp.com/wd_vpay/#/)

V 免签一般而言微信比较稳定，支付宝经常会掉线。微信存在 PC 客户端会更新的问题，而且无法关闭检测，应该可以用 `hosts` 来屏蔽更新，博主没有抓包测试，如果有抓好的朋友欢迎评论告知。

其次常见的还有当面付、Stripe 之类的，这里就不过多介绍了。

### \[](#2- 当面付不跳转)\[](#2- 当面付不跳转 "2. 当面付不跳转")2. 当面付不跳转

一般而言造成此问题可能有两个原因：

-   最可能的原因是 HTTPS 证书问题，如果做了分流，则需要检查下网站到国内的证书是否是正常的。
-   也有可能是 CDN 缓存了检查订单的链接，需要将以下路径排除在缓存外：

    ```
    `/check-order-status/*
    /pay/*`

    TEXT


    ```

### \[](#3-Stripe 接口)\[](#3-Stripe 接口 "3. Stripe 接口")3. Stripe 接口

以下修改已删除。

~ 本镜像对独角数卡的 Stripe 接口进行了一定的修改：~

-   ~ 移除了微信支付，因为我的号开不了微信。~
-   ~ 支付宝的收款货币由人民币改为了英镑，因为支付宝的人民币汇率太坑了。~
-   ~ 汇率接口替换成了我自己的接口：~

    ```


    HTTP


    ```

    ~ 该接口比独角数卡内置的更加稳定，且计算实时汇率，具体的会在 [分享我写的一些自用 Api](https://blog.dov.moe/posts/59334/)中介绍。~

~ 如果你自己有其他需求，可以将容器内的文件映射出来：~

```
`volumes:
    - ./StripeController.php:/dujiaoka/app/Http/Controllers/Pay/StripeController.php`

YAML


```

~ 然后参考 [我的 StripeController.php](https://github.com/Apocalypsor/dujiaoka-docker/blob/main/modify/StripeController.php)以及 [独角数卡的 StripeController.php](https://github.com/assimon/dujiaoka/blob/master/app/Http/Controllers/Pay/StripeController.php)进行修改。~

还有其他问题可以在 [关于](https://blog.dov.moe/about/) 中找到我的联系方式。

* * *

**广告** ：推销下 [我的小店](https://dovshop.net/)，主要售卖各大云厂商试用账号、虚拟卡、做地址证明等。 
 [https://blog.dov.moe/posts/49102/](https://blog.dov.moe/posts/49102/)
