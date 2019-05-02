---
title: 【java】SpringBoot中自动/手动实现国际化之页面语言切换
date: 2019-01-20
tags: java
---
<meta name="referrer" content="no-referrer" />

##  SpringBoot中自动/手动实现国际化之页面语言切换

###  边看边听歌
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1339042889&auto=0&height=66"></iframe>

如何做到java代码中的国际化之页面的中英文切换？

自动切换实现在于项目的页面会根据用户所在的区域显示对应的语言，比如说该用户在中国，则对该用户显示中文语言，如果该用户在美国，则对该用户显示英文语言。

手动切换实现在于给页面设置切换按钮，让用户自己设置要使用的目标语言。

这里记录我自己学习到的语言切换国际化操作

—— 首先是实现自动的语言切换 ——，这里用登录页面演示一些操作，即我要把下面的这个页面实现国际化之语言切换
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120121427833.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
这个简单的页面有这几个中文 
>用户名，密码，记住我，登录，重置

所以在idea里对这些文字做一些配置
打开idea，在项目的resource目录下新建一个文件夹![](https://img-blog.csdnimg.cn/20190120121949528.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
命名为i18n，意思为国际化，当然名字随意，接着在该文件夹下新建一个如下类型的文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120122308638.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
放大：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120122359331.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
之后弹窗这个窗口
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120122617142.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
1号处给该资源包文件命名一个基础名，这里叫login，因为是登录页面的语言资源包，接着在2号处点击加号添加语言标准：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120122834138.png)
添加一个中文，zh_CN的意思是中文_中国
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019012012292413.png)
添加一个英文，en_US的意思是英文_美国
最后如下:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120123205303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
接着在整个资源包下面就会生成三个配置文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120123259756.png)
接着任意点击一个文件，将试图切换成箭头所指的样式，点击左上角加号添加关键字的中英文，如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120123519707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
回看text视图下的英文的资源文件，显示的是英文语言，当然都是自己手动配置的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120123802555.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
回看text视图下的中文的资源文件，显示的是中文语言，当然也都是自己手动配置的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120123905992.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
接着对login的页面进行修改
使得这些文字能够根据系统的默认时区生效
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120124824310.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
以上是文字显示的主要代码片段，可以看出是使用
>th:text="#{原先资源包里配置的参数}"
>这样的形式来展示配置好的中英文显示的
>其实idea有智能提示

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120125046607.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
这也同时说明了原来配置好的中英文是能够成功加载进页面代码里的
这里显示用户名，即username，所以选择login.username即可，其他同理
有一个需要注意的地方就是
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120125438566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
这里是一个勾选框，[[#{login.remember}]]这种写法是不必写在标签体内的，也是可以生效的
接着启动项目，如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120125654292.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120130059748.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
审查响应体头部发现是响应中文语言的
测试英文怎么测试，把浏览器设置为语言英文
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120130240391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
这里用的是火狐浏览器，把en_US设置为优先语言即可，保存更改
接下来重启项目运行
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120130509619.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
发现是英文显示了
审查请求体头部信息，发现优先是英文响应
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120130615726.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

自动的国际化语言切换至此

——做到手动的国际化语言切换——首先给登录页面设置中英文切换按钮，如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019012013105139.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
这里的思路是给两个超链接文字设置传参的页面跳转
具体如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120131619223.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120133312866.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
使用thymeleaf的th标签设置超链接跳转，跳转到index.html页面，因为之前设置过访问index.html
页面会返回到登录页面，具体见我上一篇博客。
接着向后端传递一个参数 l
中文点击带过去的参数值为zh_CN
English点击带过去的参数值为en_US
接着在新建一个自定义的区域解析器类MyLocaleResolver
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120132008900.png)
该解析器的作用就是捕捉前端传过来的参数，如果不为空，则将响应进来的参数值作为请求体生效
参数值为zh_CN，则设置请求体头部信息语言为中文显示，
参数值为en_US，则设置请求体头部信息语言为英文显示。
如果为空，则显示默认的语言。
该类代码如下：

```java
package com.lagoon.springboot05.component;




import org.springframework.web.servlet.LocaleResolver;
import org.thymeleaf.util.StringUtils;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Locale;

//自定义区域解析器
public class MyLocaleResolver implements LocaleResolver {


    //解析区域信息
    @Override
    public Locale resolveLocale(HttpServletRequest httpServletRequest) {
        //获取自定义请求头信息，l的参数值
        String l=httpServletRequest.getParameter("l");
        //获取系统的默认区域信息
        Locale locale=Locale.getDefault();
        if (!StringUtils.isEmpty(l)){
            String[] split=l.split("_");
            //接收的第一个参数为：语言代码，国家代码
            locale=new Locale(split[0],split[1]);
        }
        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {

    }
}

```
>Locale对象另做了解即可，最后返回这个请求体对象即可
接着在配置类里注入生效
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120132934747.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
具体代码如下：

```java
package com.lagoon.springboot05.config;

import com.lagoon.springboot05.component.MyLocaleResolver;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
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

    //区域解析器
    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocaleResolver();
    }
}

```
主要是区域解析器后下的代码工作，看得出来是返回了原来的解析器类

运行项目测试：
当我点击中文时，页面显示中文
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120133421457.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
当我点击英文时，页面显示英文
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120133457495.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
可以注意地址栏的参数值发现异同。
这样就实现了手动的国际化语言切换。
当然更复杂的语言切换是可以引入第三方翻译的api的。

