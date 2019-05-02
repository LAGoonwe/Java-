---
title: 实战电商项目-选择linux系统和设置tomcat环境和maven环境
date: 2019/04/21
tags: java
---


<meta name="referrer" content="no-referrer" />

###  实战电商项目-选择linux系统和设置tomcat环境和maven环境

在tomcat官网下载tar.gz压缩包
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421133837721.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

参考<a href="https://www.cnblogs.com/frankdeng/p/9597699.html">linux下安装tomcat</a>
安装tomcat之前必须先安装爱JDK
把压缩包拖拽进虚拟机
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421134525947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
创建一个文件夹
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421134648579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

移动tomcat安装包进文件夹
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421134827462.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

切换到该文件夹目录下解压缩
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421135054138.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

切换到tomcat bin目录下 ls发现可执行文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421135551621.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

编辑server.xml
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421140403381.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

在端口号位置设置编码为utf-8
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421140606344.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

切换到bin目录启动tomcat
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421140803497.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
tomcat启动成功
  接下来安装配置maven
  参考<a href="https://www.jianshu.com/p/d9f25f70a26a">这里</a>
同样的下载压缩包拖拽至虚拟机中
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421142709145.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

将压缩包移动至创建好的mavne文件夹中
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421143023189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

切换到mavne文件夹下解压缩
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421143132508.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
打开环境变量配置文件
vim /etc/profile
添加环境变量如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421143451234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
保存并推出
source /etc/profile 重新加载配置文件

mvn -version测试是否成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190421143659429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
成功
