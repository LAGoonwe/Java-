---
title: Redis第七期：redis持久化的两种实现方式
date: 2019-03-31
tags: java
---
<meta name="referrer" content="no-referrer" />

##  Redis第七期：redis持久化的两种实现方式
>首先，什么是持久化，什么是redis的持久化
>持久化可以理解为存储，就是将数据存储到一个不会丢失的地方，如果把数据放在内存中，电脑关闭或重启数据就会丢失，所以放在内存中的数据不是持久化的，而放在磁盘就算是一种持久化。

>Redis的数据存储在内存中，内存是瞬时的，如果linux宕机或重启，又或者Redis崩溃或重启，所有的内存数据都会丢失，为解决这个问题，Redis提供两种机制对数据进行持久化存储，便于发生故障后能迅速恢复数据。

>redis提供两种持久化方式

>1.RDB方式
>Redis Database（RDB），就是在指定的时间间隔内将内存中的数据集快照写入磁盘，数据恢复时将快照文件直接再读到内存。
>RDB方式的数据持久化，仅需在redis.conf文件中配置即可
>配置文件redis.conf中搜索 SNAPSHOTTING
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331163225324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
>其中配置格式：
>save 900 1
>save 300 10
>save 60 10000
>示例表示在900秒内发生1次改变就保存数据，类似如此
>那么保存数据在哪
>dbfilename：设置RDB的文件名，默认文件名为dump.rdb
>dir：指定RDB和AOF文件的目录
>以上这些都可以自定义配置
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331163505648.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
>容易发现其实redis默认开启RDB持久化的
>通过查找该文件名可以证明
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331163720244.png)
>通过查找可以发现存在该文件，说明之前的操作都是RDB持久化了的，并在这个文件里被记录


>RDB持久化方式的优缺点
>优点：由于存储的是数据快照文件，恢复数据很方便，也比较快
>缺点：会丢失最后一次快照以后更改的数据

>如果你的应用能容忍一定数据的丢失，那么使用rdb是不错的选择
如果你不能容忍一定数据的丢失，使用rdb就不是一个很好的选择


>由于需要经常操作磁盘，RDB 会经常 fork 出一个子进程。如果你的redis数据库很大的话，Fork 占用比较多的时间，并且可能会影响 Redis 暂停服务一段时间（millisecond 级别），如果你的数据库超级大并且你的服务器CPU比较弱，有可能是会达到一秒。



>2.第二种持久化方式
>AOF方式
>什么是AOF方式
>Append-only File（AOF），Redis每次接收到一条改变数据的命令时，它将把该命令写到一个AOF文件中（只记录写操作，读操作不记录），当Redis重启时，它通过执行AOF文件中所有的命令来恢复数据。
>AOF方式的数据持久化，仅需在redis.conf文件中配置即可
>在redis.conf配置文件中搜索APPEND ONLY MODE
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331164423552.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)>把appendonly 改为yes保存开启AOF持久化
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331164813337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
>redis服务指定新配置开启
>先杀死服务进程
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331164922161.png)
>以新配置开启redis服务
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331165016849.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)>在命令行终端写一些测试命令
>![在这里插入图片描述](https://img-blog.csdnimg.cn/2019033116521215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)>退出回到src目录下查找aof文件
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331165313367.png)
>可以看见生成了持久化记录文件
>打开查看内容
>![在这里插入图片描述](https://img-blog.csdnimg.cn/2019033116544258.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
>可见记录文件
>选择0号库，flushall清空所有，接着set k1 v1 ,set k2 v2
>aof文件只会记录写操作，不会记录读操作


>关于一些其配置
>appendfilename：指定AOF文件名，默认文件名为appendonly.aof
>appendfsync：配置向aof文件写命令数据的策略
>auto-aof-rewrite-percentage：当目前aof文件大小超过上一次重写时的aof文件大小的百分之多少时会再次进行重写，如果之前没有重写，则以启动时的aof文件大小为依据
>auto-aof-rewrite-min-size：允许重写的最小AOF文件大小

>其中执行策略有
>no：不主动进行同步操作，而是完全交由操作系统来做（即每30秒一次），比较快但不是很安全
>always：每次执行写入都会执行同步，慢一些但是比较安全
>everysec：每秒执行一次同步操作，比较平衡，介于速度和安全之间

>总结
>append-only 文件是另一个可以提供完全数据保障的方案；
>AOF 文件会在操作过程中变得越来越大。比如，如果你做一百次加法计算，最后你只会在数据库里面得到最终的数值，但是在你的 AOF 里面会存在 100 次记录，其中 99 条记录对最终的结果是无用的；
>但 Redis 支持在不影响服务的前提下在后台重构 AOF 文件，让文件得以整理变小；
>可以同时使用这两种方式，redis默认优先加载aof文件；
