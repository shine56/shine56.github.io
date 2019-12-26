---
title: Java实现简单爬虫
date: 2019-06-26 15:38:21
edited:
tags: Java入门
categories: Java
---
怎么用Java比较简单地去网上爬一些东西呢，比如一些图片。作为Java小白（在此之前，都不知道爬虫是个什么，hhh..）最近遇到这个问题，去网上找了一些思路。大致是这样的：

* 先访问某网络得到它的网站原代码
* 用正则表达式匹配出你想要的内容。

<!--more-->

##### 例子：
现在在[https://pixabay.com/zh/images/search/%E5%BF%AB%E4%B9%90/](https://pixabay.com/zh/images/search/%E5%BF%AB%E4%B9%90/)这个网站爬一张图片试试看。
* Java常规操作访问这个网站可以返回这个网站的原代码。
```html
<head><meta charset="utf-8">
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':new 
    Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)
    [0],j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='
    +i+dl;f.parentNode.insertBefore(j,f);})(window,document,'script','dataLayer','GTM-5CF9ZN');</script>
    ...
```
我们打印出返回的结果，发现就是一大段html代码。没学过，看不懂，但是我想要爬取的图片连接，经过观察发现，他的图片连接都在这一块：
```html
div class="item" data-w="640" data-h="426">
	<img srcset="https://cdn.pixabay.com/photo/2019/03/05/12/26/toque-macaque-4036088__340.jpg 1x, https://cdn.pixabay.com/photo/2019/03/05/12/26/toque-macaque-4036088__480.jpg 2x" src="https://cdn.pixabay.com/photo/2019/03/05/12/26/toque-macaque-4036088__340.jpg" alt="">
```
发现只要是**srcset=""**引号里面的都是那个网站上面的图片连接。
* 用正则表达式将我们需要的连接从返回的一大块代码中匹配出来
```java
Pattern pattern1 = Pattern.compile("(?<=srcset=\").*?(?=\")"); 
Matcher matcher1 = pattern1.matcher(ResponseData);
//ResponseData是返回的结果，是一段字符串
ArrayList<String> list1 = new ArrayList<>();
while (matcher1.find()){
    String group1 = matcher1.group();
    list1.add(group1);
}
```
* 最后可以把list里面的内容打印出来检验一下，这是爬出来的结果其中一条连接：[https://cdn.pixabay.com/photo/2015/01/07/15/51/woman-591576__340.jpg](https://cdn.pixabay.com/photo/2015/01/07/15/51/woman-591576__340.jpg)
当然这是一张缩略图（因为网站上面的是缩略图），想要找它的详图也简单
* 匹配图片详情页链接（也在那一大段代码里面），逐一访问。
* 访问返回的数据，再匹配出详情页的图片就OK了。