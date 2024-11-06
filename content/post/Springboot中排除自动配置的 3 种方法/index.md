+++
title = 'Springboot中排除自动配置的 3 种方法'
date = 2020-12-06T20:21:00+08:00
draft = false
+++
# Springboot中排除自动配置的 3 种方法

> springboot中排除自动配置

## 1.@SpringBootApplication 注解中去掉 {#1springbootapplication-注解中去掉}

``` {.java .hljs}
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.context.properties.bind.Bindable;
import org.springframework.boot.context.properties.bind.Binder;
import org.springframework.context.ConfigurableApplicationContext;

@SpringBootApplication(exclude = {MyAppAutoConfiguration.class})
public class StartApplication {
    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(StartApplication.class, args);

        Binder binder = Binder.get(run.getEnvironment());
        String s = binder.bind("myapp.company", Bindable.of(String.class)).get();
        System.out.println(s);
    }
}
```

## 2.@EnableAutoConfiguration注解中去掉 {#2enableautoconfiguration注解中去掉}

``` {.java .hljs}
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableAutoConfiguration(exclude = {MyAppAutoConfiguration.class})
public class ConfigTest {
}
```

## 3.配置文件中去掉 {#3配置文件中去掉}

``` {.yml .hljs .language-yaml}
server:
  port: 8081
myapp:
  name: 嘿嘿嘿
  company: 我是公司
spring:
  autoconfigure:
    exclude:
      - com.fang7.spring.boot.autoconfigure.MyAppAutoConfiguration
```
