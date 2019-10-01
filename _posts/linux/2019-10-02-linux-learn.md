---
layout: post
title: Linux Learn
---

## 背景知识

- linux版本

    - 狭义上指由Linux之父**Linus Torvalds**发起的内核
        - [官网](https://www.kernel.org)
        - [GitHub](https://github.com/torvalds/linux)
        - 内核版本号分为三个部分，分别是主版本号、次版本号、末版本号，此版本号为奇数为开发版，偶数为稳定版

    - 广义上指各种linux发行版本

        - 基于Dpkg(Debian系)
            - Debian
            - Ubuntu

        - 基于RPM(Red Hat系)
            - RedHat Enterprise Linux
            - Fedora
            - CentOS

        - 其他


## 系统操作

### 帮助命令

- man

    man是manual的缩写，一共有9个section，默认是section1，显示可执行程序或shell命令

    `man -1 ls` 等价于 `man ls`

- help

    - shell自带的命令称为内部命令，其他是外部命令
        - `type ls` -> ls is aliased to `ls --color=auto'
        - `type cd` -> cd is a shell builtin

    - 内部命令使用help
       - `help cd`

    - 外部命令使用help
       - `ls --help`

- info

    info比help更详细，作为help的补充


### 文件操作

- pwd    显示当前目录名称

- ls     查看目录下的文件


    常用参数：
    - -l 长格式显示文件
    - -a 显示隐藏文件
    - -r 逆序显示
    - -t 按时间顺序显示，默认是按文件名
    - -R 递归显示

    `ls -l / /root` -> 长格式显示根目录和root用户home下的文件


- cd     更改当前的操作目录

    - `cd -`  -> 切换到上一次的目录
    - `cd`    -> 切换到当前用户home目录
    - `cd ..` -> 切换到上级目录

- mkdir   建立目录

    常用参数：
    - -p 建立多级目录，如果存在则跳过
        - `mkdir -p a/b/c/d`

- rmdir   删除空目录

- rm -r   删除非空目录，实为递归删除路径下所有文件

- cp      复制文件

    常用参数：
    - -r 递归复制，用于复制目录
    - -p 保留用户、权限、时间等文件属性

- mv      移动、重命名文件

- rm      删除文件

    常用参数：
    - -r 删除目录（包括目录下所有文件）
    - -f 删除文件不进行提示

    `rm -rf /` -> 删除根目录下所有文件

- 通配符

    - `*` 匹配任何字符串
    - `?` 匹配一个字符串



