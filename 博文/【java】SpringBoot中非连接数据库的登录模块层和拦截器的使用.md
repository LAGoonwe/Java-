---
title: 【java】SpringBoot中非连接数据库的登录模块层和拦截器的使用
date: 2019-01-22
tags: java
---
<meta name="referrer" content="no-referrer" />

##  SpringBoot中非连接数据库的登录模块层和拦截器的使用

边看边听歌
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=471403427&auto=0&height=66"></iframe>

开发简单的登录模块，不涉及到数据库的连接
拦截器是为了防止用户直接输入主页地址即可以不用登录而进入主页
理论上来说，这是不现实和不安全的

springboot中开发登录模块层
前端一个表单，前端使用的是thymeleaf模板
如下，其中的一些代码基于上一篇博客而成：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190122091122839.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)  需要具体注意的是1号处使用了th标签设置了整个表单的提交路径，即"/login" 2号处指定了表单以post请求的方式提交。
表单的主要代码如下：

```html
<form class="loginForm" th:action="@{/login}" method="post" action="../main/index.html">
				
                <div class="inputbox">
                    <label for="user" th:text="#{login.username}">Username</label>
                    <input id="user" type="text" name="username"  required/>
                </div>
                <div class="inputbox">
                    <label for="mima" th:text="#{login.password}">Password</label>
                    <input id="mima" type="password" name="password"  required/>
                </div>
				<div class="subBtn">
						<input type="checkbox"> [[#{login.remember}]]
                </div>
				<br/>
                <div class="subBtn">
                    <input type="submit" th:value="#{login.submit}" value="Sign" />
                    <input type="reset" th:value="#{login.reset}" value="Reset"/>
                </div>
				<br/>
				<div style="margin-left: 100px;">
                    <a href="#" th:href="@{/index.html(l='zh_CN')}">中文</a>
					&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                    <a href="" th:href="@{/index.html(l='en_US')}">English</a>
                </div>
            </form>
```
 对应的，后端应该设置一个登录专用的控制器，捕捉表单提交的请求和顺带的一些参数。
 LoginController代码如下，新建Controller包，包下面新建LoginController类
 具体代码如下：
 

```java
package com.lagoon.springboot05.controller;


import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PostMapping;
import org.thymeleaf.util.StringUtils;

import java.util.Map;

@Controller
public class LoginController {

    @PostMapping("/login")
    public String login(String username, String password, Map<String,Object> map){
        //判断用户名不为空
        if (!StringUtils.isEmpty(username)&&"123".equals(password)){

            //登录成功
            return "redirect:/main.html";
        }

        //登录失败
        map.put("msg","用户名或没密码错误!");
        return  "main/login";

    }
}

```
这里做的登录逻辑是简单性的，判断前端输入的用户名不为空的话，且密码等于123，则算登录成功，否则算登录失败，反馈错误信息。
这里需要注意的是，登录成功是采用了重定向的防止转发到主页的，而main.html这个页面实际上是不存在的，只是起说明登录进的是主页的作用，也是为了让防止表单数据重复提交的情况出现，让用户刷新页面时不出现重复提交表单数据的提示框。
设置一个视图控制器，让/main.html这个响应路径对应跳转的页面就是我们的主页  index.html
如下:

```java
package com.lagoon.springboot05.config;

import com.lagoon.springboot05.component.MyLocaleResolver;
import com.lagoon.springboot05.interceptor.LoginHandlerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
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
                registry.addViewController("/main.html").setViewName("main/index");

            }

            //注入一个拦截器
           @Override
           public void addInterceptors(InterceptorRegistry registry) {
                registry.addInterceptor(new LoginHandlerInterceptor())
                        .addPathPatterns("/**")
                        .excludePathPatterns("/","/index.html","/login")
                        .excludePathPatterns("/css/*","/img/*","/js/*");

           }
       };
    }

    //区域解析器
    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocaleResolver();
    }
}

```
主要是以下这个区域起作用
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190122093033775.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
接着还有一个点就是前端展示登录错误的信息
如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190122093144553.png)使用了th:if标签进行判断，使用了th:text标签进行实时显示，实时显示的前提是th:if里的标签里的内容为真值，用#调用string的判断是否为空的方法，所以意思也就是，如果存在错误信息，则展示错误信息的内容，即用户名或密码错误。如果没有错误信息从后端传过来，则啥也不展示，整个标签体里的内容不生效。

测试如下：
首先是登录成功的情况
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190122093610721.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20190122093914444.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
成功登录进系统，并且地址栏显示的是main.html，这比显示直接的响路径有什么好处
当你刷新时，是不用重复提交表单数据的。

接下来测试登录失败的情况：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190122094128888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)点击登录按钮
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190122094150117.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)表单头部显示用户登录错误信息

>登录模块至此，但是还会有一个小问题就是，用户这个时候直接在地址栏里输入http://localhost:8080/main.html
>其实也是可以进入到系统主页的，这个实际上是不安全和不应该的
>所以采用springboot的拦截器方式，来对登录请求做一些拦截处理 。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190122094506464.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
新建一个拦截器包，包下新建一个登录拦截器类
类中代码具体如下：

```java
package com.lagoon.springboot05.interceptor;

import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class LoginHandlerInterceptor implements HandlerInterceptor {

    //调用目标方法之前被拦截
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        Object loginUser = request.getSession().getAttribute("loginUser");
        if (loginUser!=null){
            return true;
        }
        request.setAttribute("msg","没有权限，请先登录!");
        request.getRequestDispatcher("/index.html").forward(request,response);
        return false;
    }
}
```
这个登录拦截器类是继承了HandlerInterceptor 这个接口的，并重写了preHandle的方法，说明在登录调用目标方法之前被拦截进行处理判断。这里就要在之前登录控制器里把登录的用户名存进session，所以之前的登录控制器类代码更改如下：

```java
package com.lagoon.springboot05.controller;


        import org.springframework.stereotype.Controller;
        import org.springframework.web.bind.annotation.PostMapping;
        import org.thymeleaf.util.StringUtils;

        import javax.servlet.http.HttpSession;
        import java.util.Map;

@Controller
public class LoginController {

    @PostMapping("/login")
    public String login(HttpSession session,String username, String password, Map<String,Object> map){
        //判断用户名不为空
        if (!StringUtils.isEmpty(username)&&"123".equals(password)){

            session.setAttribute("loginUser",username);
            //登录成功
            return "redirect:/main.html";
        }

        //登录失败
        map.put("msg","用户名或没密码错误!");
        return  "main/login";

    }
}

```

把当前登录用户名存进session，并命名为loginUser。
接着在拦截器中进行判断，取出session值，如果用户名不为空。则跳过拦截，如果用户名为空，则拦截登录请求，并返回错误信息，用户没有权限进入主页，因为用户没有进行登录

>当然到了这里只是把一个登录拦截器定义好了，要使他生效，则需要将这个拦截器进行注入
>在之前自定义的配置类里进行重写注入
>代码如下：

```java
package com.lagoon.springboot05.config;

import com.lagoon.springboot05.component.MyLocaleResolver;
import com.lagoon.springboot05.interceptor.LoginHandlerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
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
                registry.addViewController("/main.html").setViewName("main/index");

            }

            //注入一个拦截器
           @Override
           public void addInterceptors(InterceptorRegistry registry) {
                registry.addInterceptor(new LoginHandlerInterceptor())
                        .addPathPatterns("/**")
                        .excludePathPatterns("/","/index.html","/login")
                        .excludePathPatterns("/css/*","/img/*","/js/*");

           }
       };
    }

    //区域解析器
    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocaleResolver();
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190122095402608.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
>可以看出来拦截器调用addPathPatterns方法，默认拦截系统所有请求  "/**"代表所有请求
>但是至此为止，有些请求是不能被拦截的，比如登录页面的访问请求，各种静态资源的响应请求，如果被拦截了，则前端不会有样式，所以.excludePathPatterns方法排除这些需要的请求

最后测试如下
直接访问主页的情况
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019012209575144.png)

回车

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019012209593170.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

提示没有权限，请先登录！





