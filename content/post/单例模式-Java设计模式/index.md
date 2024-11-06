+++
title = '单例模式-Java设计模式'
date = 2020-09-06T08:51:07+08:00
draft = false
+++
# 单例模式

## 一 : 枚举单例 {#一--枚举单例}

> java中的枚举,只会初始化一次,所以枚举可以直接实现单例,直接上代码吧很简单

``` {.java .hljs}
public class Singleton {
    private Singleton() {}

    private enum EnumSingleton {
        INSTANCE;

        private Singleton singleton;
        EnumSingleton() {
            singleton = new Singleton();
        }

        public Singleton getSingleton() {
            return singleton;
        }
    }

    public static Singleton getSingleton() {
        return EnumSingleton.INSTANCE.getSingleton();
    }

    public static void main(String[] args) {
        Singleton singleton = Singleton.getSingleton();
        System.out.println(singleton);
    }
}
```

## 二 : 内部类单例 {#二--内部类单例}

> 在类的内部,再声明一个静态类,在静态类里边初始化外部的那个类

``` {.java .hljs}
public class SingletonInnerClass {
    private SingletonInnerClass() {

    }
    private static class Singleton {
        private static SingletonInnerClass singletonInnerClass = new SingletonInnerClass();
    }

    public static SingletonInnerClass getInstance(){
        return Singleton.singletonInnerClass;
    }
}
```

第一次看这个类的时候,没有想明白为什么不直接在外部声明一个静态变量,现在想想应该是外部类`SingletonInnerClass`{.language-plaintext
.highlighter-rouge}初始化的时候会把静态变量全都完成初始化,这样就不能实现延迟加载(只有真正用的时候才加载).

## 三 : 双重锁单例 {#三--双重锁单例}

> 这种模式最麻烦,而且注意点比较多

``` {.java .hljs}
public class SingletonDCL {
    private volatile static SingletonDCL singletonDCL;
    private SingletonDCL(){

    }

    public static SingletonDCL getSingletonDCL(){
    //判断空是因为singletonDCL初始化完成之后,不用每次都加锁,从而提高性能
        if(singletonDCL==null){
            synchronized (SingletonDCL.class){
                if(singletonDCL==null){
                    singletonDCL=new SingletonDCL();
                }
            }
        }
        return singletonDCL;
    }

}
```

volatile 这个关键字作用是:禁止指令重排序优化\
`singletonDCL=new SingletonDCL();`{.language-plaintext
.highlighter-rouge} 这句话并不是一个原子操作.\
这句话执行的具体内容为:

1.  给singletonDCL分配内存
2.  调用SingletonDCL构造函数完成初始化
3.  使singletonDCL对象的引用指向分配的内存空间

JVM
的即时编译器中存在指令重排序的优化,不能保证第二第三步骤的执行顺序,volatile可以使得按
1-\>2-\>3步骤来执行

(就写这么多吧,本博客的第一篇文章,如果以后发现有问题在修改,也欢迎在评论中指出问题,一起成长)
