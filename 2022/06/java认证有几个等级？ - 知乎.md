# java认证有几个等级？ - 知乎
Junior/Novice(初级 / 入门级 / 小白级） -> Associate(助理级 / 中级) -> Professional(专业级 / 高级)

Oracle 的体系是有不同的路径，例如 Java 8, Java 11, 数据库，[中间件](https://www.zhihu.com/search?q=%E4%B8%AD%E9%97%B4%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1467119394%7D)等，路径内包含认证，以及获得某个认证需要通过的考试 / 培训。

基于 JDK 1.8 的有:

小白级 [Java Foundations 1Z0-811](https://link.zhihu.com/?target=https%3A//education.oracle.com/java-foundations/pexam_1Z0-811) :  
基本上面向无基础小白，很少人考这个吧，不过这个报名费便宜，**¥682 (不含税)。** 

助理级 [Java SE 8 Programmer I 1Z0-808](https://link.zhihu.com/?target=https%3A//education.oracle.com/java-se-8-programmer-i/pexam_1Z0-808) :  
可以理解为中级吧，这个花一两周准备就可以过了。过了就可获得 OCA 认证。报名费 **¥1077（不含税）。** 

专业级 [Java SE 8 Programmer II 1Z0-809](https://link.zhihu.com/?target=https%3A//education.oracle.com/java-se-8-programmer-ii/pexam_1Z0-809) :  
考 JDK 高级特性，同时通过 1Z0-808 和 1Z0-809 才能获得 OCP 认证。报名费也是 **¥1077（不含税）。** 

基于 JDK 11 的有:

只有一个认证，即 [Oracle Certified Professional: Java SE 11 Developer](https://link.zhihu.com/?target=https%3A//education.oracle.com/oracle-certified-professional-java-se-11-developer/trackp_815)，可以理解为 Java 11 的 OCP。因为这个路径 Oracle 要求通过 1Z0-815 + 1Z0-816 才可以获得认证，也就是至少要考两次试，两次的报名费都是 1107。另外如果之前已经获得过 SCJP 6 (Sun 时代的 Java 认证，相当于 Java 6 高级）或 OCP 7, OCP 8，则可以考 1Z0-817(给旧证持有者升级到 Java 11 认证的考试，相对 1Z0-816 来说考点内容可能少一些)。

所以要获得 OCP 11 认证，要通过的考试可以是以下组合：

1Z0-815 + 1Z0-816（正常的新路线，Java 11 中级 + Java 高级)  
1Z0-808 + 1Z0-816 （Java 8 中级 + Java 11 高级，我走的是这条路线)  
旧 OCP 持有者 (例如 1Z0-809) + 1Z0-817

不过 1Z0-815/816 这条线在 2020-10-01 后会被废弃，改用新大纲，**1Z0-819**，到时候要求可能变了，不过知识点可能差不多。

在新大纲未启用前，1Z0-816 考试应该是有史以来难度最大的，80 到不定项选择题，考试时长 180 分钟。下边是我翻译的考试大纲：

**Java 基础**创建并使用`final`类  
创建并使用内部，嵌套，以及匿名类  
创建并使用枚举

**Java 接口**创建并使用带有默认方法的接口  
创建并使用带有私有方法的接口  
函数式接口与 Lambda 表达式  
定义并编写函数式接口  
创建并使用 Lambda 表达式，包括 Lambda 语句，局部变量作 lambda 参数

**内置函数式接口**使用`java.util.function`包里的接口  
使用核心函数式接口，包括`Predicate`,`Consumer`,`Function`和`Supplier`  
使用`java.util.function`包里基础接口的基本数据类型及二元变式

**迁移到模块化应用**迁移使用 Java SE 9 以前版本开发的应用到 SE 11，包括自上而下和自下而上迁移方式，将一个 Java SE 8 应用分模块作迁移  
使用`[jdeps](https://www.zhihu.com/search?q=jdeps&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1467119394%7D)`确定依赖关系，并识别解决循环依赖的方法。

**并发**使用`Runnable`，`Callable`创建[工作线程](https://www.zhihu.com/search?q=%E5%B7%A5%E4%BD%9C%E7%BA%BF%E7%A8%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1467119394%7D)，并使用`ExecutorService`并发地执行任务  
使用`java.util.concurrent`包里的容器和类，包括`CyclicBarrier`和`CopyOnWriteArrayList`  
编写线程安全的代码  
识别线程问题，例如死锁和活锁

**I/O (基础以及 NIO2)**使用 I/O 流从控制台和文件读写数据  
使用 I/O 流读写文件  
使用序列化读写对象  
使用`Path`接口操作文件和目录路径  
使用`Files`类去检查、删除、复制或移动一个文件或目录  
结合`Files`类使用 Stream API

**JDBC 数据库应用**使用 JDBC URLs 和`DriverManager`连接到数据库  
使用`PreparedStatement`去执行 CRUD 操作  
使用`PreparedStatement`和`CallableStatement`APIs 去执行数据库操作

**注解**表述注解的用途以及典型使用模式  
应用注解到类和方法  
描述 JDK 中常用的注解  
声明自定义注解  
异常处理与断言  
使用 try-with-resources 结构  
创建并使用自定义异常类  
使用断言测试不变性

**泛型与容器（Collections Framework)**使用包装类，自动装箱和自动拆箱  
用钻石记号和通配符创建并使用泛型类、方法  
描述容器框架并使用主要容器接口  
使用`Comparator`和`Comparable`接口  
创建并使用容器的便利方法

**Java Stream API**描述 Stream 接口和管道  
使用 lambda 表达式和方法引用  
Streams 上的 Lambda 操作  
使用`map`,`peek`和`flatMap`方法提取 stream 数据  
使用`findFirst`,`findAny`,`anyMatch`,`allMatch`和`noneMatch`方法搜索 stream 数据  
使用`Optional`类  
使用`count`,`max`,`min`,`average`和`sum`stream 操作执行计算  
使用 lambda 表达式对容器排序  
在 streams 使用`Collectors`，包括`groupingBy`和`partitioningBy`操作

**模块化应用中的服务**描述服务的组件，包括指令  
设计一个服务类型，使用`ServiceLoader`加载服务，检查服务的依赖，包括消费者和提供者模块

**并行 Streams**编写使用并行 streams 的代码  
用 streams 实现分解与归约操作

**Java SE 应用安全编码**在 Java 应用中预防拒绝服务  
在 Java 应用中保护机密信息  
实现数据一致性准则——注入和包含以及输入校验  
通过限制可访问性和可扩展性保护代码受外部攻击，妥善处理输入校验以及可变性  
安全地构建敏感对象  
保护序列化与反序列化

**本地化**使用`Locale`类  
使用资源包  
使用 Java 格式化消息、日期和数字

* * *

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-57-06/137c40ba-e313-4fc3-9bf4-f251732f6ce9.jpeg?raw=true)

[发布于 2020-09-11 20:06](//www.zhihu.com/question/376236220/answer/1467119394)

​赞同 20​​4 条评论

​分享

​收藏​喜欢收起​ 
 [https://www.zhihu.com/question/376236220/answer/1467119394](https://www.zhihu.com/question/376236220/answer/1467119394)
