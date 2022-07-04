# 换个姿势玩手机 篇四：免ROOT把旧手机改造成NAS进阶版，让你的旧手机焕发第二春（保姆版教程）_NAS存储_什么值得买
2022-06-27 22:25:30 19 点赞 57 收藏 10 评论

## 导言

在上一篇文章中，和大家分享了用超级简单的方法把手机改造成 “微型 NAS”。但是，离真正的 NAS 还是差的远，最多算是一个下载机。在这篇文章中，我将和大家一起用稍微“高级” 一丢丢的方法把旧手机改造地更像 NAS。当然，这依然是免 ROOT 的。  

## 正文

首先，和大家简单说一下原理：在[安卓手机](https://www.smzdm.com/fenlei/androidshouji/)上搭建一个 WEB[服务器](https://www.smzdm.com/fenlei/fuwuqi/)>> 建一个简单的网站 >> 网站上集成所有需要的功能（比如远程下载、文件同步、网络相册、网络共享等功能）>> 用其它设备访问网站（跟访问群晖系统一样）。

首先，我们需要在手机上安装一款叫 KSWEB 的 APP，KSWEB 是由俄罗斯人开发的一款基于 Android 的开源服务器，使用 lighttpd/nginx+php+sql 可以使你的安卓手机瞬间变成一台服务器，并且兼容多数主流 PHP 程序。这个软件的功能很强大，使用也很简单，支持中文，而且没有广告，免 ROOT 运行。不过，最新版的 KSWEB 需要付费使用，不过，用旧版本就可以了。我找了两个可以免费使用的版本，一个是 3.93，适合安卓版本 7.0 以下的安卓手机，一个是 3.96，适合安卓 8.0 以上的手机使用。文章涉及的软件和文件我都会分享在百度网盘里供大家下载使用。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/624a80d3-2537-484d-bb4e-da47dc388225.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_2/)

安装并打开 KSWEB，记好软件上 192.168.31.16 这个 IP 地址，这是你的手机地址，后面需要通过这个地址访问手机 NAS，每个人的地址都是不一样的。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/feb6b1c0-5af1-4730-ba9f-3919e65b94b0.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_3/)

在 KSWEB 最上面那栏找到 “工具”  

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/69dd2e7e-6560-4e04-bc0c-f1299b494f4f.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_4/)

点击 辅助工具 >>phpMyadmin，这个时候会下载数据，等待下载完成

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/116276c8-dbda-4816-bc77-7d060c9f79ec.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_5/)

数据下载完成后，会弹出选择服务器选项，选择 Lighttpd，确认  

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/74d4e63b-7970-4398-81c5-584331aed0a8.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_6/)

点击 确认 ，重启 KSWEB 软件。重启后在 KSWEB 最上面那栏找到 “工具”

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/9b5c766a-27f4-422b-b7e6-dbb6e83326ad.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_7/)

点击 “打开 WebFace”  

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/61cba4c6-4c59-4a78-989f-679c5f0920c7.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_8/)

弹出选择服务器选项，选择 Lighttpd，确认  

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/6a5f3ee2-024d-4777-9c51-94d76c2d8b41.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_9/)

点击 确认 后 ，重启 KSWEB 软件。

把手机通过 USB[数据线](https://www.smzdm.com/fenlei/shujuxian/)连接电脑。打开手机存储，找到 htdocs 这个[文件夹](https://www.smzdm.com/fenlei/wenjianjia/)，删除。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/15df4ffc-42aa-4187-8ae8-2d8afe19cb2e.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_10/)

htdocs 这个文件夹是 KSWEB 软件搭建网站的数据，包含了网页和 WEB 软件。我根据网上大佬发布的数据修改和整合了一个数据包，只需要下载后替换掉整个 htdocs 文件夹就可以了，不需要重头开始做网站。

在我共享的百度网盘里下载 htdocs 这个压缩包

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/957534d2-148b-472a-82e7-38d153c5592f.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_11/)

解压后打开文件夹，找到 index.html 这个文件

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/5a8a0fa5-2494-4e79-b375-de98b5e15691.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_12/)

单击鼠标左键选中 index.html 这个文件后，单击鼠标右键选择用 Notepad++ 这个软件打开（要在电脑安装好 Notepad++）。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/8672a092-eee5-40d4-8aa9-89a874ad6120.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_13/)

打开文件后往下拉，找到 内网访问地址 这一项，把里面的地址修改成你自己的地址。

[路由器](https://www.smzdm.com/fenlei/luyouqi/)那里的地址修改成自己的路由器管理地址，我的是红米路由器，默认的是 192.168.31.1

后面那几个项目就是把 192.168.31.16 这个地址修改成自己手机的地址，在 KSWEB 软件首页上可以看到。

**注意：只修改 192.168.31.16，斜杠后面的字符不要动。** 

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/3fa08236-a9ce-4628-835e-f16f53ce8d4b.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_14/)

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/264c1437-989e-4f65-bb8e-ceb405f7b8ce.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_15/)

修改完后保存并关闭文件。然后把整个 htdocs 文件夹复制到手机里。**注意路径，复制到你之前删除 htdocs 文件夹那个位置。** 

拔掉手机的 USB 连接线，在官网下载并安装 “微力同步”，这个软件是用来同步用的，功能强大而且免费。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/e524e041-f34d-488e-8dd4-2c6376d2f373.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_16/)

安装完微力同步后，打开 KSWEB 这个软件，并设置成常驻后台和自启动。在其它设备上打开浏览器，地址栏上输入手机 IP 地址：8080 访问就可以了。比如： 192.168.31.16：8080

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/3a25d994-4380-4529-bea4-05b40e737d52.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_17/)

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-15-45/a1cce8d8-004a-4ef8-99ae-254188d08c08.jpeg?raw=true)
](https://post.smzdm.com/p/az6qk4er/pic_18/)

至此，安卓 miniNAS 就算是搭建好了，下一篇我会继续讲怎么通过外网就行访问。

## 总结

其实这套系统也不是我原创的，网上已经有大佬发过相关的教程。不过，教程不是很详细，有些关键的地方说的不是很清楚，对新手不是很友好。所以，我就在原有基础上修改和增加了一些内容，重新写了一篇教程，让它更适合新手。再次感谢大佬的开源项目！ 
 [https://post.smzdm.com/p/az6qk4er/?zdm_ss=Android_9812387715_&send_by=9812387715&from=singlemessage&invite_code=zdmddxwnvfinv](https://post.smzdm.com/p/az6qk4er/?zdm_ss=Android_9812387715_&send_by=9812387715&from=singlemessage&invite_code=zdmddxwnvfinv)
