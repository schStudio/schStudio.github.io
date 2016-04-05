---
layout: post
title: Hadoop 概述
category: Hadoop
---

* content
{:toc}

### 什么是Hadoop

定义: Apache Hadoop是一个开源的软件框架, 用于产业集群上大数据集的存储与处理

* Hadoop一开始只是一个简单的批处理框架
* Hadoop的原理是移动计算到数据, 而不是移动数据到计算
* Hadoop的核心是扩展性
* Hadoop必须考虑可靠性. 因为集群中的硬件软件故障事件很容易发生

- - -

### Hadoop的基本组件

#### Hadoop Common

Hadoop Common提供了其他Hadoop模块需要的基本库和工具

#### Hadoop Distributed File System(HDFS)

HDFS是一个用Java编写的分布式文件系统

* HDFS组件: 一个NameNode, 多个DataNode
* HDFS的可靠性是通过在多个主机之间拷贝数据副本实现的
* Secondary NameNode的工作不是在Primary NameNode崩溃的时候替换, 而是记录Primary NameNode的某一时刻的信息

#### Hadoop MapReduce

MapReduce是一个编程模型, 目的是在集群上通过并行的, 分布式的算法处理大数据集

* MapReduce引擎组件: JobTracker, TaskTracker

#### Hadoop YARN

Hadoop YARN是一个资源管理平台, 负责管理计算资源, 便于用户应用调度执行

* YARN是下一代的MapReduce, 具有更强大的计算处理能力
* YARN与MapReduce等框架相比, 具有更加广阔的交互模式
* YARN与MapReduce完全兼容

### Hadoop Zoo

![hadoop-cloudera-stack]({{ site.url }}/assets/hadoop/hadoop-cloudera-stack.png)

### Hadoop生态系统

![hadoop-ecosystem]({{ site.url }}/assets/hadoop/hadoop-ecosystem.jpg)

![hadoop-ecosystem-describtion]({{ site.url }}/assets/hadoop/hadoop-ecosystem-describtion.png)
