---
layout: post
title:	'find指令学习笔记'
category: Linux
---

* content
{:toc}

### find指令的用途

	在目录层次中查找满足特定要求的文件

### find指令常用参数说明

* -type c: 按照文件类型c查找, 文件类型c说明如下:
	* b: 块设备文件
	* c: 字符设备文件
	* d: 目录
	* p: 管道文件
	* f: 普通文件
	* l: 符号链接文件
	* s: 套接字文件
* -name fname: 按照fname(不加目录前缀)查找文件
* -exec command: 对查找到的文件进行指令处理, 注意结尾以';'结束
* -mmin n: 查找n分钟之前修改过的文件
* -mtime n: 查找n天之前修改过的文件
* -xdev: 只查找当前的文件系统
* -printf: 如果find指令返回值为true, 指定格式输出结果
* -prune: 排除指定的搜索范围
* -o: 或者

- - -

### find指令常用例子

#### 查找前5个最大的文件

	find / -type f -exec du {} \; | sort -n | tail -n 5

> -exec du表示对每一个找到的文件都计算文件大小
> 
> sort -n表示对所有文件按大小升序排序
> 
> tail -n 5表示取最后5个文件

#### 查找某一段时间修改过的文件

	find / -mmin +60 -mmin -120 -type f

> -mmin +60: 表示前60分钟之前
> 
> -mmin -120: 表示前120分钟之后
> 
> 所以该指令就是查找2个小时之前的1小时内修改过的文件

#### 在多个目录中查找

	find /etc /var /mnt /media -xdev -mmin -5 -type f

> 该指令在/etc, /var, /mnt, /media这些目录下查找
> 
> -xdev表示只对该文件系统查找
> 
> -mmin -5 表示5分钟之内修改过的文件

#### 排除指定目录查找

	find / \( -name proc -o -name sys \) -prune -o -type f

> 该指令表示对根目录查找, 但是排除 /proc 和 /sys这两个目录
> 
> -prune -o表示对满足括号中条件的目录排除在外

#### 按照文件类型查找

	find . -name "*.png" -o -name "*.jpg" -o -name "*.gif" -type f

> 该指令表示在当前目录中查找格式为png, jpg, gif的文件
> 
> -o 表示或者

#### 重复文件查找

	find . -type f -exec md5sum '{}' ';' | sort | uniq --all-repeated=separate -w 24

> 该指令表示对当前目录下的文件计算MD5并通过前24位进行比较
