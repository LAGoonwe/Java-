---
title: Redis第二期：有关Redis的客户端
date: 2019-03-26
tags: java
---
<meta name="referrer" content="no-referrer" />

##  Redis第二期：有关Redis的客户端
> redis有三种客户端
> 

 1. redis命令行客户端
 2. redis远程客户端
 3. redis编程客户端

> 1.redis命令行客户端
> redis-cli（Redis Command Line Interface）是Redis自带的基于命令行的Redis客户端，
> 用于与服务端交互，可以使用该客户端来执行redis的各种命令。
> 进入命令模式
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326103609980.png)
>
>也可以指定端口的ip连接
>指定IP和端口连接redis：./redis-cli -h 127.0.0.1 -p 6379
>输入命令来体验一下
>比如设置一个键值对，k1 v1
>set k1 v1
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326103808727.png)
>查询当前所有keys
>keys *
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326103855806.png)
>可以看见查询到刚刚设置的键k1
>redis默认有16个库，默认使用0号库，使用select  数字（1-16）切换操作库
>例如select 5
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326104121481.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)可知当前为5号库状态。
>其他命令
>删除当前库的数据：flushdb
>删除所有库的数据：flushall
>config get * 获得redis的所有配置值
>查看当前数据库中key的数目：dbsize
>查看redis服务器的统计信息：info
>redis自带的客户端退出当前redis连接: exit 或 quit
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326104324373.png)



> redis远程客户端
> 常用的有Redis Desktop Manager
> 搜索下载即可
> 界面如下
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326104538919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)注意：
远程连接redis需要修改redis主目录下的redis.conf配置文件：
1、bind ip 绑定ip注释掉；
2、protected-mode yes 保护模式改为no；
在根目录下切换root用户输入 vim redis.conf
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326104824206.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326104910856.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)提示没有安装vim编辑器，安装即可，输入第一行的命令，最后用vim打开配置文件，做好相关改动就行。
最后打开远程客户端创建新连接
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326105912690.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)首先查看当前虚拟机的ip
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032611004457.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)启动redis服务
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326110321528.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)接着把原来查看的ip输入进客户端测试连接
连接成功即可，会发现在客户端里展示16个库
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326111339760.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

> 3.编程客户端
> java编程客户端
> jedis
> 新建maven项目引入jedis 节点
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326111741133.png)
编写测试类
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326111802734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

```java
import redis.clients.jedis.Jedis;

import java.util.Iterator;
import java.util.Set;

/**
 * Hello world!
 *
 */
public class App 
{
    public static void main( String[] args )
    {

        //连接jedis服务器
        Jedis jedis=new Jedis("192.168.5.128",6379);
        //查看服务是否正常运行
        System.out.println("服务正在运行:"+jedis.ping());


        Set<String> keys = jedis.keys("*");

        Iterator<String> iterator = keys.iterator();

        while (iterator.hasNext()){
            System.out.println(iterator.next());
        }
    }
}
```
> 这个例子是连接redis服务器，然后调用jedis对象的方法，接着遍历出所有keys
> 会输出正常语句并打印出所有keys值，如果没有则不打印
> 





