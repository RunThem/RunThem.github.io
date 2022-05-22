+++
title = "Debian更新源失败"
date = 2022-03-13T15:17:56+08:00
draft = false
description = "Debian更新源失败"
tags = [
	"Linux",
	"Docker",
	"apt"
]
+++

> 作者: RunThem  
> 未经允许, 禁止转载, 尤其 `CSDN`, 违者必究

# apt更新源失败
由于我使用的 `Linux` 发行版是 `Debian`, 不会向 `Arch` 那样经常需要更新, 我都是几个月才更新一次的, 最近我更新了一次, 当时并没有发生什么问题, 出现问题的是昨天晚上, 我想装 `Docker`, 但是当我按官网文档去执行的时候, 再次执行 `apt update` 就出现问题了.

经过看报错, 一共是有两个问题的
 * does not have a Release file. Updating from such a repository can't be done securely, and is therefore disabled by default
 * Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg)

一个是错误, 一个是告警, 首先说明一下, 这两个问题都是我使用的是 `Debian` 测试源的情况下出现的, 我在更新源之前是 `bullseye`, 更新后成了 `bookworm`, 你可以使用 `cat /etc/os-release` 查看你是什么版本的系统.

## 解决错误
显示解决一下添加 `docker` 源出现的错误, 显示该源没有 `Release` 文件, 下面就是执行 `docker` 官方给出的脚本命令执行后的结果.

```txt
deb [arch=amd64] https://download.docker.com/linux/debian bookworm stable
```

源连接后面的 `bookworm` 就是你的系统版本, 源更新就是从上面的源里面去找对应系统的 `Release` 文件, 那我们看一下这个 `Url` 下有什么文件.

![1.png](https://runthem.github.io/images/debian-apt-update-error/1.png)

`gpg` 是源的公钥, 用于验证的, `dists` 就是真正的包所在, 下图就是点进去的目录, 可以看到是没有我的系统版本(`bookworm`)的源的, 这就是更新源报错的问题.

![2.png](https://runthem.github.io/images/debian-apt-update-error/2.png)

解决的方法就是将源连接中系统版本调整成源中存在的与你的版本最近的版本, 比如我是 `bookworm`, 就改成 `bullseye`, 就可以了, 这两个版本差别不大, 一个是稳定版, 一个是测试版.

## 解决告警
告警的原因有两种, 一是 `源公钥的路径` 有问题, 其实就是上一个版本的 `源公钥` 的存放地址是 `/usr/share/keyrings`.

```sh
iccy@RunThem /u/share [1]> cd keyrings/
iccy@RunThem /u/s/keyrings> ls
debian-archive-bullseye-automatic.gpg           debian-archive-removed-keys.gpg
debian-archive-bullseye-security-automatic.gpg  debian-archive-stretch-automatic.gpg
debian-archive-bullseye-stable.gpg              debian-archive-stretch-security-automatic.gpg
debian-archive-buster-automatic.gpg             debian-archive-stretch-stable.gpg
debian-archive-buster-security-automatic.gpg    docker-archive-keyring.gpg
debian-archive-buster-stable.gpg                vscodium-archive-keyring.gpg
debian-archive-keyring.gpg
```

新版本的 `源公钥` 放在 `/etc/apt/trusted.gpg.d`下了, 所以你的第三方 `源公钥` 没有修改位置的话, 就会告警, 如: vscodium, sublime等, 解决的方式就是将 `源公钥` 复制到新路径下.

```sh
iccy@RunThem /e/a/trusted.gpg.d> ls
debian-archive-bullseye-automatic.gpg           debian-archive-stretch-automatic.gpg
debian-archive-bullseye-security-automatic.gpg  debian-archive-stretch-security-automatic.gpg
debian-archive-bullseye-stable.gpg              debian-archive-stretch-stable.gpg
debian-archive-buster-automatic.gpg             docker-archive-keyring.gpg
debian-archive-buster-security-automatic.gpg    google-chrome.gpg
debian-archive-buster-stable.gpg                v2raya.asc
```

第二是 `源公钥` 自身有问题, `源公钥` 是直接下载的话是明文的, 在新的 `Debian` 中这样是不行的, 如:

```sh
# 这是sublime官网上的公钥添加方式, 有两个问题, 第一就是路径问题, 第二是apt-key add -被废弃了
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -

# 正确的方式如下
curl -fsSL  https://download.sublimetext.com/sublimehq-pub.gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/sublimehq-pub.gpg
```

这样既修改了目标路径, 又使用了 `gpg` 加密 `源公钥`.

这样应该就没有问题了, 至少我这边是没有问题的, 笑.
