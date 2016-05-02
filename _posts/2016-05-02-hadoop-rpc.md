---
layout: post
title: Hadoop RPC 原理与实例
category: Hadoop
---

* content
{:toc}

### RPC简介

摘自维基百科:

> 远程过程调用（英语：Remote Procedure Call，缩写为 RPC）是一个计算机通信协议。该协议允许运行于一台计算机的程序调用另一台计算机的子程序，而程序员无需额外地为这个交互作用编程.

### 原理

#### 远程代理

RPC的实现架构一般是远程代理模式, 如下图所示:

![hadoop-rpc-architecture]({{ site.url }}/assets/hadoop/hadoop-rpc-architecture.png)

这些类的工作如下:

* Client: 客户类, 调用远程方法的地方
* Proxy: 远程代理接口, 提供远程方法接口, 隐藏了与远程服务器的交互细节
* ProxyImpl: 远程代理接口实现类, 实现远程方法接口
* Server: 服务器类, 注册了远程接口, 启动服务, 提供远程方法调用服务

> 有关代理模式的探索可以参考我的另一篇博文: [代理(Proxy)模式](http://schstudio.github.io./2016/05/01/design-pattern-proxy/)

#### 请求响应流程

当客户类调用远程方法时, 远程代理类做了一些不为人知的事情:

1. 序列化调用参数
2. 连接RPC服务器
3. 通知RPC服务器执行对应的远程方法

接下来, 服务器也相应做出一系列动作:

1. 反序列化调用参数
2. 根据调用参数执行远程方法
3. 序列化返回值
4. 把返回值响应给远程代理类

#### RPC协议实现

其整体的工作原理就是上面所说的交互过程, 接下来我们继续深入学习一下Hadoop RPC协议的实现.

根据上面的请求响应流程我们能够知道RPC远程调用必须做两件事:

1. 序列化和反序列化Java对象
2. 根据RPC协议传输序列化数据

序列化和反序列化这里就不详细说明了, 我们来看一下RPC协议的实现流程.

通过查阅文档知道协议定义在`org.apache.hadoop.ipc.Client`和 `org.apache.hadoop.ipc.Server`这两个类中.

客户调用远程方法的时候会从`Client.call`调用开始. 一开始创建连接并开始RPC握手, 其头部如下表所示

    +----------------------------------+
    |  "hrpc" 4 bytes                  |
    +----------------------------------+
    |  Version (1 byte)                |
    +----------------------------------+
    |  Service Class (1 byte)          |
    +----------------------------------+
    |  AuthProtocol (1 byte)           |
    +----------------------------------+

* "hrpc": 这是一个固定的字符串, 表示hadoop rpc
* Version: 这一个是RPC协议版本, 目前是9
* Service Class: RPC中的服务类编号
* AuthProtocol: RPC调用之前是否使用SASL认证

接下来设置通信环境, 通信环境定义了**目标RPC协议名称**(下面的实例中的`protocolName="ping"`)和**请求用户是谁**.

举例:

    //h   r     p     c
    0x68, 0x72, 0x70, 0x63,
    // version, service class, AuthProtocol
    0x09, 0x00, 0x00,
    // size of next two size delimited protobuf objets:
    // RpcRequestHeader and IpcConnectionContext
    0x00, 0x00, 0x00, 0x32, // = 50
    // varint encoding of RpcRequestHeader length 
    0x1e, // = 30
    0x08, 0x02, 0x10, 0x00, 0x18, 0xfd, 0xff, 0xff, 0xff, 0x0f,
    0x22, 0x10, 0x87, 0xeb, 0x86, 0xd4, 0x9c, 0x95, 0x4c, 0x15,
    0x8a, 0xb0, 0xd7, 0xbc, 0x2e, 0xca, 0xca, 0x37, 0x28, 0x01,
    // varint encoding of IpcConnectionContext length 
    0x12, // = 18
    0x12, 0x0a, 0x0a, 0x08, 0x65, 0x6c, 0x65, 0x69, 0x62, 0x6f,
    0x76, 0x69, 0x1a, 0x04, 0x70, 0x69, 0x6e, 0x67,

RPC握手头部发送之后, 客户类就可以发送请求了, 如果需要SASL认证则先认证, 这里不讨论.

请求报文的格式如下所示:

    +------------------------------------------+
    |  length(rpcRequestheader, rpcRequest)    |
    +------------------------------------------+
    |  RpcRequestHeader                        |
    +------------------------------------------+
    |  RpcRequest                              |
    +------------------------------------------+

例如: 对应下面的[实例](#section-3)

    // Size of size delimited
    // RpcRequestHeader + RpcRequest protobuf objects
    0x00, 0x00, 0x00, 0x3f, // = 63
    // varint size of RpcRequest Header
    0x1a, // = 26
    0x08, 0x01, 0x10, 0x00, 0x18, 0x00, 0x22, 0x10, 0x87, 0xeb,
    0x86, 0xd4, 0x9c, 0x95, 0x4c, 0x15, 0x8a, 0xb0, 0xd7, 0xbc,
    0x2e, 0xca, 0xca, 0x37, 0x28, 0x00,
    // RPC Request writable. It's not size delimited
    // long - RPC version
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x02, // = 2
    // utf8 string - protocol name
    0x00, 0x04, // string legnth = 4
    // p     i     n     g
    0x70, 0x69, 0x6e, 0x67,
    // utf8 string - method name
    0x00, 0x04, // string legnth = 4
    // p     i     n     g
    0x70, 0x69, 0x6e, 0x67,
    // long - client version
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x01, // = 1
    // int - client method hash
    0xa0, 0xbd, 0x17, 0xcc,
    // int - parameter class length
    0x00, 0x00, 0x00, 0x00,

响应报文格式如下所示:

    +------------------------------------------+
    |  Message Length                          |
    +------------------------------------------+
    |  length of RpcResponseHeaderProto        |
    +------------------------------------------+
    |  RpcResponseHeader                       |
    +------------------------------------------+
    |  Writable response value                 |
    +------------------------------------------+

例如: 对应下面的[实例](#section-3)

    // size of entire request
    0x00, 0x00, 0x00, 0x33,
    // varint size of RpcResponseHeader
    0x1a, // 16 + 10 = 26
    0x08, 0x00, 0x10, 0x00, 0x18, 0x09, 0x3a, 0x10, 0x9b, 0x19,
    0x9b, 0x41, 0x4d, 0x86, 0x42, 0xd7, 0x94, 0x79, 0x3f, 0x4b,
    0x16, 0xa0, 0x22, 0x7c, 0x40, 0x00,
    // Writable response
    // short - length of declared class
    0x00, 0x10,
    // j     a     v     a     .     l     a     n     g     .
    0x6a, 0x61, 0x76, 0x61, 0x2e, 0x6c, 0x61, 0x6e, 0x67, 0x2e,
    // S     t     r     i     n     g 
    0x53, 0x74, 0x72, 0x69, 0x6e, 0x67,
    // short - length of value
    0x00, 0x04,
    // p     o     n     g
    0x70, 0x6f, 0x6e, 0x67,

### 实例

现在我们搭建一个简单的例子:

RPCServer注册了一个远程接口`ping()`, 然后RPCClient调用该远程接口获取信息.

给出下面的架构图, 可以帮助理解整个应用流程

#### 架构

![hadoop-rpc-architecture-example]({{ site.url }}/assets/hadoop/hadoop-rpc-architecture-example.png)

工作流程如下:

1. RPCServer注册远程调用接口`PingRPC`和实现类`PingRPCImpl`
2. RPCServer启动服务
3. RPCClient获取远程代理接口`PingRPC`
4. RPCClient调用远程接口`PingRPC.ping()`

#### 代码

* PingRPC

        import org.apache.hadoop.ipc.ProtocolInfo;

        @ProtocolInfo(protocolName = "ping", protocolVersion = 2)
        public interface PingRPC {
            String ping();
        }

* PingRPCImpl

        public class PingRPCImpl implements PingRPC {

            @Override
            public String ping() {
                System.out.println("Server get request");
                return "pong";
            }

        }

* RPCServer

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.ipc.RPC;
        import org.apache.hadoop.ipc.RPC.Server;

        public class RPCServer {

            public static void main(String[] args) throws Exception {
                Server server = new RPC.Builder(new Configuration())
                                        .setBindAddress("localhost")
                                        .setPort(8888)
                                        .setInstance(new PingRPCImpl())
                                        .setProtocol(PingRPC.class)
                                        .build();
                server.start();
            }
        }
        // 接受到请求会输出: 
        // Server get request

* RPCClient

        import java.io.IOException;
        import java.net.InetSocketAddress;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.ipc.RPC;

        public class RPCClient {

            public static void main(String[] args) throws IOException {
                PingRPC proxy = RPC.getProxy(PingRPC.class, 
                        RPC.getProtocolVersion(PingRPC.class), 
                        new InetSocketAddress("localhost", 8888),
                        new Configuration());
                String response = proxy.ping();
                System.out.println(response);
            }
        }
        // 请求结果:
        // pong