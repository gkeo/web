# 合并多个Redis dump.rdb 到一个rdb的多个database - 鞋带Er - 博客园
公司的服务器上运行了多个 redis，现在希望合并到一个 redis，用上 redis 的多 database 特性。

在网上找了一圈发现没有比较好的工具可以进行这个处理。

看过一个 redis-dump 号称可以导出 json 再进行导入，结果 alpha 版本的程序真心不靠谱，运行后报错：

_undefined method_ \`_select_!\` for \["x"]:Array

后来没办法只好自己研究起了 RDB。其实 RDB 也很简单，在 redis 的官方网站上有完整说明

[https://github.com/sripathikrishnan/redis-rdb-tools/wiki/Redis-RDB-Dump-File-Format](https://github.com/sripathikrishnan/redis-rdb-tools/wiki/Redis-RDB-Dump-File-Format)

通过文档了解到像我正在用的这种单 db 的 rdb，其数据结构很简单，前 9 个字节都是 REDIS 的 MAGIC STRING 和 RDB 版本号，其后跟着两个字节 FE 00 表示 0 号 db，同理如果是 1 号 db 应该是 FE 01，再之后就是这个 db 内所有数据，在文件最后以一个字节 FF 表示文件结束。

由此得出要合并 3 个 db 只需要把每个文件去掉开头 9 个字节和最后一个字节，然后把对应的 db index 改为需要合并到的 db index，最后合并到一个文件，并在开头加上之前去掉的 9 个字节，再在末尾加上一个 FF 即可完成合并。

合并完成后别忘了用 redis-check-dump 检查一下

linux 下可以用 bvi 进行二进制编辑，用法与 vi 基本一致。另外在编辑前可能需要执行一下 :set memmove 命令。

如果不想安装 bvi，只用 dd 命令也是可以做到的，只是处理大文件的时候可能会比较慢（因为 block size=1byte 了，我不知道怎么在 bs 设的较大的情况下精确控制 skip 和 count）。

dd bs=1 if=dump.rdb of=out.rdb skip=9 count={你的 dump.rdb 的文件大小 - 10，单位字节}

这样执行后生成的 out.rdb 只包含从 FEXX 开始的数据信息。如果要合并到一个库，可以去掉第一个文件的 EOF 标识，去掉第二个文件的前 9 个字节。再 cat 到一个文件即可。

注意：导出前记得用 redis-cli save 并关闭服务以保证数据一致 
 [https://www.cnblogs.com/suifengbingzhu/p/4259926.html](https://www.cnblogs.com/suifengbingzhu/p/4259926.html)
