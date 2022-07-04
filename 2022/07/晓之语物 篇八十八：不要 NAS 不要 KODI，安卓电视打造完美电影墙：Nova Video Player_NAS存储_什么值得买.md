# 晓之语物 篇八十八：不要 NAS 不要 KODI，安卓电视打造完美电影墙：Nova Video Player_NAS存储_什么值得买
2022-06-30 17:48:17 163 点赞 1182 收藏 222 评论

大家好，我是晓飞影！

一个数码爱好者，也喜欢在众多平行领域探究摸索，让生活多一点乐趣。我找到一个神级软件，建议收藏再看，免得忘记下载地址。

## 起因

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/e874fc3b-5b2c-48eb-8b21-081da700d618.png?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_2/)

本来买了一台电视 OPPO K9 是想分享一下的，结果电视的优缺点很明显，重点是发现了另外一个神器，就是本文的重点，电视就简单说几句吧。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/3228f50d-052f-40fb-bd96-a6a28170bf42.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_3/)

55 寸电视，4K HDR，无广告，语音操控，京东自营 12 期免息，2 年延保，1379 元到手，光看这些参数简直是太值了。之前是 4000 元买的 小米电视 2 49 寸，主要是给父母的卧室看，用了这么多年，刷机后无广告，但是反应还是慢点，颜色差一点，如今能 1000 多元买到这个参数的电视，感觉比换手机还便宜。

不过安装上 OPPO K9 后体验了一番还是能看出一些端倪。

### 优点很明显：

便宜，4K 画质细腻，颜色比较准，云游戏 START 还是能玩一些大作，试了下拳皇 14 延时比较低，原神也能玩，30W 的大音箱确实音效比之前要好，平时不会开太高，也支持无线投屏。

### 缺点其实也有：

首先开机确实无广告，但是开机要 20 秒的时间一直停留在 OPPO 的 logo，问了客服说 25 秒是行业标准，家里的索尼开机 3 秒左右。刚开始使用会有点卡，开久了之后，操作起来确实流畅很多，毕竟是 2G 的内存，希望以后不要轻易升级系统变卡。语音助手也是一样，开始有点智障，要开久了后才灵活识别。但是最坑的是系统自带的文件管理器，是没有局域网访问功能的，也就是不能访问 NAS 里的电影，我家里的小米、乐视、索尼电视自带的都支持。菜单界面逻辑不是太清晰，适合年轻人不适合老年人，我为了父母看电影方便，开了极光 TV 的会员，首页左滑还会有[爱奇艺](https://pinpai.smzdm.com/37404/)、酷喵等其他的会员，父母要是不小心点错还是会提示要钱，所以我还是耐心要教几回。

这个电视的主要使用者是父母，所以不访问 NAS 也没问题，所以电视我压根没想折腾就给父母用了，目前反响还可以。

…… 说是这么说，我还是忍不住开始折腾了。

## 开始折腾

安卓电视上观赏电影海报墙可能还是我的执念，如果是玩 NAS 的用户知道用 Jellyfin、Emby、Plex 做服务器，浏览器就可以看电影墙，iOS 用户可以用 infuse，除了贵外没缺点，安卓端用的最多的就是 nplayer、VLC，播放器是厉害，但是也没有海报墙，唯一羡慕的就是芝杜盒子自带的海报墙了，在便捷程度上可能还是效果最好的，可惜不能开放出来给其他设备使用。至于 KODI，我自己也写过攻略，但是那些反人类的设置，每次换个设备重新调整一遍都要吐了，新手就更不愿意玩了。

难道真的没有办法了吗？

尽管这个电视不是我看，我还是开始了搜索之旅，还真找到了一个我没听过的安卓软件：Nova Video Player

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/bcd05375-e1de-49cd-8131-42bae36e3bdd.png?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_4/)

链接地址放在上面了，如果看不到地址就去搜索这个名字吧，在 apk 下载的地方我看到了好几个 apk，我也不确定安卓电视用哪个，于是全部下载了。因为 OPPO K9 本地软件不支持 SMB，还没有当贝市场，我就顺便把所有的 apk 都放在一个 U 盘内，插在电视后面的 USB 接口上了。经过测试，只有图中的 universal 版本可以安装，其他的全部失败，大家可以只下这个版本。可以看到这个作者更新的很勤快，昨天还更新了一个版本，是真的在用心做软件。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/ed642225-59c6-4dbb-baec-c44a5fd4567b.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_5/)

打开电视，在精选下滑动，或者往最右找到我的应用，打开文件管理。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/bcc12212-e2cd-4b1e-8dcd-b8aeff87d172.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_6/)

文件管理内可以看到 U 盘，进入。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/c2340038-ab34-419d-9242-034ccb642c09.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_7/)

如图安装带 universal 字样的版本，最好顺便装个沙发管家或者当贝市场用于下其他的软件，自带的应用中心特别简陋。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/afd5d249-2e5b-4f71-8014-9b5c753ad1fc.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_8/)

安装完毕后就可以看到 Nova Video Player 了，可以看到我也安了 nPlayer 解决局域网访问 NAS，不过在我用过 Nova 之后，nPlayer 简直被吊打放弃了。

## 软件配置

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/85f5cf70-e999-48a5-83cd-492f0824b161.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_9/)

打开 Nova Video Player，这个首页画面就非常舒适了，至少是比 KODI 要好看很多，接近现在流行的风格设计，主要菜单都在最左侧，从上到下是电影、电视节目、动画、网络和文件、首选项。右侧就是一个缩略图版的海报墙。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/d5750aa3-5da8-4979-9e32-b3b15ce6e94c.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_10/)

首先看一下首选项，首选项里面的功能特别多，可以说把能给用户选择的功能都开启了，比如可以强制软件解码来解决一些格式无法硬解播放的问题，甚至这个软件都支持第三方视频播放器调用（心真大），还有许多其他选项，一般默认不变比较好。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/ddc57637-c523-411c-97b3-48b9f91ab653.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_11/)

首选项下面有一个内容很重要，就是字幕的下载地址，这里支持 OpenSubtitles 网站自动搜索，而且不需要 VIP，只要自己注册一个免费账号就行。我在 emby 的配置里也能填写这个 OpenSubtitles 字幕插件，但是毫无作用，很少成功下到字幕，这个 Nova Media Player 是实测真的有用。

[**注册地址**](https://www.opensubtitles.org/zh)

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/762fa5e4-597b-440d-b617-e0e55ee96327.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_12/)

再往下看电影 / 电视节目提供方是 TMDB，这里不可选，不过 TMDB 的刮削已经很主流了，基本都可以搜索到。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/89b116dd-1a51-4951-aca1-4b9a06f2f162.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_13/)

隐私模式就很有意思了，我想应该有很多用户在家里下电影估计都会用到，因为是家庭使用，肯定不想自己刚下的某岛国电影突然自动刷新在首页，又或者留下自己的浏览记录。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/50003b53-7397-4bad-843a-417e2fcc2235.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_14/)

然后传统页面选择的话就是这个样子，非常像老式的文件管理器，虽然结构很清晰，但是不如第一个图的新版页面好看。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/05218be9-a36a-4299-a844-93672b084782.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_15/)

现在进入重头戏，选择视频来源，首先电视如果本地有视频，或者能接[移动硬盘](https://www.smzdm.com/fenlei/yidongyingpan/)的话就很简单，直接选择内部存储或者外部驱动器就可以。它还支持网络来源，也就是 SMB/UPnP 等 协议，让我可以取代 nPlayer。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/2da130ef-8251-48d8-a93b-d1028e398b65.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_16/)

打开网络后，首先会自动扫描到本地支持 SMB 的网络服务器。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/181af6e1-ab04-4016-a4a4-2d87bd518f08.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_17/)

如果没找到的话，不用着急，点击浏览网络共享，手动输入 NAS 的网络地址，并且支持选择协议，然后输入账号和密码就可以登录了。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/290b0f09-ff39-4145-a497-9c929305b6e0.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_18/)

这里我先添加了一个 2D 电影的文件夹，已经开始自动刮削媒体资源了，如果因断电或退出不小心中断的话，可以手动点击重新扫描继续刮削。

## 电影刮削

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/5e70fc06-0360-426e-a445-53b9b556413d.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_19/)

进入「电影」版块，点进所有电影，很快就刷出了部分电影海报了，非常准确，全部都识别出来了，仔细看右上角有个数字就是还没扫描完的媒体文件。这里有个细节，海报墙是如果按照名称来排序的话，确实是按拼音的顺序来排序的，比较符合国内用户，有些海报墙的按名称顺序并不是按照拼音。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/81eaec1f-caf0-407c-90a5-51b5e5305523.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_20/)

在首页可以直接通过分类来查看海报墙。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/0ba3ec40-c914-4dfb-8362-176fc2614fb6.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_21/)

也可以直接通过电影上映的年份来查找海报墙。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/9496e9da-ccb3-4320-8ba9-c23599270d3b.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_22/)

点击一个电影海报进入，里面的 UI 界面也很好看，底色是一个巨大的海报画面，中间以悬浮卡片式样来隔开每一个信息内容，最上方有电影介绍，年代、时长、评分、分级制度、分辨率、音效这几个最需要关注的信息都列出来了，有剧情简介，然后还有一些选项可以操作，比如从库里删除，还是直接删除掉资源。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/21859c5c-6d00-4053-a7e0-4f478fc485f4.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_23/)

字幕这里也很重要，电影里包含的字幕信息都有了，并且这里就可以提前在线获取字幕了，也支持选择已下好的字幕。如果忘记下载了，在播放时也是支持在线下字幕的。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/5e94f7bb-a87a-4b4c-a0bc-7e5c77405afb.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_24/)

直接测试下字幕下载功能，我发现每次都会提示开头第一句的「找不到针对 XXX 的字幕文件」，可能是因为标题不是 100% 一样的，但是这个 Nova Media Player 很智能，依然列出了所有相关的字幕供选择，实际就是非常准确。并且后来还试了一个非常冷门的几百集的动画，居然也能搜出一个字幕对应上。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/9ccd471a-8c0b-4655-8d0b-f847558661b6.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_25/)

不过最下方的导演和演员，即使列的清清楚楚，但是没有头像展示，这点比起 emby 之类的还是差一点，并且没办法点进演员来查看他所参与的电影列表，希望作者后期能够加入。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/6921ad6a-bbc3-419f-b12d-72b5540847a6.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_26/)

点进电影进行查看，字幕已经下载好了，然后按左右来控制进度条，也是线性加速的，按的越久，跳的越快，很好掌控，除了进度条外，对于几百集的动画来查找后几集来说，也可以在几秒内快速定位。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/a2febd36-8da9-463e-8adf-c8c14d2bd45f.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_27/)

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/7e4efed5-a0bc-464c-a0cb-347b81402ed2.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_28/)

如果播放时忘记下载字幕，长按上键就可以进入菜单选项，可以在线搜索字幕，调整延时，选择音频轨道语言等等。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/aa65b94b-9efe-4fbb-93b6-42ce1c4bf6e8.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_29/)

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/2fc76036-dbb5-4fc7-a23b-095588e233a5.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_30/)

只需要稍等一会，中文字幕就下载好了，替换了英文字幕。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/6bbe3ebf-4b3c-44f0-bab8-a36827948b15.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_31/)

音频轨除了可以选择内置已有的中英文之类，还有一个音频增强模式，我试了下音频增强和夜间模式都会在已有的音量下，音量突然变大一些，不知道有啥区别，可能有些电视声音上限比较小的话，会默认给媒体资源的音频增加增益吧。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/b4f7eb3a-03aa-4ce8-a009-5c1ede763e91.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_32/)

信息就和视频外浏览的信息页面一样了，只是电影还在播放，想看一下这个视频的大小，分辨率和视频格式等信息，是否是 H265 需要开启电视的 HDR 模式 之类的。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/c666fe32-138a-47b9-a692-7acd0fce5da5.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_33/)

然后我还发现一个非常有意思的小细节，我选了六个电影点进去看详情页面，发现没有？里面的卡片信息颜色不是一成不变的，并且是自动感知到海报的颜色细节，卡片的颜色也随之改变，这样整个画面就非常和谐，观感很好，如果有的家庭电视背景也有 RGB 神光同步的效果的话，想象一下晚上的观影体验会非常不错。

## 电视节目刮削

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/ca7e3203-6568-4135-8857-fcf4f663e621.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_34/)

然后我也加入了一些 TV 剧集的文件夹进行扫描，那么按道理，这些剧集就会归类到「电视节目」下面。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/7d9c718b-bed6-4609-82eb-0ee75ad39684.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_35/)

电视节目刮削的也很准确，并且名字下面会列出该剧总共的集数。不过很奇怪我的 TV 文件夹下，动漫也归类于此，却看不到一个动漫的海报。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/f6e7385c-7b06-4ffd-894e-0b6629cb8d59.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_36/)

但是如果电视节目按照分来来查找的话，第一个分类就是动画，又可以刮削我的动画剧集，我还在纳闷是不是一个小 BUG，往下看解答了我的疑惑。

## 动画电影刮削

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/8245b38a-5e64-467b-aaf8-37be16a63594.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_37/)

这个作者似乎是有自己的分类逻辑，专门分出了一个「动画」版块，其中子列有「动画电影」，我的 2D Movies 文件夹里其实是把院线上映的电影都放在一起，所以刮削后我在电影模块下找不到我的动画电影，仔细一看原来全部归类在这个动画电影模块下了。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/97aab87a-4d63-4441-8bb8-9f1779231608.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_38/)

那么同样的，在这个「动画」总分类下，电视节目这一个子列，可以看到所有的动画剧集，也就是说哪怕「电影」和「电视节目」的按类别分类可以找到动漫电影和动漫分类，这个作者也依然将「动画」作为平级的第三个选择，然后将所有动漫电影和动漫剧集都汇集在这里，看来作者也是老[二次元](https://pinpai.smzdm.com/171019/)了。

PS.「爱、死亡、机器人」我以为会归类在美剧属于电视节目，结果也归在「动画」下了，看来只要是带一点卡通元素都要放在这里。

[![](https://github.com/gkeo/img/blob/main/2022/07/2022-7-4%2009-14-17/06ec4f70-f4e9-4133-bf54-6d0a44a35a71.jpeg?raw=true)
](https://post.smzdm.com/p/a90o9pw7/pic_39/)

点进其中一个剧集，可以看到季 1 到季 4 全部都列好了，每一集的封面，名称都很清晰。其实光「动画」这一个版块的分类，我都觉得要比芝杜的海报墙 4.0 还要厉害一点了。

## 总结

不管是不是 NAS 用户，我相信只要爱看电影、剧集的人，在家里都喜欢准备硬盘，下载资源，哪怕不看也要收集起来，能拥有自己的一个海报墙，哪怕没时间看，偶尔刷刷屏幕都很有快感。

都说[电视盒子](https://www.smzdm.com/fenlei/gaoqingbofangqi/)的终极形态是 Apple TV，是因为 infuse 的海报墙确实做的不错，并且 Mac、iPad、iPhone、TV 所有终端都通用，但是价格很贵不便宜。

安卓用户还记得当年都研究 KODI 换皮肤，换字体，换选区解决乱码，又比较反人类的界面和繁琐的设置吗？ 我真的很久都没用 KODI 了。

现在看到这个 Nova Media Player 感觉像安卓盒子安卓电视的光环一样出现，并且作者还是开源免费的，我没有深入了解，但是从每个细节感觉都是站在用户的使用角度去优化更新。安卓用户不再是只能用 nplayer、VLC 来将就。如果不想折腾 NAS 装 EmbyJellyfinPlex 的用户，也有了入门级却高体验的 APP。甚至我家里客厅专门留着的芝杜 X9S 都不再是唯一亮眼的存在，海报墙 4.0 迎来了真正对手，还略强的那种。这两天体验下来觉得这软件真的是一个精品，很久没有这么兴奋了，迫不及待的分享给大家，希望人人都能免费玩起[家庭影院](https://www.smzdm.com/fenlei/jiatingyingyuan/)的海报墙！

作者声明本文无利益相关，欢迎值友理性交流，和谐讨论～

![](https://res.smzdm.com/pc/pc_shequ/dist/img/the-end.png) 
 [https://post.smzdm.com/p/a90o9pw7/?zdm_ss=Android_9812387715_&send_by=9812387715&from=singlemessage&invite_code=zdmddxwnvfinv](https://post.smzdm.com/p/a90o9pw7/?zdm_ss=Android_9812387715_&send_by=9812387715&from=singlemessage&invite_code=zdmddxwnvfinv)
