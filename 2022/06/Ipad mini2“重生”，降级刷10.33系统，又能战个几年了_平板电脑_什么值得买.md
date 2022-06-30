# Ipad mini2“重生”，降级刷10.33系统，又能战个几年了_平板电脑_什么值得买
# Ipad mini2“重生”，降级刷 10.33 系统，又能战个几年了

2021-11-12 13:19:37 71 点赞 677 收藏 132 评论

## 写在前面

上次给小米平板 1 代刷了第三方固件使用了一段时间，发现这设备是真的太老了，使用起来还说能明显感觉到有卡顿的。这次来整一下我的另一个平板——苹果 iPad mini2。  

[小米平板一代 “重生”，刷第三方神盾固件](https://post.smzdm.com/p/a5dr5ewx/)

写在前面上回折腾了下小米盒子 3 增强版这个老物，让它又能战个几年了，这次就来整下另一个老物——小米平板 1 代。小米平板 1 是小米在 2014 年推出的一款安卓平板电脑，距离至今已有 7 个年头。当初因为不错的性能，多彩外壳的设计，最主要还是非常实惠的价格，入手了一台给我妈看视频用，可如今这台平板已经非常卡顿了，看[嗨小马](https://zhiyou.smzdm.com/member/7405177756/)\| _赞\_49 _评论_80 _收藏\_275[查看详情](https://post.smzdm.com/p/a5dr5ewx/)

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-30%2016-59-02/0a163fb2-521b-40e4-b41f-7566bb02ebbe.jpeg?raw=true)
](https://post.smzdm.com/p/ad2005vp/pic_2/)

iPad mini2 是 13 年的产品了，我是 14 年购买的，当初因为 mini3 还说沿用 A7 的处理，只是多了 Touch ID，所以还说购买了更实惠的 mini2 。  

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-30%2016-59-02/38e29972-87d7-4df6-9b5c-978c27c697bf.jpeg?raw=true)
](https://post.smzdm.com/p/ad2005vp/pic_3/)

mini2 目前的系统是 12.5，也不要求什么性能，平时就给我儿子看看动画片什么的，不过现在即使是打开 \[爱奇艺

关注

[](javascript:;)

品牌 

粉丝：

-   商品百科
-   好价
-   社区文章

]([https://pinpai.smzdm.com/37404/](https://pinpai.smzdm.com/37404/)) 也是能感觉到明[显卡](https://www.smzdm.com/fenlei/xianka/)顿的，不过播放起来后就流畅了。

我就琢磨着能不能把系统版本刷低一点，来保证流畅性，网上搜罗一圈后发现 A7 芯片是有个漏洞的，可以用来强制降级，网上也有各路大神制作的工具。

## 降级准备篇

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-30%2016-59-02/61465024-358f-4fd8-bad1-5698d0d96a15.png?raw=true)
](https://post.smzdm.com/p/ad2005vp/pic_4/)

目前一般有三种工具可以用来降级，分别是 Vieux、1033-OTA-Downgrader、LeetDown。不过这些工具是只能在 Mac 电脑上使用，这边我还踩了几个坑，M1 芯片的 Mac 是不行的（我在这上面折腾了一天的时间，还以为是我哪里没操作好）。这边我经过一些尝试后，推荐使用 2.01 的版本，不要使用最新的 2.1 版本（说起来也是一阵的苦，浪费了好多时间）。以上三个工具都可以在 GitHub 里面找到。

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-30%2016-59-02/ca36b45a-e92c-4439-8910-24a4f4c8cef6.png?raw=true)
](https://post.smzdm.com/p/ad2005vp/pic_5/)

需爱思助手里把 10.33 版本固件下载后，后面刷机的时候要用到，iPad mini2 的具体型号为 A1489，在设备的背面能看到。  

## 降级 10.33

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-30%2016-59-02/195eebb6-eaec-475f-b8cc-0020d38b221e.png?raw=true)
](https://post.smzdm.com/p/ad2005vp/pic_6/)

首先需要把 ipad 进入 DFU 模式，这里说下 DFU 模式是纯黑屏什么都不显示的并非是提示 iTunes 连接的，那个是恢复模式，两个是不一样的。DFU 模式长按电源键和 home 键，等设备重启屏幕亮了后约 3 秒左右，放开电源键继续长按 home 键 10 秒左右就可以了，这个模式下屏幕是纯黑的，可以连接爱思查看。

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-30%2016-59-02/4cb85523-e5d9-4675-af36-e449678a24e2.png?raw=true)
](https://post.smzdm.com/p/ad2005vp/pic_7/)

打开 Leetdown 这个软件，如果没有进入 DFU 模式这边也是有提示的。点击软件中的第一个按钮 Select 10.3.3 iPSW，然后选择刚才下载的 10.33 固件，接着点击 Downgrade 就开始刷机。

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-30%2016-59-02/beae4dce-62f4-4b46-bf02-6e52244b5244.png?raw=true)
](https://post.smzdm.com/p/ad2005vp/pic_8/)

一般这个降级刷机不是百分之百成功的，中途可能会提示失败，需要重启设备，就需要重启[手机](https://www.smzdm.com/fenlei/zhinengshouji/)进入 DFU 模式开始重新来一遍，一般多弄个几次肯定会有成功的，据说使用原装线成功几率会高很多。

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-30%2016-59-02/5f0a71cf-7228-4a6d-b13b-d0c569129c26.jpeg?raw=true)
](https://post.smzdm.com/p/ad2005vp/pic_9/)

多尝试几次，如果中途设备的屏幕如果绿了一下就说明这次成功了，然后就开始漫长地刷机了，这个过程大概有个 7，8 分钟时间。  

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-30%2016-59-02/2d03ffcb-80fb-41f4-bac9-163690238c4d.png?raw=true)
](https://post.smzdm.com/p/ad2005vp/pic_10/)

刷到 10.33 系统后记得把系统更新屏蔽掉，要不然每天自动更新了就凉凉了。

## 尾巴——小结

之前我觉得 ipad 卡顿，开始以为是[电池](https://www.smzdm.com/fenlei/dianchi/)不行了，降频导致的，然后自己动手换了电池，还把设备的外屏边角给撬坏了。实际原因就是新版本系统资源吃的太多了，新版本的 App 资源也吃的多就导致卡顿了。好在回到 10.33 后基本又能流畅地运行了，感觉再战个几年也不成问题。

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-30%2016-59-02/3ddb41b1-44ad-4eff-9737-05543ba35baa.jpeg?raw=true)
](https://post.smzdm.com/p/ad2005vp/pic_11/)

这边总结几个点，必须使用 Mac 的电脑，我使用的系统是 10.15，网上搜罗出来是 10.13-10.15 都是可以的，据说黑苹果也是可以的（个人未尝试）。M1 芯片的 Mac 是不行的，即使 Leetdown 官网上面写了已经支持 M1，个人实测不行，必须是 intel 的。Vieux、1033-OTA-Downgrader 两个方法都是需要搭建 python 环境的对新手来说，太麻烦了。还是 Leetdown 这个傻瓜式的软件来的方便好用，唯一的缺点就是成功率不高，得多尝试几次。

![](https://res.smzdm.com/pc/pc_shequ/dist/img/the-end.png) 
 [https://post.smzdm.com/p/ad2005vp/](https://post.smzdm.com/p/ad2005vp/)
