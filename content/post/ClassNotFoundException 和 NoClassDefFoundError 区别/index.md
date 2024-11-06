+++
title = 'ClassNotFoundException 和 NoClassDefFoundError 区别'
date = 2021-07-11T14:33:00+08:00
draft = false
+++
# ClassNotFoundException 和 NoClassDefFoundError

## 1. *ClassNotFoundException* {#1-classnotfoundexception}

> *Class.forName()*, *ClassLoader.loadClass()* or
> *ClassLoader.findSystemClass()* 一般是这几个方法,运行的时候会报错
>
> 因为这个去加载不存在的类
>
> 编译期间就没有这个类

``` {.java .hljs}
public void givenNoDrivers_whenLoadDriverClass_thenClassNotFoundException() 
  throws ClassNotFoundException {
      Class.forName("oracle.jdbc.driver.OracleDriver");
}
```

## 2.*NoClassDefFoundError* {#2noclassdeffounderror}

> 一般是因为类初始化 没有成功
> 因为类初始化只会初始化一次,第二次调用这个class
> 用于生成对象或者是静态变量,都会报错
>
> 编译期间有这个类

``` {.java .hljs}
public class ClassWithInitErrors {
    static int data = 1 / 0;
}

public class NoClassDefFoundErrorExample {
    public ClassWithInitErrors getClassWithInitErrors() {
        ClassWithInitErrors test;
        try {
            test = new ClassWithInitErrors();
        } catch (Throwable t) {
            System.out.println(t);
        }
        test = new ClassWithInitErrors();
        return test;
    }
}

@Test
public void givenInitErrorInClass_whenloadClass_thenNoClassDefFoundError() {
 
    NoClassDefFoundErrorExample sample
     = new NoClassDefFoundErrorExample();
    sample.getClassWithInitErrors();
}
```
