---
layout: post
title: Hadoop HDFS 架构
category: Hadoop
---

* content
{:toc}

### HDFS简介

HDFS是一个分布式文件系统, 用于商业硬件平台. HDFS跟其他分布式文件系统有很多共性, 但是不同的是HDFS有以下重要特性:

* 高容错性
* 可部署于低端硬件

HDFS提供了**高吞吐量的数据访问方式**, 而且非常适合具有**大数据集**的应用使用.

HDFS放宽了POSIX的规范要求, 目的是为了**数据的流式访问**.

HDFS一开始是为了Nutch Web搜索引擎项目而创建的, 现在是Apache Hadoop的核心项目

- - -

### 设计的期望与目标

* 硬件故障 : HDFS是应用于集群的, 所以硬件故障的发生概率是很高的, 设计时是必须提前考虑
* 数据流式访问 : 流式访问可以提高数据传输的吞吐量
* 大数据集 : 基于HDFS的应用一般都是拥有大数据集的, 否则分布式没有意义
* 简单的一致性模型 : HDFS期望应用对数据的使用情况是写一次, 读多次的, 而且对数据的修改不是立刻发生的, 这样有利于提高吞吐量
* 移动计算到数据 : 基于大数据集的情况, 移动数据很耗性能, 所以要支持移动计算到数据
* 可移植性 : 可移植性提高了HDFS的使用范围

- - -

### NameNode与DataNode

* NameNode : 管理文件系统命名空间, 管理文件访问操作
* DataNode : 保存数据
* HDFS架构模型

	![hadoop-hdfs-architecture]({{ site.url }}/assets/hadoop/hadoop-hdfs-architecture.png)

- - -

### 文件系统命名空间

HDFS的命名空间支持传统的文件组织结构(也就是树形层次结构)

HDFS的命名空间是由NameNode管理的

- - -

### 数据的重复因子

* 副本放置 : 理想条件下, 我们会把副本放在相互远离的地方; 但是考虑到性能问题, 一般情况下把2个副本放在同一个`rack`, 1个副本放在另一个`rack`(假设重复因子为3)
* 副本读取 : 从性能角度出发, HDFS会优先选择离Client最近的副本为Client提供服务
* Safemode模式 : 该模式是NameNode刚开启时的模式, 这个模式下的NameNode会接受Heartbeat和Blockreport, 收集完Blockreport之后就会退出该模式. 退出该模式之后如果发现有数据块的重复因子小于设定的下限, 那么NameNode就负责复制数据块直到满足条件为止.

> safemode模式下是没有副本相关信息的

- - -

### metadata持久化

HDFS的metadata持久化通过2种文件(都是本地保存)来实现:

* FsImage : 保存了数据块的映射信息, 文件系统的属性
* EditLog : 事务日志, 记录了所有的HDFS操作记录

checkpoint机制: 当NameNode启动的时候, 首先读取FsImage和EditLog进内存, 然后根据EditLog修改FsImage, 再把FsImage更新到磁盘上, 最后对EditLog进行缩减

> checkpoint机制只有在NameNode启动时才会触发(目前)

- - -

### 通信协议

HDFS的通信协议都是基于TCP/IP协议的. 主要有2种协议:

* ClientProtocol : 用于Client和NameNode之间的通信
* DataNodeProtocol : 用于DataNode和NameNode之间的通信

> 这2种协议都通过`RPC`进行了抽象封装

- - -

### 健壮性

健壮性是HDFS的一个重要特性. HDFS为了健壮性作出了下面这些保证:

* 重新复制副本 : NameNode会时常检查各个数据块的重复因子, 保证重复因子达到预设状态
* 集群负载重平衡 : HDFS兼通数据重平衡方案
* 数据完整性 : Client端通过DataNode获取文件之后会通过checksum检查数据的完整性
* metadata失效恢复 : NameNode保留了多份FsImage和EditLog以便恢复数据
* 数据快照 : 数据快照就是某一时刻数据的副本, 可用于回滚操作

> 重复因子变化的原因多种多样 : DataNode无效, 副本失效, 硬盘故障, 重复因子变大

- - -

### 数据的组织

* 数据块 : 基于HDFS的应用一般都是拥有大数据集的, 并且通常对数据的操作是写一次, 读多次的. 所以为了提高数据传输的吞吐量, 数据以数据块的方式保留, 而且以流式方式传输.
* 分段运输 : Client如果要写入数据到DataNode. Client首先会把数据写入自己的缓冲区中, 如果缓冲区的数据达到数据块的大小, 那么Client就会向NameNode发送写入数据的请求, NameNode进行metadata的修改之后返回DataNode的id, 让Client与对应的DataNode连接并传输数据. 数据传输完成后Client通知NameNode关闭文件, 最后NameNode进行事务日志的写入, 到此操作完成. 如果日志写入失败, 那么此次操作也是失效的.
* 副本管道流 : 因为副本的原因, Client在写入数据到DataNode的时候是要写入很多份的, HDFS的处理机制则是以副本管道流的形式. Client每传输一部分数据, 第一个DataNode就会写入数据并把数据传递给第二个DataNode, 第二个DataNode也是和第一个DataNode一样, 如此推进就形成了管道流.

> HDFS的数据块大小一般是64MB, 而且NameNode尽量把数据块放在不同的DataNode

- - -

### 访问方式

HDFS的访问方式有以下3种:

* FS Shell : 类似于bash的命令行方式
* DFSAdmin : 给HDFS的administrator使用的命令行方式
* Browser Interface : 借助浏览器的http协议进行访问

- - -

### 空间回收

* 文件删除 : Client在删除文件的命令发出后, HDFS的释放空间动作并没有同步执行, 具有一定的时间差. 被删除的文件会保存在`/user/<username>/.Trash`目录中. 在这段时间差内, 数据是可恢复的. 真正释放空间的时机是由checkpoint决定的.
* 副本因子降低 : 如果数据块的副本因子变小了, 那么NameNode就会发送心跳包给对应的DataNode, 告诉它把副本删除. 同样, 删除过程也是会有时间差.

> 默认情况下Trash机制是关闭的, 需要配置才能开启