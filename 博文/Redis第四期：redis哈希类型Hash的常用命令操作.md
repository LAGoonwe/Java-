---
title: Redis第四期：redis哈希类型Hash的常用命令操作
date: 2019-03-29
tags: java
---
<meta name="referrer" content="no-referrer" />

##  Redis第四期：redis哈希类型Hash的常用命令操作
>Redis hash 是一个string类型的field和value的映射表，hash特别适合用于存储对象。
>示例：就是value值里又是一些键值对
>![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032922214756.png)

>1.hset：将哈希表 key 中的域 field 的值设为 value
>（方便示例，先清空所有keys）
>给键k1设置一个键值对叫做(name,lagoon)
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329222429719.png)

>2.hget：获取哈希表 key 中给定域 field 的值
>示例如上图

>3.hmset：同时将多个 field-value (域-值)对设置到哈希表 key 中
>示例：
>给键k2设置id为10，name为tom，age为20，同时设置
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329222710901.png)

>4.hmget：获取哈希表 key 中一个或多个给定域的值
>示例如上图


>5.hgetall：获取哈希表 key 中所有的域和值
>示例：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329222919385.png)

>6.hdel：删除哈希表 key 中的一个或多个指定域field
>示例：删除k2的name和age后再查看k2
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329223128227.png)
>只剩id了

>7.hkeys：查看哈希表 key 中的所有field域
>示例：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329223232976.png)

>8.hvals：查看哈希表 key 中所有域的值
>示例：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329223316885.png)

>9.hlen：获取哈希表 key 中域field的个数
>示例：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329223436295.png)

>10. hexists：查看哈希表 key 中，给定域 field 是否存在
>示例：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329223539492.png)
>age已经是被删除了的，所以返回0，而id还存在，所以返回1


>11. hincrby：为哈希表 key 中的域 field 的值加上增量 increment 
>示例：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329223715234.png)
>k2原来的id值为10，返回变成了15


>12.hincrbyfloat：为哈希表 key 中的域 field 加上浮点数增量 increment 
>示例：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190329223835792.png)
>15又加上了9.7


>13. hsetnx：将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在的时候才设置，否则不设置
>示例：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032922401667.png)
>id存在则设置不成功，name不存在则设置成功
