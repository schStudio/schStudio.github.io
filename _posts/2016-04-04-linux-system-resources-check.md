---
layout: post
title:	'系统资源的查看'
category: Linux
---

* content
{:toc}

### `free`: 查看内存使用情况

	free [options]

#### `free`参数说明

| 参数 | 说明 |
|--------|--------|
|`-b`|B|
|`-k`|KB|
|`-m`|MB|
|`-g`|GB|
|`-t`|显示总量统计|

#### `free`使用实例

* 显示目前系统的内存容量

	  free -m -t

	![linux-free-m-t]({{ site.url }}/assets/linux/linux-free-m-t.png)

> 系统中内存的使用量非常高, 而且`buffers`和`cached`也非常大, 这是linux善于利用内存的表现, 它把`最常使用到的数据`和`最近使用到的数据`都缓存下来了, 下次访问就直接从内存读取, 而不需要再去访问磁盘, 提高了访问速度.

- - -

### `uname`: 查看系统内核相关信息

	## 默认是 uname -s
	uname [options]

#### `uname`参数说明

| 参数 | 说明 |
|--------|--------|
|`-a`|所有系统内核信息|
|`-s`|系统内核名称|
|`-r`|内核版本|
|`-m`|硬件名称|
|`-p`|CPU类型|
|`-i`|硬件平台|

#### `uname`使用实例

* 显示系统内核所有信息

	  uname -a

	![linux-uname-a]({{ site.url }}/assets/linux/linux-uname-a.png)

- - -

### `uptime`: 查看系统启动时间与工作负载

	uptime [options]

#### `uptime`参数说明

| 参数 | 说明 |
|--------|--------|
|`-p`|以完整形式显示|
|`-s`|从`yyyy-mm-dd HH:MM:SS`开始统计|

#### `uptime`使用实例

* 显示系统启动时间与工作负载

	  uptime

	![linux-uptime]({{ site.url }}/assets/linux/linux-uptime.png)

> 工作负载分别表示: 1分钟内 / 5分钟内 / 15分钟内 的平均工作负载

- - -

### `netstat`: 查看跟踪网络

#### `netstat`参数说明

| 参数 | 说明 |
|--------|--------|
|`-a`|显示系统所有连接|
|`-t`|列出tcp网络数据包的数据|
|`-u`|列出udp网络数据包的数据|
|`-n`|以端口号替换服务名称|
|`-l`|显示正在网络监听的服务|
|`-p`|显示服务PID|

#### `netstat`使用实例

* 显示所有打开的网络连接与`unix socket`状态

	netstat -a

	![linux-netstat-a]({{ site.url }}/assets/linux/linux-netstat-a.png)
	![linux-netstat-a2]({{ site.url }}/assets/linux/linux-netstat-a2.png)

- - -

### `dmesg`: 查看内核产生的信息

#### `dmesg`使用实例

* 输出所有内核开机时的信息

	  dmesg | more

    ![linux-dmesg]({{ site.url }}/assets/linux/linux-dmesg.png)

* 查找开机时硬盘相关的信息

	  dmesg | grep -i hd

    ![linux-dmesg-grep-sd]({{ site.url }}/assets/linux/linux-dmesg-grep-sd.png)

- - -

### `vmstat`: 检测系统资源变化

	vmstat [options] [delay [count]]

> `delay`: 更新延迟
> 
> `count`: 更新次数

#### `vmstat`参数说明

| 参数 | 说明 |
|--------|--------|
|`-a`|显示活跃与非活跃状态的内存量|
|`-f`|显示系统启动至今的fork数量|
|`-s`|显示事件导致的内存变化列表|
|`-S`|统计单位k/m(10进制), K/M(2进制)|
|`-d`|列出磁盘读写总量|
|`-p`|分区磁盘读写总量|

#### `vmstat`使用实例

* 显示系统资源状态, 更新频率1秒, 更新次数3次

	  vmstat 1 3

	![linux-vmstat-1-3]({{ site.url }}/assets/linux/linux-vmstat-1-3.png)

	参数说明:

	| 参数 | 说明 |
    |--------|--------|
    |`r`|等待运行的进程数量|
    |`b`|阻塞的进程数量|
    |`swpd`|虚拟内存使用总量|
    |`free`|空闲内存总量|
    |`buff`|缓冲内存总量|
    |`cache`|缓存内存重量|
    |`si`|磁盘换进内存的总量|
    |`so`|内存换出磁盘的总量|
    |`bi`|从磁盘读取的块数量|
    |`bo`|写入到磁盘的块数量|
    |`in`|每秒的中断数量|
    |`cs`|每秒的上下文切换数量|
    |`us`|非内核状态CPU使用百分比|
    |`sy`|内核状态CPU使用百分比|
    |`id`|闲置状态CPU使用百分比|
    |`wa`|IO等待CPU使用百分比|
    |`st`|虚拟机占用CPU使用百分比|

> 有关参数详细说明可以通过`man vmstat`查询