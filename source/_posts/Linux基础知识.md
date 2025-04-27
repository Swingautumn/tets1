---
title: Linux笔记
copyright_author: 王耀福
abbrlink: 24884
---
**Linux默认分区**包括根分区、启动分区、交换分区、用户分区还有、var分区等等

**Linux关于压缩**

Linux常用压缩包格式包括：tgz、gz、tar、bz2、xz等等

Linux解压缩，个人常用命令——tar -zxvf …. -C

Linux压缩命令——tar -zcvf

**Linux关于权限**

相关的命令：chown修改所属情况(所有者、所属组、其他用户)、chmod修改读写执行权限(读4写2执行1)

Linux与用户、组有关的命令：useradd、groupadd、getent(与cat、grep相似)、passwd等等

拓展：关于Sticky Bit，Linux的文件具有读写执行权限，为了防止文件被意外删除，额外添加了一种权限——t，即Sticky Bit

**Linux的定时服务**

cron命令、《/etc/crontab文件》《crontab -e》、《anacron》.《at命令》等也可以借用第三方工具如：fcron、dcron等

**Linux的shell脚本**——有个格式需要注意：“#!/bin/bash”

**运维常用命令**

`	`管理文件类型：ls、cd、mkdir、cp、mv、tail -f等

`	`进程管理类型：kill、ps -ef、top/htop、nohup等

`	`服务器系统类型：shutdown、uname、uptime、df、du、free等

**关于RAID**
**
`	`指Linux中的Redundant Arrays of Independent Disks，独立磁盘冗余阵列技术，是一种数据存储技术，旨在通过组合多个物理磁盘来提供数据冗余、增加系统容量，提高系统性能。该技术允许用户将多个磁盘组合成一个逻辑单元，从而提高容错率，增强数据包或和系统的I/O性能

**关于inode、硬链接、软链接**

`	`Inode即index node，索引节点，是Linux中的一个重要概念，主要是用于描述文件系统中的文件和目录。参考Linux系统中表面的文件名，那个文件名就是近似于索引节点。硬链接的概念也差不多，通过新建一个硬链接指向对应的节点实现访问，它的命令使用ln 原xxx 新xxx，当删除原xxx时，新建的xxx仍旧能使用，而软链接近似于windows的快捷方式，通过ln -s创建

**关于LILO**

`	`LILO是Linux的引导加载程序。它主要用于将Linux操作系统加载到主内存中，以便它可以开始运行

**Vim命令相关：**

`	`Set list、set nu、noh、%d、dd、yy、p、/xxx、v/a(视图/编辑)模式

event not found——可能是用echo命令时其中有感叹号

**关于Linux内核**

`	`也叫Linux kernel，是操作系统的核心组件之一，其功能众多包括进程管理，文件系统等，其中文件系统采用的是overlayFS（UnionFS(联合文件系统）的一种实现）方法，其他组件包括shell和GUI，系统实用程序和应用程序

**关于Linux系统安全操作**

`	`Root账户改密、禁用多余服务、更新系统软件、配置防火墙、设置SSH安全策略、管理用户权限

**关于Linux文件系统**
**
`	`Linux文件系统是一种组织和管理磁盘上文件和目录的层次结构。它采用树形结构，以根目录（/）为起点，包含各种子目录和文件。文件系统不仅负责数据存储，还支持文件访问控制、磁盘空间分配等功能，采用的是overlayFS（UnionFS(联合文件系统）的一种实现方法）系统

**关于Linux系统运行级别：**

常用的运行级别为3和5。

切换默认运行级别: 

systemctl set-default multi-user.target

临时切换运行级别：

Init x

![](./resource/Linux基础知识.001.png)