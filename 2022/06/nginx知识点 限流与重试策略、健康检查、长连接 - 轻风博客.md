# nginx知识点:限流与重试策略、健康检查、长连接 - 轻风博客
记录日常工作中遇到的坑\~~

### ngx_http_limit_req_module 限流

nginx 做为网关，限流能力是一项必备的能力，在面对突发大流量时，为保护后端服务和 DB 不被突然出现的流量洪峰冲垮, 对请求实施限流，可能是一项有效的止血方案。限流对业务是有损的，因为它意味着丢弃部分请求，而只允许请求以允许的速率请求到后端，所以，通常情况下，限流只做为一项应急处置手段来使用。

限流通常会和降级配合使用，在微服务中这一块现在比较热，不过 nginx 做为网关层，会简单粗暴得多，nginx 的限流通常直接返回特定的状态码，比如 429，没有优雅的降级，降级则由客户端来处理了。

ngx_http_limt_req_module 的文档地址：[http://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req](http://nginx.org/en/docs/http/ngx_http_limit_req_module.html#limit_req)

使用比较简单，但也有需要注意的坑。

##### 对 uri 实施限流

官方文档中的配置示例是对特定来源的 ip 进行访问限流的

limit_req_zone $binary_remote_addr zone=perip:10m rate=1r/s;

在实际使用中，往往不会对来源 ip 进行限速，一个是若有多层网关，来源的 ip 并不能代表用户的真实 ip，另外，就是也不太适合精确控制应用的总请求速率，所以，采用**$uri**来做为限速。

##### 重要的 burst

使用 limit_req 限速时，一定要加上 burst 参数，否则会产生误差，实际值会低于目标值，且没有规率性

如果是不使用 burst 时的限流效果：

![](http://pdf.us/wp-content/uploads/2021/08/Dingtalk_20210827134746.jpg)

可以看到，实际的限流效果会有较大的偏差，并且，随着限流流量的加大，偏差也越来越大。解决办法就是需要使用 burst 参数，在网上找到一段话，大致说出了原因：

`Yes, the module is limited by millisecond timer resolution, but using so high rate values without the burst parameter is meaningless.`

上面这段话来自：[https://www.ruby-forum.com/t/possible-limitation-of-ngx-http-limit-req-module/241917](https://www.ruby-forum.com/t/possible-limitation-of-ngx-http-limit-req-module/241917)

##### 配置示例

\|  \| 

\# http 区块

limit_req_zone  $uri  zone\\=limit_to_0:5m  rate\\=1r/m;

limit_req_zone  $uri  zone\\=limit_to_0.2k:5m  rate\\=200r/s;

limit_req_zone  $uri  zone\\=limit_to_1k:5m  rate\\=1000r/s;

\# server 或 location 区块

\# 在需要限速的 location 下添加以下配置 # nodelay

limit_req zone\\=limit_to_0.2k  burst\\=8;

limit_req_status  429;

 \|

nginx ngx_http_limit_req_module 讲解  
[https://www.freecodecamp.org/news/nginx-rate-limiting-in-a-nutshell-128fe9e0126c/](https://www.freecodecamp.org/news/nginx-rate-limiting-in-a-nutshell-128fe9e0126c/)  
[https://www.nginx.com/blog/rate-limiting-nginx/](https://www.nginx.com/blog/rate-limiting-nginx/)

### proxy_next_upstream 重试机制

ngx_http_proxy_module 是 nginx 的核心模块，里面有个指令是 proxy_next_upstream 一般如果没有显式配置过的话，它会有一个默认值：

proxy_next_upstream error timeout;

这个指令的意思是，在发生连接错误 (建立连接、传递请求或者在读响应头) 时，或者在后端超时时，会发起重试请求。

这个默认行为可能并不是我们期望的，特别是超时重试这个，在遇到比较慢的后端时，会造成同一个请求多次请求到后端，这就会有两个后果，一个是如果接口不是幂等的，会产生脏数据，影响到数据的准确性；产生超时一般就是后端响应太慢，间接说明该请求非常消耗资源，这时，在第一个请求还没有计算出结果时，又重复发起第二个相同的请求，势必造成后端压力，在极端情况下，因为请求放大，甚至后端会被打挂。

所以，必须关闭超时时产生的重试，发生错误时产生的重试往往可以保留，它有一定的益处。还需要注意一点，就是，如果是多级代理，在设置每一层的超时时间时，必须要有梯度，如果都设置成相同的超时时间，如 600 秒 (proxy_read_timeout 600;)，那么必然会产生重试。

建议的配置：

\|  \| 

\# http 区块

proxy_next_upstream error;

 \|

既然网关重试不是我们所希望的结果，那么如何从日志中发现当前网关是否发生过重试呢？  
实际上，如下日志片段 ，upstream_addr 中会出现多个后端 ip(一个请求的最大重试次数小于等于 upstream 中的 server 数)，request_time 异常大，upstream_response_time 中也会有每次重试的响应时间

\# 发生重试的日志片段  
"upstream_addr":"172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80, 172.16.xx.xxx:80",  
"request_time":"5100.769",  
"upstream_response_time":"300.001, 300.000, 300.000, 300.000, 300.000, 300.000, 300.000, 300.000, 300.000, 300.000, 300.000, 300.000, 300.000, 300.000, 300.000, 300.000, 300.000, 0.769",

### upstream server 的主动健康检查和被动健康检查

\|  \| 

upstream  test  {

server  127.0.0.1:1234  weight\\=2  max_fails\\=2  fail_timeout\\=4;

}

 \|

在配置 upstream 时，server 里面有一个参数 max_fails 用于控制**被动健康检查**，max_fails 的默认值是 1，即被动健康检查默认是开启的。如果需要关闭，需要显试的设置其值为 0。

当在 fail_timeout（默认为 10 秒）时间内，如果标识 server unavailable 的请求次数达到 max_fails 的值，那么后端服务会暂时从 upstream 中下掉，即会下线服务节点。而被认可为 fails 的请求是由 proxy_next_upstream 这类指令配置的，proxy_next_upstream 默认是 error 和 timeout，注意这个 timeout，也就是说，在后端某个接口响应非常慢的情况下，会会导致该后端 server 从 upstream 中下线，情况严重一点会使整个 upstream 中服务器全部下掉！这就非常严重了，相当于可以由一个特别慢的接口拉挂整个服务。如果这个服务是单个服务还好，如果 upstream 里是下一层网关，那后果就更加严重，相当于整个代理全挂。

所以可靠的作法就是关闭被动健康检查，即设置 max_fails=0，但这又会导致某个节点异常下线，无法被 nginx 感知到，那么解法是再配合**主动健康检查**

主动健康检查就是周期性的由 nginx 发起探针请求到后端，通过请求结果判断后端是否是存活的，这需要用到模块 ngx_http_upstream_check_module [文档在此](http://tengine.taobao.org/document/http_upstream_check.html)  
这是 tengine 里面的一个模块，还是很有用的，语法如下：

\|  \| 

upstream  cluster1  {

\# simple round-robin

server  192.168.0.1:80;

server  192.168.0.2:80;

check interval\\=3000  rise\\=2  fall\\=5  timeout\\=1000  type\\=http;

check_http_send  "HEAD / HTTP/1.0\\r\\n\\r\\n";

check_http_expect_alive http_2xx http_3xx;

}

 \|

### keepalive 后端长连接

最后说一下 keepalive，后端服务使用长连接可以减少频繁创建连接，增加性能，但需要注意，长链接需要相应的在 location 中配置 proxy_http_version 1.1; 和 proxy_set_header Connection ""; 才会生效，而如果有 10 个 location 是到这个 upstream 的，必须这 10 个 location 全部配置才有用，因为长连接会被打断，即在连接的复用过程中，只要有一个请求来至于没有配置前面那两个指令，这个请求结果后，这个长连接也就被释放了，通过抓包可以很清楚的看到这一点。其他倒还好

转载请注明：[轻风博客](https://pdf.us/) » [nginx 知识点: 限流与重试策略、健康检查、长连接](https://pdf.us/2021/08/27/4158.html) 
 [https://pdf.us/2021/08/27/4158.html](https://pdf.us/2021/08/27/4158.html)
