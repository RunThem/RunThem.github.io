+++
title="Linux tar 用法"
date=2022-03-10T23:15:46+08:00
draft=false
description = "tar 归档的用法"
tags = [
    "Linux",
    "tar"
]
+++

> 作者: RunThem  
> 未经允许, 禁止转载, 尤其 `CSDN`, 违者必究

# tar - 归档
`tar` 可以将许多文件/目录一起保存至一个单独的文件中, 这样, 避免了文件多, 分散导致难以管理; 还有就是有些命令只能对文件其作用, 无法对目录使用(如压缩命令), `tar` 就可以解决这个问题了.

> `tar` 归档后的文件使用 `.tar` 结尾

### 启动子命令
`tar` 使用参数很多, 但只有 `5` 个参数分别启动不同的子命令, 以下的参数都每次只能存在一个, 可以与其他的参数连用:

> -c: 创建档案  
> -x: 解压档案  
> -t: 在不解压的情况下查看档案中包含的文件/目录  
> -r: 向已有的档案中添加文件  
> -u: 更新原档案中已存在的文件  

### 固定参数
> -f: 档案的名字, 该参数是必须的, 只能是最后一个参数, 后面必须接档案名字

### 常见的可选参数
> -z: 使用gzip压缩解压缩  
> -j: 使用bz2压缩解压缩  
> -J: 使用xz压缩解压缩  
> -v: 显示详细过程  
> -O: 将文件解开到标准输出  
> -C {{path}}: 解压到制定的路径  

### 使用格式
```shell
tar -[zjJ][cxtru][v]f *.tar.{gz,bz2,xz} *
```

```shell
tar -cf all.tar *.jpg    # 将所有.jpg的文件打成一个名为all.tar的包
tar -rf all.tar *.gif    # 将所有.gif的文件增加到all.tar的包里面去
tar -uf all.tar logo.gif # 更新原来tar包all.tar中logo.gif文件
tar -tf all.tar          # 列出all.tar包中所有文件
tar -xf all.tar          # 解出all.tar包中所有文件

tar -zcvf all.tar.gz *.jpg   # 将所有.jpg的文件打成一个名为all.tar的包, 再使用gzip进行压缩, 显示详细信息
tar -Jxvf all.tar.xz         # 先使用xz解出all.tar.xz得到all.tar, 再使用tar去解压all.tar, 显示详细信息
```

### 常见压缩文件解压
```shell
*.tar                # tar -xvf
*.gz                 # gzip -d
*.tar.gz, *.tgz      # tar -xzf
*.bz2                # bzip2 -d
*.tar.bz2            # tar -xjf
*.Z                  # uncompress
*.tar.Z              # tar -xZf
*.rar                # unrar e
*.zip                # unzip
```
