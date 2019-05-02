---
title: 【java】SpringBoot中通过自定义配置类实现设置系统的默认欢迎页
date: 2019-01-20
tags: java
---
<meta name="referrer" content="no-referrer" />

##  SpringBoot中通过自定义配置类实现设置系统的默认欢迎页
###  边看边听歌
> <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1314927356&auto=0&height=66"></iframe>

在使用IDEA做java项目的时候，以前很多时候都是用xml文件里来配置路径或者直接在tomcat的设置里设置完成以达到为系统设置默认欢迎页的效果

但是springboot官方是提倡无配置文件也就是不使用xml文件的。

>所以这里记录自己所学习到的在springboot中通过自定义配置类来实现设置系统默认欢迎页

>总的来说，如下，我要达成以下目标，即我访问  _'/'_ 或者访问 _'/index.html'_
>都能够访问到我设置的默认的系统欢迎页，login.html即是系统的默认欢迎页，每个用户使用系统都需要登录
>所以目前的任务是当我在地址输入栏里输入'/'或者'/index.html'都能够跳转到login.html这个页面。

新建一个包叫config，里面新建一个类叫做MySpringConfigurer （类名自定义即可）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120112824681.png)
类里写上如下代码：

```java
package com.lagoon.springboot05.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class MySpringMvcConfigurer {

    @Bean
    public WebMvcConfigurer webMvcConfigurer(){
       return new WebMvcConfigurer(){
           //添加视图控制器配置默认的欢迎页
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("/").setViewName("main/login");
                registry.addViewController("/index.html").setViewName("main/login");

            }
        };
    }
 }

```

>整个类上需要一个注解@Configuration标注这是一个配置类，系统启动时会自动加载这些配置
>主要来说就是添加一个视图控制器，其中设置把访问的 '/' 和 ' /index.html' 路径跳转到视图名称为main/login的视图里，这里设置的跳转路径和自己的项目目录结构有关，因为我的login.html页面在如下的结构里：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120113516128.png)
>所以系统通过配置类会自动去找到这个页面显示出来。

最后结果如下：
启动项目以后，输入如下地址：http://localhost:8080/
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120113906480.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
接着输入：http://localhost:8080/index.html访问测试
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120114025565.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
可以看到并没有进入主页，而是跳转到了登录页面
这样就实现了通过自定义配置类来实现设置默认的欢迎页
