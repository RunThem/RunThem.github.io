+++
title = "lsof 查看端口占用情况"
date = 2022-05-22T18:12:44+08:00
draft = false
description = "lsof 查看端口占用情况"
tags = [
	"Debian",
	"Linux",
	"lsof"
]
+++

> 作者: RunThem
> 未经允许, 禁止转载, 尤其 `CSDN`, 违者必究

# Linux查看端口占用情况
在 `Linux` 使用过程中经常需要看看某个端口是由哪个进程占用了, 本文来记录以下Linux下 `lsof` 的常见用法.

## lsof
`lsof(list open files)` 是一个列出当前系统打开文件的工具, 开源在{{< url src="https://github.com/lsof-org/lsof" >}}.

`lsof` 查看端口占用语法格式:

```shell
lsof -i:{port}
```

### 实例
让我们来看以下 `3000` 端口的占用, 这是 `gitea` 默认的端口.

```shell
$ sudo lsof -i:3000 # lsof需要root权限哦
COMMAND PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
gitea   848 gitea   13u  IPv6  23025      0t0  TCP *:3000 (LISTEN)
# COMMAND: 进程的名称
# PID:     进程标识符
# USER:    进程所有者
# FD:      文件描述符
# TYPE:    文件类型
# DEVICE:  指定磁盘的名称
# SIZE:    文件大小
# NODE:    索引节点
# NAME:    打开文件的确切名称
```

### 更多用法
```shell
lsof -i:8080         # 查看8080端口占用
lsof abc.txt         # 显示开启文件abc.txt的进程
lsof -c abc          # 显示abc进程现在打开的文件
lsof -p 1234         # 列出进程号为1234的进程所打开的文件
lsof -g gid          # 显示归属gid的进程情况
lsof +d /usr/local/  # 显示目录下被进程开启的文件
lsof +D /usr/local/  # 同上, 递归调用, 时间较长
lsof -d 4            # 显示使用fd为4的进程
lsof -i -U           # 显示所有打开的端口和UNIX domain文件
```

## other
`netstat` 是一个很老牌的网络工具.
`ss` 和 `ip` 都是新的取代 `netstat` 的工具, 但是旧的Linux发行版不一定有这些.
