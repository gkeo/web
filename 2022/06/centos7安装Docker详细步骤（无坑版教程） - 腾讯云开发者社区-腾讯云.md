# centos7安装Docker详细步骤（无坑版教程） - 腾讯云开发者社区-腾讯云
### centos7 安装[Docker](https://cloud.tencent.com/product/tke?from=10680)全过程记录（无坑版教程）

## 一、安装前必读

在安装 Docker 之前，先说一下配置，我这里是 Centos7 Linux 内核：官方建议 3.10 以上，3.8 以上貌似也可。

注意：本文的命令使用的是 root 用户登录执行，不是 root 的话所有命令前面要加 `sudo`

**1. 查看当前的内核版本**

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-57-00/697a822b-f417-4584-9841-746093d5b2e4.png?raw=true)

我这里是 3.10 ，满足条件。

**2. 使用 root 权限更新 yum 包（生产环境中此步操作需慎重，看自己情况，学习的话随便搞）**

这个命令不是必须执行的，看个人情况，后面出现不兼容的情况的话就必须 update 了

    注意​ 
    yum -y update：升级所有包同时也升级软件和系统内核；​ 
    yum -y upgrade：只升级所有包，不升级软件和系统内核

**3. 卸载旧版本（如果之前安装过的话）**

    yum remove docker  docker-common docker-selinux docker-engine

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-57-00/4963e0f5-da71-40b3-ac90-ee789a60a773.png?raw=true)

## 二、安装 Docker 的详细步骤

**1. 安装需要的软件包， yum-util 提供 yum-config-manager 功能，另两个是 devicemapper 驱动依赖**

    yum install -y yum-utils device-mapper-persistent-data lvm2

**2. 设置 yum 源**

设置一个 yum 源，下面两个都可用

    yum-config-manager --add-repo http://download.docker.com/linux/centos/docker-ce.repo（中央仓库）

    yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo（阿里仓库）

3\. 选择 docker 版本并安装 （1）查看可用版本有哪些

    yum list docker-ce --showduplicates | sort -r

（2）选择一个版本并安装：`yum install docker-ce - 版本号`

    yum -y install docker-ce-18.03.1.ce

出现下图说明安装成功

4\. 启动 Docker 并设置开机自启

    systemctl start docker
    systemctl enable docker

over！ 
 [https://cloud.tencent.com/developer/article/1701451](https://cloud.tencent.com/developer/article/1701451)
