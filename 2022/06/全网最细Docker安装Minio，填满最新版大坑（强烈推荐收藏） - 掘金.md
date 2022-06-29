# 全网最细Docker安装Minio，填满最新版大坑（强烈推荐收藏） - 掘金
通常在企业中我们会将一些图片，视频，文档等相关数据存储在对象存储中，常见的对象存储服务有阿里云的 OSS 对象存储、FastDFS 分布式文件系统以及公司的私有云平台等等，以便于数据的存储和快速获取。但随着业务的快速发展，我们需要存储一些身份信息用于审核和实名相关的数据，这部分数据较为敏感，因此对于敏感数据的存储我们选择了使用兼容 S3 协议的开源分布式对象存储 - Minio 来进行自建服务。

> MinIO 对象存储系统是为海量数据存储、人工智能、大数据分析而设计，基于 Apache License v2.0 开源协议的对象存储系统，它完全兼容 Amazon S3 接口，单个对象最大可达 5TB，适合存储海量图片、视频、日志文件、备份数据和容器 / 虚拟机镜像等。MinIO 主要采用 Golang 语言实现，整个系统都运行在操作系统的用户态空间，客户端与存储服务器之间采用 http/https 通信协议。

## 1. 查询 minio 服务版本

```java
docker search minio
```

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/ffed6c77-a637-498b-9530-71852d99d40e.webp?raw=true)

## 2. 拉取 minio

执行命令 docker pull minio/minio 下载稳定版本镜像，使用命令 docker images 查看下载的镜像。

```java
docker pull minio/minio
```

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/51d6a29d-0ebd-4d61-aa74-46c6b5f70c09.webp?raw=true)

## 3. 启动

如果是 docker 安装的，启动命令如下：

```java
docker run  -p 9000:9000 --name minio \
 -d --restart=always \
 -e MINIO_ACCESS_KEY=minio \
 -e MINIO_SECRET_KEY=minio@123 \
 -v /usr/local/minio/data:/data \
 -v /usr/local/minio/config:/root/.minio \
  minio/minio server /data  --console-address ":9000" --address ":9090"
```

如果是 linux 版安装的，启动命令如下：

```java
./minio server /usr/local/minio/data --console-address ":9090"
```

启动成功，IP+9000 访问，我顿时傻逼了？？？？

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/cce939ae-8ada-4f8e-ac97-50e6f6b78c0b.webp?raw=true)
 最新版长成这样了？？？心里奔溃啊，那个简洁的界面没了，我们需要安装以前的版本，所以您是在我写这篇文章之后才安装的 minio，千万别装最新版的了，别用这个命令了：

> docker pull minio/minio

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/73fc7124-f37a-48bc-8ab1-93e750785f5f.webp?raw=true)

## 4.docker hub 下载其他版本

[hub.docker.com/r/minio/min…](https://link.juejin.cn/?target=https%3A%2F%2Fhub.docker.com%2Fr%2Fminio%2Fminio%2Ftags%3Fpage%3D1%26ordering%3Dlast_updated "https&#x3A;//hub.docker.com/r/minio/minio/tags?page=1&ordering=last_updated")

安装今年七月份以前的稳定版本。

```java
docker pull minio/minio:RELEASE.2021-06-17T00-10-46Z-28-gac7697426
```

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/6d835104-bef0-4c5e-b7a2-d4af3a0292f4.webp?raw=true)

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/476734dc-510a-4bbf-9efe-ecc4dc6be2a9.webp?raw=true)

## 1、启动测试

```java
docker run -p 9000:9000 minio/minio:RELEASE.2021-06-17T00-10-46Z-28-gac7697426 server /data
```

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/9f7656aa-bc2a-4674-9fba-68a9c765b698.webp?raw=true)
 心态爆炸啊，这个版本也不行，我们继续找下一个版本。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/c4f3f99b-e13a-4224-96fa-20849691d0e5.webp?raw=true)

## 1、下载旧版本

通过以上的坑，我们发现 minio 的版本更新迭代特别快，最新版本的已经变得面目全非，不认识了。我查阅了最新官网发布版本。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/ddf2b83b-a1ea-4313-a94f-1894c67789ca.webp?raw=true)

大大小小已经发布了 200 多个版本了，我们继续找下一个版本，**2021 年 6 月 17 号的**。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/2dc87f76-edd6-4a5f-bbf5-6ff3da6d863d.webp?raw=true)

```java
docker pull minio/minio:RELEASE.2021-06-17T00-10-46Z
```

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/ba37f6c9-969a-4481-a6d6-5ebe473345d3.webp?raw=true)

## 2、启动

```java
docker run -p 9000:9000 minio/minio:RELEASE.2021-06-17T00-10-46Z server /data
```

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/102007e1-9564-43fe-b20c-aa10be4d3f7d.webp?raw=true)

看到这里我暗自大喜，终于填满了这个坑。

## 3、访问测试

输入 IP+9000 访问 web 页面：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/3f344190-c738-42d9-8934-ee4cc9ff0cfd.webp?raw=true)

还是熟悉的味道啊，真香！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/11d80055-0935-4fd0-9c0d-68203fd64513.webp?raw=true)

## 3、MinIO 自定义 Access 和 Secret 密钥

```java
docker run -p 9000:9000 minio/minio server /data
```

> MinIO 需要一个持久卷来存储配置和应用数据。不过, 如果只是为了测试一下, 您可以通过简单地传递一个目录（在下面的示例中为 / data）启动 MinIO。这个目录会在容器启动时在容器的文件系统中创建，不过所有的数据都会在容器退出时丢失。

所以我们在工作中，真正的开发中，不能这么干，以上命令仅供测试使用，那么在工作当中应该注意什么呢？首先你的 key 和 secret 是哼重要的，就相当于你的账号密码，不要设置的那么简单，还有就是你的文件存放，假设服务宕机了，文件也不丢失，服务迁移了，文件也能跟着迁移，这才是工作中应当注意的地方。

**要覆盖 MinIO 的自动生成的密钥，您可以将 Access 和 Secret 密钥设为环境变量。 MinIO 允许常规字符串作为 Access 和 Secret 密钥。** 

```java
docker run -p 9000:9000 --name minio\
  -e "MINIO_ACCESS_KEY=admin" \
  -e "MINIO_SECRET_KEY=admin1973984292@qq.com" \
  -v /usr/local/minio/data:/data \
  -v /usr/local/minio/config:/root/.minio \
  minio/minio:RELEASE.2021-06-17T00-10-46Z server /data
```

```java
/usr/local/minio/data 
/usr/local/minio/config 
```

再次启动之后，我们就可以发现重新登录，就可以使用我们自己的 Access 和 Secret 密钥了。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/2ec7f161-026c-4004-9f84-de3c338f8c53.webp?raw=true)

新建一个存储桶，往里面添加一个文件：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/35bb97c6-3e1e-442d-a35d-689398903045.webp?raw=true)

上传成功之后，我们去到我们的服务 data 数据卷存储位置，查看文件是否存在。

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/9ce96df2-fba7-447c-8569-3baa673e1353.webp?raw=true)

## 4、MinIO 后台的方式启动

**如果要后台运行 加入 -d 参数**

```java
docker run -d -p 9000:9000 --name minio\
  -e "MINIO_ACCESS_KEY=admin" \
  -e "MINIO_SECRET_KEY=admin1973984292@qq.com" \
  -v /usr/local/minio/data:/data \
  -v /usr/local/minio/config:/root/.minio \
  minio/minio:RELEASE.2021-06-17T00-10-46Z server /data
```

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2014-59-20/c2b678d8-50dd-4be4-9c61-15b2e22a260c.webp?raw=true)

另外小编给大家一个中文文档方便学习 Minio：[docs.minio.org.cn/docs/master…](https://link.juejin.cn/?target=http%3A%2F%2Fdocs.minio.org.cn%2Fdocs%2Fmaster%2Fminio-docker-quickstart-guide "http&#x3A;//docs.minio.org.cn/docs/master/minio-docker-quickstart-guide")

好了，今天的文章就收尾了，在安装 Minio 的过程中，特别艰辛，最新版本实在巨坑，小编走了，不少弯路，喜欢的点赞，评论！！！！！ 
 [https://juejin.cn/post/6988340287559073799](https://juejin.cn/post/6988340287559073799)
