---
title: Redis第一期：为redis准备好虚拟机运行环境
date: 2019-03-26
tags: java
---
<meta name="referrer" content="no-referrer" />

# Redis第一期：为redis准备好虚拟机运行环境
##  第一期主要是准备好为redis运行的linux环境
> 这里选取的是Ubuntu的操作系统环境

从网易开源镜像网站中下载ubuntu桌面版镜像文件
> http://mirrors.163.com/


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190325231044275.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
选择一个版本号
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190325231122655.png)

下载带有desktop字样的桌面化镜像文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190325231213460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
 >接着在vmware里安装，这里就省略了

wmware里创建虚拟机引入该镜像文件，接着选择一切默认配置，设定好用户名和密码（系统登录会用到）
接着静静等待安装吧。(时间会有些久，如果发现哪里卡住了，点击展开后的跳过此项按钮也可)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190325231443530.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)接着进行一些最后的配置（时间也会有一些久，安装过程中静静等待就好了）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190325232759552.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)安装好以后就是这样的

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032600002948.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)双击用户名输入密码进入页面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326000211117.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
首先更换页面语言，点击右上角设置按钮
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326000300854.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)选择以下选项
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326000353952.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
点击管理安装更多语言
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326000419227.png)
取消点击的第一次弹窗
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326000517675.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)再点击该按钮弹窗
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326000602762.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)勾选简体中文并应用
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032600063550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)输入用户名密码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326000703491.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)等待下载简体中文语言依赖（依然会很慢，慢慢等待）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326000733896.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)下载完成后会出现一个汉语中国语言选项
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326001945613.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)这个时候用鼠标按住拖拽至第一行，如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326002037300.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)调整formats为hongkong香港，接着重启虚拟机。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326002150155.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)更新文件夹名称，界面成中文语言的了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326002318429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
从redis官网<a href="https://redis.io/download">下载tar.gz压缩包</a>

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326002519232.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)接着为Ubuntu操作系统创建最高权限用户root用户（<a href="https://www.cnblogs.com/biehongli/p/5730233.html">图片摘自</a>）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326082309777.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)


接着使用vmtools中的文件拖拽功能实现该压缩包往ubuntu里放入
><a href="https://blog.csdn.net/zxf1242652895/article/details/78203473"> Ubuntu中安装vmtools参考</a>
> 
> （如果一直未成功，在虚拟机自带的浏览器火狐里下载也可）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326085257527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
vmtools安装完成以后记得重启虚拟机
总之最后redis文件存在于虚拟机中
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032608554446.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

解压：tar -zxvf redis-5.0.4.tar.gz
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326085813588.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)切换至redis解压缩出来的文件夹主目录下右键打开终端，切换成root用户准备输入命令
输入gcc找不到命令根据提示安装gcc，因为redis文件需要gcc编译器编译
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326090038424.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)等待安装完成（依然很慢）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326090146912.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)安装完成后继续执行sudo apt install gcc命令.等待安装完成
root用户下，先输入make distclean命令
接着输入make命令（这些命令都需要在redis文件夹根目录下root权限用户执行）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326094855189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)编译完成
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326094917222.png)
有人在make执行之后再执行 make install，该操作则将 src下的许多可执行文件复制到/usr/local/bin 目录下
（可以不用执行，但也可以执行）
> cd src切换到src目录下，接着输入ll命令
> 可以看到有一个执行文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326095112553.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)这就是启动redis服务的可执行文件
后台启动服务后台启动： ./redis-server & 
以下说明服务启动成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326095330874.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)后台启动并输出日志到nohup.out文件：nohup /home/lag/redis-3.2.9/src/redis-server &(redis安装目录)

关闭服务
切换到 redis-5.0.4/src/ 目录执行：./redis-cli shutdown
或者kill pid 或者 kill -9 pid
至此，redis的基本环境，下载安装已经完成了。
