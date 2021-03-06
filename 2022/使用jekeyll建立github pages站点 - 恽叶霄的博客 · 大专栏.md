# 使用jekeyll建立github pages站点 - 恽叶霄的博客 · 大专栏
## 前言

GitHub 提供的 GitHub Pages 服务非常方便，可以让程序员迅速的建立起自己的博客。GitHub Pages 提供了简洁高效的新手引导，可以非常快速的建立起一个基础的博客项目，而该项目即是博客。但是这仅限于 Hello World 的水平。在 GitHub Help 页面能找到[Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)教程。故决定按照教程走，充实自己的博客。

## Step 1: Ruby&Bundler Installation

    $ sudo apt-get install ruby
    $ ruby -v
    ruby 2.3.3p222 (2016-11-21) [x86_64-linux-gnu]
    $ gem install bundler
    $ bundler -v
    Bundler version 1.16.6 

gem 是指用 ruby 语言写的类库，类似 c 的 lib 或是 java 的 jar 文件；而 bundler 便是用来管理这些 gem 的 gem，类似 python 的 pip 之类的工具。

## Step 2: Jekyll Installation

Jekyll 是 ruby 的一个 gem，由于已经安装了 bundler，自然可以用它来安装 Jekyll。

### Step 2.1

首先进入项目文件夹，此处为~/Project/yunyexiao.github.io/。一般该文件夹下没有 Gemfile 文件，故需生成。

    $ bundle init 

该命令在项目根目录下生成了一个简单的 Gemfile 文件。内容大概为：

    # frozen_string_literal: true

    source "https://rubygems.org"

    git_source(:github) {|repo_name| "https://github.com/#{repo_name}" }

    # gem "rails" 

虽然看不太懂，不过没关系。当然也可以手动新建这个文件。

### Step 2.2

接着修改这个 Gemfile 文件。根据 GitHub Help 的引导，需要添加一行：

    gem 'github-pages', group: :jekyll_plugins 

### Step 2.3

正式的安装 Jekyll 及其依赖。从这里开始就踩坑了。理论上只需要执行下面这句命令：

    $ bundle install 

然而，中国大陆有着坚固的万里长城。这里按下回车后会卡住，或是半天后提示连接超时。

#### Solution 1: Mirror

首先尝试一些镜像。理论上，只需要将 Gemfile 中`source`一行的源改为大陆可访问的镜像即可。然而，`http://ruby.taobao.org/`，`https://gems.ruby-china.org/`均已无效。**然而`https://gems.ruby-china.com/`有效**，ruby-china 不过是换了个域名，这一点我后来才发现。尝试了前两个镜像后，我稍带失望，只能另寻他路。

#### Solution 2: Proxy

接着开始尝试给终端翻墙。以前翻墙都是采用 SSR 或是修改浏览器配置实现的，如今必须手动设置终端的代理。经实验，可以使用 SSR+privoxy 的方式实现。SSR 已安装，故不再次赘述。安装 privoxy:

    $ sudo apt-get install privoxy
    $ privoxy --version
    Privoxy version 3.0.26 (https://www.privoxy.org/) 

进入 / etc/privoxy 目录下修改 config 文件，加入`forward-socks5 / 127.0.0.1:1080 .`，保存退出。这是采用全局代理，不使用 PAC 模式是因为 PAC 需要一个 PAC 列表，有点麻烦。接着，

    $ export http_proxy=http://127.0.0.1:8118
    $ export https_proxy=http://127.0.0.1:8118
    $ export no_proxy=localhost
    $ systemctl start privoxy.service 

其中 8118 是 privoxy 默认的监听端口。注意，此处 export 命令的使用注定了这个代理规则仅适用于本次登录的终端，退出该终端后便失效。最后一条命令中 start 替换为 stop 即可结束该服务。

#### Problem: commonmarker installation error

终于能在终端科学上网了，`purl www.google.com`成功把谷歌首页的源代码输出了。接着便是在项目根目录下执行`bundle install`。这次很多依赖都成功下载完成，然而在安装时，又来了一个新坑:

    An error occurred while installing commonmarker (0.17.13), and Bundler
    cannot continue.
    Make sure that `gem install commonmarker -v '0.17.13' --source
    'https://gems.ruby-china.com/'` succeeds before bundling.

    In Gemfile:
      github-pages was resolved to 192, which depends on
        jekyll-commonmark-ghpages was resolved to 0.1.5, which depends on
          jekyll-commonmark was resolved to 1.2.0, which depends on
            commonmarker 

按照错误提示输入`gem install commonmarker -...`命令后：

    Fetching: commonmarker-0.17.13.gem (100%)
    ERROR:  While executing gem ... (Gem::FilePermissionError)
        You don't have write permissions for the /var/lib/gems/2.3.0 directory. 

前面加上了 sudo 前缀后：

    ERROR:  Error installing commonmarker:
        ERROR: Failed to build gem native extension.

        current directory: /var/lib/gems/2.3.0/gems/commonmarker-0.17.13/ext/commonmarker
    /usr/bin/ruby2.3 -r ./siteconf20181012-11498-15xwmw7.rb extconf.rb
    mkmf.rb can't find header files for ruby at /usr/lib/ruby/include/ruby.h

    extconf failed, exit code 1

    Gem files will remain installed in /var/lib/gems/2.3.0/gems/commonmarker-0.17.13 for inspection.
    Results logged to /var/lib/gems/2.3.0/extensions/x86_64-linux/2.3.0/commonmarker-0.17.13/gem_make.out 

经过艰苦卓决的 google，终于成功安装了 commonmarker：

    $ sudo apt-get install ruby2.3-dev
    ...(此处省略)
    $ sudo gem install commonmarker -v '0.17.13' --source 'https://rubygems.org/'
    Building native extensions.  This could take a while...
    Successfully installed commonmarker-0.17.13
    Parsing documentation for commonmarker-0.17.13
    Installing ri documentation for commonmarker-0.17.13
    Done installing documentation for commonmarker after 1 seconds
    1 gem installed 

原来是缺少 ruby-dev 包的相关文件。接着再次执行`bundle install`命令，看到下面这条后，终于算是安装完了：

    Post-install message from html-pipeline:
    -------------------------------------------------
    Thank you for installing html-pipeline!
    You must bundle Filter gem dependencies.
    See html-pipeline README.md for more details.
    https://github.com/jch/html-pipeline#dependencies
    ------------------------------------------------- 

## Step 3: Construct the Jekyll Structure

接下来就简单多了，完全没遇到坑。

另找一个目录（最好不要在现有的 github 仓库下）执行如下命令：

    $ bundle exec jekyll _3.3.0_ new JekyllBlog 

当然，`JekyllBlog`可以换成任意其他名字，这会在当前目录生成一个对应的项目。接着 cd 进去编辑其中的 Gemfile 文件，删除其中的`gem "jekyll", "~> 3.7.4"`这一行并将`gem "github-pages", group: :jekyll_plugins`这行前面的注释符号`#`去除。

运行方式：

    $ jekyll s
    Configuration file: /home/aruji/Project/yunyexiao.github.io/_config.yml
                Source: /home/aruji/Project/yunyexiao.github.io
           Destination: /home/aruji/Project/yunyexiao.github.io/_site
     Incremental build: disabled. Enable with --incremental
          Generating... 
                        done in 0.192 seconds.
     Auto-regeneration: enabled for '/home/aruji/Project/yunyexiao.github.io'
        Server address: http://127.0.0.1:4000/
      Server running... press ctrl-c to stop. 

打开`http://localhost:4000`就可以查看效果了。

## Step 4: Add it to Git Repo

接着有三种方式利用这些自动生成的文件。

1.  若没有 github pages 的博客项目，则干脆把这个项目作为博客，注意必须将项目名改为`<github-username>.github.io`，其中`<github-username>`替换为自己的 github 用户名；然后在 github 远端也建立同名仓库，关联起来。所有在这个仓库的 master 分支上的网页都会变为博客的一部分。
2.  若有对应的博客项目，将这里的 Jekyll 项目中的文件复制过去即可。只要有 Gemfile，就可以利用`jekyll s`命令运行。
3.  还可以将该项目关联至一个任意其他 github 仓库如 RepoA，并将变动提交至`gh-jekyll`分支 (若没有就新建)，这个分支上的 md 文件也会变为博客的一部分。但是，这里的 url 是:`http://<github-username>.github.io/<repo-name>`。其中`<github-username>`是 github 用户名，`<repo-name>`是仓库名 RepoA。

所以以上三种方法的区别在于：方法 1、2 对应的博客地址为`http://<github-username>.github.io/`而方法 3 为`http://<github-username>.github.io/<repo-name>`。方法 3 可以看做是为特定项目所作的博客。本人选择了方法 2。

## Step 5: Push and Enjoy

剩下的事情就是 git add-commit-push 一条龙服务了。 
 [https://www.dazhuanlan.com/zimuhai/topics/1600631](https://www.dazhuanlan.com/zimuhai/topics/1600631)
