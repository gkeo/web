# 周先生 - Docker部署EMBY
因为买了[嘎鱼饭 GD 团队盘影片库](https://gayufan.com/)，所以最近在折腾 EMBY。在 NAS、OPENWRT 软路由、PVE 下的 WINDOWS 虚拟机、VPS 上都试过，都不太爽，原因其实还是本地科学上网环境的问题，造成体验很差。遂开始琢磨在 VPS 上搭建 EMBY。但影片库实在太大，搜刮效率实在太低，同时还特别费流量，以至于活生生地干死我一台新买的[CloudCone](https://app.cloudcone.com/?ref=5837)机器。后来从卖家的一篇[《嘎鱼饭 N1 盒子家庭媒体中心解决方案》](https://docs.google.com/document/d/1wTbSRLWofIyxyMeHibSGuzZRt3yqS81X3QvLWq12Z54/) 受到启发，研究出 Docker 安装 EMBY，然后导入卖家数据的方式。省时省力，节约了流量也提升了体验。

### **1. 安装 Rclone**

```null

curl https://rclone.org/install.sh | sudo bash 


rclone config 



apt-get update
apt-get install -y fuse 

yum install -y fuse
```

### **2. 创建文件夹**

```null
mkdir -p emby 
cd emby 
mkdir -p config 
mkdir -p share1 
mkdir -p share2
```

### **3. 挂载 Rclone**

若需要挂多个盘

`#第一步`

```null
cat > /etc/systemd/system/rclone@.service <<EOF
[Unit]
Description=rclone mount %I drive
After=network.target
[Service]
Type=simple
ExecStart=/usr/bin/rclone mount %i: /root/wow/%i --umask 0000 --default-permissions --no-check-certificate --allow-non-empty --allow-other --low-level-retries 200 --vfs-read-chunk-size 64M --vfs-read-chunk-size-limit 1G --buffer-size 512M --config /root/.config/rclone/rclone.conf
[Install]
WantedBy=multi-user.target
EOF
```

#### UPDATE：20220105

今日得到一技术大佬真传，可用如下 Rclone 参数优化 Emby 播放时的速度体验。

```null
cat > /etc/systemd/system/rclone@.service <<EOF
[Unit]
Description=rclone mount %I drive
After=network.target
[Service]
Type=simple
ExecStart=/usr/bin/rclone mount %i: /root/wow/%i --use-mmap --umask 000 --default-permissions --no-check-certificate --allow-other --allow-non-empty --dir-cache-time 24h --cache-dir=/home/cache --vfs-cache-mode full --buffer-size 256M --vfs-read-ahead 512M --vfs-read-chunk-size 32M --vfs-read-chunk-size-limit 128M --vfs-cache-max-size 20G --low-level-retries 200 --config /root/.config/rclone/rclone.conf
[Install]
WantedBy=multi-user.target
EOF
```

`#第二步`

```null
for a in `grep '^\[' /root/.config/rclone/rclone.conf`
do
    b=${a:1:-1}
    [ ! -d "/root/wow/${b}" ] && mkdir /root/wow/${b}
    systemctl enable rclone@${b}
    systemctl start rclone@${b}
done
```

`#第三步`

```null
mkdir /root/wow
systemctl enable rclone@xx
systemctl start rclone@xx
```

### **4. 安装 Docker**

```null
curl -sSL https://get.docker.com/ | sh 
systemctl start docker 
systemctl enable docker
```

### **5. 拉取和安装 EMBY**

```null
docker pull emby/embyserver:latest
docker run -d --name=emby --restart=always -v /root/emby/config:/config -v /root/emby/share1:/mnt/share1 -v /root/emby/share2:/mnt/share2 -p 8096:8096 -p 8920:8920 -e UID=1000 -e GID=100 -e GIDLIST=100 emby/embyserver:latest
```

### **6. 停止 Docker**

```null
docker stop emby 


docker start emby 
docker restart emby 
docker kill emby 
docker rm emby 
```

### **7. 下载搜刮数据**

`#下载数据包`

```null
wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=1xvHa1VMl6sLyr2EJvZU0H4GQbQa61gUC' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=1xvHa1VMl6sLyr2EJvZU0H4GQbQa61gUC" -O embydata.zip && rm -rf /tmp/cookies.txt
```

`#解压数据包`

```null
unzip -o embydata.zip -d /
```

`#递归文件夹所有权`

```null
chown -R root:root /root/emby/config
```

> `其他命令`

`备份数据`

```null

tar -zcvf config.tar.gz /root/emby/config

rsync -avPh config.tar.gz #已用Rclone挂载的目录
或
rclone copy -P config.tar.gz [Rclone的盘名，如：GD]:[盘中的目录，如：/Temp]
```

`还原数据`

```null

rsync -avPh 网盘中备份的config.tar.gz地址 ~/ 

tar -zxvf config.tar.gz 
rm -rf /root/emby/config 
mv /root/root/emby/config /root/emby 
rm -rf /root/root
```

### **8. 重启 Docker**

```null
docker start emby
```

### **9. 安装 BBRPlus**

脚本来自：[秋水逸冰](https://teddysun.com/489.html)  
如果是 Ubuntu 或者 Debian 之类，安装过程很简单，如果是 CentOS，建议去[源站](https://teddysun.com/489.html)查看完整版教程。

```null
wget --no-check-certificate -O /opt/bbr.sh https://github.com/teddysun/across/raw/master/bbr.sh 
chmod 755 /opt/bbr.sh 
/opt/bbr.sh
```

检查

```null
uname -r

sysctl net.ipv4.tcp_available_congestion_control


sysctl net.ipv4.tcp_congestion_control

sysctl net.core.default_qdisc

lsmod | grep bbr

```

### **10. 配置虚拟内存**

```null
wget https://gd.5533.eu/0:/swap/swap.sh
chmod +x swap.sh
./swap.sh
```

**访问：[http://yourip:8096，账号：root，密码：111。](http://yourip:8096，账号：root，密码：111。)** 

### **11. 破解 Docker 版**

破解方法来自[jiawei](https://blog.jiawei.xin/?p=469)，为了方便，我直接打包成 Docker 了。

**UPDATE 20220626**

4.7.4.0 版，更新 X86 和 ARM 两个 DOCKER 破解版

此处内容需要 [回复](#respond "回复") 后才能查看

### **12. 中文字幕插件**

UPDATE2021.07.10，今天在[MJJ 论坛](https://hostloc.com/thread-866274-1-1.html)看到[Meiam](https://hostloc.com/space-uid-2984.html)大神开发了适用 Emby 的[中文字幕插件 - Emby.MeiamSub](https://github.com/91270/Emby.MeiamSub)，遂记录。

如果你已经把 config 文件夹映射出来，则：

```null
apt install unzip
wget https://github.com/91270/MeiamSubtitles/releases/download/1.0.8.0/Emby.MeiamSub.zip
unzip Emby.MeiamSub.zip
rm -rf Emby.MeiamSub.zip
mv ~/Emby.MeiamSub.Thunder.dll ~/emby/config/plugins
mv ~/Emby.MeiamSub.Shooter.dll ~/emby/config/plugins
docker restart emby
```

### **13、破解 OSX 客户端**

在 mac 上开启终端，[来源](https://embywiki.911997.xyz/use-on-various-devices/use-on-macos/using-official-client-on-macos/crack-with-script.html)

```null
/bin/bash -c "$(curl -fsSL https://git.io/EmbyPremiereUnlock.sh)"
```

相关阅读：

[![](https://mrchou-website-1251076893.cos.ap-beijing.myqcloud.com/wp-content/uploads/2020/07/1595954303-Emby-The-open-media-solution-769x409-1.png)
](https://mrchou.com/internet/how-to-set-emby-by-docker.html)

因为买了嘎鱼饭 GD 团队盘影片库，所以最近在折腾 EMBY。在 NAS、OPENWRT 软路由、PVE 下的 WINDOWS 虚拟机、VPS 上都试过，都不太爽，原因其实还是本地科学上网环境的问题，造成体验很差。遂开始琢磨在 VPS 上搭建 EMBY。

2020-07-28

[![](https://mrchou-website-1251076893.cos.ap-beijing.myqcloud.com/wp-content/uploads/2020/12/1608142453-emby-e1641833666923.jpg)
](https://mrchou.com/internet/lets-talk-about-private-media-library-build-by-emby-and-google-drive.html)

经过几个月的摸索，基于 Emby+Google Drive 的私人影视库算是进入生产模式了。  
今天来谈谈我的部署，希望对你有所启发。

2020-12-17

[![](https://mrchou-website-1251076893.cos.ap-beijing.myqcloud.com/wp-content/uploads/2020/08/1597510861-armbian.png)
](https://mrchou.com/topic/n1-armbian-emby.html)

在 OpenWrt 环境下运营 Rclone+Docker+Emby，但 NFS 挂载 NAS 出现问题。憋了几天，特别郁闷，也不想全网搜解决小问题的办法，心想 OpenWrt 整不明白，Linux 我会呀。于是，心一横，把 N1 刷成了 Armbian 服务器。痛快了！遂记录。

2020-08-16

**打完收工！**

阅读量: 7,492 
 [https://mrchou.com/internet/how-to-set-emby-by-docker.html](https://mrchou.com/internet/how-to-set-emby-by-docker.html)
