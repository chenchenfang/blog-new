+++
title = 'Linux下的atime mtime ctime查看以及修改'
date = 2022-04-18T22:27:00+08:00
draft = false
+++
## 1.三个时间的含义 {#1.%E4%B8%89%E4%B8%AA%E6%97%B6%E9%97%B4%E7%9A%84%E5%90%AB%E4%B9%89 tabindex="-1"}

-   access time （atime）\
    当"该文件的内容被取用"时，就会更新这个读取时间 （access）。
-   modification time （mtime）\
    当该文件的"内容数据"变更时，就会更新这个时间，内容数据指的是文件的内容，而不是文件的属性或权限。
-   status time （ctime）\
    当该文件的"状态 （status）"改变时，就会更新这个时间。\
    举例来说，像是权限与属性被更改了，文件内容修改，都会修改这个时间。

## 2.查看这三个时间的命令 {#2.%E6%9F%A5%E7%9C%8B%E8%BF%99%E4%B8%89%E4%B8%AA%E6%97%B6%E9%97%B4%E7%9A%84%E5%91%BD%E4%BB%A4 tabindex="-1"}

### 1.stat命令 {#1.stat%E5%91%BD%E4%BB%A4 tabindex="-1"}

``` {.shell .hljs}
[root@agent-54 ~]# stat a.txt
  File: "a.txt"
  Size: 0           Blocks: 0          IO Block: 4096   普通空文件
Device: fd01h/64769d    Inode: 394816      Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2021-01-01 01:01:00.000000000 +0800
Modify: 2021-01-01 01:01:00.000000000 +0800
Change: 2022-04-18 21:50:49.546704098 +0800
```

里边三个时间分别对应介绍的三个时间

### 2.ls查看 {#2.ls%E6%9F%A5%E7%9C%8B tabindex="-1"}

``` {.shell .hljs}
[root@agent-54 ~]# ls  --full-time --time=atime a.txt
-rw-r--r-- 1 root root 0 2021-01-01 01:01:00.000000000 +0800 a.txt
[root@agent-54 ~]# ls  --full-time  a.txt
-rw-r--r-- 1 root root 0 2021-01-01 01:01:00.000000000 +0800 a.txt
[root@agent-54 ~]# ls  --full-time --time=ctime a.txt
-rw-r--r-- 1 root root 0 2022-04-18 21:50:49.546704098 +0800 a.txt
```

## 3.touch修改access time和modification time {#3.touch%E4%BF%AE%E6%94%B9access-time%E5%92%8Cmodification-time tabindex="-1"}

> ctime是修改不了的，它是随着另外两个时间的变化而变化

首先我们来看一个 touch的help信息

``` {.shell .hljs}
[root@agent-54 ~]# touch --help
用法：touch [选项]... 文件...
将每个文件的访问时间和修改时间改为当前时间。

不存在的文件将会被创建为空文件，除非使用-c 或-h 选项。

如果文件名为"-"则特殊处理，更改与标准输出相关的文件的访问时间。

长选项必须使用的参数对于短选项时也是必需使用的。
  -a            只更改访问时间
  -c, --no-create   不创建任何文件
  -d, --date=字符串    使用指定字符串表示时间而非当前时间
  -f            (忽略)
  -h, --no-dereference      会影响符号链接本身，而非符号链接所指示的目的地
                (当系统支持更改符号链接的所有者时，此选项才有用)
  -m            只更改修改时间
  -r, --reference=文件    使用指定文件的时间属性而非当前时间
  -t STAMP      使用[[CC]YY]MMDDhhmm[.ss] 格式的时间而非当前时间
  --time=WORD       使用WORD 指定的时间：access、atime、use 都等于-a
            选项的效果，而modify、mtime 等于-m 选项的效果
      --help        显示此帮助信息并退出
      --version     显示版本信息并退出

请注意，-d 和-t 选项可接受不同的时间/日期格式。

请向bug-coreutils@gnu.org 报告touch 的错误
GNU coreutils 项目主页：<http://www.gnu.org/software/coreutils/>
GNU 软件一般性帮助：<http://www.gnu.org/gethelp/>
请向<http://translationproject.org/team/zh_CN.html> 报告touch 的翻译错误
要获取完整文档，请运行：info coreutils 'touch invocation'
```

其中 -a -m 用来修改不同的时间，-t 指定要修改的具体时间

``` {.shell .hljs}
[root@agent-54 ~]# touch -a -t 202002030355 a.txt
[root@agent-54 ~]# stat a.txt
  File: "a.txt"
  Size: 0           Blocks: 0          IO Block: 4096   普通空文件
Device: fd01h/64769d    Inode: 394816      Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2020-02-03 03:55:00.000000000 +0800
Modify: 2021-01-01 01:01:00.000000000 +0800
Change: 2022-04-18 22:23:12.490390037 +0800


[root@agent-54 ~]# touch -m -t 202006060606 a.txt
[root@agent-54 ~]# stat a.txt
  File: "a.txt"
  Size: 0           Blocks: 0          IO Block: 4096   普通空文件
Device: fd01h/64769d    Inode: 394816      Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2020-02-03 03:55:00.000000000 +0800
Modify: 2020-06-06 06:06:00.000000000 +0800
Change: 2022-04-18 22:26:28.588715086 +0800
```
