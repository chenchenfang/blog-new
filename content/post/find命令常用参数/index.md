+++
title = 'find命令常用参数'
date = 2020-10-25T17:53:07+08:00
draft = false
+++
# find命令

find \[-H \| -L \| -P\] \[-EXdsx\] \[-f path\] path \... \[expression\]

## -name {#-name}

``` {.shell .hljs}
find . -name "*.java"
find . -iname "*.java"
```

iname 不区分大小写

## -type {#-type}

``` {.shell .hljs}
find . -type d
```

按类型查找

类型参数列表：

-   **f** 普通文件
-   **l** 符号连接
-   **d** 目录
-   **c** 字符设备
-   **b** 块设备
-   **s** 套接字
-   **p** Fifo

## -size {#-size}

文件大小单元：

-   **b** ------ 块（512字节）
-   **c** ------ 字节
-   **[w](http://man.linuxde.net/w)** ------ 字（2字节）
-   **k** ------ 千字节
-   **M** ------ 兆字节
-   **G** ------ 吉字节

``` {.shell .hljs}
find / -size +10M
find / -size -10M
find / -size 10M
```

+搜索大于10M的

-搜索小于10M的

## maxdepth mindepth

向下最大深度限制为3

``` {.shell .hljs}
find / -maxdepth 3 -type f 
```

搜索出深度距离当前目录至少2个子目录的所有文件

``` {.shell .hljs}
find / -mindepth 2 -type f
```

## -perm {#-perm}

#### 根据文件权限/所有权进行匹配

``` {.shell .hljs}
//权限是 644的文件
find . -perm 644 -type f
//用户是tom
find . -user tom
//组是hadoop
find . -group hadoop
```

其中 perm还有 `-perm +644`{.language-plaintext .highlighter-rouge}
`-perm -644`{.language-plaintext .highlighter-rouge}

拿二进制比较好说一点

644 ===\> 110100100

`+`{.language-plaintext .highlighter-rouge}是说 位数为1的匹配上一个就行

`-`{.language-plaintext .highlighter-rouge}是说
位数为1的必须都匹配上才行

`+`{.language-plaintext .highlighter-rouge} `-`{.language-plaintext
.highlighter-rouge}的0位都不用管

## -exec {#-exec}

``` {.shell .hljs}
find . -type f -name "*.txt" -exec cat {} \;
```

{}代表的是 前边查出来的那些文件的路径及文件名

``` {.shell .hljs}
❯ find . -size +1M -exec ls -l {} \;
-rw-r--r--@ 1 ziang  staff  184354523 10 23 17:45 ./spark-2.0.0-bin-hadoop2.6.tgz

❯ find ~/Downloads -size +1M -exec ls -l {} \;
-rw-r--r--@ 1 ziang  staff  184354523 10 23 17:45 /Users/ziang/Downloads/spark-2.0.0-bin-hadoop2.6.tgz
```

可见{}的内容是相对于 之前输入的路径的

然后可以对他们进行操作

exec 必须以 `;`{.language-plaintext .highlighter-rouge}结尾 然后
`;`{.language-plaintext .highlighter-rouge}属于字符
需要转义所以结尾成了`\;`{.language-plaintext .highlighter-rouge}
