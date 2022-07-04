# 群晖NAS的Moments坑 – Chiang E's Blog
> 最近玩 Moments 又遇到群晖的坑，今天终于解决，所以记录一下。

## Moments 的安装

这一步比较简单，就略过好了。值得一提的是要用 Moments 就必须有群晖的 Drive，并且启用家目录`home`/`homes`。而 Moments 的照片和视频就会存储在当前用户对应的`home/Drive/Moments`下。

## 坑 No.1 之所在

最大的问题就是这个`home`目录所在位置必须在本机的存储空间中。但是由于我这是用 miniPC 自己搭的群晖，主要容量都在外接设备中。因此就希望想办法把 Moments 数据存储在外接设备，而不是 miniPC 的存储空间。

![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2017-11-46/c88e9828-3ecf-4859-a2c3-68d1a377d288.png?raw=true)

家目录

## 坑 No.1 之解决办法

设置开机任务，将外接设备的目录挂载到存储空间的`home/Drive`目录下。这样 Moments 存储照片和视频的时候消耗的就是外接设备的存储容量。

![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2017-11-46/548a2c76-734f-4910-9faf-1c16f5b94189.png?raw=true)

挂载

具体操作需要在`home/`下新建一个空的`Drive`文件夹，同样在外接设备的某个地方新建一个`user1/Drive`的空文件夹。加上`user1`是为了方便管理多用户。然后通过`sudo mount -B`命令把`user1/Drive`挂载到`home/Drive`即可。

> 可能有的小伙伴想到创建一个`user1/Drive`到`home/Drive`的软链接，是不是也能成功呢？  
> 反正我试过是成功不了。可能是权限设置问题？如果有解决了的朋友，望指教。

## 坑 No.2 之所在

第二个坑一句话就可以说完：**上传的视频无法显示缩略图**。  
直到我搜到一位大哥的 Blog，我才解决这个问题.

> 参考：[Shanks’s Blog: 利用 FFmpeg 和 Python3 解决群晖 Moments 和 Video Station 中视频不显示略缩图问题](https://yudelei.com/79.html)

根据这位大哥的解决办法，问题应该是原系统的 ffmpeg 无法将 Moments 的视频转码。

## 坑 No.2 之解决办法

### 安装 ffmpeg 和 python3

1.  套件中心 -> 常规 -> 信任层级：任何发行者 -> 确定
2.  套件中心 -> 套件来源 -> 新增 -> 名称：随意，位置：[https://packages.synocommunity.com/](https://packages.synocommunity.com/) -> 确定
3.  套件中心 -> 开发工具 -> 第三方：Python3：安装套件
4.  套件中心 -> 右下角：社群 ->ffmpeg：安装套件

### 解决 Moments 中视频无略缩图问题

1.  控制面板 -> 应用程序—> 终端机和 SNMP：启动 SSH 功能 -> 确定
2.  SSH 连接到群晖系统
3.  将新的新安装的 ffmpeg 软链到原 ffmpeg

\|  \| 

\# 备份原 ffmpeg 为 ffmpeg.old

sudo mv  /usr/bin/ffmpeg  /usr/bin/ffmpeg.old

\# 创建软链接

sudo ln  -s  /volume1/\\@appstore/ffmpeg/bin/ffmpeg  /usr/bin/

 \|

5.  Moments-> 左下角头像 -> 设置 -> 常规 -> 索引：选中全部重建索引，并点击重建索引
6.  等待索引重建完成，刷新即可

效果如下图。  

![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2017-11-46/f21f397d-aeb2-4e59-be28-2630e494ab3f.png?raw=true)

朋友家的可爱柴犬抹茶

## 坑 No.3 之所在

解决了上面的问题之后，我重启了群晖，测试挂载和 Moments 是否正常。测试结果是运行正常。但是我发现我本机的存储空间的剩余容量居然蒸发了 10 多 G。于是我意识到我遇到了第 3 个坑。

一开始我怀疑是挂载的问题，有没有可能是既存储在了`user1/Drive`也存储在了`home/Drive`？但是简短的测试后发现不是这个问题。

于是我就开始找消失的容量都消耗在哪里了。结果发现

![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2017-11-46/442ca0d6-a247-45da-a902-8b7a4ea827a3.png?raw=true)

消失的容量

每次容量都增加在`@synologydrive`这个文件夹中。于是我就想到肯定是 Drive 的版本管理设置的问题。因此仅需要在 Drive 的管理控制台中禁用`我的文件`的版本控制即可。

![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2017-11-46/63bbda4b-c8d7-482e-9823-4f339b68cb62.png?raw=true)

禁用版本控制

具体原因应该是每次重新挂载的时候，Drive 以为全都是新的文件，于是就又做了一版版本管理，于是就占用了很大的空间。

> 至此 Moments 坑踩完，债见 =\_=

-   [Moments](http://www.chiange.com/tag/moments/)
-   [NAS](http://www.chiange.com/tag/nas/)
-   [Synology](http://www.chiange.com/tag/synology/) 
    [http://www.chiange.com/%E7%BE%A4%E6%99%96nas%E7%9A%84moments%E5%9D%91/](http://www.chiange.com/%E7%BE%A4%E6%99%96nas%E7%9A%84moments%E5%9D%91/)
