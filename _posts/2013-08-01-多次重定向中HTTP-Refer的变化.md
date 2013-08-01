---

layout: post
title: "多次重定向中HTTP-Refer的变化"
category: Technology
tags: [W3C, HTTP]

---

先初步了解下URL重定向，Http Referer两个概念

>URL重定向（URL redirection，或称网址重定向或网域名称转址），是指当使用者浏览某个网址时，将他导向到另一个网址的技术。

说白了就是URL重定向就是把一个URL重新定向到另一个URL上去，重定向可以把一个目录或者文件的访问请求转发至另外一个目录或者文件。

>HTTP Referer是header的一部分，当浏览器向web服务器发送请求的时候，一般会带上Referer，告诉服务器我是从哪个页面链接过来的，服务器籍此可以获得一些信息用于处理。

也就是说，比如从百度搜索找到这篇文章后，用户点击访问到这篇文章，那么我就能够从HTTP Referer中统计出每天有多少用户访问我这个页面来自于百度。

* 在ASP中获取REFERER的方式是：Request.ServerVariables(“HTTP_REFERER”)
* 在JSP中获取REFERER的方式是：request.getHeader(“REFERER”);
* PHP中获取REFERER的方式是：$_SERVER['HTTP_REFERER'];

下列情况是能取得到REFERER：

1. 使用A标记访问页面<a href=”http://www.crackezone.com/”>点击这里</a>
2. 使用javascript点击事件访问页面<a href=”#” onclick=’window.location.href=”referer.php”;return false;’>点击这里</a>
3. 用Submit input提交的表单(POST or GET)
4. 使用Javascript提交的表单(POST or GET)，其实与2类似。

重点是这个，下列情况是取不到REFERER：

1. 从收藏夹点击链接，浏览器自定义主页打开或是在浏览器上手动输入http地址
2. 使用Javascript重定向：location.href; location.replace()
3. 使用服务器语言重定向，如ASP: Response.Redirect; PHP: header(‘Location: ‘)
4. 使用HTTP重定向方式：
5. 用XML加载地址

关于二次以上重定向，保存referer的方式。

举一个例子，倘若我们的一个链接已经被百度或者谷歌收入了，如：

[http://www.baidu.com/s?wd=sdda](http://www.baidu.com/s?wd=sdda) 记为：**搜索页**

找到一条自己网站的页面

[http://www.domain.com/shopping.php](http://www.domain.com/shopping.php) 记为：**页面A**

而此时我这个页面可能已经不需要了，这时候我需要重定向到我的一个淘宝主页：如：

[http://me1.taobao.com/](http://me1.taobao.com/) 记为：**页面B**

上述过程可以看做为二次重定向。

根据上面所说的情况，在**页面A**是可以找到Referer，这个Referer就是**搜索页**，

当重定向到**页面B**上的时候，我们会发现**页面B**得到的Referer其实也是**搜索页**。

对此找到的解释为：大部分浏览器的行为。

>[http://stackoverflow.com/questions/2158283/will-a-302-redirect-maintain-the-referer-string](http://stackoverflow.com/questions/2158283/will-a-302-redirect-maintain-the-referer-string)

因此不能过分的依赖referer，如要正确的获得这个referer，可以在页面A中把referer作为参数，从地址栏中传递到下个面。如shopping.php

```
    $url = 'http://'.$_SERVER['HTTP_HOST'].$_SERVER['REQUEST_URI'];    
    $url = base64_encode($url);
    header ('Location: http://me1.taobao.com/?referer='.$url);
```
