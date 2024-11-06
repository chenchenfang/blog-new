+++
title = 'maven-shade-plugin 使用方法'
date = 2021-01-01T21:14:00+08:00
draft = false
+++
# maven-shade-plugin 使用方法

> 之所以发这个,是因为前段时间,spark项目打包,到服务器上之后,出现了不同版本jar包冲突的问题,然后使用了这个插件解决了问题.

## 一: relocation的使用

因为服务器上的jar不能删除,也不能替换,但是我用的jar包也只能用和服务器上不同的版本.

使用这个功能可以完美的解决这个问题

``` {.xml .hljs}
<plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.2.2</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <relocations>
                                <relocation>
                                    <!--原本的包名-->
                                    <pattern>okhttp3</pattern>
                                    <!--要替换成的包名-->
                                    <shadedPattern>fang7.okhttp3</shadedPattern>
                                </relocation>
                            </relocations>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
```

这样就可以解决包名冲突的问题

## 二: artifactSet excludes 去掉依赖

``` {.xml .hljs}
                               <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.2.2</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <artifactSet>
                                <excludes>
                                    <exclude>com.squareup.okhttp3:okhttp</exclude>
                                </excludes>
                            </artifactSet>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
```

将该工程依赖的部分 Jar 包 include/exclude 掉。
