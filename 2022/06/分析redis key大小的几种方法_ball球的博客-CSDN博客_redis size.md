# 分析redis key大小的几种方法_ball球的博客-CSDN博客_redis size
当 redis 被用作缓存时，有时我们希望了解 key 的大小分布，或者想知道哪些 key 占的空间比较大。本文提供了几种方法。

## 一. bigKeys

这是 redis-cli 自带的一个命令。对整个 redis 进行扫描，寻找较大的 key。例：

```
redis-cli -h b.redis -p 1959 --bigkeys

```

输出：

```
# Scanning the entire keyspace to find biggest keys as well as
# average sizes per key type.  You can use -i 0.1 to sleep 0.1 sec
# per 100 SCAN commands (not usually needed).

[00.00%] Biggest hash   found so far 's_9329222' with 3 fields
[00.00%] Biggest string found so far 'url_http://mini.eastday.com/mobile/170722090206890.html?qid=sgllq&ch=east_sogou_push&pushid=13' with 8 bytes
[00.00%] Biggest string found so far 'foo' with 40 bytes
[00.00%] Biggest hash   found so far 's_9329084' with 4 fields
[00.23%] Biggest zset   found so far 'region_hot_菏泽地' with 625 members
[00.23%] Biggest zset   found so far 'region_hot_葫芦岛' with 914 members
[00.47%] Biggest string found so far 'top_notice_list' with 135193 bytes
[00.73%] Biggest zset   found so far 'region_hot_自贡' with 2092 members
[01.90%] Biggest hash   found so far 'uno_facet_2018-12-20' with 59 fields
[11.87%] Biggest zset   found so far 'region_hot_上海' with 2233 members
[27.05%] Biggest set    found so far 'blacklist_set_key' with 31832 members
[73.87%] Biggest string found so far 'PUSH_NEWS' with 3104237 bytes
[86.18%] Biggest zset   found so far 'region_hot_北京' with 2688 members

-------- summary -------

Sampled 4263 keys in the keyspace!
Total key length in bytes is 174847 (avg len 41.02)

Biggest string found 'PUSH_NEWS' has 3104237 bytes
Biggest    set found 'blacklist_set_key' has 31832 members
Biggest   hash found 'uno_facet_2018-12-20' has 59 fields
Biggest   zset found 'region_hot_北京' has 2688 members

1616 strings with 3771161 bytes (37.91% of keys, avg size 2333.64)
0 lists with 0 items (00.00% of keys, avg size 0.00)
1 sets with 31832 members (00.02% of keys, avg size 31832.00)
2353 hashs with 7792 fields (55.20% of keys, avg size 3.31)
293 zsets with 333670 members (06.87% of keys, avg size 1138.81)

```

说明：

1.  该命令使用 scan 方式对 key 进行统计，所以使用时无需担心对 redis 造成阻塞。
2.  输出大概分为两部分，summary 之上的部分，只是显示了扫描的过程。summary 部分给出了每种数据结构中最大的 Key。
3.  统计出的最大 key 只有 string 类型是以字节长度为衡量标准的。list,set,zset 等都是以元素个数作为衡量标准，不能说明其占的内存就一定多。所以，如果你的 Key 主要以 string 类型存在，这种方法就比较适合。

更多关于 bigkeys 的说明可以参考[这里](https://redis.io/topics/rediscli#scanning-for-big-keys)。

## 二. debug object key

redis 的命令，可以查看某个 key 序列化后的长度。  
例：

```
连接上redis后执行如下命令

b.redis:1959> hmset myhash k1 v1 k2 v2 k3 v3
OK
b.redis:1959> debug object myhash
Value at:0x7f005c6920a0 refcount:1 encoding:ziplist serializedlength:36 lru:3341677 lru_seconds_idle:2

```

关于输出的项的说明：

-   Value at：key 的内存地址
-   refcount：引用次数
-   encoding：编码类型
-   serializedlength：序列化长度
-   lru_seconds_idle：空闲时间  
    关于 refcount, encoding, lru_seconds_idle 的更详细解释可以[参考这里](https://blog.csdn.net/qmhball/article/details/85342007)。

#### 几个需要注意的问题

-   serializedlength 是 key 序列化后的长度 (redis 在将 key 保存为 rdb 文件时使用了该算法)，并不是 key 在内存中的真正长度。这就像一个数组在 json_encode 后的长度与其在内存中的真正长度并不相同。不过，它侧面反应了一个 key 的长度，可以用于比较两个 key 的大小。
-   serializedlength 会对字串做一些可能的压缩。如果有些字串的压缩比特别高，那么在比较时会出现问题。比如下列：


```
b.redis:1959> set str1 aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
OK
b.redis:1959> set str2 abcdefghijklmnopqrstuvwxyz1234
OK
b.redis:1959> debug object str1
Value at:0x7f007c035b80 refcount:1 encoding:embstr serializedlength:12 lru:3342615 lru_seconds_idle:13
b.redis:1959> debug object str2
Value at:0x7f00654df400 refcount:1 encoding:embstr serializedlength:31 lru:3342622 lru_seconds_idle:7

```

两个字串的实际长度都是 30, 但 str1 的 serializedlength 为 12， str2 的为 31。

-   redis 的[官方文档](https://redis.io/commands/debug-object)不是特别建议在客户端使用该命令，可能因为计算 serializedlength 的代价相对高。所以如果要统计的 key 比较多，就不适合这种方法。

## 三. redis rdb tools

这是一个 redis rdb file 的分析工具，可以根据 rdb file 生成内存报告。

### 3.1 安装

需要 python2.4 以上版本和 pip。

```
pip install rdbtools

```

### 3.2 生成内存报告

首先我们需要有一份 rdb 文件，如果你在配置中开启了 rdb，那么 redis 会自动生成 rdb 文件。如果没有，可以手动执行 bgsave。如果是线上机器，执行时要考虑机器负载等问题。拿到 rdb 文件后，我们就可以生成内存报告了。命令如下：

```
rdb -c memory file 

```

例：

```
rdb -c memory /tmp/dump.rdb 
database,type,key,size_in_bytes,encoding,num_elements,len_largest_element,expiry
0,hash,data:index_flow_yingshi,10492,hashtable,1,8992,2019-01-14T08:20:10.236000
0,hash,data:index_movie,22068,hashtable,7,2896,2019-01-14T07:29:19.685000
0,string,block:index_module_novel,8296,string,7694,7694,2019-01-13T00:27:46.128000
0,string,block:index_bottom_baike_aikan,8296,string,7632,7632,2019-01-14T02:27:11.850000
0,string,block:index_bottom_tools,5224,string,4549,4549,2019-01-13T01:02:09.171000
0,string,block:index_module_travel,7272,string,6408,6408,2019-01-13T00:43:39.478000
...

```

输出了 db，数据类型，key, 大小, 编码等多列信息。至于分析数据，你可以用 shell，也可以保存成 csv 用 excel 排序，或者干脆存到 db 里，想怎么排怎么排。

如果只要知道最大的 N 个 key, 可以使用 - l 选项。例：

```
[@sjs_73_171 ~]$ rdb -c memory -l 3 /tmp/dump.rdb  
database,type,key,size_in_bytes,encoding,num_elements,len_largest_element,expiry
0,hash,city_tong,724236,hashtable,3113,216,2019-01-14T01:10:59.407000
0,hash,iplocsearch,406292,hashtable,383,180190,2019-01-30T05:37:56.082000
0,hash,weather_tong3,583844,hashtable,319,1658,2019-01-07T10:22:33.742000

```

### 3.3 查看单个 key

如果我们只需要查询单个 key 所使用的内存可以不必依赖 rdb file, 使用 redis-memory-for-key 命令即可。  
例：

```
[@sjs_73_171 WEB-INF]$ redis-memory-for-key -s b.redis -p 1959 myhash
Key                             myhash
Bytes                           83
Type                            hash
Encoding                        ziplist
Number of Elements              3
Length of Largest Element       2

[@sjs_73_171 WEB-INF]$ redis-memory-for-key -s b.redis -p 1959 str1
Key                             str1
Bytes                           80
Type                            string

[@sjs_73_171 WEB-INF]$ redis-memory-for-key -s b.redis -p 1959 str2
Key                             str2
Bytes                           80
Type                            string

```

### 3.4 更多

-   工具得出的内存值为近似值，这点可以参看[作者的说明](https://github.com/sripathikrishnan/redis-rdb-tools/wiki/FAQs)。“Why doesn’t reported memory match actual memory used?”
-   工具通过分析 rdb file 中的 key 及 value，反算出该 kv 在内存中的大小。计算时充分考虑了数据类型的影响，key 本身长度的影响，内存分配等多种因素。虽然得出的大小不是真实值，但用于 key 大小的比较是完全可以的。
-   rdb 的功能不仅于此，它还可以将 kv 导成 json 格式，也可以按正则表达式只导出部分 key，  
    更多使用方法可以查看


```
rdb --help

```

也可以查看 git 上的[帮助文档](https://github.com/sripathikrishnan/redis-rdb-tools)。

## 四. 总结

-   如果想粗略的看下最大 key, 可以使用 bigKeys。
-   如果查询的 key 不多，key 的压缩比又没有明显差异，可以使用 debug object key。
-   如果不介意安装个工具，那么 redis rdb tools 似乎是最佳选择。 
    [https://blog.csdn.net/qmhball/article/details/86063466](https://blog.csdn.net/qmhball/article/details/86063466)
