---
title: Redis的一个小坑
date: 2019-03-26
tags: wrongs
---
<meta name="referrer" content="no-referrer" />

##  Redis的一个小坑
> 又忘了，修改了redis的配置文件就一定要重新制定配置文件启动
> 太坑了，记录一下
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190327173254216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)测试成功

```java
package com.lagoon;

import redis.clients.jedis.Jedis;

/**
 * Hello world!
 *
 */
public class App 
{
    public static void main( String[] args )
    {
        //根据端口和ip连接redis
        Jedis jedis=new Jedis("192.168.5.128",6379);
        //输入密码
        jedis.auth("123456");
        System.out.println("服务正在运行"+jedis.ping());
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032717334883.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
