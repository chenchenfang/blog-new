+++
title = 'spring MVC 校验Get请求的参数'
date = 2021-05-16T07:22:00+08:00
draft = false
+++
# spring MVC 校验Get请求的参数

> 很多时候都是使用
> POST请求然后传入请求体，用类来接受请求数据，但有时候就只有一个参数，不想去创建类了，然后也想使用校验注解去校验这个参数，那么接下来会介绍这种方法。

``` {.java .hljs}
//在类上声明注解 @Validated 下方的参数可以直接使用 校验注解 进行校验
@Validated
public class UserAdminController {

public JsonResult generatePortalJwtTokenByAdminGroupId( @NotNull Integer adminGroupId) throws CybercloudException {
}
}
```
