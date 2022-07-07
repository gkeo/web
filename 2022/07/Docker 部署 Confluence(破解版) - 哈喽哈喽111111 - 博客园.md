# Docker 部署 Confluence(破解版) - 哈喽哈喽111111 - 博客园
一、 说明  
1.1 素材

本文采用素材如下：  
Docker 镜像 Github 链接 ([https://github.com/cptactionhank](https://github.com/cptactionhank))  
破解工具 Gitee 链接 ([https://gitee.com/pengzhile/atlassian-agent](https://gitee.com/pengzhile/atlassian-agent)) ([https://zhile.io/2018/12/20/atlassian-license-crack.html](https://zhile.io/2018/12/20/atlassian-license-crack.html))

采用以上工具，理论上可以破解几乎全部版本。

本地下载地址：[https://files.cnblogs.com/files/sanduzxcvbnm/atlassian-agent-v1.2.3.zip](https://files.cnblogs.com/files/sanduzxcvbnm/atlassian-agent-v1.2.3.zip)

1.2 数据库

如果是选择外部数据库，大家可以按照这样创建：

```null
# 创建confluence数据库及用户
CREATE DATABASE confdb CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
grant all on confdb.* to 'confuser'@'%' identified by '7FxxxzhO';

# confluence要求设置事务级别为READ-COMMITTED
set global tx_isolation='READ-COMMITTED';

```

二、 安装 Confluence(7.4.0)  
Atlassian Confluence（简称 Confluence）是一个专业的 wiki 程序。它是一个知识管理的工具，通过它可以实现团队成员之间的协作和知识共享。

2.1 制作 Docker 破解容器

编写 Dockerfile 文件：

```null
FROM cptactionhank/atlassian-confluence:latest

USER root


COPY "atlassian-agent.jar" /opt/atlassian/confluence/


RUN echo 'export CATALINA_OPTS="-javaagent:/opt/atlassian/confluence/atlassian-agent.jar ${CATALINA_OPTS}"' >> /opt/atlassian/confluence/bin/setenv.sh
```

2.2 下载破解文件

在 gitee 中下载编译好的即可，放置在 Dockerfile 同目录下

-   JIRA  
    --Dockerfile  
    --atlassian-agent.jar

2.3 构建镜像

```null
docker build -t confluence:latest .
```

结果如下：

```null
Sending build context to Docker daemon  976.9kB
Step 1/4 : FROM cptactionhank/atlassian-confluence:latest
 
Step 2/4 : USER root
 
Removing intermediate container 016cda821c07
 
Step 3/4 : COPY "atlassian-agent.jar" /opt/atlassian/confluence/
 
Step 4/4 : RUN echo 'export CATALINA_OPTS="-javaagent:/opt/atlassian/confluence/atlassian-agent.jar ${CATALINA_OPTS}"' >> /opt/atlassian/confluence/bin/setenv.sh
 
Removing intermediate container 68588c4f146c
 
Successfully built 45a74f5420da
Successfully tagged confluence:latest
```

2.4 启动容器

```null

docker run -d --name confluence \
  --restart always \
  -p 18010:8090 \
  -e TZ="Asia/Shanghai" \
  -v /home/confluence:/var/atlassian/confluence \
  confluence:latest
```

2.5 访问 confluence  
访问 IP:18010，参照 JIRA 的安装流程，进行操作。可在引导过程中，与之前安装的 JIRA 进行绑定关联。  
![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013151943287-2110073625.png)

我们就选择一个应用吧  
![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013152025404-110926332.png)

2.6 破解

生成 confluence 许可命令参照如下：

```null
# 设置产品类型：-p conf， 详情可执行：java -jar atlassian-agent.jar 
java -jar atlassian-agent.jar -d -m test@test.com -n BAT -p conf -o http://192.168.0.89 -s BTW4-2T4Y-9BTK-R0DP
```

复制服务器 ID: BTW4-2T4Y-9BTK-R0DP  
在本地存放 atlassian-agent.jar 的目录下执行命令，生成许可证：  
需替换邮箱（test@test.com）、名称（BAT）、访问地址（[http://192.168.0.89](http://192.168.0.89/)）、服务器 ID（BTW4-2T4Y-9BTK-R0DP）为你的信息

```null
java -jar atlassian-agent.jar \
  -d -m test@test.com -n BAT \
  -p jira -o http:
```

例如我的信息如下，生成许可证：

```null
java -jar atlassian-agent.jar \
  -d -m wangzan18@126.com -n BAT \
  -p conf -o http://confluence.wzlinux.com \
  -s BTW4-2T4Y-9BTK-R0DP







Your license code(Don't copy this line!!!): 

AAABXQ0ODAoPeJx1kV9vgjAUxd/7KUj2XG1R5p+EZArEmYEsotteK7tqEyikLW7s069UzJJlS/rQn
HtPf/fc3mWNcBLWOpQ41JuT6dylTpDtHJe4BCWMCw2CiRyiz5rLNmQafJdMZ5hMzEExz0Gof4ohq
FzyWvNK+HtR8JJreHeKq8U5tM5Z61rNh8OvMy9gwCsUVEKzXG9YCf5ysUMZyAvIdegvR49jHNL0F
a+25A0/Be4K5ZU4DjZNeQCZHvcKpPIxRak8McEVs9QOYN7vOosGTIpBXpXS1MRJddcbMDJBC/+Di
dMXE3T6QN17W7aIPuOurcEOFqRJEm2D9SJGgQQL6pO7BBMPU3Jbixk8XodZtMEx9aYzMht7hI4mH
jKS/4dscWYcfgFfywZQdGFFc41yZIUC9NzI/MwU/AZmzeFn19Zq38o0kxpkb7aScbIARKfavv47X
sz6Oq/7DWevrTUwLgIVAIEyoNFjmUFyTJOVUzmxTJTM14S8AhUAkaRbRjdl4D9MZtO6l5nCHcR2B
80=X02h9
```

将生成的许可证复制到页面，完成破解。  
![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013152518259-783178591.png)

选择单机模式，并设置数据库  
![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013152620260-458232693.png)

需要事先创建数据库，并且设置如下：  
![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013153303842-1204286496.png)

还有开头的事务级别

```null
mysql> set global tx_isolation='READ-COMMITTED';
Query OK, 0 rows affected (0.00 sec)
```

注意一下：数据库 url 连接中用的是 utf8, 不能用 utf8mb4.

```null
jdbc:mysql://192.168.0.254/confluence?useUnicode=true&characterEncoding=utf8
```

![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013152708106-355960221.png)

2.7 配置 confluence  
我们做个示范站点  
![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013153745363-878959299.png)

配置用户管理，这里我们选择之前创建好的 jira  
![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013153813296-59979739.png)

配置连接信息  
![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013153856300-124843698.png)

同步数据  
![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013153917902-1173835991.png)

![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013153927591-1288298794.png)

登陆查看授权情况  
站点管理，一般设置，授权细节  
![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013161121558-799346154.png)

![](https://img2020.cnblogs.com/blog/794174/202010/794174-20201013153950382-114407398.png)

三、乱码问题

在我们正常安装之后，中文可能会有乱码，我们修改一下连接字符串，在 confluence 的家目录下面，有一个配置文件 confluence.cfg.xml，找到 hibernate.connection.url，在数据库字符串后面加上如下字符，整体结果如下：

```null
jdbc:mysql://172.17.64.10/confdb?useUnicode=true&characterEncoding=utf8
```

记住，里面的 amp; 不要省略。

如果可以的话，把数据库的字符串改成 utf8mb4  
[https://confluence.atlassian.com/doc/database-setup-for-mysql-128747.html](https://confluence.atlassian.com/doc/database-setup-for-mysql-128747.html)

还有一个文档说字符串改成 utf8，不是 utf8mb4，具体区别我也不知道，大家可以去测试一下  
[https://www.cwiki.us/display/CONFLUENCEWIKI/Database+Setup+For+MySQL](https://www.cwiki.us/display/CONFLUENCEWIKI/Database+Setup+For+MySQL) 
 [https://www.cnblogs.com/hahaha111122222/p/13809276.html](https://www.cnblogs.com/hahaha111122222/p/13809276.html)
