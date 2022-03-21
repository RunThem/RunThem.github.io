+++
title = "Golang设置代理"
date = 2022-03-21T00:09:03+08:00
draft = false
description = "市地方个好人市地方后"
tags = [
	"Linux",
	"Golang"
]
+++

> 作者: RunThem  
> 未经允许, 禁止转载, 尤其 `CSDN`, 违者必究

# Golang设置代理
在最近编译一个 `go` 语言的包, 由于国内某些原因, 依赖根本下不下来, 只能使用代理了.

国内比较好的镜像站是 [`https://goproxy.io`](https://goproxy.io) 这个网站, 执行以下命令即可:

```sh
go env -w GOPROXY=https://proxy.golang.com.cn,direct
```

这样就可以了.