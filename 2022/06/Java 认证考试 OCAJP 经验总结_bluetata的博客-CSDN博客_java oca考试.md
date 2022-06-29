# Java 认证考试 OCAJP 经验总结_bluetata的博客-CSDN博客_java oca考试
关于认证考试（无论什么认证）是否有用？这个话题无论是在哪里都有人问。这个问题就好比上大学是否有用吗一样，有的人没上过大学一样年薪百万。**认证这种东西需要的时候即有用，不需要的时候就没用。有，并没有什么坏处。说实话个人感觉这证件没什么大用。** 而自己想考的理由完全是想自我 check 下，逼自己复习学习基础。如果你是刚毕业的 GH 或者在校的，手里有些零花钱的可以考虑下，有这个证 / 或许 / 可以证明你 Java 基础还不错。  

OCAJP 全称为：Oracle Certified Associate Java SE 8 Programmer Certification 也叫 OCA。改版前认证为 OCJP，在 Sun 公司没有被 Oracle 收购前叫 SCJP。  
这里要提一下，之前的 OCJP 已经 Retire(停考 / 退休)。Java 7 的认证也即将停考。所以如果想考试建议直接考 Java SE 8 的也就是  
Java SE 8 Programmer I 认证，考试编号为 1Z0-808，下一个考试为：Java SE 8 Programmer II | 1Z0-809。  

​![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-57-46/5ff60f70-774e-4d21-8ed0-501485992ed0.png?raw=true)
​​

上图的考试科目链接：[https://education.oracle.com/oracle-certification-exams-list](https://education.oracle.com/oracle-certification-exams-list)

在上图中前两个 1Z0-803 和 804 是之前获取 Java 7 的认证考试；  
1Z0-805 是已经获取过 Java6 认证（也就是 OCJP/SCJP）的升级成为 Java7 的认证  
1Z0-808 和 809 是 Java 8 I 和 II 的认证，也就是 OCAJP 和 OCPJP 认证。  
其他不建议，会在 2019 年底随着 Java 7 的认证考试取消，而随着均会停考。  
另外在官网中可以查看到后缀带有 JPN 的考试科目，该考试为日语认证，考试等同于英语的 1Z0-808 和 809，只是考试语言为日语。有日语要求的可以考虑该版本考试。考试内容一样。  

SE 考试费用均为 1,077 RMB，我是在 VUE 网站报名，使用的 VISA 信用卡支付的美元，下文会介绍如何报名缴费。

考试时长为**150 分钟**，总共**70 道**题，正确率要高于**65%**以上（至少答对**46 道**）。  

先要注册一个 [Person|VUE](https://wsr.pearsonvue.com/testtaker/signin/SignInPage.htm?clientCode=ORACLE) 账号，如果已经有账号跳过，遇到问题可以看这个 [OCJP 考试 VUE 约考流程](https://jingyan.baidu.com/article/72ee561a537855e16138df2b.html)（友情提醒：该 VUE 账号和 ORACLE 账号一定要自己记好，找个笔记软件或者便签软件）。VUE 是一个综合认证考试的服务机构，几乎所有的 Global 级别的认证都可以在 VUE 系统中参加考试。在预约考试的时候会让你选择考场，选择离自己比较近的。防止堵车什么的。  

SE 的两个考试还是比较简单的，只要基础牢靠应该都没有问题。

备考书籍：《Java 编程思想》只要认真看过的肯定没问题。另外清华大学出版社的《OCA Java SE 8 程序员认证考试指南 Exam 1Z0-808》也可以，我个人没有看过这本，当时考试遇到了一个同样考试的同学，他说他备考用的这本书，也过了。

备考的时候可以做一些模拟题，锻炼自己的阅题速度，模拟控制和掌控考试时间。

​![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-57-46/074d0fb8-5d53-4871-aa38-3da969ebaf85.png?raw=true)
​​  

-   提前 15 分钟到达考场，携带两个本人证件。**再次提醒：参加考试时必须要准备两个证件：学生党可以用 学生证和身份证，工作后的可以选择 工牌和身份证，或者护照，驾驶证，行用卡均可。** 
-   确认身份，录入信息，进入 VUE 考试机构，会有人所要你的证件，会在他们系统中确认你的信息，并会进行头像采照。
-   进行签字，考试声明之类的，进入考场时间等。
-   进行安全检查，和高考检查差不多，所有身外之物，以及金属饰品都会强制要求放入专门的柜子中。眼睛会确认是否有摄像，最后扫描金属和机场安检差不多。
-   最后会领取可涂擦手写板、笔。
-   以上准备完后，工作人员会带你进入考场， **再次提醒：一定要在进入考场前也就是安全监测前，WC 厕所的事情和喝水的事情都解决掉，考试中途去厕所会算考试时间，并不会终止考试。** 
-   考场中有各种监控，考试过程中有任何问题需要举手在监控中示意监考工作人员，可提前结束考试（举手示意监考人员）。
-   考试结束，工作人员会打印考试回执证明，ORACLE 考试不会当场出成绩，结束考试后 30 分钟可以查询成绩测试（下文会说）。
-   取个人物品，签字，和考试结束离开时间，离开考场。

**考试结果会以电子邮件 Email 的形式发送到你的 ORACLE 账号关联邮箱。**  邮件标题，如下图：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-57-46/21c0f433-2ffe-4aef-8765-5688cbcf6000.png?raw=true)
​​

进入邮件，如下图点击查看结果：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-57-46/cf6fcda8-7f90-436d-82a8-06a4e844a0d2.png?raw=true)
​

登陆 Oracle 账号后：

​![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-57-46/3e04af07-3ba3-4dfb-a90c-7327eb85ee2f.png?raw=true)
​​

点击第一个查询结果：

​![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-57-46/e2b69233-0fb8-43dd-b66d-a53acc44a9f2.png?raw=true)
​​

48 小时后可以查看或打印电子证书（点击上图的：Download my eCertificate）：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-57-46/a026ff22-2a52-440d-b567-9507ab331590.png?raw=true)
​

结果证书：

![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-57-46/b61cdd47-e516-479a-b6f7-20509cc16b06.png?raw=true)
​​

认证徽章：  
通过认证后除会有证书外，还会有一个认证徽章（收集在 ACClaim 系统中）：

该认证的 Acclaim 系统连接：[https://www.youracclaim.com/badges/3596105c-ade8-4875-95d6-ee4c65272f60](https://www.youracclaim.com/badges/3596105c-ade8-4875-95d6-ee4c65272f60)

​![](https://github.com/gkeo/img/blob/main/2022/2022-6-29%2015-57-46/5bacea7d-7e3f-4989-9856-1e556c8aa4e6.png?raw=true)
​​  

OCAJP 考试基本都是考的基础，整体知识点都在 Java 编程思想中集合之前的内容，没有高级编程线程 IO 流等的内容，有基本的 JDK8 新特性，比如 Switch 和 Lambda 语法等。

多数为程序题，看输出结果等，我的考题大约有 10 到左右理论基础题，异常和 error 的区别，Java 特性的描述等等基础理论题。

在答程序题的时候先确认是否有编译异常，这点会省很多时间，一部分题看一眼就能发现有编译错误，后面的就不用看了。

答题板很重要，我基本没有用考试系统中的 mark 功能，都是在答题板中记录，标注不确定，和必要回头 check 两个标记。

整体答完 70 道题 150 分钟还剩下 32 分钟左右，我一共在答题板 mark 了大约 15-17 道题，更改了大约 2-4 道题的答案，其他的都进行了 double check。个人感觉我答的这套题理论题有点多，大约有 8-10 道题都是理论描述，全英文。有 3 道题中有超过 3 个 Key Work 不确定是什么意思。在上图查询结果中能看到自己的错误点，其中 checked Exception 和 unchecked 异常的这题在我的 double check list 中，仍然错了。其他的错误题大部分不再我回头检查的 double check 中。当我看到结果的时候感觉自己过于肯定答案，没有标记 double check，以至于没有仔细检查。

考前我大约做了 3 天的网上模拟题，基本都在 80% 以上的正确率。大约周期都是在 1 个小时的时间或者 2-3 个小时，1 个小时做 20 道左右，正确的题不再继续答，错误的题汇总统一后继续二次作答（有需要模拟题的留下邮箱，我看到会发给你）。

**注：** 本文原创由\`_**blue\*\***t**a**t\*\*a_\`发布于 blog.csdn.**net**、转载请务必注明出处。

![](http://s11.flagcounter.com/count2/Ztk/bg_FFFFFF/txt_000000/border_CCCCCC/columns_2/maxflags_12/viewers_0/labels_0/pageviews_0/flags_0/percent_0/) 
 [https://blog.csdn.net/dietime1943/article/details/86585965](https://blog.csdn.net/dietime1943/article/details/86585965)
