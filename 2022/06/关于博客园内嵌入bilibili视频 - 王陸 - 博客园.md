# 关于博客园内嵌入bilibili视频 - 王陸 - 博客园
## 一、原理

使用 iframe 标签，更改其中 src 对应 bilibili 视频的 aid 和 cid，组装新的 HTML 源码，即可在文章内嵌入 bilibili 视频。

## 二、获取 aid 和 cid

aid 为视频的 av 号，但是每个 av 号下不一定只有 1p，所以 B 站用 cid 来管理视频的真正 id，那么也可以说如果视频只有 1p，那么 cid 就无用了，我测试直接填 1 也是可以的。

这里介绍两种获取 aid 和 cid 的方法：

### 方法一

先观察视频的 URL

 其中 84267566 就是 av 号。

或者直接点右键——查看网页源代码——ctrl+f——搜索'aid='、‘cid=’ 就可以了。

### 方法二 (推荐)

我们在转发视频的时候直接可以看到嵌入代码

![](https://img2018.cnblogs.com/i-beta/1358881/202002/1358881-20200206150838006-392318847.png)

这是官方准备的嵌入代码，可以直接拿来用，但是显示效果不是很理想，样式不是我们希望的，需要调整一下。

复制代码

<iframe src\="//player.bilibili.com/player.html?aid=84267566&cid=145147963&page=1" scrolling\="no" border\="0" frameborder\="no" framespacing\="0" allowfullscreen\="true"\> </iframe\>

从嵌入代码中我们直接得到了 aid 和 cid

我们重新设置一下功能、大小、样式，得到可用的 HTML 代码

复制代码

<iframe src\="//player.bilibili.com/player.html?aid=84267566&amp;cid=145147963&amp;page=1" frameborder\="no" scrolling\="no" width\="95%" height\="600"\></iframe\></p\>

以后插入需要的 bilibili 视频只需要改变上面的 aid 和 cid 就可以了！

## 三、移动端适配

大佬提出移动端出现不适配的问题，我当时其实并没有考虑移动端的问题，固定了播放器的高度造成了这个问题。

![](https://img2018.cnblogs.com/i-beta/1358881/202002/1358881-20200217230556219-1666126249.png)

 大佬已经给出了解决方案

复制代码

<div style\="position: relative; padding: 30% 45%;"\>
<iframe style\="position: absolute; width: 100%; height: 100%; left: 0; top: 0;" src\="https://player.bilibili.com/player.html?cid=145147963&aid=84267566&page=1&as\_wide=1&high\_quality=1&danmaku=0" frameborder\="no" scrolling\="no"\></iframe\>
</div\>

可以用这个代码作为样板，以后只需要改变 src 的 id 好就可以了！

再次感谢大佬**[ExperDot](https://www.cnblogs.com/experdot/)**的帮助！

## 四、嵌入方法

选择 tinyMCE 编辑器，这是博客园默认的编辑器，选择编辑 html 原代码，插入上面的代码即可。

![](https://img2018.cnblogs.com/i-beta/1358881/202002/1358881-20200206151748141-1744878901.png)

 ![](https://img2018.cnblogs.com/i-beta/1358881/202002/1358881-20200206153114389-1534665027.png)

## 五、参数说明

本来这篇博客是我的游戏之作，但没想到捧场的朋友这么多，我看到评论区有朋友讲清晰度的问题，我这里再说一下几个参数。

复制代码

[https://player.bilibili.com/player.html?cid=145147963&aid=84267566&page=1&as\\\_wide=1&\*\*high\\\_quality=1\*\*&danmaku=0](https://player.bilibili.com/player.html?cid=145147963&aid=84267566&page=1&as\_wide=1&**high\_quality=1**&danmaku=0)

| key                   | 说明                                   |
| --------------------- | ------------------------------------ |
| aid                   | 视频 ID                                |
| 就是 B 站的 avxxxx 后面的数字  |                                      |
| cid                   | 应该是客户端 id, clientId 的缩写 (推测的, 不一定准确) |
| 经过测试, 这个字段不填也没关系      |                                      |
| page                  | 第几个视频, 起始下标为 1 (默认值也是为 1)            |
| 就是 B 站视频, 选集里的, 第几个视频 |                                      |
| as_wide               | 是否宽屏                                 |
| 1: 宽屏, 0: 小屏          |                                      |
| high_quality          | 是否高清                                 |

1: 高清, 0: 最低视频质量 (默认)  
如视频有 360p 720p 1080p 三种, 默认或者 high_quality=0 是最低 360p  
high_quality=1 是最高 1080p |
| danmaku | 是否开启弹幕  
1: 开启 (默认), 0: 关闭 |

所以只要设置**high_quality=1**就能开启最高画质了。

B 站官方并没有给出文档说明..... 但我发现论坛上有一些相关的讨论

[**相关链接**](http://link.acg.tv/forum.php?mod=viewthread&tid=22308&highlight=%E6%92%AD%E6%94%BE%E5%99%A8%E5%8F%82%E6%95%B0%E7%9A%84%E5%AE%98%E6%96%B9%E6%96%87%E6%A1%A3%E5%9C%A8%E5%93%AA%E9%87%8C%E5%91%A2)

经测试 high_quality 参数可以正常使用，此参数控制外链播放器的默认清晰度：  
\\=1 时默认清晰度是最高非大会员清晰度，例如：  
（1）原视频清晰度有 360P、480P、720P，外链播放器默认为最高的 720P，  
（2）原视频清晰度有 360P、480P、720P、1080P，外链播放器默认为最高的 1080P，  
（3）原视频清晰度有 360P、480P、720P、1080P、1080P+，外链播放器默认为 1080P，  
选择其他清晰度会打开原视频页面，

\\= 其他数值或没有此参数时默认清晰度是 360P，选择其他清晰度会打开原视频页面。

## 六、示例

这里给出 2020 拜年祭的《万古生香》

万古千秋，代代有玲珑气象！  
风云史往，页页赋秀骨生香！

复制代码

<div style\="position: relative; padding: 30% 45%;"\>
<iframe style\="position: absolute; width: 100%; height: 100%; left: 0; top: 0;" src\="https://player.bilibili.com/player.html?cid=145147963&aid=84267566&page=1&as\_wide=1&high\_quality=1&danmaku=0" frameborder\="no" scrolling\="no"\></iframe\>
</div\>

本文作者：王陸

本文链接：[https://www.cnblogs.com/wkfvawl/p/12268980.html](https://www.cnblogs.com/wkfvawl/p/12268980.html)

版权声明：本作品采用知识共享署名 - 非商业性使用 - 禁止演绎 2.5 中国大陆许可协议进行许可。 
 [https://www.cnblogs.com/wkfvawl/p/12268980.html](https://www.cnblogs.com/wkfvawl/p/12268980.html)
