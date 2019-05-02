---
title: 【java】SpringBoot中th:attr动态生成并捕捉删除路径进行删除的表单提交操作
date: 2019-01-27
tags: java
---
<meta name="referrer" content="no-referrer" />

###  SpringBoot中动态生成并捕捉删除路径进行删除的表单提交操作
边看边听歌
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=27679286&auto=0&height=66"></iframe>

> 简单来说，遇到的需求和需要解决的是，如下:
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190127093811148.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)当我点击删除按钮以后，弹出确认框，如下图
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190127093858579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)点击确定则提交一个删除表单去后端控制器进行删除处理，显而易见，这里是需要传一个参数值去后端的，也就是供应商的编号，以确定具体删除哪个供应商。
> 后端控制器的代码（删除部分）如下：
>

```java
@DeleteMapping("/provider/{pid}")
    public String delete(@PathVariable("pid") Integer pid){
        logger.info("删除供应商pid="+pid);
        providerDao.delete(pid);
        return "redirect:/providers";
    }
```
在执行路径中通过注解接收一个来自前台的pid值，然后调用DAO操作，删除这个pid对应的供应商，接着回显删除后的页面。

>所以问题是，怎么动态在前端获取到这个pid，并在删除表单中传给后端控制器处理
>这里提供一种js+th标签（thymeleaf）的解决方案：
>首先在前端找到删除按钮，代码如下：

```html
<a href="#" class="delete" ><img th:src="@{/img/schu.png}" alt="删除" title="删除"/></a>
```
然后前端的删除弹窗代码如下：

```html
<!--点击删除按钮后弹出的页面-->
<div class="zhezhao"></div>
<form method="post" id="deleteForm" >
    <input type="hidden" name="_method" value="delete">
    <div class="remove" id="removeProv">
       <div class="removerChid">
           <h2>提示</h2>
           <div class="removeMain" >
               <p>你确定要删除吗？</p>
               <a href="#" id="yes">确定</a>
               <a href="#" id="no">取消</a>
           </div>
       </div>
    </div>
</form>
```
其中，这两行代码

```html
<form method="post" id="deleteForm" >
    <input type="hidden" name="_method" value="delete">
```
是指定了表单的提交方式为delete，所以在后端控制器用DeleteMapping注解，具体的参考restful架构风格
（thymeleaf中th标签的使用必须在html标签头部引入命名空间）
接着开始在删除按钮中动态捕捉要点击的路径，具体如下：
>将删除按钮的相关代码更改如下:

```html
<a th:attr="del_uri=@{/provider/}+${p.pid}" href="#" class="delete" ><img th:src="@{/img/schu.png}" alt="删除" title="删除"/></a>
```
>通过th:attr标签给这个删除按钮设置一个属性叫做del_uri，他的值是路径（@符号用在路径上，具体参考th标签的使用）加上一个动态值（$符号获取的）,这个动态值就是对应的每个供应商的编号pid

纵观整个相关代码：
```html
 <tr th:each="p:${providers}">
                <td th:text="${p.pid}"></td>
                <td th:text="${p.providerName}"></td>
                <td th:text="${p.people}"></td>
                <td th:text="${p.phone}"></td>
                <td th:text="${p.fax}"></td>
                <td th:text="${#dates.format(p.createDate,'yyyy-MM-dd')}"></td>
                <td>
                    <a th:href="@{/provider/}+${p.pid}" href="view.html"><img th:src="@{/img/read.png}"  alt="查看" title="查看"/></a>
                    <a th:href="@{/provider/}+${p.pid}+'?type=update'" href="update.html"><img th:src="@{/img/xiugai.png}" alt="修改" title="修改"/></a>
					<a th:attr="del_uri=@{/provider/}+${p.pid}" href="#" class="delete" ><img th:src="@{/img/schu.png}" alt="删除" title="删除"/></a>
                </td>
            </tr>
```
>可以看出实在总头部tr中循环得到所有供应商放进p中，然后可以用p.  的方式一一获取供应商的相关属性。（具体参考th标签的使用方法）

现在设置了这个del_uri属性，接着我们在浏览器中使用调试工具测试：
我用的是火狐浏览器
来到页面
按f12，点击页面选择器
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190127100146202.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)鼠标悬浮选择查看
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190127100309705.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)可以看出此时删除按钮有一个属性叫做，del_uri，并且值为 /provider/2001
看看别的
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019012710043686.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)也有这样一个属性，值2004.
可以看出是做到了动态获取当前pid的，然后怎么传到表单里去提交
>可以看出，这个del_uri属性的值其实是一个路径，所以可以将他赋值给表单的action属性，就可以直接作为表单的提交路径，其实是更方便操作的。
>这样的实现可以交给js来。
>写一个js文件，比如叫js.js
>如下

```javascript
$(function () {
    $('.delete').click(function () {
        //灰背景遮挡效果
        $('.zhezhao').css('display', 'block');
        $('#removeProv').fadeIn();
        $("#deleteForm").attr("action", $(this).attr("del_uri"));
    });
```
再放一下那个删除弹窗的代码方便对照:

```html
<!--点击删除按钮后弹出的页面-->
<div class="zhezhao"></div>
<form method="post" id="deleteForm" >
    <input type="hidden" name="_method" value="delete">
    <div class="remove" id="removeProv">
       <div class="removerChid">
           <h2>提示</h2>
           <div class="removeMain" >
               <p>你确定要删除吗？</p>
               <a href="#" id="yes">确定</a>
               <a href="#" id="no">取消</a>
           </div>
       </div>
    </div>
</form>
```
再放一下那个按钮代码方便对照:

```html
<a th:attr="del_uri=@{/provider/}+${p.pid}" href="#" class="delete" ><img th:src="@{/img/schu.png}" alt="删除" title="删除"/></a>
```
>可以看出，在js中是根据class="delete"获取到按钮元素，设置点击事件，当按钮被点击时，
>把背景遮罩效果启用，弹窗展示，然后下面这一句：
> $("#deleteForm").attr("action", $(this).attr("del_uri"));
> 意思是，得到当前点击按钮的del_uri元素并复制给一个id为deleteForm的表单的action属性

所以，通过，这样的方式，把一个动态属性路径动态赋值给了删除表单的action路径。
js的生效不用多说了，在前端引入即可。
接着设置表单提交
补充js为以下：

```javascript
//列表页面上点击删除按钮弹出删除框
$(function () {
    $('.delete').click(function () {
        //灰背景遮挡效果
        $('.zhezhao').css('display', 'block');
        $('#removeProv').fadeIn();
        $("#deleteForm").attr("action", $(this).attr("del_uri"));
    });
    //点击 确定
    $('#yes').click(function () {
        $("#deleteForm").submit();
        $('.zhezhao').css('display', 'none');
        $('#removeProv').fadeOut();
    });
    //点击 取消
    $('#no').click(function () {
        $('.zhezhao').css('display', 'none');
        $('#removeProv').fadeOut();
    });
});

```
加上的代码意义为，如果点击确定按钮，也即是设置了id为yes的那个表单按钮，则整个表单执行提交，接着取消遮罩效果，收起弹窗。

但是如果点击取消按钮，则单纯的取消遮罩效果收起弹窗即可不提交表单

最后就是后端处理的事了


测试一下：
删除供应商2002
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190127101837897.png)点击
弹窗
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190127101858226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)确定
然后回显数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190127101947303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)2002编号供应商数据没有啦

js+thymeleaf其实也是一种很好的解决方案

补充一下，引入js.js文件如下：

```html
<script th:src="@{/js/js.js}"></script>
```

加油！
