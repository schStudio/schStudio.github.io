---
layout: post
title: Hadoop HDFS 用户手册
category: Hadoop
---

* content
{:toc}

### HDFS 概述

* HDFS是高可配置的, 而且默认配置能够满足很多需求
* HDFS是基于Java平台编写的并且支持各种主流平台
* HDFS命令操作是通过类似shell的交互平台完成的
* HDFS支持Web界面查看有关信息, 默认地址是: `http://namenode-name:50070/`

- - -

### HDFS 管理

![hadoop-hdfs-web-interface]({{ site.url }}/assets/hadoop/hadoop-hdfs-web-interface.png)

- - -

### HDFS 命令操作

HDFS的命令操作类似shell, 但是分为普通命令和管理员命令

以下简单举例DFSAdmin的一些常用指令: `hdfs dfsadmin [options]`

| options | 说明 |
|--------|--------|
|`-report`|查看HDFS的相关信息|
|`-safemode`|进入安全模式|
|`-finalizeUpgrade`|删除更新|
|`-refreshNodes`|更新节点, 由`dfs.hosts`和`dfs.hosts.exclude`指定|
|`-printTopology`|打印输出HDFS的拓扑图|

> 普通指令查看: `hdfs dfs -help <command>`

- - -

### Secondary NameNode

Secondary NameNode的功能是**周期性地同步FsImage和Editlog**, 目的是为了**防止EditLog过于庞大**.

FsInage和EditLog的同步配置参数:

* `dfs.namenode.checkpoint.period` : 同步周期时间, 默认是`1`小时
* `dfs.namenode.checkpoint.txns` : 同步触发条件, 默认是`1百万`个unckechpoint

> * EditLog过于庞大的原因是NameNode只有在开启的时候才会更换使用新的EditLog
> * Secondary NameNode的FsImage和EditLog和Primary NameNode是同样的组织方式, 所以Primary NameNode可以随时的获取Secondary NameNode的数据

- - -

### CheckPoint Node

Checkpoint Node的功能和Secondary NameNode的功能是一样的.

Checkpoint Node的访问地址设置参数:

* `dfs.namenode.backup.address`
* `dfs.namenode.backup.http-address`

- - -

### Backup Node

Backup Node的功能和Checkpoint Node的功能是一样的. 不过还有一个主要的功能: **保存namespace的副本, 并且不断更新**. 保存了最新的namespace就可以供Primary NameNode和Secondary NameNode获取, 方便数据恢复

- - -

### 导入Checkpoint

NameNode导入Checkpoint的3个步骤:

1. 指定源目录`dfs.namenode.name.dir`
2. 指定目的目录`dfs.namenode.checkpoint.dir`
3. 加上参数`-importCheckpoint`启动NameNode

- - -

### Balancer

HDFS的Balancer有以下的策略和考虑因素:

* 写入数据到DataNode的时候, 在该节点同时保存一个副本
* 至少存在副本保存在不同的rack, 防止整个rack崩溃数据全部丢失
* 副本尽量保存在同一个rack, 减少网络资源消耗
* NameNode需要均匀分配数据到集群中的DataNode

- - -

### Rack

Rack是HDFS进行数据分配和副本分配考虑的单位, 同一个Rack里面的网络传输速度很快, 跨越Rack传输数据比较消耗网络资源.

HDFS通过设置`net.topology.script.file.name`来指定每个node的归属Rack

- - -

### 安全模式

HDFS安全模式是NameNode刚启动时进入的模式, 在该模式下NameNode会接受各个DataNode发送的心跳包和数据块报告, 获取到这些数据之后NameNode才可以正常工作.

除了NameNode启动时进入该模式, 还可以通过命令进入安全模式: `hdfs dfsadmin -safemode`

> 在安全模式下的HDFS是只读的

- - -

### fsck

fsck的作用是**检查数据的一致性**. 与Linux的fsck类似, 不过该命令不会修正检查到的错误, 而是由NameNode自动负责修复

* 通过指令`hdfs fsck`执行fsck操作
* fsck默认情况下忽略已经打开的文件, 不过也可以修改配置参数开启

- - -

### fetchdt

fetchdt的作用是**获取Delegation Token**并保存到本地.

* 通过指令`hdfs fetchdt <DTfile>`
* `<DTfile>`的保存目录通过`HADOOP_TOKEN_FILE_LOCATION`指定

token是非安全环境访问安全环境(如NameNode)的手段. token可以通过`RPC`和`HTTPS`获取

- - -

### 恢复模式

恢复模式下NameNode可以恢复数据.

* 指令`hdfs namenode -recover`开启恢复模式

- - -

### 更新和回滚操作

更新的步骤如下:

1. 更新之前需要删除backup, 可以通过`hdfs dfsadmin -upgradeProgress`查看
2. 停止集群并分发新版本Hadoop
3. 开始更新...(`sbin/start-dfs.sh -upgrade`)
4. 如果更新后正常, 那么可以删除更新
5. 如果需要回滚:
	* 停止集群并分发旧版本的Hadoop
	* 开始回滚...(`bin/hdfs namenode -rollback`)
	* 回滚完成后开启集群(`sbin/start-dfs.sh -rollback`)

更新的时候注意要重命名或者删除保留路径, 有2种方式:

* 回滚并且删除或重命名保留路径
* 更新的时候加上参数: `-upgrade -renameReversed [k-v pairs]`

> 更新之前推荐先执行`hdfs -saveNamespace`, 防止之后Editlog应用到了之前的FsImage.

- - -

### DataNode磁盘热交换

磁盘热交换的意思是HDFS不需要关闭就可以加入新的磁盘, 替换旧的磁盘.

更换步骤如下:

1. format和mount新的磁盘
2. 更新`dfs.datanode.data.dir`, 指定新的存储目录
3. 重启配置`hdfs dfsadmin -reconfig datanode HOST:PORT`
4. 完成之后umount和移除旧的磁盘

- - -

### 文件权限和安全

HDFS拥有和Linux一样的文件权限.

> 未来的HDFS将会支持网络认证协议(例如Kerberos), 用于用户认证和数据加密传输

- - -

### 扩展性

扩展性是HDFS的一个重要特性, 不过由于目前的HDFS是单独一个NameNode的, 我们知道NameNode是管理namespace的地方, 而且namespace是在NameNode的内存上的, 所以目前**内存的大小是NameNode扩展性的限制因素**.