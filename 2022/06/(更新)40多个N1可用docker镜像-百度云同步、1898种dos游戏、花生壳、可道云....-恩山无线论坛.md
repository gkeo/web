# (更新)40多个N1可用docker镜像-百度云同步、1898种dos游戏、花生壳、可道云....-恩山无线论坛
_本帖最后由 我是四天 于 2021-8-17 09:17 编辑_

理论上 armbian、omv 或其他系统都支持，**更新请重新拉取，并清除挂载目录中的旧版本程序**，不喜请按 Alt+F4  
**N1 已出售，此贴停止更新！**

**2021.02.19 更新：  
1、花生壳 5.1 的 SN 码无法登录，降级为 3.0.2；\*\***2021.02.19 更新：\*\*  
**1、更新蒲公英版本为 2.2.1**  
**2、更新花生壳版本为 5.1\*\***2020.06.29 更新：  
1、修复 dosgame 挂载目录后不能启动的 bug，同时更名为 lstcml/dosgame\***\*2020.06.24 更新：**  
**1、新镜像：lstcml/speedtest\*\***2020.03.13 更新：\*\*  
**1、更新 n2n 镜像：**  
**a、支持客户端与服务端**  
**b、支持 v2 与 v2s 切换**  
**c、新增 OTHER 参数，用于添加其他参数命令，如加密 - k xxx 等**  
**d、支持更新 v2 版本，docker exec n2n update.sh**  
**2、更正创建 \*\***Aria2\***\* 容器命令中映射地址为 / mnt/sda1/download**

**2020.03.12 更新：**  
**1、修复易有云初始化报错，新增必填参数 TOKEN**

**2、新增 zfile 镜像 \*\***1、花生壳内网版 \*\*

作用：用于内网穿透，远程访问 N1 的 http 或 tcp

Uasge：

1.  docker run -d \\  
2.    \--restart=always \\  
3.    \--name oray \\  
4.    \--network host \\  
5.    lstcml/oray

_复制代码_

官方内网穿透教程请参考 -->[飞机直达](http://service.oray.com/question/4287.html)  
获取 SN 码：ssh 登录 N1 后，输入 docker exec oray phddns status 回车（oray 是容器名称，如有更变自行替换）  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/e143872a-65fe-4062-9b99-280a7392d423.jpeg?raw=true)
**2、花生壳蒲公英**

作用：免费 3 个成员，同时加入一个虚拟局域网，可相互访问

**目前发现与 zerotier、n2n 有冲突，导致容器重启后无法加入局域网，建议三选一**

Uasge：

1.  docker run -d \\  
2.    \--restart=always \\  
3.    \--device=/dev/net/tun \\  
4.    \--net=host \\  
5.    \--cap-add=NET_ADMIN \\  
6.    \--cap-add=SYS_ADMIN \\  
7.    \--name pgy \\  
8.    lstcml/n1_pgy

_复制代码_

进入容器：  
按照提示输入花生壳注册的账号登录，然后手机安卓或水果机或 PC 电脑安装蒲公英，启动用输入虚拟局域网 ip 即可访问 N1（默认全端口映射），[官网资料](http://service.oray.com/question/5063.html)

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/cc399756-9042-4a8c-8ee6-65283b17fcde.jpeg?raw=true)

**3、sshd**  
作用：基于 alpine 的 ssh 客户端与服务端，可用于作为跳板访问其他设备

Uasge：

1.  docker run -d \\  
2.    \--name sshd \\  
3.    \--restart=always  \\  
4.    lstcml/sshd

_复制代码_

注意，默认不设置密码下，每次重启容器 root 密码会更变，SSH 账户为 root，默认密码请查看容器日志  
启动容器时设置 ROOT 密码  

1.  docker run -d --restart=always --name ssh -e PASSWORD=test123 lstcml/n1_sshd

_复制代码_

**4、百度云同步工具 bypy**

作用：可映射（挂载）主机的目录，通过 bypy 同步到百度云盘

Uasge：

1.  docker run -dit \\  
2.    \--name bypy \\  
3.    \--restart=always \\  
4.    \-v /docker/bypy/sync:/sync \\  
5.    lstcml/bypy

_复制代码_

具体使用方法找了个百度的，自己看看 -->[飞机直达](https://www.jianshu.com/p/c9467daf701f)

**5、docker 版可道云**

作用：类似 windows 的 web 资源文件管理器目前最新版 4.40

Uasge：

1.  docker run -d \\  
2.    \--name  kdcloud \\  
3.    \-p 8068:80 \\  
4.    \--restart=always \\  
5.    \-v /docker/kdcloud:/var/www/html \\  
6.    lstcml/n1_kodcloud

_复制代码_

浏览器中输入 N1 的 ip 地址 + 1000 端口，如[http://192.168.8.8:8068](http://192.168.8.8:1000/)即可见证奇迹

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/a64c537c-2767-416e-a41c-8a52266de81a.jpeg?raw=true)

**6、轻量版 Nginx+PHP**

作用：用于建站，可以自行安装数据库

Uasge：

1.  docker run \\  
2.    \--name alpine-nginx-php \\  
3.    \--restart=always \\  
4.    \-d -p 8069:80 \\  
5.    \-v /docker/alpine-nginx-php:/var/www/html \\  
6.    \-e PHP_ERRORS=1 \\  
7.    \-e PHP_UPLOAD_MAX_FILESIZE=250 \\  
8.    lstcml/alpine-nginx-php

_复制代码_

N1 中 / var/www/html 即为站点根目录，浏览器中输入 N1 的 ip 地址 + 8069 端口，如[http://192.168.8.8:806](http://192.168.8.8:8088/)9 即可见证奇迹

**7、静态化博客 hexo**

作用：可用于搭建博客 (**2019.10.12 修复映射目录无法启动问题**)

Uasge：

1.  docker run -d \\  
2.    \--name blog \\  
3.    \--restart=always \\  
4.    \-p 8070:4000 \\  
5.    \-v /docker/hexo:/hexo \\  
6.    lstcml/hexo

_复制代码_

浏览器中输入 N1 的 ip 地址 + 4000 端口，如[http://192.168.8.8:8070](http://192.168.8.8:4000/)即可见证奇迹，hexo 具体使用教程请自行百度

**8、阅后即焚 naiveboom**

作用：可生成阅后即消的消息, 即一次性消息，当别人打开链接后，其他人或者再次打开链接，消息已自动清除

Uasge：

1.  docker run -d \\  
2.    \--name naiveboom \\  
3.    \-p 8071:3000 \\  
4.    lstcml/n1_naiveboom

_复制代码_

浏览器中输入 N1 的 ip 地址 + 3000 端口，如[http://192.168.8.8:8071](http://192.168.8.8:2000/)即可见证奇迹**9、蚂蚁笔记 leanote**

作用：自建笔记服务端 (**已修复映射宿主机目录后启动失败问题，20190909 再次修复重启后服务启动的 bug，感谢 418 楼的坛友**)

Uasge：

1.  docker run -d \\  
2.    \--name leanote \\  
3.    \--restart=always \\  
4.    \-p 8072:9000 \\  
5.    \-v /docker/leanote/leanote/data:/data \\  
6.    lstcml/n1_leanote

_复制代码_

浏览器中输入 N1 的 ip 地址 + 8072 端口，如[http://192.168.8.8:80](http://192.168.8.8:8000/)72 即可见证奇迹，windows、mac、ios、Android 的客户端可以前往官网下载 ->[飞机直达](https://leanote.com/)  
**注：客户端登录自建服务时，注意服务器地址最后不带 /，如[http://192.168.8.8:80](http://192.168.8.8:8000/)**72**或[http://xxx.xxx.com](http://xxx.xxx.com/)即可**  
admin/abc123 (管理员用户)  
[demo@leanote.com](mailto:demo@leanote.com)/demo@leanote.com (体验用户)  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/f4ce1bc9-f46e-4b06-be30-fb47992212df.jpeg?raw=true)

**10、Webdav**

作用：自建 webdav 服务器，支持中文文件或目录

脚本必须以管理员身份运行

Uasge：

1.  docker run -d \\  
2.    \--name webdav \\  
3.    \--restart=always \\  
4.    \-e USERNAME=webdav \\  
5.    \-e PASSWORD=123456 \\  
6.    \-v /media/webdav:/webdav \\  
7.    \-p 8073:80 \\  
8.    lstcml/n1_webdav

_复制代码_

**2019.09.05 23:16 更新**  
**ps：**  
**1、如未自定义账号密码，默认为 admin/passwd**  
~**2、添加 https 方式：进入容器，取消注释 / usr/local/caddy/Caddyfile 文件中的 tls，并修改 tls 后面证书位置即可**~  
**3、已开启目录浏览，浏览地址为 IP + 端口号，如：** **如 192.168.8.8:8073**  
**4、挂载 webdav 地址为 IP\*\***+ 端口号 \***\*/webdav，如 192.168.8.8:8073/webdav**  
**5、部分 Windows 无法映射 webdav 的情况是因为 win7 以上默认不支持 http，开启同时支持 http 和 https：下载附件解压双击脚本即可**  
 ![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/7e8cf65b-22f6-4a01-9681-997a77cb29e1.gif?raw=true)
  [webdav.zip](https://www.right.com.cn/forum/forum.php?mod=attachment&aid=MzA4OTQ4fDE3MWQxNTdlfDE2NTY0ODgxNDh8MHw5MTEzNzU%3D) _(536 Bytes, 下载次数: 311)_ **11、KMS**

作用：自建激活神器服务端

Uasge：

1.  docker run -d \\  
2.    \--name kms \\  
3.    \--restart=always \\  
4.    \-p 1688:1688 \\  
5.    lstcml/kms

_复制代码_

使用方法可以参考这个 ->[飞机直达](https://www.cnblogs.com/cc1207270165/articles/5394080.html)

**12、Dosgame**

作用：强大的回忆杀，重返小时候的小霸王、世嘉、Gameboy 时代

Uasge：

1.  docker run -d \\  
2.    \--name dosgame \\  
3.    \--restart=always \\  
4.    \-p 8074:6000 \\  
5.    \-v /docker/dosgame:/app/static/games/ \\  
6.    lstcml/dosgame

_复制代码_

注：打包的镜像中只有 42 种游戏，下载更多游戏方法（目前共**1898**种游戏，共**31.9**G，所以悠着点，需要更多游戏的时候记得创建容器的时候把[/app/static/games/](http://app/static/games/)挂载到移动硬盘后再去下载，否则后果不堪设想）：  

1.  docker exec -it dosgame sh

_复制代码_

进入容器后操作

1.  mv /app/static/games/games.json /app/static/games/42games.json

_复制代码_

1.  mv /app/static/games/1898all_games.json /app/static/games/games.json

_复制代码_

1.  python /app/static/games/download_data.py

_复制代码_

等待下载完后重启容器即可  

浏览器中输入 N1 的 ip 地址 + 8074 端口，如[http://192.168.8.8:](http://192.168.8.8:2000/)8074 即可见证奇迹  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/b77496cf-97ad-49fe-a405-6ee124a9228b.jpeg?raw=true)
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/e345e5fd-3a92-4ec1-974c-c792806f7b5b.jpeg?raw=true)
**13、在线 MD 编辑器**

作用：在线编辑 MD 文本，支持导出本地

Uasge：

1.  docker run -d \\  
2.    \--name markdown \\  
3.    \--restart=always \\  
4.    \-p 8075:2000 \\  
5.    lstcml/n1_markdown

_复制代码_

浏览器中输入 N1 的 ip 地址 + 8075 端口，如[http://192.168.8.8:](http://192.168.8.8:2000/)8075 即可见证奇迹

**14、微力同步**

作用：局域网通过同步工具，几乎覆盖全平\*（windows、linux、群晖、安装、ios、openwrt 等等）

Uasge：

1.  docker run -d \\  
2.    \--name verysync \\  
3.    \--restart=always \\  
4.    \-p 8076:8886 \\  
5.    \-v /docker/versync/sync:/media \\  
6.    lstcml/n1_verysync

_复制代码_

浏览器中输入 N1 的 ip 地址 + 8076 端口，如[http://192.168.8.8:](http://192.168.8.8:2000/)8076 即可见证奇迹，使用教程 ->[灰机直达](http://www.verysync.com/manual/)  
主要是通过宿主机需同步目录映射到容器目录中，在容器中进行实时同步

**啰嗦一句，反正我是觉得微力同步配置比较简单**

**15、喜马拉雅转泛播客**

作用：一款

喜马拉雅转泛播客的工具

Uasge：

1.  docker run -d \\  
2.    \--restart=always \\  
3.    \--name xmlypf \\  
4.    \-p 8077:5000 \\  
5.    lstcml/ximalaya-podcast-factory

_复制代码_

浏览器中输入 N1 的 ip 地址 + 5000 端口，如[http://192.168.8.8:](http://192.168.8.8:2000/)8077 即可见证奇迹，使用很简单，打开就知道

**16、Miniflux**

作用：轻量级 WEB 版 RSS 阅读器

Uasge：

1.  docker run -d \\  
2.    \--name miniflux \\  
3.    \--restart=always \\  
4.    \-p 8078:8080 \\  
5.    \-v /docker/postgresql/data:/miniflux/postgresql/data \\  
6.    lstcml/n1_miniflux

_复制代码_

浏览器中输入 N1 的 ip 地址 + 8080 端口，如[http://192.168.8.8:807](http://192.168.8.8:8080/)8 即可见证奇迹，已设置默认为中文，登录后就可以玩了

默认 miniflux 账号密码：miniflux/miniflux

默认连接数据库：miniflux

postgres 默认密码：postgres

**已知问题：仅支持 http，且[http://xxx.xxx.xxx/feed.php](http://xxx.xxx.xxx/feed.php)这种的好像也不支持，能力有限，能解决的可以 M 我 \*\***17、Samba\*\*

作用：局域网共享

Uasge：

host 模式：

1.  docker run -d \\  
2.    \--name samba \\  
3.    \--restart=always  \\  
4.    \--net=host \\  
5.    \-v /docker/samb/conf:/etc/samba \\  
6.    \-v /docker/samb/share:/mnt/share \\  
7.    \-v /docker/samb/data:/mnt/data \\  
8.    lstcml/samba

_复制代码_

非 host 模式：  

1.  docker run -d \\  
2.    \--name samba \\  
3.    \--restart=always  \\  
4.    \-p 137:137/udp \\  
5.    \-p 138:138/udp \\  
6.    \-p 139:139/tcp \\  
7.    \-p 445:445/tcp \\  
8.    \-v /docker/samb/conf:/etc/samba \\  
9.    \-v /docker/samb/share:/mnt/share \\  
10.   \-v /docker/samb/data:/mnt/data \\  
11.   lstcml/samba

_复制代码_

**注意：**  
**1、如果是小钢炮或其他安装了 samba 服务的系统需要关闭自带 samba 服务，否则端口占用导致创建容器失败 \*\***2、如果访问没权限，说明映射到宿主机的目录没有权限，如 \***\*/docker/samba/data\*\***，需基于对应读写权限 (完整权限：chmod 777\*\* **/docker/samba/data)**默认生成两个目录，默认用户账户密码：admin/admin

/etc/samba：配置文件目录

/mnt/share：匿名访问目录，拥有读写删权限

/mnt/data：用户访问目录，拥有读写删权限

**18、mmPlayer**

作用：也是个人音乐厅，接口来自网易云，支持登录网易云账号，可听个人歌单

**注意：这个搭建可能相对麻烦一点，请仔细看清楚**

Uasge：

1.  docker run -d \\  
2.    \--name mmplayer \\  
3.    \-p 8079:8080 \\  
4.    \-p 服务端端口: 3000 \\  
5.    \-e HOST = 宿主机 IP \\  
6.    \-e SPORT = 服务端端口 \\  
7.    lstcml/n1_mmplayer

_复制代码_

实例：

1.  docker run -d \\  
2.    \--name mmplayer \\  
3.    \-p 8079:8080 \\  
4.    \-p 3210:3000 \\  
5.    \-e HOST=192.168.8.188 \\  
6.    \-e SPORT=3210 \\  
7.    lstcml/n1_mmplayer

_复制代码_

**HOST：输入宿主机，也就是 N1 的 IP 地址或域名也行**

**SPORT：必须与映射 3000 端口的宿主机端口一致**

**19、chfs**作用：一款漂亮的 web 管理器支持上传下载，支持 webdav

Uasge：

1.  docker run -d \\  
2.    \--name chfs \\  
3.    \--restart=always  \\  
4.    \-e TIMEOUT=3600 \\  
5.    \-p 8080:8080 \\  
6.    \-v /docker/chfs:/data \\  
7.    lstcml/chfs

_复制代码_

TIMEOUT：session 过期时间，默认 1440（24 小时），单位为分钟权限：  
创建用户：  

1.  docker exec chfs addusr 用户名 密码 权限

_复制代码_

实例：  

1.  docker exec chfs addusr lstcml 123456 RWD

_复制代码_

删除用户：  

1.  docker exec chfs delusr 用户名

_复制代码_

实例：  

1.  docker exec chfs delusr lstcml

_复制代码_

**注：不管创建还是删除用户记得重启容器 \*\***20、zerotier\*\* 作用：内网穿透，把不同局域网机器加入同一虚拟局域网实现相互访问，类似蒲公英、n2n

Uasge：

1.  docker run -d \\  
2.    \--name zerotier \\  
3.    \--restart=always \\  
4.    \--device=/dev/net/tun \\  
5.    \--net=host \\  
6.    \--cap-add=NET_ADMIN \\  
7.    \--cap-add=SYS_ADMIN \\  
8.    lstcml/n1_zerotier

_复制代码_

查看状态：  

1.  docker exec zerotier zerotier-cli info

_复制代码_

加入网络：  

1.  docker exec zerotier zerotier-cli join NETWORK_ID

_复制代码_

如果提示 tun 不存在：**docker: Error response from daemon: linux runtime spec devices: error gathering device information while adding custom device "/dev/net/tun": no such file or directory**

可尝试启动：

**21、minidlna**作用：实现流媒体播放

Uasge：

1.  docker run -d \\  
2.    \--name minidlna \\  
3.    \--net=host \\  
4.    \-v /docker/minidlna:/etc/minidlna \\  
5.    \-v /docker/minidlna/media:/media \\  
6.    lstcml/minidlna

_复制代码_

标签说明：

latest：不支持 rmvb，镜像大小较小

rmvb：支持 rmvb，镜像大小较大

目录说明：

/etc/minidlna：配置文件目录

/media：扫描的媒体文件目录

浏览器打开：[http://localhost:8200 可查看扫描或设备连接情况](http://localhost:8200可查看扫描或设备连接情况)

Mobile App 推荐：海贝音乐、nplay

**22、nfsd**作用：类似 samba 的局域网共享

Uasge：

host 模式

1.  docker run -d \\  
2.    \--name nfsd \\  
3.    \--privileged \\  
4.    \--restart=always  \\  
5.    \--net=host \\  
6.    \-v /docker/nfsd/data:/data \\  
7.    lstcml/nfsd

_复制代码_

非 host 模式  

1.  docker run -d \\  
2.    \--name nfsd \\  
3.    \--privileged \\  
4.    \-p 111:111 \\  
5.    \-p 111:111/udp \\  
6.    \-p 2049:2049 \\  
7.    \-p 2049:2049/udp \\  
8.    \-p 32765-32768:32765-32768 \\  
9.    \-p 32765-32768:32765-32768/udp \\  
10.   \-v /docker/nfsd/data:/data \\  
11.   lstcml/nfsd

_复制代码_

**注：**  
**1、如果是小钢炮或其他安装了 nfsd 服务的系统需要关闭自带 nfs 服务，否则端口占用导致创建容器失败**  
**2、如果访问没权限，说明映射到宿主机的目录没有权限，如 \*\***/docker/nfsd/data\***\*，需基于对应读写权限 (完整权限：chmod 777** **/docker/nfsd/data)**  
**3、windows 挂载中文会乱码，这个是通病，windows 挂载方法 -->\*\***[飞机直达](https://www.jianshu.com/p/edc928e58183)\*\*

**23、vsftp**

作用：ftp 服务端，支持多用户

Uasge：

host 模式

1.  docker run -d \\  
2.    \--name ftp \\  
3.    \--net=host \\  
4.    \-v /docker/ftp/vsftpd:/etc/vsftpd \\  
5.    \-v /docker/ftp/home/ftpusr:/home/ftpusr \\  
6.    lstcml/vsftpd

_复制代码_

创建用户

1.  docker exec 容器名称 addusr 用户名 密码

_复制代码_

实例  

1.  docker exec ftp addusr lstcml 123456

_复制代码_

**注意：**  
**/etc/vsftpd：包含 vsftp.conf 和 chroot_list**  
**chroot_list：需要禁用某个用户直接编辑此文件，删除对应用户名即可**  
**所有用户的访问范围只限制于自身 home 目录（多用户访问独立）**  
**如果访问没权限，说明映射到宿主机的目录没有权限，如 \*\***/docker/ftp/home/ftpusr，需基于对应读写权限 (完整权限：chmod 777\*\* **/docker/ftp/home/ftpusr\*\***)\*\*  

**如果是小钢炮或其他安装了 vsftp 服务的系统需要关闭自带 \*\***vsftp\***\* 服务，否则端口占用导致创建容器失败 \*\***24、firefox\*\* 作用：docker 中的火狐浏览器

Uasge：

1.  docker run -d \\  
2.    \--restart=always \\  
3.    \--name firefox \\  
4.    \-p 8081:6080 \\  
5.    lstcml/firefox

_复制代码_

说明：  
默认密码为 admin，暂无输入法，浏览器中输入[http://192.168.8.8:](http://192.168.8.8:6080/)8081/vnc.html 即可见证奇迹

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/bdcb4594-2533-46e0-97bc-e76e5ae5e3ae.jpeg?raw=true)

**25、n2n**

作用：一款类似 zerotier、蒲公英\*\*内网穿透的开源工具

v2 版本为 20191016 编译，启动默认不加密，如需修改命令可修改挂载卷中的运行脚本

Uasge：

客户端：

1.  docker run -d \\  
2.    \--name n2n \\  
3.    \--restart=always \\  
4.    \--device=/dev/net/tun \\  
5.    \--network host \\  
6.    \-e RUN=edge \\  
7.    \-e VERSION=v2 \\  
8.    \-e DEVICE_NAME=docker_n2n \\  
9.    \-e N2N_IP=10.10.10.2 \\  
10.   \-e N2N_GROUP=mygroup \\  
11.   \-e SU_NODE=xxx.xxx.com:10088 \\  
12.   \-e OTHER="-k xxx" \\  
13.   \--cap-add=NET_ADMIN \\  
14.   \--cap-add=SYS_ADMIN \\  
15.   lstcml/n2n

_复制代码_

服务端：

1.  docker run -d \\  
2.    \--name n2n \\  
3.    \--restart=always \\  
4.    \--network host \\  
5.    \--device=/dev/net/tun \\  
6.    \-e RUN=supernode \\  
7.    \-e VERSION=v2 \\  
8.    \-e SU_PORT=1000 \\  
9.    \--cap-add=NET_ADMIN \\  
10.   \--cap-add=SYS_ADMIN \\  
11.   lstcml/n2n

_复制代码_

参数说明：

RUN：运行程序，edge 为客户端，supernode 服务端  
VERSION：n2n 运行版本，目前仅支持 v2 和 v2s  
DEVICE_NAME：n2n 虚拟网卡名称  
N2N_IP：n2n 虚拟网 ip  
N2N_GROUP：n2n 虚拟网组名  
SU_NODE：N2N 服务节点 + 端口号，格式为 ip:port  
SU_PORT：服务端监听端口  
OTHER：用于其他参数命令，如 - k xxx 等等（选填）

在线更新：

1.  docker exec n2n update.sh

_复制代码_

**26、frp（20191021 更新）**  
作用：一款强大的内网穿透工具，支持客户端或服务端，分别提供在线和离线版本

**latest**

frpc 与 frps 在线双版本，支持 0.8.1 以上及后续更新版本，镜像极小，每次切换版本需重新创建容器，不能同时运行 frpc 与 frps，有需求可以创建两个不同的容器  
**20191021 新增自动识别下载最新版本（比如 frp 更新到 0.30.0，此容器默认到支持最新的 0.30.0），创建容器时，FRP_VERSION=latest 即可使用最新版本**

**offline**

frpc 与 frps 离线双版本，仅支持 0.8.1-0.29.0，每次切换版本需重新创建容器，不能同时运行 frpc 与 frps，有需求可以创建两个不同的容器

**默认为 frpc，版本为 0.25.2**

FRP_RUN：指定运行客户或服务端，字段为 frpc 或 frps

FRP_VERSION：指定运行 frp 版本，字段以官方版本号为准，类似 0.25.2  
Uasge：

1.  docker run -d \\  
2.    \--name frp \\  
3.    \--restart=always \\  
4.    lstcml/frp

_复制代码_

或  

1.  docker run -d \\  
2.    \--name frp \\  
3.    \--restart=always \\  
4.    lstcml/frp:offline

_复制代码_

或  

1.  docker run -d \\  
2.    \--name frp \\  
3.    \--restart=always \\  
4.    \-v /tmp/frp:/frp \\  
5.    \-e FRP_RUN=frps  \\  
6.    \-e FRP_VERSION=0.29.0 \\  
7.    lstcml/frp

_复制代码_

或  

1.  docker run -d \\  
2.    \--name frp \\  
3.    \--restart=always \\  
4.    \-v /tmp/frp:/frp \\  
5.    \-e FRP_RUN=frps  \\  
6.    \-e FRP_VERSION=0.29.0 \\  
7.    lstcml/frp:offline

_复制代码_

**27、d\*\***uplicati\*\*  
作用：备份、同步工具，支持 FTP、Webdav、dropbox、Goodle Drive 等等

说明：基于 linuxserver/duplicati 镜像修改为在线版，首次启动较慢，自动更新为最新版，默认时区已设置为 Asia/Shanghai

Uasge：

1.  docker run -d \\  
2.    \--name=duplicati \\  
3.    \--restart=always  \\  
4.    \-p 8082:8200 \\  
5.    lstcml/duplicati

_复制代码_

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/e2f7f1bf-1b1d-483c-b1bc-7f5aff5268c5.jpeg?raw=true)
**28、qq**作用：聊天工具

Uasge：

1.  docker run -d \\  
2.    \--restart=always  \\  
3.    \--name qq \\  
4.    \-p 8083:6080 \\  
5.    lstcml/qq

_复制代码_

说明：  
默认密码为 admin，暂无输入法，浏览器中输入[http://192.168.8.8:](http://192.168.8.8:6080/)8083/vnc.html 即可见证奇迹  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/28971f44-4766-4364-b16e-f3e8c0e9eb87.jpeg?raw=true)
**29、\*\***forsaken-mail\*\* 作用：临时邮箱

Uasge：

1.  docker run -d \\  
2.    \--restart=always  \\  
3.    \--name forsaken-mail \\  
4.    \-p 25:25 \\  
5.    \-p 8084:3000 \\  
6.    lstcml/forsaken-mail

_复制代码_

无公网在 N1 中的使用方法：  
前提条件：域名与云服务器（vps）~1、域名（test.com）解析

a、把 test.com 用 MX 解析到 mail.test.com（这个是子域名，自定义）  
b、把 mail.test.com 用 A 记录解析到云服务器 IP

~

2、云服务器开放 25 端口，并启用 frps 或 ngrok 等等内网穿透工具（用于 N1 的内网穿透）

3、N1 中新建内网穿透把 25 端口 tcp 映射到云服务器的 25 端口，3001 端口 http 映射到 mail.test.com  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/d104ce5f-627e-4f0e-a2a7-d893873a185c.jpeg?raw=true)

4、浏览器打开 mail.test.com，获取到临时邮箱地址，尝试用其他邮箱发送一条邮件，接收成功即 OK  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/24715b3a-4f51-4e52-bc6d-f868f3b90e43.jpeg?raw=true)

**30、\*\***freenom\*\* 作用：freenom 域名自动续期

Uasge：

1.  docker run -d \\  
2.    \--restart=always  \\  
3.    \--name freenom \\  
4.    \-v /docker/freenom/cron:/var/spool/cron/crontabs \\  
5.    \-v /docker/freenom/conf:/conf \\  
6.    lstcml/freenom

_复制代码_

仅支持 qq 邮箱，默认为每天 8 点定时执行，如无续期的域名，不会收到邮件

使用方法：

修改 / docker/freenom/conf（对应自行挂载的目录）中 config.php 对应字段发件与收件邮箱

可选：修改 / docker/freenom/cron（对应自行挂载的目录）中 root 定时执行时间

测试环节：

把 config.php 中 freenom 登录账户或密码设置错误，重启容器，能收到邮件即为成功

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/4a70286a-c38f-474e-8fd6-f90379edb5d2.png?raw=true)

**31、\*\***mutt\*\* 作用：封装在镜像中的 mutt，可用于邮箱监控提醒

Uasge：

1.  docker run -d \\  
2.    \--restart=always  \\  
3.    \--name mutt \\  
4.    \-e SMTP=smtp.163.com \\  
5.    \-e PORT=25 \\  
6.    \-e EUSER=XXX@163.com \\  
7.    \-e EPASSWD=XXX \\  
8.    \-e NAME=XXX \\  
9.    lstcml/mutt

_复制代码_

1.
2.  SMTP：SMTP 服务器  
3.  PORT：smtp 端口，目前只支持 25  
4.  EUSER：邮箱账号  
5.  EPASSWD：邮箱密码或授权码  
6.  NAME：发件人名称（自己的名称，自定义）

_复制代码_

发送邮件（标题不支持空格）：  

1.  docker exec 容器名称 send 邮件标题 邮件正文 收件人邮箱地址

_复制代码_

例：  

1.  docker exec mutt send  "Information_from_N1""Frp has stopped" \[email]xxx@163.com\[/email]

_复制代码_

\\n 用于换行：  

1.  docker exec mutt send  "来自 N1 的监控信息" "Frpc 服务已经停止！\\nSamba 服务已停止 " xxx@qq.com

_复制代码_

**32、\*\***Easy-Explorer\*\* 作用：易有云，一款牛\*的同步 + 管理工具

Uasge：

1.  docker run -d \\  
2.    \--name easy-explorer \\  
3.    \--restart=always \\  
4.    \-p 8085:8899 \\

_复制代码_

1.  docker run -d \\  
2.    \--name easy-explorer \\  
3.    \--restart=always \\  
4.    \-e VERSION=20181206.150400 \\  
5.    \-e PORT=8809 \\  
6.    \-e TOKEN=xxx \\  
7.    \-p 8085:8809 \\  
8.    \-v /docker/easy-explorer:/media \\  
9.    lstcml/easy-explorer

_复制代码_

VERSION：以官方下载目录命名，类似 20181206.150400、20190509.153050 等

TOKEN：登录 ddnsto.com 获取的令牌  
PORT：自定义访问 easy-explorer 端口  
![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-36-03/9e5bbef7-fafd-4732-aa1d-a4f772487290.jpeg?raw=true)

官方使用手册：[飞机直达](https://koolshare.cn/thread-129199-1-1.html)

**33、oneindex**作用：利用 OneDrive 打造专属分享型网盘

Uasge：

1.  docker run -d \\  
2.    \--name oneindex \\  
3.    \-p 8086:80 \\  
4.    \--restart=always \\  
5.    \-v ~/oneindex/config:/var/www/html/config \\  
6.    \-v ~/oneindex/cache:/var/www/html/cache \\  
7.    \-e REFRESH_TOKEN='0 \* \* \* \*' \\  
8.    \-e REFRESH_CACHE='\*/10 \* \* \* \*' \\  
9.    lstcml/oneindex

_复制代码_

![](https://i.loli.net/2020/02/26/gvPTIKEMp7D3Wyr.jpg)

说明：  

浏览器中输入[http://192.168.8.8:](http://192.168.8.8:8085/)8086 即可见证奇迹

**REFRESH_TOKEN：使用 crontab 进行 token 更新，默认 0 \* \* \* \*，即每小时更新一次**

**REFRESH_CACHE：使用 crontab 进行缓存更新，默认\*/10 \* \* \* \*，即每 10 分钟更新一次**

**34、tyyp**作用：利用天翼云盘打造专属分享型网盘 - PHP 版

Uasge：

1.  docker run -d \\  
2.    \--name ttyp \\  
3.    \-p 8087:80 \\  
4.    \--restart=always \\  
5.    lstcml/tyyp

_复制代码_

![](https://i.loli.net/2020/02/26/uEgNdUTIfp8czWC.jpg)

说明：  

浏览器中输入[http://192.168.8.8:](http://192.168.8.8:8086/)8087 即可见证奇迹

使用：

1、解压源码到空间或云服务器

2、获取并写入 cookie，访问 [http:// 你的域名或本地 IP 地址 / api/api.php?login=0&username = 天翼云登录手机号码 & password = 天翼云登录密码](http://你的域名或本地IP地址/api/api.php?login=0&username=天翼云登录手机号码&password=天翼云登录密码)

3、可创建定时任务访问 2 中的 url，获取新 cookie，保持 cookie 长期有效

**35、tywp**作用：利用天翼云盘打造专属分享型网盘 - Python 版**更新：20200228 修复识别无后缀文件为目录的 bug**  
Uasge：

1.  docker run -d \\  
2.    \--name tywp \\  
3.    \-p 8088:5000 \\  
4.    \-e NAME = 我的资源站 \\  
5.    \-e USERNAME=13800138000 \\  
6.    \-e PASSWORD=123456 \\  
7.    \-e FOLDERID=1130136913768131 \\  
8.    lstcml/tywp

_复制代码_

![](https://i.loli.net/2020/02/27/DXVCk9ZQpUhI5nN.jpg)

浏览器中输入[http://192.168.8.8:](http://192.168.8.8:8087/)8088 即可见证奇迹，界面更为美观，主题为 oneindex 默认主题

说明：

NAME：网站标题

USERNAME：登录天翼云盘的手机号码

PASSWORD：天翼云盘登录密码

FOLDERID：需要分享天翼云盘目录的 ID，获取方式：登录网页版天翼云盘后，进入需要分享的目录，复制浏览器地址栏中最后一串数字，如：

_[https://cloud.189.cn/main.action#home/folder/31715148631019663](https://cloud.189.cn/main.action#home/folder/31715148631019663)_中的**31715148631019663\*\***36、Aria2\*\* 作用：下载神器  
Uasge：

1.  docker run -d \\  
2.    \--name aria2 \\  
3.    \-e GUI=on \\  
4.    \-e RPC_PASSWORD=123456 \\  
5.    \-p 6800:6800 \\  
6.    \-p 8089:80 \\  
7.    \-v /media/sda1/download:/mnt/sda1/download \\  
8.    \-v /docker/aria2:/etc/config/aria2 \\  
9.    lstcml/aria2

_复制代码_

说明：  

浏览器中输入[http://192.168.8.8:](http://192.168.8.8:8089/)8089 即可见证奇迹

参数说明：

重启容器会触发自动更新 bt-tacker

GUI：为 on 时启用 Aria-Ng

RPC_PASSWORD：RPC 密码

**37、Sharelist**作用：一个易用的网盘工具，支持快速挂载 GoogleDrive、OneDrive ，天翼云盘可通过插件扩展功能  

Uasge：

1.  docker run -d \\  
2.    \--name sharelist \\  
3.    \-e VERSION=latest \\  
4.    \-p 8090:33001 \\  
5.    lstcml/sharelist

_复制代码_

![](https://i.loli.net/2020/03/06/bvqzFkpfdHWSAUN.jpg)

说明：  

浏览器中输入[http://192.168.8.8:](http://192.168.8.8:8090/)8090 即可见证奇迹

参数说明：

VERSION：为 latest 时默认拉取最新代码，否则使用 20200306 代码

**38、新版可道云 kodbox**

作用：类似 windows 的 web 资源文件管理器目前最新版可道云 kodbox

Uasge：

1.  docker run -d \\  
2.    \--name  kodbox \\  
3.    \-p 8091:80 \\  
4.    \--restart=always \\  
5.    \-v /docker/kdcloud:/var/www/html \\  
6.    lstcml/kodbox

_复制代码_

![](https://i.loli.net/2020/03/08/RzQw2ixjTplHs6t.jpg)

说明：

浏览器中输入[http://192.168.8.8:](http://192.168.8.8:8091/)8091 即可见证奇迹

**39、Zfile**

作用：一个在线文件目录的程序, 支持各种对象存储和本地存储

Uasge：

1.  docker run -d \\  
2.    \--name  zfile \\  
3.    \-p 8092:8080 \\  
4.    \--restart=always \\  
5.    \-v /docker/zfile/:/root/.zfile/ \\  
6.    lstcml/zfile

_复制代码_

在线更新：

docker exec zfile update.sh

**40、speedtest**

作用：一个 HTML 在线测试网络工具

Uasge：

1.  docker run -dit \\  
2.    \--name speedtest \\  
3.    \-p 8093:80 \\  
4.    \--restart=always \\  
5.    \-e MODE=standalone \\  
6.    lstcml/speedtest

_复制代码_ 
 [https://www.right.com.cn/forum/thread-911375-1-1.html](https://www.right.com.cn/forum/thread-911375-1-1.html)
