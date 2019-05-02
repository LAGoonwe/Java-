---
title: Redis第六期：redis对事务控制的实现
date: 2019-03-31
tags: java
---
<meta name="referrer" content="no-referrer" />

##  Redis第六期：redis对事务控制的实现
>首先，什么是事务？
>事务是指一系列操作步骤，这一系列的操作步骤，要么完全地执行，要么完全地不执行。 
>比如微博中：A用户关注了B用户，那么A的关注人列表里面就会有B用户，B的粉丝列表里面就会有A用户。
这个关注与被关注的过程是由一系列操作步骤构成：
（1）A用户添加到B的粉丝列表里面
（2）B用户添加到A的关注列表里面；
这两个步骤必须全部执行成功，整个逻辑才是正确的，否则就会产生数据的错误，比如A用户的关注列表有B用户，但B的粉丝列表里没有A用户；
要保证一系列的操作都完全成功，提出了事务控制的概念。


>redis事务
>Redis中的事务（transaction）是一组命令的集合，至少是两个或两个以上的命令，redis事务保证这些命令被执行时中间不会被任何其他操作打断。


redis对事务控制的实现大致有以下几种情况：

>一，正常情况
>（测试中为避免冲突先flushall清空所有keys）
>1.用MULTI命令告诉Redis，接下来要执行的命令你先不要执行，而是把它们暂时存起来 （开启事务）
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331153622184.png)
>开启事务，等待命令进入队列
>2.输入执行命令，进入命令队列
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331153734604.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)queued返回值表示命令进入执行队列（注意此时还未执行）
>3.输入exec命令告知redis执行前面发送的命令（提交事务）
>![在这里插入图片描述](https://img-blog.csdnimg.cn/2019033115391417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)可以看到exec后命令执行成功并且能够查询到相关值


>二，异常情况
>1.开启事务
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331154046870.png)
>2.输入一个正常命令
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331154133223.png)
成功进入队列等待执行
>3.输入一个错误命令
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331154222884.png)
首先返回语法错误
>4.最后提交事务查看
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331154312892.png)
提交事务失败


>三，例外情况
>1.开启事务
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331154434294.png)
>2.输入一个正常命令
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331154513170.png)
>3.输入一个正常命令（本身没有语法错误）
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331154612151.png)
>成功进入执行等待队列
>4.提交事务
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331154649678.png)
>提示第一个命令执行成功
>第二个命令执行出错
>事务依然提交了，k5的值被设置为v5，自增操作执行失败，但整个事务没有回滚.


>四，放弃情况
>1.开启事务
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331154803648.png)
>2.输入两个正常命令进入执行队列
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331154844240.png)
>3.放弃事务
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331154925897.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
>放弃事务后，k6，k7并没有被赋值
>放弃事务，则命令队列不会被执行


>五，复杂情况
>这里要提到悲观锁和乐观锁概念


>悲观锁
>悲观锁(Pessimistic Lock)， 顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改该数据，所以每次在拿数据的时候都会先上锁，这样别人想拿这个数据就会block阻塞直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁，让别人无法操作该数据。

>乐观锁
>乐观锁(Optimistic Lock)，顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改该数据，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这条数据，一般使用版本号机制进行判断。乐观锁适用于多读的应用类型，这样可以提高吞吐量。

>乐观锁大多数情况是基于数据版本号（version）的机制实现的。何谓数据版本？即为数据增加一个版本标识，在基于数据库表的版本解决方案中，一般是通过为数据库表添加一个“version”字段来实现读取出数据时，将此版本号一同读出，之后更新时，对此版本号加1。此时，将提交数据的版本号与数据库表对应记录的当前版本号进行比对，如果提交的数据版本号大于数据库表当前版本号，则予以更新，否则认为是过期数据，不予更新。

>图例：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331155224726.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)



>redis的watch机制实现乐观锁
>监视一个(或多个) key ，如果在事务exec执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断
>这里开启两个终端来演示
>1.执行一个正常命令
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331155424951.png)
>2.开启对money的监控
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331155527541.png)
>3.开启事务
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331155601497.png)
>4.在事务等待命令的这段时间里，在另一个终端里改变money值（注意回车）
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331155714186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)5.接着在第一个终端里事务改写money值
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331155801875.png)
>6.提交事务查看
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331160205462.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)事务提交不了，因为发现在事务等待过程中，money值被监控已经发小被修改了，所以无法提交事务
>7.查看money值
>![在这里插入图片描述](https://img-blog.csdnimg.cn/2019033116031914.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)发现值最终是另一个终端改动影响的
