+++
title = "[cmd] Doxygen 生成文档"
date = 2022-11-07T22:52:21+08:00
draft = false
description = "description"
tags = [
	"Debian",
	"Linux",
	"doxygen"
]
+++

> 作者: RunThem
> 未经允许, 禁止转载, 尤其 `CSDN`, 违者必究

# 引言

学习编程也有几年了, 对于编码过程中的注释只是一个暧昧的态度, 看开源代码时觉得有注释/文档真棒, 到自己写代码时对于注释就有点随意了, 写肯定是写的, 但是写的比较随意, 最近想要写一个库, 但是呢, 对于写文档就不太喜欢了, 觉得太麻烦了, 可是一个库怎么可以没有文档呢, 于是就查了查, 有这么一个东西 -- `Doxygen`, 可以通过注释生成文档.

---

## 1. 介绍

`Doxygen` 文档生成器, {{< url src="https://doxygen.org" >}}. 安装过程就省略了.

## 2. 配置 Doxygen 工作环境

在一个工程中先需要生成 `Doxygen` 的配置文件, 默认的配置文件名为 `Doxygen`.
```shell
doxygen -s -g {{Doxygen config filename}} # `-s` 去掉配置文件中的注释, 结尾也可以指定配置文件名
doxygen -s -g doxygen.ini # 我的常用命令
```
