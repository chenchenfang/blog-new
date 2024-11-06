+++
title = 'mysql中 GROUP_CONCAT() 的使用'
date = 2020-11-04T21:57:00+08:00
draft = false
+++
> 最开始是让优化一个报表的SQL,因为时间太长了,然后看到join了一个表,这个表
> 是个group by ,然后select中使用了GROUP_CONCAT()

``` {.sql .hljs}
SELECT
    place_id,
    GROUP_CONCAT( appname SEPARATOR ' ' ) appname,
    GROUP_CONCAT( appid SEPARATOR ' ' ) appid,
    type,
CASE
        
        WHEN GROUP_CONCAT( appid SEPARATOR '' ) = '' THEN
        'PC' ELSE GROUP_CONCAT( platform SEPARATOR ' ' ) 
    END AS platform 
FROM
    places_info 
GROUP BY
    place_id
```

这个SQL就用20秒,然后考虑做一个cache表,这样查起来就很快也很方便.\
做cache表用的是 insert into select ,\
然后报错

``` {.hljs .language-yaml}
1260 - Row 20094 was cut by GROUP_CONCAT(), Time: 0.153000s
```

查了一下说是,GROUP_CONCAT()最大允许结果长度默认是1024,这个值可以设置
[group_concat_max_len](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_group_concat_max_len)

``` {.sql .hljs}
SET global group_concat_max_len=15000;
SET session group_concat_max_len=15000;
```

两种方式可以设置\
我选择的是session 只针对当次回话有效.然后再执行
`insert into select`{.language-plaintext .highlighter-rouge}就OK了
