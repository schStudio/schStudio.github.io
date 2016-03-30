---
layout: post
title:	'进程的查看'
category: Linux
---

* content
{:toc}

### ps指令

#### ps参数说明

| 参数 | 说明 |
|--------|--------|
|    -A    |   显示所有的进程     |
|    -a    |   排除与终端有关的进程     |
|    -u    |   指定特定用户的进程     |
|     x    |   显示更完整的信息, 与a参数一起使用    |
|     l    |   长格式     |
|     j    |   工作格式     |
|    -f    |   完整格式     |

#### ps使用实例

* 查看与bash相关的进程

		ps -l

	![ps-l]({{ site.url }}/assets/linux/linux-ps-ps-l.png)

    | 参数 | 说明 |
    |--------|--------|
    |F|4表示进程权限为root, 1表示进程只能fork, 不能执行|
    |S|进程的STATE, R->运行, S->睡眠, D->阻塞, T->停止, Z->僵尸|
    |UID|用户ID|
    |PID|进程ID|
    |PPID|父进程ID|
    |C|CPU使用率|
    |PRI/NI|进程优先级相关, 越小优先级越高|
    |ADDR|进程内存地址|
    |SZ|消耗的内存|
    |WCHAN|运行状态|
    |TTY|终端接口|
    |TIME|进程运行累计时间|
    |CMD|进程指令|

* 查看系统所有的进程

		ps aux

	![ps-aux]({{ site.url }}/assets/linux/linux-ps-ps-aux.png)

	| 参数 | 说明 |
    |--------|--------|
    |USER|用户|
    |PID|进程ID|
    |%CPU|CPU使用率|
    |%MEM|内存使用率|
    |VSZ|虚拟内存占用量|
    |RSS|内存占用量|
    |TTY|终端接口|
    |STAT|进程状态|
    |START|进程启动时间|
    |TIME|进程运行累计时间|
    |COMMAND|进程指令|

* 查看系统某进程

		ps aux | egrep 'xrgsu'

	![ps-egrep]({{ site.url }}/assets/linux/linux-ps-ps-egrep.png)


- - -

### t-o-p指令



#### t-o-p参数说明

| 参数    | 说明 |
|--------|--------|
|-d|更新频率, 以秒为单位|
|-b|批处理, 通常配合数据流重定向|
|-n|与-b配合, 指定批处理总次数|
|-p|检测指定进程PID|

#### t-o-p程序指令

|?|查看top程序指令帮助|
|C|按照CPU使用率大小排序|
|M|按照内存使用率大小排序|
|N|按照PID排序|
|T|按照CPU运行总时长排序|
|k|kill进程|
|r|重新给进程nice值|
|q|退出top|


#### t-o-p使用实例

* 每隔2秒更新一次资源状态

		top -d 2

	![top-d2]({{ site.url }}/assets/linux/linux-top-d2.png)

	| 参数 | 说明 |
    |--------|--------|
    |PID|进程ID|
    |USER|用户|
    |PR/NI|优先级|
    |VIRT|虚拟内存使用量|
    |RES|内存使用量|
    |SHR|共享内存使用量|
    |%CPU|CPU使用率|
    |%MEM|内存使用率|
    |TIME+|CPU使用累积时间|
    |COMMAND|进程指令|

* 把资源状态结果输出到文件

		top -b -n 2 > /tmp/top.txt

- - -

### pstree指令

#### pstree参数说明

| 参数 | 说明 |
|--------|--------|
|-A|以ASCII字符显示进程树|
|-U|以UTF8字符显示进程树|
|-u|显示进程所属用户|
|-p|显示进程PID|

#### pstree使用实例

	pstree -Uup

![pstree-Uup]({{ site.url }}/assets/linux/linux-pstree-Uup.png)
