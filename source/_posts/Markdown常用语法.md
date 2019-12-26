---
title: Markdown(md)常用语法
date: 2019-01-20 20:08:20
edited:
tags: md
categories: hexo博客搭建
---
# Markdown简介
> **Markdown即md语法，是一种轻量级标记语言，创始人为约翰·格鲁伯(英语：John Gruber)它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML(或者HTML)档”。该语法对于图片，图表、数学式都有支持，当前许多网站都广泛使用 Markdown 来撰写帮助文档或是用于论坛上发表消息。例如：GitHub、reddit、简书、hexo博客。————维基百科**

# 常用语法 
<!--more-->

## 标题
【语法】：#+空格+内容

【示例】

	# 一级标题
	## 二级标题
	### 三级标题
	#### 四级标题
	##### 五级标题

【效果】：
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题

## 区块
md有三种语法都可以实现区块的【效果】
第一种 【语法】：四个空格(缩进)+文字

【示例】就是【效果】：

	我是区块
	
第二种 【语法】：>+文字

	>怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了
	
【效果】：


>怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了怎么突然没网了


第三种通常用于写代码，而且每种语言对应不同的高亮风格。
[语法]：以java为例

	```
	```Java
	public class hello{
		public static void main{
			system.out.println("hello world");
		}
	}
	``````

【效果】：


```Java
public class hello{
	public static void main{
		ystem.out.println("hello world");
	}
}
```
## 强调

【示例】：

	*斜体*（中间有空格）
	**加粗**
	***斜体加粗***
	
【效果】：
*斜体*
**加粗**
***斜体加粗***

## 插入图片

【示例】：

	![]()
	![这里写图片说明](这里写图片地址)
	如：
	![Iron Man](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577114320
	784&di=0169979de7c056d71d67f484a887b0ec&imgtype=0&src=http%3A%2F%2Fimage.samanlehua.com%2Fnews%2F12%2F352312.jpg)
	


【效果】：
![Iron Man](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577114320784&di=0169979de7c056d71d67f484a887b0ec&imgtype=0&src=http%3A%2F%2Fimage.samanlehua.com%2Fnews%2F12%2F352312.jpg)

	附：<div style="width: 200px; margin: auto">![图片说明](图片地址)</div>     //css语法设置图片（可设置图片高度和宽度）
	

## 插入链接

【示例】：

	[]()
	[百度](http://baidu.com/)


【效果】：
[百度](http://baidu.com/)

## 分割线

【示例】：

	---
	***
	- - - -

【效果】：

---
***
- - - -

## 列表
【示例】：

	* 文字（中间有空格）
	- 文字
	+ 文字


无序列表：

* 文字
- 文字
+ 文字

## 表格
【示例】：

	时间|学习|生活                 //表格首部
	:---|:---:|---:                //冒号在哪边就往哪边对齐，两边都有冒号则居中
	1.21|Java1-100页|慢跑5000米    //表格内容，可以往下增加
	1.22||杭州之旅

【效果】：

时间|学习|生活                
:---|:---:|---:               
1.21|Java1-100页|慢跑5000米  
1.22||杭州之旅

## 换行
	【语法】：文字后+两个空格+回车，即有换行效果。

# 编辑工具
[dillinger ](https://dillinger.io/) 功能强大，来源国外，不太稳定
[mahua](http://mahua.jser.me/)在线编辑，界面一般
[Notepad++](https://notepad-plus-plus.org/download/v7.6.2.html) 大众编辑器，线下编辑，界面一般











