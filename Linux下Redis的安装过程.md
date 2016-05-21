---
title: Linux下Redis的安装过程
date: 2016-05-21 10:50:40
tags: [redis,linux]
categories: [开发笔记]
---
### 前言

在linux操作系统下安装redis的教程，网上已经烂大街了，笔者觉着自己在配置过程中有些操作不太理解，所以在此重新梳理一下安装过程，并对一些要点着重进行说明。

笔者的服务器为CentOS 6.5，redis下载的版本为当前最新的稳定版本。

### 下载redis

下载redis的方式主要有两种，一种是自己下载tar.gz包，然后上传到服务器上进行解压缩。另一种是通过wget命令直接下载redis的压缩包到服务器，然后解压缩。

```
 wget http://download.redis.io/releases/redis-3.0.5.tar.gz
```

看到这个教程时，根据自己的需要去选择redis的版本。

### 编译安装redis

下载好redis的tar.gz包后，在指定的位置进行解压缩：

```
 tar -xzvf redis-3.0.5.tar.gz
```

进入解压缩后的程序目录，使用make对redis进行编译：

```
 cd redis-3.0.5

 make
```

执行完make后，会在src目录中发现生成了几个可执行文件，如：

- redis-server：redis服务器启动程序。
- redis-cli：Redis客户端操作工具。也可以用telnet根据其纯文本协议来操作。
- redis-benchmark：Redis性能测试工具
- redis-check-aof：数据修复工具
- redis-check-dump：检查导出工具

接着执行安装命令，redis默认会将上述的可执行文件生成到/usr/local/bin目录下，你也可以通过命令参数来指定安装的目录位置，具体查看redis程序目录下的ReadMe文件。

编译安装过后，就可以在任意的位置执行redis服务了，不需要每次执行都要带上执行文件的路径位置了。

在make命令执行后，你可以选择不执行make install命令，自己手动的将程序目录下src目录中的对应可执行文件copy到/usr/bin下，这招是从oschina的红薯[那里](http://www.oschina.net/question/12_18065?fromerr=uNX17fsi)学来的。

当然你可能会有疑惑，make install是安装到了/usr/local/bin下，而自己copy为何在/usr/bin下。其实没有差别，这两个不同的位置都可以起到同一个作用，在任何位置执行redis相关命令。具体可搜索/usr/bin和/usr/local/bin目录的作用。

### 配置redis

### 运行redis

### redis重点配置项说明
