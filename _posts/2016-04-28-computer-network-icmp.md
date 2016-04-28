---
layout:	post
title:	"ICMP学习笔记"
categories: 计算机网络
---

* content
{:toc}

### ICMP概述

#### ICMP用途

**ICMP(Internet Control Message Protocol)**是为了更**有效地转发IP数据报**和**提高交付成功的机会**.

ICMP允许主机或路由器报告**差错情况**和提供有关**异常情况**的报告.

ICMP是属于网络层的协议, 其ICMP报文作为IP层数据报的数据, 加上数据报的首部, 组成IP数据报发送出去.

#### ICMP报文格式

![icmp-header]({{ site.url }}/assets/computer-network/icmp-header.gif)

#### ICMP报文种类

ICMP的报文类型有两种: **ICMP差错报告报文**和**ICMP询问报文**.

![icmp-kinds]({{ site.url }}/assets/computer-network/icmp-kinds.jpg)

* ICMP差错报告报文的类型解释

| 数据报类型 | 解释 |
|--------|--------|
|终点不可达|主机或路由器**无法交付数据报**时向源点发送的ICMP差错报告报文|
|源点抑制|主机或路由器由于拥塞而丢弃数据报时向源点发送的ICMP差错报告报文, 使源点知道应当**放慢发送速率**|
|时间超过|主机或路由器收到**生存时间为0**的数据报而丢弃该数据报时向源点发送的ICMP差错报告报文|
|参数问题|目的主机或路由器收到的**数据报首部中字段的值不正确**而丢弃该数据报时向源点发送的ICMP差错报告报文|
|改变路由(重定向)|路由器把重定向报文告知源点, 让其知道**下次应将数据报发送给另外的路由器**|

* ICMP询问报文的类型解释

| 数据报类型 | 解释 |
|--------|--------|
|回送请求和回答|源主机发送回送请求报文, 目的主机响应回送回答报文. **用来测试目的站是否可达以及有关状态**|
|时间戳请求和回答|源主机发送时间戳请求报文, 目的主机响应一个`32`位的字段, 代表`1900.1.1`起到现在的秒数. **用来进行时钟同步和测量时间**|

### ICMP应用举例

#### Ping程序

Ping(Packet InterNet Groper), 分组网间探测, **用来测试两个主机之间的连通性**.

Ping使用了ICMP回送请求与回送回答报文.

> 注: Ping是应用层直接使用网络层ICMP的一个例子

例如:

![icmp-ping-example]({{ site.url }}/assets/computer-network/icmp-ping-example.jpg)

#### Traceroute程序

**Traceroute用来跟踪一个分组从源点到终点的路径**.

工作原理如下:

Traceroute从源主机向目的主机发送一连串的IP数据报. 数据报中封装的是无法交付的UDP用户数据报.

* 第一个数据报P1的生存时间TTL设置为1, 当P1到达路径上的第一个路由器R1时, R1先收下它, 接着把TTL的值减1. 由于TTL的值变为0, R1就把P1丢弃, 并向源主机发送一个ICMP**时间超过**差错报告报文
* 第二个数据报P2的生存时间TTL设置为2, 当P2到达路径上的第一个路由器R1时, R1先收下它, 接着把TTL的值减1再转发给路由器R2, R2收下之后把TTL的值减1, 由于TTL的值变为0, R2就把P2丢弃, 并向源主机发送一个ICMP**时间超过**差错报告报文
* `......`
* 当最后一个数据报刚刚到达目的主机时, 数据报的TTL是1. 主机不转发数据报, 也不把TTL的值减1. 但是因为IP数据报中封装的是无法交付的运输层的UDP用户数据报, 因此目的主机要向源主机发送ICMP**终点不可达**差错报告报文

这样, 源主机就能获取路径上每个节点的信息.

![icmp-traceroute-example]({{ site.url }}/assets/computer-network/icmp-traceroute-example.jpg)