# 再见，micro: 迁移go-micro到纯gRPC框架 - Blog | Songrgg
[micro](https://github.com/micro/micro)是基于 golang 的微服务框架，之前华尔街见闻架构升级中谈到了我们是基于 go-micro 的后端架构，随着我们对服务网格的调研、测试和实施，为了打通不同语言之间的服务调用，我们选择了 gRPC 作为服务内部的通用协议。

go-micro 框架的架构非常具有拓展性，它拥有自己的 RPC 框架，通过抽象 codec,transport,selector 等微服务组件，你既可以使用官方实现的各种插件[go-plugins](https://github.com/micro/go-plugins)进行组装，又可以根据实际的情况实现自己的组件。然而，我们打算利用服务网格的优势，将微服务的基础组件下沉到基础设施中去，将组件代码从代码库中剥离开来。

这样一来，我们相当于只需要最简的 RPC 框架，只需要服务之间有统一、稳定、高效的通信协议，由于 micro 在我们新架构中略显臃肿，于是我们选择逐渐剥除 micro。还有一个重要原因，我们选择的服务网格方案是 istio，它的代理原生支持 gRPC，而 micro 只是将 gRPC 当做 transport 层，相当于复写了 gRPC 的服务路由逻辑，这样有损于 istio 的一些特性，譬如流量监控等功能。

出于这些考虑，第一步需要将 micro 改成纯 gRPC 模式，这里的改造部分我们考虑只应该去更改基础库的代码，而尽量不要使业务代码更改，减少对已有逻辑的影响，和一些软性的譬如开发人员的工作量。

## [](#服务发现模块 "服务发现模块")服务发现模块

istio 的服务发现支持 etcd、consul 等，我们需要将其改成使用 kubernetes 的服务名进行访问。通过实现[istio selector](https://gist.github.com/songrgg/22999fb7ab76a30dcac17cb28ed412d4)的方式，我们将 RPC 调用的服务名与 k8s 的服务名端口做映射。

```go hljs
package istio

import (
	"fmt"

	"github.com/micro/go-micro/registry"
	"github.com/micro/go-micro/selector"
)

const (
	svcPort = 10088
)

var (
	serviceHostMapping = map[string]string{
		"payment": "payment.common",
		"content": "content.ns1",
		"user":    "user.ns1",
	}
)

type istio struct{}

func (r *istio) Select(service string, opts ...selector.SelectOption) (selector.Next, error) {
	if host, exist := serviceHostMapping[service]; exist {
		node := &registry.Node{
			Id:      service,
			Address: host,
			Port:    svcPort,
		}
		return func() (*registry.Node, error) {
			return node, nil
		}, nil
	}
	return nil, fmt.Errorf("service %s(%s) not found", service, svc)
}

...
```

这里注意的是由于服务之间调用需要指定 service name 和端口，所以在这里我们把端口设置了一个 magic number。

## [](#传输模块 "传输模块")传输模块

go-micro 采用各种协议作为传输报文的工具，可以从[transport](https://github.com/micro/go-plugins/tree/master/transport)中了解到，有 http/grpc/tcp/utp 等，我们曾先后使用过 tcp、utp、gRPC，经过测试 gRPC 是其中最为稳定的。之前提到过 gRPC 只是作为传输信道，micro 定义了自己的 RPC 接口，实现了 RPC 的路由、传输、重试等功能，通过自定义了 protobuf 的插件生成符合 micro 标准的 proto 文件。

为了向后兼容，在使用 grpc 替换原 RPC 时，我也需要根据 protobuf 文件生成新的 golang 代码，其中包括 client 端、server 端代码的变更，实际上我的更新是针对[protoc-gen-go](https://github.com/golang/protobuf/tree/master/protoc-gen-go)，fork 之后修改[grpc/grpc.go](https://github.com/golang/protobuf/blob/master/protoc-gen-go/grpc/grpc.go)部分，在`func generateService()`中对 Client 端代码进行 micro 的适配。

### [](#Client "Client")Client

```go hljs
func (c *someClient) DoSomething(ctx context.Context, in *SomeRequest, opts ...grpc.CallOption) (*SomeResponse, error)
```

### [](#Server "Server")Server

Server 端代码的更新除了接口的适配，有一个关注点是 micro 的接口设计是

```go hljs
DoSomething(context.Context, *SomeRequest, *SomeResponse) error
```

而 gRPC 是

```go hljs
DoSomething(context.Context, *SomeRequest) (*SomeResponse, error)
```

micro 接口设计的好处是支持服务端缓存的应用，当服务端 handler 触发时，cache interceptor 可以将 response 缓存，当缓存命中时可以将其返回。而由于 gRPC 的版本将 response 放在了返回值中，运行时无法将譬如 redis 中字符串格式的 response 解码成`SomeResponse`，而 micro 版本由于将其放在了参数位置，所以可以通过

```go hljs
json.Unmarshall([]byte(""), &SomeResponse{})
```

从字符串恢复 response。

我们的做法是不改变接口的设计，而是在自动生成的 golang 代码中，在 interceptor 运行之前将 response object 塞入 context。

```go hljs

ctx = ctx.WithValue("IstioResponseBody", &SomeResponse{})


obj := ctx.Value("IstioResponseBody")
json.Unmarshall([]byte(""), obj)
```

## [](#Interceptor模块 "Interceptor 模块")Interceptor 模块

一些 interceptor 的迁移工作，例如 logger、cache、recover 等。

## [](#注意事项 "注意事项")注意事项

1.  向后兼容  
    更新的过程开发人员实际上并不是特别感兴趣，我们 SRE 能做的是对他们基本上无改动，第一个版本不要让开发感知。虽然实际上测试时由于我们的变动，测试环境不断受到稳定性质疑。
2.  micro 和 gRPC 的 context 传递不同  
    gRPC 是基于 http2，metadata 是基于 HTTP header 传递，Client 利用`metadata.NewOutgoingContext(ctx, MD)`携带 metadata，而 Server 端利用`metadata.FromIncomingContext(ctx)`提取 metadata。而 micro 则用`context.WithValue(metaKey{}, MD)`传递。
3.  使用环境变量做一些特殊处理  
    我们通过环境变量在基础库里做了一些判断，在新老架构并存下进行一些特殊处理，譬如刚才说到的 metadata 获取逻辑。
4.  关于 proto 自动生成  
    我们通过在 CI 中配置 istio 版本镜像的编译打包逻辑和原有逻辑共存，在编译前我们运行 proto 编译工具，生成 gRPC 的 golang 文件，从而打出新的镜像。

## [](#总结 "总结")总结

micro 陪伴了我们一年多的时间，期间它的架构设计给了我们很大的弹性可以去适配一些我们的架构，可以选择很多好的开源工具，譬如 zipkin、prometheus、etcd 等，适合于刚上微服务不久的项目进行技术摸索和选择，而这一年的时间，我们的架构也逐渐明晰和稳定，我们更倾向于一个精简的基础库，并且由于在线项目的不断增加，运维成本可以通过下沉基础组件进行降低。 
 [https://songrgg.github.io/microservice/goodbye-micro/](https://songrgg.github.io/microservice/goodbye-micro/)
