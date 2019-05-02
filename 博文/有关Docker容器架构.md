---
title: 有关Docker容器架构
date: 2019-04-08
tags: Docker
---
<meta name="referrer" content="no-referrer" />

##  有关Docker容器架构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190408110120125.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

>Docker客户端与服务器
>Docker是一个客户端-服务器的（C/S）架构程序，Docker的客户端只需要向Docker服务器或者守护进程发出请求，服务器或者守护进程将完成所有工作并返回结果。Docker提供了一个命令行工具和一整套RESTful API。你可以在同一台主机上运行Docker守护进程和客户端，也可以从本地的Docker客户端连接到运行在另一台宿主机上的远程Docker守护进程。
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190408110539327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

>Docker镜像
>镜像（Image）是Docker中的一个模板。通过Docker镜像来创建Docker容器，一个镜像可以创造出多个容器。镜像是由一系列指令一步一步构建出来，例如：
>

 1. 添加一个文件
 2. 执行一个命令
 3. 打开一个窗口
>j镜像与容器的关系类似于java中类与对象的关系镜像体积很小，非常“便携”，易于分享，存储和更新。
>
| Docker | Java |
|--|--|
| 镜像| 类 |
| 容器| 对象 |

```java
class Emp{} //镜像：
Emp e1 = new Emp(); //容器1
Emp e2 = new Emp(); //容器2
```

>Docker容器
>容器（Container）是基于镜像创建的运行的实例，一个容器中可以运行一个或者多个应用程序（jdk+开发的java应用程序）。
>Docker可以帮助你构建和部署容器，你只需要把自己的应用程序或者服务打包放进容器即可。
>可以认为，镜像是Docker生命周期的构建或者打包阶段，而容器则是启动或者执行阶段。
><strong>可以理解容器中有包含：一个精简版的Linux环境+要运行的应用程序</strong>



>Docker仓库
>仓库（Repository）是集中存放镜像文件的场所。
>有时候会把仓库（Repository）和仓库注册服务器（Registry）混为一谈，但并不严格区分。实际上，仓库注册服务器上往往存放着多个仓库，每个仓库中又包含了多个镜像，每个镜像有不同的标签（tag）。
>仓库分为公有仓库（Public）和私有仓库（Private）两种。
>Docker公司运营的公共仓库叫做 Docker Hub （https://hub.docker.com/），存放了数量庞大的镜像供用户下载。用户可以在Docker Hub注册账号，分享并保存自己的镜像。（说明：在Docker Hub下载镜像巨慢）
>国内的公有仓库包括阿里云 、网易云 等，可以提供大陆用户更稳定快速的访问。
>当用户创建了自己的镜像之后就可以使用 push 命令将它上传到公有或者私有仓库，这样下次在另外一台机器上使用这个镜像时候，只需要从仓库上 pull 下来就可以了。
>Docker 仓库的概念跟 Git 类似，注册服务器可以理解为 GitHub 这样的托管服务。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190408112040550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
