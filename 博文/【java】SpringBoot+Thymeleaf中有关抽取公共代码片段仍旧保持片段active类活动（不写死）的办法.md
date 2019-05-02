---
title: 【java】SpringBoot+Thymeleaf中有关抽取公共代码片段仍旧保持片段active类活动（不写死）的办法
date: 2019-01-12
tags: java
---
<meta name="referrer" content="no-referrer" />

##  SpringBoot+Thymeleaf中有关抽取公共代码片段仍旧保持片段active类活动（不写死）的办法

最近在进阶练习java框架springboot时，因为用的是thymeleaf模板，在使用过程中因为每个页面都有左侧导航栏的形式，大概如图：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190112205111920.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
因为在右侧展示的每个具体页面都有这个左侧导航栏，所以这部分左侧导航栏的代码可作为公共代码片段抽取出来，供每个页面引入使用。

这里首先把这个左侧导航栏的有关代码抽取出来放进一个public.html，这个public.html是新建的空白模板页面，如下：

```html
<!DOCTYPE html>
<html>
<!-- 命名head头部代码片段id为public_head -->
<head lang="en">
    <meta charset="UTF-8">
    <title>公共代码片段</title>
</head>
<body>
<!-- 把左侧导航栏代码片段抽取到这并设置id为public_left -->
    <div class="left" id="public_left">
        <h2 class="leftH2"><span class="span1"></span>功能列表 <span></span></h2>
        <nav>
            <ul class="list">
                <li><a href="../bill/list.html">账单管理</a></li>
                <li><a href="../provider/list.html" id=“active”>供应商管理</a></li>
                <li><a href="../user/list.html">用户管理</a></li>
                <li><a href="../main/password.html">密码修改</a></li>
                <li><a href="../main/login.html">退出系统</a></li>
            </ul>
        </nav>
    </div>
</body>
</html>
```
>这里设置id的作用就是好让其他页面引入这整个代码片段
接着在供应商展示主页（主要代码）引入（其实算替换）这个代码片段：

```html
<!--主体内容-->
<section class="publicMian ">
<!-- 展示左侧导航栏的div块 -->
    <div class="left" th:replace="main/public:: #public_left">
    </div>
    <div class="right">
        <div class="location">
            <strong>你现在所在的位置是:</strong>
            <span>供应商管理页面</span>
        </div>
        <div class="search">
            <span>供应商名称：</span>
            <input type="text" placeholder="请输入供应商的名称"/>
            <input type="button" value="查询"/>
            <a href="add.html">添加供应商</a>
        </div>
        <!--供应商操作表格-->
        <table class="providerTable" cellpadding="0" cellspacing="0">
            <tr class="firstTr">
                <th width="10%">供应商编码</th>
                <th width="20%">供应商名称</th>
                <th width="10%">联系人</th>
                <th width="10%">联系电话</th>
                <th width="10%">传真</th>
                <th width="10%">创建时间</th>
                <th width="30%">操作</th>
            </tr>
            <tr>
                <td>PRO-CODE—001</td>
                <td>测试供应商001</td>
                <td>韩露</td>
                <td>15918230478</td>
                <td>15918230478</td>
                <td>2015-11-12</td>
                <td>
                    <a href="view.html"><img th:src="@{/img/read.png}"  alt="查看" title="查看"/></a>
                    <a href="update.html"><img th:src="@{/img/xiugai.png}" alt="修改" title="修改"/></a>
					<a href="#" class="delete" ><img th:src="@{/img/schu.png}" alt="删除" title="删除"/></a>
                </td>
            </tr>
            <tr>
                <td>PRO-CODE—001</td>
                <td>测试供应商001</td>
                <td>韩露</td>
                <td>15918230478</td>
                <td>15918230478</td>
                <td>2015-11-12</td>
                <td>
                    <a href="view.html"><img th:src="@{/img/read.png}"  alt="查看" title="查看"/></a>
                    <a href="update.html"><img th:src="@{/img/xiugai.png}" alt="修改" title="修改"/></a>
                    <a href="#" class="delete" ><img th:src="@{/img/schu.png}" alt="删除" title="删除"/></a>
                </td>
            </tr>
        </table>

    </div>
</section>
```

> 这里主要分析以上代码里的这一句
 

```html
<div class="left" th:replace="main/public:: #public_left"></div> 
```
可以看出这里采用了th标签，它是thymeleaf模板提供的命名标签，所以这个页面一定要引入thymeleaf的命名规则，如下：


```html
<html xmlns:th="http://www.thymeleaf.org">
```

即在html标签头部里加入这个网址即可
然后th:replace其实它的一个用法是替换这个通过replace后的内容替换当前的div块的内容或者其他html标签。
replace后面通过引号引起来的路径则是存着公共代码片段的html网页，在这里即是：main/public  
他的含义是main文件夹下的public网页，即public.html
因为我的路径如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190112212244187.png)

.html后缀可以不用写
接着在路径后用上两个英文冒号 ::
最后像一般的js逻辑一样
> #public_left呼应了刚刚public.html页面里对左侧导航栏设置的id名
所以这行代码的意识就是通过利用th标签找到main文件夹下的public.html网页里的id为public_left的标签内的代码，用这段代码替换当前的代码，当前代码div里并没有写东西，直接填充进来就行。如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190112213213243.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)

这样就能成功被各个页面引入公共代码片段
但是这里就出现了一个问题，因为这里对左侧导航栏设置了active类，点击高亮，如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/201901122134077.png)

所以如果当我点击账单管理的时候，并在账单管理页面里引入公共代码片段，那岂不是还是供应商管理这个栏目高亮？
>这里就需要代码片段引用并且传递参数

首先在要引用的地方改成如下，即：
```html
<div class="left" th:replace="main/public:: #public_left(activeUri='provider')"></div> 
```
即给这个引入同时设置一个叫做activeUri的参数并设置值为provider
然后public代码里的代码就改成了：

```html
<!DOCTYPE html>
<html>
<!-- 命名head头部代码片段id为public_head -->
<head lang="en">
    <meta charset="UTF-8">
    <title>公共代码片段</title>
</head>
<body>
<!-- 把左侧导航栏代码片段抽取到这并设置id为public_left -->
    <div class="left" id="public_left">
        <h2 class="leftH2"><span class="span1"></span>功能列表 <span></span></h2>
        <nav>
          <ul class="list">
                <li th:id="${activeUri == 'bill' ? 'active': ''}"><a href="../bill/list.html">账单管理</a></li>
                <li th:id="${activeUri == 'provider' ? 'active': ''}"><a href="../provider/list.html">供应商管理</a></li>
                <li th:id="${activeUri == 'user' ? 'active': ''}"><a href="../user/list.html">用户管理</a></li>
                <li th:id="${activeUri == 'pwd' ? 'active': ''}"><a href="../main/password.html">密码修改</a></li>
                <li><a href="../main/login.html">退出系统</a></li>
            </ul>
        </nav>
    </div>
</body>
</html>
```

通过给每个列表头部设置th:id并作出判断，判断前端要用的参数activeUri在后台是否有匹配的值即provider，是的话则id=active，即显示高亮，否则则id=''，即不显示任何active效果，显而易见，只有供应商管理作出的参数值判断和前端用的参数值匹配，所以供应商管理这个栏目显示高亮，这个意思就是
>th:id="是否前端的参数值等于bill？是则为active，不是则为空"
>th:id="是否前端的参数值等于provider？是则为active，不是则为空"
>th:id="是否前端的参数值等于user？是则为active，不是则为空"
>th:id="是否前端的参数值等于pwd？是则为active，不是则为空"

所以可以看出只有第二个栏目成功匹配，就会显示id="active"
其他的不匹配，则显示id="",也就是空
这个时候也就达到了对应栏目引入代码片段动态产生高亮的效果
所以，如果要在密码修改页面引入public页面里的这段公共代码要怎么做
当然是

```html
<div class="left" th:replace="main/public:: #public_left(activeUri='pwd')"></div> 
```

那么这个时候密码修改栏目成功匹配，就会显示高亮了。

