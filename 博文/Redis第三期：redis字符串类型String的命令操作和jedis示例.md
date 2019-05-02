---
title: Redis第三期：redis字符串类型String的命令操作和jedis示例
date: 2019-03-26
tags: java
---
<meta name="referrer" content="no-referrer" />

##  Redis第三期：redis字符串类型String的命令操作和jedis示例
> redis字符串类型String命令常用操作如下：
> 

 1.  set：将字符串值 value 设置到 key 中
 命令行客户端示例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190328230256591.png)
 2. get：获取 key 中设置的字符串值
  命令行客户端示例：同上
 3. incr：将 key 中储存的数字值加1，如果 key 不存在，则 key 的值先被初始化为 0 再执行 INCR 操作
（只能对数字类型的数据操作）
 命令行客户端示例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190328230505193.png)
 4. decr：将 key 中储存的数字值减1，如果 key 不存在，则么 key 的值先被初始化为 0 再执行 DECR 操作（只能对数字类型的数据操作）
 命令行客户端示例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032823061381.png)
 5. setex：set expire的简写，设置key的值 ，并将 key 的生存时间设为 seconds (以秒为单位)  
 命令行客户端示例： 等待四秒后，k4不存在
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190328230731655.png)
 
 6. setnx：setnx 是 set if not exists 的简写，如果key不存在，则 set 值，存在则不设置值
 命令行客户端示例： 
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190328230839727.png)
 7. getset：设置 key 的值为 value ，并返回 key 的旧值
 命令行客户端示例： 
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190328230957650.png)

>k6原本就不存在，所以只能返回空值
>而k5设置过为v5,再设置成v55，getset返回旧值v5，get查看到新值v55

>使用java客户端来操作
>同样引入jedis包
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190328231601548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

```java
 <dependency>
          <groupId>redis.clients</groupId>
          <artifactId>jedis</artifactId>
          <version>2.9.0</version>
      </dependency>
```

进行综合示例前保证redis服务打开，关闭保护模式
接着在命令行中清空所有key值，为了让演示不冲突
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190328231829871.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
查看到所有key值并删除：flushall命令
综合示例：

```java
package com.lagoon;

import redis.clients.jedis.Jedis;

/**
 * Hello world!
 *
 */
public class App 
{
    public static void main( String[] args ) throws InterruptedException {
        //根据端口和ip连接redis
        Jedis jedis=new Jedis("192.168.5.131",6379);
        //输入密码
        jedis.auth("123456");
        System.out.println("服务正在运行"+jedis.ping());

        //String类型的相关操作
        jedis.set("k1","v1");
        String v1=jedis.get("k1");
        System.out.println(v1);//v1

        System.out.println(jedis.incr("k2"));//k2不存在，设置为0,所以0+1=1

        System.out.println(jedis.decr("k2"));//k2为1，减1，为0

        jedis.setex("k3",3,"v3");
        System.out.println(jedis.get("k3"));//为过期前为v3
        Thread.sleep(4000);//让线程睡眠四秒
        System.out.println(jedis.get("k3"));//四秒后过期，为null

        jedis.setnx("k4","v4");
        System.out.println(jedis.get("k4"));//k4不存在，直接设置值，为v4

        String getset=jedis.getSet("k5","v5");
        System.out.println(getset);//k5本身就不存在，所以返回的值为null

        //关闭与redis的链接
        jedis.close();
    }
}

```

运行结果如下，符合String命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190328232025945.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)


