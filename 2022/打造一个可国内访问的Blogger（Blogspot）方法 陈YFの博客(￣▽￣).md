# 打造一个可国内访问的Blogger（Blogspot）方法 || 陈YFの博客(￣▽￣)"
Blog‍‌‍‌ger, 一个干爽、免费的博客发布平台，作为主流的发布，提供免费的托管，免去了 Typecho&Wordpress 高昂的服务器费用，避免了 Hexo&Jekyll 静态博客无后台的缺陷，与 CSDN、简书相比，可以绑定域名，界面干净，无广告【当然可以自己放自己的广告】。

实际上，当今写博客的软件数不胜数，主要分为一下三类：

-   服务器部署：典型代表：Wordpress\\Typecho
-   无服务器托管：典型代表：Hexo\\Jekyll\\Gidea\\Hugo\\Hola 等等
-   集成型网站：Blogger、简书、CSDN、cnblog、wodemo 等等

上面所有软件，优缺点都有，具体看个人选择

当然，个人认为 Typecho 适合做个人博客，Hexo 可以作为要求不高的人。集成性网站最主要的是只要安安心心写文章，不用管后端乱七八糟的代码。当然，最有问题的是大多数都不支持绑定域名，而且经常往网站上塞广告，自定义范围也不够。

接下来，我们扯扯集成博客中的一股清流：Blogger

> Blogger.com 是由 Pyra Labs 公司创立，是目前全球用户数量最多的个人网志服务提供商。Pyra Labs 和 Blogger.com 均被 Google 公司收购，成为其旗下的一项服务内容。  
> Blogger 提供免费主机 Blogspot.com 存放博客，用户不必写任何代码或者安装服务器软件或脚本，通过所见即所得界面轻松地创建、发布、维护和修改自己的网志。  
> Blogger 允许有经验的用户自行设计博客界面，其模板支持使用 HTML 和 CSS 进行编辑

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-24-57/4c81ebef-42d5-4458-8be5-6568077dc466.jpeg?raw=true)
](https://npm.elemecdn.com/chenyfan-oss@1.0.1/2020-07-29%20084428.jpg)

实际上，由于 Blogger 托管于谷歌，写作域名 `www.blogger.com` 和托管域名 `*.blogspot.com` 均被 MainLand 所 Ban。但是接下来来，我会讲讲如何打造一个能在国内大陆访问的 Blogger

> 众所周知，请善用技术上网。

用谷歌账号登录 ([https://www.blogger.com](https://www.blogger.com/)) ，没有？不是⑧不是吧不是扒，都 0202 年了，还不会去注册一个谷歌账号？

进入控制台，新建一个博客：

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-24-57/147ba0af-7d77-40b7-be9c-7bb6338e0ffa.jpeg?raw=true)
](https://npm.elemecdn.com/chenyfan-oss@1.0.1/2020-07-29%20084652.jpg)

绑定域名, 这一步随意, 因为我们还要绑定一个自定义域名.

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-24-57/491ae0ff-9f7e-4804-980a-20fb3ff89912.jpeg?raw=true)
](https://npm.elemecdn.com/chenyfan-oss@1.0.1/2020-07-29%20084847.jpg)

点击确定, 一个博客就搭建好了! 快吧?

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-24-57/7c9ccc26-cf13-4439-8d37-7384f3748abe.jpeg?raw=true)
](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-24-57/aeaf71d1-e605-46b7-a72f-6c31afbd0ba7.jpeg?raw=true)

这时候, 我们**善用技术上网**访问之前的域名, 我这里是: `cyfblogger.blogspot.com`，打开：

[![](https://npm.elemecdn.com/chenyfan-oss@1.0.1/2020-07-29%20085021.jpg)
](https://npm.elemecdn.com/chenyfan-oss@1.0.1/2020-07-29%20085021.jpg)

搞定！搞完之后才知道，Blogger 真正做到了什么叫 1 分钟创建一个博客，但这还不够，中国访客实际上还是打不开这个网站[1](https://blog.cyfan.top/p/%E5%A4%A7%E5%A4%9A%E8%A2%AB%E6%B1%A1%E6%9F%93%E5%88%B0%E8%8E%AB%E5%90%8D%E5%85%B6%E5%A6%99%E7%9A%84ip%E5%8E%BB%E4%BA%86,%E4%BE%8B%E5%A6%82Facebook%E5%92%8CTwitter)。所以，接下来，我们要让这个网站能在中国访问。

目前我们有一下思路:

-   选择一个未被官方屏蔽的 Blogger 节点, 此方法几年前还是可用的, 最近不大灵光了, 被屏蔽的差不多了
-   使用服务器反向代理 Blogger, 不推荐, 有这个闲工夫你还不如直接用 Typecho 呢
-   CloudFlareCDN 反向代理, 这似乎是唯一一个解决的办法.

由于 `*.blogspot.com` 泛域名被屏蔽, 所以你必须要有一个自己的域名.

不推荐免费域名，因为免费域名无法自选 CF 节点，当然你也可以换别的方式。

这里推荐一个买域名的好网站：`https://namebeta.com/` namebeta 并不是购买域名的地方, 但是它可以帮你货比三家, 让你跳出更优惠的方案, 并且自带各种温馨提示, 包括能不能备案, 有没有被注册等等, 比较适合刚入门想买域名的小白.

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-24-57/ebb437a6-856c-4d25-bcd8-c742781083c7.jpeg?raw=true)
](https://npm.elemecdn.com/chenyfan-oss@1.0.1/2020-07-29%20090113.jpg)

购买绑定 NS 不在谈论范围内, 有域名的看下去

以下演示方便在 CloudFlare 官网里进行, 采用免费域名演示, 实际用笨牛 CNAME 解析为好.

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-24-57/3b83f2c7-3f1e-4f7d-9199-84110750a023.jpeg?raw=true)
](https://npm.elemecdn.com/chenyfan-oss@1.0.1/2020-07-29%20091514.jpg)

点击【设置】-【正在发布】-【自定义域名】

接着会报错：

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-24-57/d5817869-9ff6-40eb-81a9-355cbcc1c22e.jpeg?raw=true)
](https://npm.elemecdn.com/chenyfan-oss@1.0.1/2020-07-29%20092548.jpg)

提示没验证，没关系，去 CloudFlare 加上 CNAME，注意验证过程中切记把橙色云朵点灰[2](https://blog.cyfan.top/p/%E5%AE%9E%E9%99%85%E4%B8%8A%E4%B8%8B%E9%9D%A2%E9%82%A3%E6%9D%A1%E8%AE%B0%E5%BD%95%E6%98%AF%E4%B8%BA%E4%BA%86%E9%AA%8C%E8%AF%81%E4%BD%A0%E6%9C%89%E6%B2%A1%E6%9C%89%E4%B8%BB%E5%9F%9F%E5%90%8D%E6%8E%A7%E5%88%B6%E6%9D%83%E3%80%82)：

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-24-57/69063947-8939-46ee-a101-84937cb19901.jpeg?raw=true)
](https://npm.elemecdn.com/chenyfan-oss@1.0.1/2020-07-29%20092736.jpg)

回去，点击确定，勾选 HTTPS 可用性。

谷歌 SSL 验证和办法真的慢，没办法，喝杯茶，写点作业，过了 10min，显示颁发成功：

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-24-57/53adfcdf-1455-4203-bcb9-13455770ef53.jpeg?raw=true)
](https://npm.elemecdn.com/chenyfan-oss@1.0.1/2020-07-29%20093118.jpg)

回到 CloudFlare，把访客访问的那条记录点亮橙色云朵，作为验证的记录不点亮.

国内访问试试？

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-24-57/9260176d-27f6-4b33-8647-43cc3661b218.jpeg?raw=true)
](https://npm.elemecdn.com/chenyfan-oss@1.0.1/2020-07-29%20094026.jpg)

这时候我们发现，虽然国内能直连了，但是背景图加载不出来，整个网页加载缓慢。控制台以下，我们就会发现，Blogspot 请求了国外谷歌边缘节点的内容，包括背景图片和部分 js。

所以，我们还要对博客主题进行加工。

> 演示方便，用 Contempo Light 主题  
> 实际上用其它主题也差不多的，注意替换掉外链即可

[![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-24-57/f8c12eb3-6e13-4137-b26d-0e58f253403f.jpeg?raw=true)
](https://npm.elemecdn.com/chenyfan-oss@1.0.1/2020-07-29%20094333.jpg)

点击备份，下载主题:

用 Notepad++ 打开，方便处理

## [](#屏蔽加载 "屏蔽加载")屏蔽加载

将`<head>`替换为`&lt;!--<head>--&gt;&lt;head&gt;`

将`</head>`替换为`&lt;/head&gt;&lt;!--</head>--&gt;`

将`</body>`替换为`&lt;!--</body>--&gt;&lt;/body&gt;`

## [](#替换背景 "替换背景")替换背景

在 49 行左右，有这么一段代码：

```null
<Variable name="body.background" description="Background"
      color="$(body.background.color)"
      type="background"
      default="$(color) none repeat scroll top left"  value="$(color) url(https://themes.googleusercontent.com/image?id=L1lcAxxz0CLgsDzixEprHJ2F38TyEjCyE3RSAjynQDks0lT1BDc1OxXKaTEdLc89HPvdB11X9FDw) no-repeat scroll top center /* Credit: Michael Elkan (http://www.offset.com/photos/394244) */;"/>
```

将其中的

替换为背景外链.

## [](#必要的JS替换 "必要的 JS 替换")必要的 JS 替换

打开 F12, 我们会发现, 有一个资源阻塞了请求:

`https://resources.blogblog.com/blogblog/data/res/2629068285-indie_compiled.js`

但这个是必须的, 我们要保留, 搜索 `<b:template-script async='true' name='indie' version='1.0.0'/>` 并删除, 将其替换成:  
`<script async='async' src='https://cdn.jsdelivr.net/gh/chenyfan/chenyfan.github.io/1468123664-indie_compiled.js'></script>`

> 当然你也可以用自己的链接.

## [](#解决缩略图问题 "解决缩略图问题")解决缩略图问题

【感谢阿虚同学的博客解决此方法】

将 此 JS 代码放置于标签前：

```null
<b:if cond='data:blog.pageType in {"index","searchQuery","searchLabel","archive"}'> 
    <script defer='defer'>
        
        var postThumbnails = document.getElementsByClassName("post-thumbnail");
        var postContents = document.getElementsByClassName("post-text");
        for (var i=0;i<postContents.length;i++)
        {
            var postContent = postContents[i].innerText;
            var imgReg = /<img.*?(?:>|\/>)/gi;
            var srcReg = /src=[\'\"]?([^\'\"]*)[\'\"]?/i;
            var imgTags = postContent.match(imgReg);
            imgSrcs = imgTags[0].match(srcReg);
            imgSrc = imgSrcs[1];
            postThumbnails[i].setAttribute('src', imgSrc);
        }
        
    </script>
</b:if>
```

修改模板，搜索 data:post.featuredImage，在缩略图处改成下代码：

```null
<b:if cond='data:post.featuredImage'>  
    <div class='snippet-thumbnail'>  
        <img class='post-thumbnail' sizes='(max-width: 800px) 20vw, 128px' src='https://ae01.alicdn.com/kf/HTB1Gb7LUmzqK1RjSZFL5jcn2XXac.gif'/>  
        <textarea class='post-text' style='display:none;'><data:post.body.escaped/></textarea> 
    </div>
</b:if>
```

## [](#头像、icon设置 "头像、icon 设置")头像、icon 设置

搜索 `profile-img` 约 3899 行：

```null
<img class='profile-img' expr:alt='data:messages.myPhoto' expr:height='data:authorPhoto.height' expr:src='data:authorPhoto.image' expr:width='data:authorPhoto.width'/>
```

将其直接改成

```null
<img class='profile-img' src="你的头像外链地址">
```

## [](#字体问题 "字体问题")字体问题

我测试时没有发现字体相关问题，请求字体的网址 gstatic.com 在国内可以访问，虽然部分地区莫名其妙解析到澳大利亚 facebook，但大多数都正确解析时国内谷翔。

## [](#评论问题 "评论问题")评论问题

默认的 Google 评论肯定时不行的，留着还拖慢加载，推荐用 DiqusJS 或 Valine.

修改主题，搜索 `<b:includable id='comments' var='post'>` 约 3453 行至 `</b:includable>` 3511 行, 全部删除, 将以下代码填写回原处

```null
<b:includable id='comments' var='post'>
                      <div id='disqus_thread'/>
                      
</b:includable>
```

在末端 `&lt;!--</body>--&gt;&lt;/body&gt;` 前添加:

```null
<script>
var dsqjs = new DisqusJS({
    shortname: '',
    siteName: '',
    identifier: '',
    url: '',
    title: '',
    api: '',
    apikey: '',
    admin: '',
    adminLabel: ''
});
</script>
```

以上代码具体配置前往 \[[https://github.com/SukkaW/DisqusJS#%E4%BD%BF%E7%94%A8](https://github.com/SukkaW/DisqusJS#%E4%BD%BF%E7%94%A8) ] 配置

完

Demo&& 以后的日记本:\[[https://moe.cyfan.top](https://moe.cyfan.top/) ]

* * *

推荐自选 CDN 加速.

注意以后写文章图片必须是外链, 可以试试 sm.ms 
 [https://blog.cyfan.top/p/620f3e8d.html](https://blog.cyfan.top/p/620f3e8d.html)
