+++
title = 'java 四舍五入问题'
date = 2021-01-02T15:42:00+08:00
draft = false
+++
# java 四舍五入问题

> 前两天,在工作中遇到了一个问题,公司运营说:\"我上传数据,没有给我四舍五入,是不是你程序有问题?\".那我当时肯定是想程序都用这么长时间了\...不能有问题吧.
> 但是还是说:\"把具体的问题数据,以及什么可以重现,都说一下\",接下来是解决问题的心路历程.

## 复现bug

问题数据已经有了,接下来就是如何重现这个问题.主要的问题代码就是下边

``` {.java .hljs}
long round = Math.round(Double.valueOf("150.795") * 100);
        System.out.println(round);
```

**其中`Double.valueOf("150.795")`{.language-plaintext
.highlighter-rouge}没有问题,然后 做`*100`{.language-plaintext
.highlighter-rouge}操作,这里结果就有问题了,结果是`15079.499999999998`{.language-plaintext
.highlighter-rouge}.**

之前都说 double不准什么的,也没体会到,现在真的是体会到了\...\...

## 解决问题

最终是需要实现
四舍五入保留两位小数,那就去看`BigDecimal`{.language-plaintext
.highlighter-rouge}这个吧,都说这个计算是准确的.

``` {.java .hljs}
    private Double round(double f) {
        BigDecimal b =  BigDecimal.valueOf(f);
        return b.setScale(2, BigDecimal.ROUND_HALF_UP).doubleValue();
    }
```

最终使用的是这个解决方案.

但是这里也有个坑,就是传入的double最好是没有计算过的,不然也会出现上边
`*100`{.language-plaintext .highlighter-rouge}之后结果有问题

一般来说界面填写数值,然后传入到后端获取都是 `String`{.language-plaintext
.highlighter-rouge}类型的,直接调用方法就行,如果需要计算的话,用`BigDecimal`{.language-plaintext
.highlighter-rouge}计算
