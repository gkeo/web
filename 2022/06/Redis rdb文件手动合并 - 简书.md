# Redis rdb文件手动合并 - 简书
线上服务使用的阿里云的集群版本 redis 服务，数据量 1 千万，rdb 文件 4GB。

线下测试环境只有单节点 redis，需要导入线上全量数据。

8 个 rdb 文件，每个 500MB。

RDB 格式如下：

头 5 个字节是字符 REDIS。

之后 4 个字节代表版本号，阿里的版本分别是 00 00 00 06

之后 2 个字节 FE 00，FE 是标识 00 是数据库，还好我们只有一个库。

最后的结尾 9 个字节 。

FF 加上 8 个字节的 CRC64 校验码（实在没空弄，后来偷了一个懒） 。

一、生成对应的文件

\#文件 1 大小 566346503，截取尾部的 9 个字节

dd bs=1 if=src_1.rdb of=1.rdb count=566346494

\#文件 2 大小 570214520，跳过头部的 11 个字节，再截取尾部的 9 个字节

\#dd bs=1 if=src_2.rdb of=2.rdb skip=11 count=570214400

...

\#文件 8 大小 569253061，跳过头部的 11 个字节，再截取尾部的 8 个字节，保留 FF。

dd bs=1 if=src_8.rdb of=8.rdb skip=11 count=569253042

二、合并文件

cat 1.rdb > dump.rdb

cat 2.rdb >> dump.rdb

...

cat 8.rdb >> dump.rdb

备份文件合并完毕。

三、检查备份文件

redis-check-rdb dump.rdb 

应该会提示没有 crc 校验。

四、修改配置文件

因为数据库备份文件里面不包含 crc64 的校验码，配置文件中关闭选项。

rdbchecksum no

数据恢复到此结束，此方法只适合用于临时恢复和导出数据，数据完整性不敢保证。

参考的文章

[https://github.com/sripathikrishnan/redis-rdb-tools/wiki/Redis-RDB-Dump-File-Format](https://github.com/sripathikrishnan/redis-rdb-tools/wiki/Redis-RDB-Dump-File-Format)

 [https://www.jianshu.com/p/097c0b01ea69](https://www.jianshu.com/p/097c0b01ea69)
