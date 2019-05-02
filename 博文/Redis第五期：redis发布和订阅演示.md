---
title: Redis第五期：redis发布和订阅演示
date: 2019-03-29
tags: java
---
<meta name="referrer" content="no-referrer" />

##  Redis第五期：redis发布和订阅演示
>reis的数据类型操作不再多说了，参考 <a href="http://redisdoc.com/">redis命令中文手册</a>


>演示redis的发布和订阅分别在命令行客户端和编程客户端演示


>1.命令行客户端演示
>首先开启redis服务不多说了
>这里模拟一个发布者，三个订阅者
>在redis src下开启四个命令行终端，进入命令行模式，如下图：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329233947818.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
>选取第一台作为发布者，其它三台作为订阅者


>首先在后三台终端上输入SUBSCRIBE hello 命令
>（如果有密码得先auth 密码命令输入密码）
>表示让这三台终端订阅hello这个频道，（分别回车）如图
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329234545918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)


>接着在第一台电脑上发布消息
>使用publish+频道名+消息  命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329234646291.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
回车


结果如下（注意观察）


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329234801733.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
三台终端同时跳出被发布的消息



>2.用jedis编程客户端来演示

源码如下：
同样要引入jedis节点jar包

```java
<dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
      <version>2.9.0</version>
    </dependency>
```

消息订阅者角色

```java
package com.lagoon;


import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPubSub;

/**
 * redis消息订阅者
 */
public class RedisSubscriber extends JedisPubSub {

    //当接收到消息后触发该方法

    @Override
    public void onMessage(String channel, String message) {
        super.onMessage(channel, message);
        System.out.println("频道["+channel+"]发布了一条消息["+message+"]");
    }

    //main方法接收消息，开启监听状态
    public static void main(String[] args) {
        //连接到jedis
        Jedis jedis=new Jedis("192.168.5.131",6379);
        jedis.auth("123456");

        RedisSubscriber redisSubscriber=new RedisSubscriber();

        //从频道订阅消息
        jedis.subscribe(redisSubscriber,"channel");
    }
}

```

消息发布者角色

```java
package com.lagoon;

import redis.clients.jedis.Jedis;

//消息发布者
public class RedisPublisher {

    public static void main(String[] args) {
        Jedis jedis=new Jedis("192.168.5.131",6379);
        jedis.auth("123456");

        //发布消息
        jedis.publish("channel","大家好");

        jedis.close();
    }
}

```

开始演示，首先，运行消息订阅者类里的main方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329235112562.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)程序处于监听状态，表示在等待消息发布

接着运行发布消息类main方法
如下
运行完以后切换到消息订阅者运行窗口会发现收到了已经发布的消息
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329235252436.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

这里甚至可以对代码进行一些改动，比如手动输入消息发布，不演示了

