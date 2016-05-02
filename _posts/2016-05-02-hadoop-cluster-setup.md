---
layout: post
title: Hadoop 集群安装
category: Hadoop
---

* content
{:toc}

### 安装简介

Hadoop集群的安装, 实际上就是解压软件到所有的机器或者通过包管理系统安装到所有的机器, 然后开始配置每个机器的职责.

集群上的机器可以分为两种类型: master(主), slave(从)

* master: NameNode, ResourceManager
* slave: DataNode, NodeManager

除此之外还有一些服务: Web App Proxy Server, MapReduce Job History, 这些都是用专门的机器来提供服务.

> 一般情况下, NameNode和ResourceManager属于不同的机器, DataNode和NodeManager由同一机器负责.

### 配置非安全模式的Hadoop

Hadoop的Java配置文件主要有两种类型: 只读配置文件, 地点配置文件

* 只读配置文件:

        core-default.xml
        hdfs-default.xml
        yarn-default.xml
        mapred-default.xml

* 地点配置文件

        core-site.xml
        hdfs-site.xml
        yarn-site.xml
        mapred-site.xml

* Hadoop脚本文件:

        hadoop-env.sh
        yarn-env.sh
        mapred-env.sh

接下来看一下这些配置文件的详细设置参数:

#### Daemons环境变量设置

|Daemon进程|环境变量|
|--------|--------|
|NameNode|HADOOP_NAMENODE_OPTS|
|DataNode|HADOOP_DATANODE_OPTS|
|Secondary NameNode|HADOOP_SECONDARYNAMENODE_OPTS|
|ResourceManager|YARN_RESOURCEMANAGER_OPTS|
|NodeManager|YARN_NODEMANAGER_OPTS|
|WebAppProxy|YARN_PROXYSERVER_OPTS|
|Map Reduce Job History Server|HADOOP_JOB_HISTORYSERVER_OPTS|

例如想要配置NameNode使用parallelGC, 那么可以在`hadoop-env.sh`文件中添加:

	export HADOOP_NAMENODE_OPTS="-XX:+UseParallelGC"

除了上面所说的环境变量之外, 还可以设置的参数有:

|环境变量|参数说明|
|HADOOP_PID_DIR|daemons进程id文件存储位置|
|HADOOP_LOG_DIR|daemons日志文件存粗位置|
|HADOOP_HEAPSIZE / YARN_HEAPSIZE|最大的堆大小|

大多数情况下都应该指定`HADOOP_PID_DIR`和`HADOOP_LOG_DIR`这两个位置, 这样才能保证文件由对应的daemon进程去修改. 否则可能会出现符号链接攻击.

传统地, 人们经常配置HADOOP_PREFIX环境变量, 这个环境变量用于shell环境, 例如在`/etc/profile.d`脚本文件中加入下面的命令:

    HADOOP_PREFIX=/path/to/hadoop
    export HADOOP_PREFIX

至于堆大小的环境变量如下表所示:

|Daemon进程|堆大小环境变量|
|--------|--------|
|ResourceManager|YARN_RESOURCEMANAGER_HEAPSIZE|
|NodeManager|YARN_NODEMANAGER_HEAPSIZE|
|WebAppProxy|YARN_PROXYSERVER_HEAPSIZE|
|Map Reduce Job History Server|HADOOP_JOB_HISTORYSERVER_HEAPSIZE|

#### Daemons设置

* `core-site.xml`

    |参数|取值|说明|
    |--------|--------|
    |fs.defaultFS|NameNode URI地址|hdfs://host:port/|
    |io.file.buffer.size|131072|SequenceFiles读写缓冲区大小|

* `hdfs-site.xml`

	针对NameNode:

    |参数|取值|说明|
    |--------|--------|
    |dfs.namenode.name.dir|本地文件系统上, NameNode保存namespace和transactions logs的目录|如果这是一个逗号分割的目录集合, 那么这些目录都会保存|
    |dfs.hosts / dfs.hosts.exclude|允许/排除的DataNode集合|控制管理DataNode|
    |dfs.blocksize|268435456|HDFS blocksize(256M)|
    |dfs.namenode.handler.count|100|管理RPC的线程数, 这些RPC对象是DataNode|

	针对DataNode:

    |参数|取值|说明|
    |--------|--------|
    |dfs.datanode.data.dir|DataNode节点在本地文件系统保存block的目录|如果这是一个逗号分割的目录集合, 那么这些目录都会保存|

* `yarn-site.xml`

	针对ResourceManager和NodeManager:

    |参数|取值|说明|
    |--------|--------|
    |yarn.acl.enable|true / false|允许ACLs? 默认值是false.|
    |yarn.admin.acl|Admin ACL|集群的管理权限ACL. 默认值是*, 表示所有人. ' '表示没有一个人|
	|yarn.log-aggregation-enable|false|日志整合开关|

    针对ResourceManager:

    |参数|取值|说明|
    |--------|--------|
    |yarn.resourcemanager.address|ResourceManager host:port|客户提交job的地址. 如果设置则覆盖yarn.resourcemanager.hostname|
    |yarn.resourcemanager.scheduler.address|ResourceManager host:port|ApplicationMasters向Scheduler获取资源的地址. 如果设置则覆盖yarn.resourcemanager.hostname|
    |yarn.resourcemanager.resource-tracker.address|ResourceManager host:port|供NodeManagers访问的地址. 如果设置则覆盖yarn.resourcemanager.hostname|
    |yarn.resourcemanager.admin.address|ResourceManager host:port|供管理员命令访问的地址. 如果设置则覆盖yarn.resourcemanager.hostname|
    |yarn.resourcemanager.webapp.address|ResourceManager host:port.|供web-ui访问的地址. 如果设置则覆盖yarn.resourcemanager.hostname|
    |yarn.resourcemanager.hostname|ResourceManager host|设置hostname全局变量, 如果其他setting指明则被覆盖掉|
    |yarn.resourcemanager.scheduler.class|ResourceManager Scheduler class|CapacityScheduler (推荐), FairScheduler (推荐), 或者FifoScheduler|
    |yarn.scheduler.minimum-allocation-mb|integer|container request的内存分配下限(MB为单位)|
    |yarn.scheduler.maximum-allocation-mb|integer|container request的内存分配上限(MB为单位)|
    |yarn.resourcemanager.nodes.include-path / yarn.resourcemanager.nodes.exclude-path|允许/排除的NodeManagers集合|控制管理NodeManagers|

    针对NodeManager:

    |参数|取值|说明|
    |--------|--------|
    |yarn.nodemanager.resource.memory-mb|integer|内存容量使用限制|
    |yarn.nodemanager.vmem-pmem-ratio|integer|虚拟内存使用限制, 可以比物理内存多使用的比率|
    |yarn.nodemanager.local-dirs|中间结果保存的目录集合|如果这是一个逗号分割的目录集合, 那么这些目录都会保存|
    |yarn.nodemanager.log-dirs|日志文件保存的目录集合|如果这是一个逗号分割的目录集合, 那么这些目录都会保存|
    |yarn.nodemanager.log.retain-seconds|10800|日志文件默认保存时间(秒). 日志整合开关是关闭状态才有效|
    |yarn.nodemanager.remote-app-log-dir|/logs|应用日志文件保存目录. 需要适当的权限. 日志整合开关是开启状态才有效|
    |yarn.nodemanager.remote-app-log-dir-suffix|logs|应用日志文件后缀. 格式是${yarn.nodemanager.remote-app-log-dir}/${user}/${thisParam}. 日志整合开关是开启状态才有效|
    |yarn.nodemanager.aux-services|mapreduce_shuffle|MapReduce应用的Shuffle service|

	针对History Server:

    |参数|取值|说明|
    |--------|--------|
    |yarn.log-aggregation.retain-seconds|-1|整合日志的保存时间. -1表示无效参数. 如果设置太小会导致大量信息发送给NameNode|
    |yarn.log-aggregation.retain-check-interval-seconds|-1|检查整合日志的保存时间. 0或者-1表示整合日志保存时间的1/10. 如果设置太小会导致大量信息发送给NameNode|

* `mapred-site.xml`

	针对MapReduce应用:

    |参数|取值|说明|
    |--------|--------|
    |mapreduce.framework.name|yarn|计算框架|
    |mapreduce.map.memory.mb|1536|map的内存限制|
    |mapreduce.map.java.opts|-Xmx1024M|map的子jvm堆大小|
    |mapreduce.reduce.memory.mb|3072|reduce的内存限制|
    |mapreduce.reduce.java.opts|-Xmx2560M|reduce的子jvm堆大小|
    |mapreduce.task.io.sort.mb|512|用于数据排序的附加内容容量|
    |mapreduce.task.io.sort.factor|100|排序时的合并流附加数量|
    |mapreduce.reduce.shuffle.parallelcopies|50|并发复制的附加数量|

    针对MapReduce JobHistory Server:

    |参数|取值|说明|
    |--------|--------|
    |mapreduce.jobhistory.address|MapReduce JobHistory Server host:port|默认端口是10020|
    |mapreduce.jobhistory.webapp.address|MapReduce JobHistory Server Web UI host:port|默认端口是19888|
    |mapreduce.jobhistory.intermediate-done-dir|/mr-history/tmp|MapReduce jobs的历史文件保存目录|
    |mapreduce.jobhistory.done-dir|/mr-history/done|MR JobHistory Server合并的历史文件目录|

### NodeManagers健康状况管理

NodeManager的健康状况管理是通过脚本的方式来监听的, 一旦变成不健康状况, 监听脚本就会向标准输出ERROR信息, 而监听标准输出的脚本一旦检测到ERROR信息, 就会向ResourceManager报告从而使得NodeManager变为不健康状况, 被列入黑名单之中.

在这之后监听脚本还是继续运行, 如果NodeManager恢复为健康状况, 那么会自动地从ResourceManager的黑名单中排除.

NodeManager的健康状况是可以通过web ui进行查看的.

在`yarn-site.xml`配置文件中可以设置管理监听脚本参数:

|参数|取值|说明|
|--------|--------|
|yarn.nodemanager.health-checker.script.path|Node health script|监听健康状态脚本|
|yarn.nodemanager.health-checker.script.opts|Node health script options|脚本参数|
|yarn.nodemanager.health-checker.script.interval-ms|Node health script interval|监听脚本运行时间间隔|
|yarn.nodemanager.health-checker.script.timeout-ms|Node health script timeout interval|监听脚本超时时间|

> 出现少量磁盘错误是不会导致ERROR信息产生的, 只有达到阀值`yarn.nodemanager.disk-health-checker.min-healthy-disks`才会触发ERROR信息产生

### slave文件

Hadoop支持对集群上多个机器的不同文件同时进行操作, 这些文件访问路径要写在`etc/hadoop/slaves`文件中, 然后通过辅助脚本进行执行操作.

> 为了支持这个机制, hadoop运行账户的ssh信任必须建立.

### Hadoop Rack识别

很多hadoop组件都能够进行rack识别, 因此可以充分利用网络拓扑结构, 从而提升性能和安全.

hadoop daemon是通过调用管理员配置模块进行rack信息的收集. 有关rack识别请参考[这里](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/RackAwareness.html)

> 一般情况下都推荐开启这个机制.

### 日志记录

hadoop的日志机制是借助Apache Commons Logging框架的apache log4j进行的. 通过配置`etc/hadoop/log4j.properties`来自定义我们hadoop daemon日志配置.

### Hadoop集群操作

一旦所有的配置都完成了, 那么就把配置文件分发给集群上所有机器的`HADOOP_CONF_DIR`目录. 这样才能统一集群机器的配置, 但是要保证所有机器的`HADOOP_CONF_DIR`都是一样的.

通常情况下, 我们需要有两个管理用户, 一个hdfs用户管理HDFS相关操作, 另一个yarn用户管理YARN相关操作

下面介绍集群的开启操作和关闭操作:

* hadoop集群开启操作

开启hadoop集群需要两个步骤: 开启HDFS集群, 开启YARN集群, 开启MapReduce JobHistory服务器[可选]

**开启HDFS集群**:

1. 开启HDFS, 需要格式化

		[hdfs]$ $HADOOP_PREFIX/bin/hdfs namenode -format <cluster_name>

2. 开启HDFS NameNode

		[hdfs]$ $HADOOP_PREFIX/sbin/hadoop-daemon.sh --config $HADOOP_CONF_DIR --script hdfs start namenode

3. 开启所有HDFS DataNode

		[hdfs]$ $HADOOP_PREFIX/sbin/hadoop-daemons.sh --config $HADOOP_CONF_DIR --script hdfs start datanode

如果slave文件机制开启了且hdfs账户也通过了ssh信任, 那么可以使用脚本来执行上面的步骤:

	[hdfs]$ $HADOOP_PREFIX/sbin/start-dfs.sh

**开启YARN集群**:

1. 开启ResourceManager

		[yarn]$ $HADOOP_YARN_HOME/sbin/yarn-daemon.sh --config $HADOOP_CONF_DIR start resourcemanager

2. 开启所有NodeManager

		[yarn]$ $HADOOP_YARN_HOME/sbin/yarn-daemons.sh --config $HADOOP_CONF_DIR start nodemanager

3. 开启WebAppProxy服务器

		[yarn]$ $HADOOP_YARN_HOME/sbin/yarn-daemon.sh --config $HADOOP_CONF_DIR start proxyserver

同样, 如果slave文件机制开启了且yarn账户也通过了ssh信任, 那么可以使用脚本来执行上面的步骤:

	[yarn]$ $HADOOP_PREFIX/sbin/start-yarn.sh

**开启MapReduce JobHistory服务器**:

	[mapred]$ $HADOOP_PREFIX/sbin/mr-jobhistory-daemon.sh --config $HADOOP_CONF_DIR start historyserver

* hadoop集群关闭操作

**关闭HDFS集群**:

1. 关闭NameNode

		[hdfs]$ $HADOOP_PREFIX/sbin/hadoop-daemon.sh --config $HADOOP_CONF_DIR --script hdfs stop namenode

2. 关闭所有的DataNode

		[hdfs]$ $HADOOP_PREFIX/sbin/hadoop-daemons.sh --config $HADOOP_CONF_DIR --script hdfs stop datanode

如果slave文件机制开启了且hdfs账户也通过了ssh信任, 那么可以使用脚本来执行上面的步骤:

	[hdfs]$ $HADOOP_PREFIX/sbin/stop-dfs.sh

**关闭YARN集群**:

1. 关闭ResourceManager

		[yarn]$ $HADOOP_YARN_HOME/sbin/yarn-daemon.sh --config $HADOOP_CONF_DIR stop resourcemanager

2. 关闭所有的NodeManager

		[yarn]$ $HADOOP_YARN_HOME/sbin/yarn-daemons.sh --config $HADOOP_CONF_DIR stop nodemanager

3. 关闭WebAppProxy服务器

		[yarn]$ $HADOOP_YARN_HOME/sbin/yarn-daemon.sh --config $HADOOP_CONF_DIR stop proxyserver

同样, 如果slave文件机制开启了且yarn账户也通过了ssh信任, 那么可以使用脚本来执行上面的步骤:

	[yarn]$ $HADOOP_PREFIX/sbin/stop-yarn.sh

**关闭MapReduce JobHistory服务器**:

	[mapred]$ $HADOOP_PREFIX/sbin/mr-jobhistory-daemon.sh --config $HADOOP_CONF_DIR stop historyserver

### Web管理接口

|daemon进程|Web接口|说明|
|--------|--------|
|NameNode|http://nn_host:port/|默认HTTP端口是50070|
|ResourceManager|http://rm_host:port/|默认HTTP端口是8088|
|MapReduce JobHistory Server|http://jhs_host:port/|默认HTTP端口是19888|