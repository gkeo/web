# 重新学习并解锁emby4.6.7,4.7.*版本 - 如意云-莘家小站
**pojie 其他组织的软件不利于官方社区的发展，试用的老哥们建议以后争取入正，省去麻烦。** 教程仅供参考学习，文章采用 [知识共享署名 - 相同方式共享 4.0 国际许可协议](http://creativecommons.org/licenses/by-sa/4.0/) 进行许可, 但是不建议使用文章内容进行经济活动, 你的行为会对圈子和原开发者造成极大的伤害_**。** _

试用会员功能觉得满意的话，你应该**购买官方正版**。 你应该去支持一下官方的工作，价格毕竟也不贵，还能得到官方社区的技术支持，后续版本升级也方便。

**官方正版购买链接** [https://emby.media/premiere.html](https://emby.media/premiere.html)

**本文并不适合完全小白食用。**  
**记得备份一下元数据，防止出问题。** 

_**有问题请加群**_ [(点击加群)](https://jq.qq.com/?_wv=1027&k=Ys3XV1BZ) _**，评论区已关闭。** _

[](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/2BBDF9EA9ECE45AB247C09CC2AAA1EEC.webp)[![](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/2BBDF9EA9ECE45AB247C09CC2AAA1EEC.webp)
](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/2BBDF9EA9ECE45AB247C09CC2AAA1EEC.webp)

一群已满，二群 1031304710

实时请求次数: 103687

## 版本历史与建议

    2022.6.27: 更新4.7.5版本, 更新docker镜像, 更新双端文件, 更新群晖一键脚本. 相比于4.7.4, , emby官方 1. 修复了4.7.4版本web端字幕太小的问题. 2.修复一些表格式图的问题 3.修复直播录制的一些问题.

    2022.6.20: 更新4.7.4版本, 更新docker镜像, 更新双端文件, 更新群晖一键脚本. 相比于4.7.2, 4.7.3, emby官方 0. 修复了4.7.3版本群晖和威联通缺库文件, 因而无法运行的问题.1.增加剧集介绍检测和**跳过片头功能(相比4.7.2, 彻底支持一键开启)**.2. 增加新的视图--表格视图。3. 在海报视图中增加更多的选项供选择。4. 增加新的主题。5. 修复下载后, 自动重新下载的问题。6. 即使用户选项被设置为无，也能记住字幕选择。7. 修复转码内封字幕时的图形字幕的位置。8. 修复重新排列电视直播频道时的零星错误。9. 改进xml tv的自动频道映射。10. 支持按作曲家排序.

    2022.6.4: 更新4.7.2版本. 更新docker镜像,更新双端文件,更新群晖一键脚本. 更新4.7.\*版本群晖脚本的sb小bug.相比于4.7.1.0版本, emby官方 1.在媒体库选项中增加剧集介绍检测功能. 2.增加用户配置选项，可以跳片头(需要改配置文件才能生效). 3.修复从文件名中解析集数时的集数倒退问题. 4.修复家庭录像的文件名在文件名的最后一个点之后被截断的问题. 5.修复删除下载任务时重新下载的问题. 6.如果启用字幕选择，记住即使用户字幕模式被设置为默认为无.**从这个版本开始不再做所谓的美化包, 不再更新ubuntu脚本, 由于更新一次全端耗时太长,除非有重大更新,否则以后只能做到随缘更新了.** **请各位手动关掉自动更新, 位置在 设置 - 自动更新 - 允许服务器自动重启以应用更新. 取消勾选然后点击保存.**

    2022.5.26: 更新4.7.1版本. 相比于4.7.0.60, 官方 1.修复了windows上不必要的ffmpeg依赖问题, 2.提升了内嵌中文字幕和音轨的体验, 3.修复了一堆错乱抽风的按钮(4.7.0.60版本所有人都会遇到), 4.歌词开始显示时间轴. 已经安装了4.7.0.60的同学可以考虑升级。

    2022.5.20: 修复群辉一键脚本，更新ubuntu一键脚本的sb小bug，更新群晖命令。修复受到影响的docker镜像。

    2022.5.19: 更新4.7.0.60正式版，更新群辉一键脚本，更新ubuntu一键脚本，更新docker，更新美化包。更新unraid模板。

    2022.5.9: 更新群晖一键脚本到v1.2，解决运行脚本后套件无法启动的问题。更新4.7.38双平台测试版。

    2022.4.8: 更新unraid模板, 默认加上了代理的字段, 需要自己设置好http和https代理地址。不需要的话请删掉这两个字段

[](https://cdn2.jiawei.xin/wp-content/uploads/2022/04/qwed126ef5.webp)[![](https://cdn2.jiawei.xin/wp-content/uploads/2022/04/qwed126ef5-1024x771.webp)
](https://cdn2.jiawei.xin/wp-content/uploads/2022/04/qwed126ef5-1024x771.webp)

## 前言

以前，emby 开心用户靠 neko.re 老哥的第三方激活服务器和加速套件得以存活，今天，发现老哥的服务器已经全关了，意料之中，早晚的事，幸亏能用的时候就备份了老哥原来的返回格式。

```

//https://mb3admin.com/admin/service/registration/validateDevice  
//https://crackemby.mb6.top/admin/service/registration/validateDevice.php?  
{"cacheExpirationDays": 365,"message": "Device Valid","resultCode": "GOOD"}

//https://crackemby.mb6.top/admin/service/registration/getStatus.php  
{"deviceStatus":"0","planType":"Lifetime","subscriptions":{}}

//https://crackemby.mb6.top/4670/registration/validate.php  
{"featId":"MBSupporter","registered":true,"expDate":"2030-01-01","key":114514}

```

搜索一番，发现并没有老哥接手，所以做了这个帖子。

不想看原理的话，可以用我的激活服务器试用一下，直接跳到文章末尾。

当初保存了 neko 老哥的一个页面快照，要是下文看不懂的话，可以进去看看老哥原来咋写的，文中大部分方法已经失效或过时。

[https://act.jiawei.xin:8090/ns/sharing/B1EhF](https://act.jiawei.xin:8090/ns/sharing/B1EhF)

## 搭建激活服务器

首先，关闭 emby 服务器。

~windows 系统运行 ipconfig /flushdns 来清除 dns 缓存，为屏蔽更新做准备。~

~ 屏蔽更新，屏蔽以下网址来屏蔽 emby 的更新，防止更新后 pojie 失效。方法请自行百度。~

[~https://emby.media/community/index.php?/blog/rss/1-media-browser-developers-blog~](https://emby.media/community/index.php?/blog/rss/1-media-browser-developers-blog)

屏蔽请求的拦截成功率太低，放弃这种方法，已经在 pojie 文件里完成禁止后台更新。

屏蔽 https 请求有时候会失效，所以还修改了一个 windows 端负责更新的 Emby.Server.Updater.dll，只是不自动更新，但是还是有提示更新的。如果愿意，可以把这个也替换进去。屏蔽 emby 更新，并不会屏蔽插件更新。

配置服务器，并**设置允许跨域请求**

[](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/QQ%E5%9B%BE%E7%89%8720210925174049.webp)[![](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/QQ%E5%9B%BE%E7%89%8720210925174049.webp)
](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/QQ%E5%9B%BE%E7%89%8720210925174049.webp)

validateDevice 请求

[](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/QQ%E5%9B%BE%E7%89%8720210925174044.webp)[![](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/QQ%E5%9B%BE%E7%89%8720210925174044.webp)
](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/QQ%E5%9B%BE%E7%89%8720210925174044.webp)

getStatus 请求

1.  修改 Emby.Web.dll 的官方认证链接为自建服务器地址，pojie 网页端。

[](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/698527.webp)[![](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/698527.webp)
](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/698527.webp)

pojie 硬解，网页

2. 修改 MediaBrowser.Model.dll 来显示激活页面（属于强迫症了，非要能显示这个页面。其实不做这步也能使用）

[](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/QQ%E5%9B%BE%E7%89%8720210925174039-1.webp)[![](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/QQ%E5%9B%BE%E7%89%8720210925174039-1-1024x585.webp)
](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/QQ%E5%9B%BE%E7%89%8720210925174039-1-1024x585.webp)

显示小金牌

3. 更改 embypremiere.js 内的认证地址为自建服务器地址，pojie 网页播放器。

[](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/74938615-1024x649-1.webp)[![](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/74938615-1024x649-1.webp)
](https://cdn2.jiawei.xin/wp-content/uploads/2021/09/74938615-1024x649-1.webp)

lifetime 许可证

4.Emby.Server.Implementations.dll 暂时不写了，这个需要改 IL，挺麻烦。这个文件必须替换进去. 授权客户端, 硬解, 照片备份, 离线视频等.

如果想要尝试可以参考 [这篇群友提供的笔记](https://act.jiawei.xin:8090/ns/sharing/imDUa) 由群友 @Four seasons 提供.

5\. 插件加速代理暂时不做了，安装离线包 ba。

## emby-server 端 pojie 文件的替换

**windows 服务器**

所有需要替换的关键 dll 都在安装目录 Emby-Server\\system 下。

embypremiere.js 在你的安装目录 Emby-Server\\system\\dashboard-ui\\embypremiere 下。

connectionmanager.js 在 system\\dashboard-ui\\bower_components\\emby-apiclient\\下。

之后去激活页面随便填个激活码，重启 emby 服务器，就可以试用了。

## 客户端的解决方案

安卓客户端请依然使用 emby 白嫖小秘的方案，针对小米机型还加入了魔改。

ios 用户请花 30 块购买单设备永久授权, 来支持 emby 官方的开发 [https://emby.media/premiere.html](https://emby.media/premiere.html) 或者其他方法? 八仙过海各显神通.

电脑用户无需 pojie，请使用网页版。或者 exe 吧，emby 小秘的。

**关于浏览器端**

**替换完成启动之前需要清除浏览器缓存**，可以参考此视频：[清除 emby 网页缓存](https://act.jiawei.xin:10086/tmp/emby/1637754815094.mp4) ，不然的话出不了金徽章。有时候使用本地回环地址访问 (127.0.0.1,localhost) 也会出现跨域问题，需要 ssl 或者非回环地址访问才能解决。建议使用 Chrome 最新版。

## 整合网盘，服务端替换文件

整合网盘：[https://act.jiawei.xin:10086/tmp/?dir=emby/](https://act.jiawei.xin:10086/tmp/?dir=emby/)

服务端历史版本: [https://github.com/MediaBrowser/Emby.Releases/releases/](https://github.com/MediaBrowser/Emby.Releases/releases/)

**解锁的 emby 插件**： 插件安装目录元数据文件夹里，\\programdata\\plugins，一般这个目录是挂载出来的。

[https://github.com/AlifeLine/Emby.Plugins.Douban](https://github.com/AlifeLine/Emby.Plugins.Douban) 可以参考豆瓣插件的安装方法。

LDAP 插件可以让 emby 和群晖公用同一套账号 剩下一个插件是剧院模式插件。

[https://act.jiawei.xin:10086/tmp/?dir=emby%2Fplugins](https://act.jiawei.xin:10086/tmp/?dir=emby%2Fplugins)

**美化包** (从 4.7.1 版本开始不再提供所谓的美化包)

4.6.7 版本：[https://act.jiawei.xin:10086/tmp/emby/4.6.7.0/beautify01.7z](https://act.jiawei.xin:10086/tmp/emby/4.6.7.0/beautify01.7z)

4.7.0 版本：[https://act.jiawei.xin:10086/tmp/emby/4.7.0.60/beautify.zip](https://act.jiawei.xin:10086/tmp/emby/4.7.0.60/beautify.zip)

4.7.1.0 版本: [https://act.jiawei.xin:10086/tmp/emby/4.7.1.0/beautify.7z](https://act.jiawei.xin:10086/tmp/emby/4.7.1.0/beautify.7z)

**客户端 apk：** 不再提供，请使用电报 emby 小秘的资源.

**windows64 位替换文件**

4.6.7 版本：[https://act.jiawei.xin:10086/tmp/emby/4.6.7.0/win64-4.6.7.7z](https://act.jiawei.xin:10086/tmp/emby/4.6.7.0/win64-4.6.7.7z)

~4.7.0.60 版本：[https://act.jiawei.xin:10086/tmp/emby/4.7.0.60/win64_with_beautify_4.7.0.60.7z](https://act.jiawei.xin:10086/tmp/emby/4.7.0.60/win64_4.7.0.60.7z)~

4.7.1.0 版本: [https://act.jiawei.xin:10086/tmp/emby/4.7.1.0/win64_4.7.1.7z](https://act.jiawei.xin:10086/tmp/emby/4.7.1.0/win64_4.7.1.7z)

4.7.2.0 版本: [https://act.jiawei.xin:10086/tmp/emby/4.7.2.0/win64_4.7.2.7z](https://act.jiawei.xin:10086/tmp/emby/4.7.2.0/win64_4.7.2.7z)

4.7.4.0 版本: [https://act.jiawei.xin:10086/tmp/emby/4.7.4.0/win64_4.7.4.7z](https://act.jiawei.xin:10086/tmp/emby/4.7.4.0/win64_4.7.4.7z)

4.7.5.0 版本: [https://act.jiawei.xin:10086/tmp/emby/4.7.5.0/win64_4.7.5.7z](https://act.jiawei.xin:10086/tmp/emby/4.7.5.0/win64_4.7.5.7z)

**linux x64 通用替换文件**

4.6.7 版本：[https://act.jiawei.xin:10086/tmp/emby/4.6.7.0/linux64-4.6.7.7z](https://act.jiawei.xin:10086/tmp/emby/4.6.7.0/linux64-4.6.7.7z)

~4.7.0.60 版本： [https://act.jiawei.xin:10086/tmp/emby/4.7.0.60/linux64%26docker_with_beautify_4.7.0.60.7z](https://act.jiawei.xin:10086/tmp/emby/4.7.0.60/linux64%26docker_with_beautify_4.7.0.60.7z)~

4.7.1.0 版本: [https://act.jiawei.xin:10086/tmp/emby/4.7.1.0/linux64_docker_4.7.1.7z](https://act.jiawei.xin:10086/tmp/emby/4.7.1.0/linux64_docker_4.7.1.7z)

4.7.2.0 版本: [https://act.jiawei.xin:10086/tmp/emby/4.7.2.0/linux64_docker64_4.7.2.7z](https://act.jiawei.xin:10086/tmp/emby/4.7.2.0/linux64_docker64_4.7.2.7z)

4.7.4.0 版本: [https://act.jiawei.xin:10086/tmp/emby/4.7.4.0/linux64_docker64_4.7.4.7z](https://act.jiawei.xin:10086/tmp/emby/4.7.4.0/linux64_docker64_4.7.4.7z)

4.7.5.0 版本: [https://act.jiawei.xin:10086/tmp/emby/4.7.5.0/linux64_docker64_4.7.5.7z](https://act.jiawei.xin:10086/tmp/emby/4.7.5.0/linux64_docker64_4.7.5.7z)

**ubuntu-x64 一键 pojie 脚本**  _(4.7.1.0 版本后不再提供, 请尝试手动替换)_

```

\# 4.6.7版本  
wget https://act.jiawei.xin:10086/tmp/emby/4.6.7.0/ubuntu\_crack.sh  
sudo bash ubuntu\_crack.sh

```

```

\# 4.7.1.0版本  
wget https://act.jiawei.xin:10086/tmp/emby/4.7.1.0/ubuntu\_crack\_4.7.1.sh  
sudo bash ubuntu\_crack\_4.7.1.sh

```

**DSM 替换脚本**

推荐**优先使用 wget**执行：

```

#4.6.7.0版本  
curl https://act.jiawei.xin:10086/tmp/emby/4.6.7.0/synology6\_7crack.sh | bash

```

```

#4.7.1.0版本  
curl https://act.jiawei.xin:10086/tmp/emby/4.7.1.0/synology6-7crack\_4.7.1.sh | bash

```

```

\# 4.7.2.0版本  
curl https://act.jiawei.xin:10086/tmp/emby/4.7.2.0/synology6-7crack\_4.7.2.sh | bash

```

```

\# 4.7.4.0版本  
curl https://act.jiawei.xin:10086/tmp/emby/4.7.4.0/synology6-7crack\_4.7.4.sh | bash

```

```

\# 4.7.5.0版本  
curl https://act.jiawei.xin:10086/tmp/emby/4.7.5.0/synology6-7crack\_4.7.5.sh | bash

```

wget 方式：

```

\# 4.6.7版本  
wget https://act.jiawei.xin:10086/tmp/emby/4.6.7.0/synology6\_7crack.sh  
sudo bash synology6\_7crack.sh

```

```

\# 4.7.1.0版本  
wget https://act.jiawei.xin:10086/tmp/emby/4.7.1.0/synology6-7crack\_4.7.1.sh  
sudo bash synology6-7crack\_4.7.1.sh

```

```

\# 4.7.2.0版本  
wget https://act.jiawei.xin:10086/tmp/emby/4.7.2.0/synology6-7crack\_4.7.2.sh  
sudo bash synology6-7crack\_4.7.2.sh

```

```

\# 4.7.4.0版本  
wget https://act.jiawei.xin:10086/tmp/emby/4.7.4.0/synology6-7crack\_4.7.4.sh  
sudo bash synology6-7crack\_4.7.4.sh

```

```

\# 4.7.5.0版本  
wget https://act.jiawei.xin:10086/tmp/emby/4.7.5.0/synology6-7crack\_4.7.5.sh  
sudo bash synology6-7crack\_4.7.5.sh

```

DSM 解锁套件: 移步矿神论坛: [https://imnks.com/1780.html](https://imnks.com/1780.html)

**docker-x64 版替换文件**

```

docker pull xinjiawei1/emby\_unlockd:latest

```

**UNRAID** (6.9.2 版本才能用, 之后版本给阉了)

unraid 模板 ：[https://github.com/xinjiawei/unraidtemplates](https://github.com/xinjiawei/unraidtemplates)

## 发现的问题

零丶黑裙转码问题 参考矿神论坛 [https://imnks.com/385.html](https://imnks.com/385.html)

一丶

    日志中出现报错:Error in Directory watcher for: "/media/movies" System.IO.IOException: The configured user limit (8192) on the number of inotify watches has been reached.

```

\# 群晖为例,保守修改为20480,别的例子都是修改为204800. 改大之后耗费内存.  
root@XeIrs:~# cat /proc/sys/fs/inotify/max\_user\_watches  
8192  
root@XeIrs:~# echo 20480 &gt; /proc/sys/fs/inotify/max\_user\_watches  
root@XeIrs:~# cat /proc/sys/fs/inotify/max\_user\_watches  
20480  
root@XeIrs:~# echo "fs.inotify.max\_user\_instances = 256" &gt;&gt; /etc/sysctl.conf

```

二丶

    日志中出现报错:System.IO.IOException: System.IO.IOException: The configured user limit (128) on the number of inotify instances has been reached, or the per-process limit on the number of open file descriptors has been reached.

```

\# 群晖为例,保守修改为256,别的例子都是修改为1000. 改大之后耗费内存.  
root@XeIrs:~# cat /proc/sys/fs/inotify/max\_user\_instances  
128  
root@XeIrs:~# echo 256 &gt; /proc/sys/fs/inotify/max\_user\_instances  
root@XeIrs:~# cat /proc/sys/fs/inotify/max\_user\_instances  
256  
root@XeIrs:~# echo "fs.inotify.max\_user\_instances = 256" &gt;&gt; /etc/sysctl.conf


```

三，群晖执行脚本报错：

erro: could not open HSTS store at ‘/root/.wget-hsts’. HSTS will be disabled.

此错误无影响。

**问题还有很多，欢迎评论指出或修复。** 

## 参考

[https://blog.hotwill.cn/emby%E5%90%AF%E7%94%A8https.html](https://blog.hotwill.cn/emby%E5%90%AF%E7%94%A8https.html)

[https://bbs.pediy.com/thread-263534.htm](https://bbs.pediy.com/thread-263534.htm)

[https://neko.re/archives/128.html](https://neko.re/archives/128.html)

[https://jellyfin.org/docs/general/administration/troubleshooting.html#real-time-monitoring](https://jellyfin.org/docs/general/administration/troubleshooting.html#real-time-monitoring) 
 [https://blog.jiawei.xin/?p=469](https://blog.jiawei.xin/?p=469)
