---
title: Android修改状态栏
date: 2019-09-14 15:48:55
edited:
tags: Android入门
categories: Android
---
##### 效果图
这里的处理都是Android5.0以上的。
## 修改状态栏颜色
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191004165832122.png)
```java
Window window = activity.getWindow();
window.setStatusBarColor(Color.MAGENTA);
```

<!--more-->

## 隐藏状态栏
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191004165449215.png)
* 法一
```java
Window window = activity.getWindow();
View decorView = window.getDecorView();
decorView.setSystemUiVisibility(View.SYSTEM_UI_FLAG_FULLSCREEN);
```
* 法二
```java
Window window = activity.getWindow();
window.setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,WindowManager.LayoutParams.FLAG_FULLSCREEN);
```
* 
## 半透明状态栏
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191004165945790.png)
```java
Window window = activity.getWindow();
window.addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
```

## 全透明状态栏实现沉浸式
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191004170018316.png)
```java
Window window = activity.getWindow();
/*如果之前是办透明模式，要加这一句需要取消半透明的Flag
window.clearFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);*/
window.getDecorView().setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_STABLE |View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN);
window.setStatusBarColor(Color.TRANSPARENT);
```
## 修改状态栏字体颜色
* 设置状态栏图标和文字颜色为黑色
```java
Window window = activity.getWindow();
window.getDecorView().setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN | View.SYSTEM_UI_FLAG_LIGHT_STATUS_BAR);
```
* 设置状态栏图标和文字颜色为白色
```java
window.getDecorView().setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN|View.SYSTEM_UI_FLAG_LAYOUT_STABLE);
```
参考大大博客
[https://www.cnblogs.com/ldq2016/p/8353190.html](https://www.cnblogs.com/ldq2016/p/8353190.html)
[https://www.jianshu.com/p/31c4b324894e](https://www.jianshu.com/p/31c4b324894e)