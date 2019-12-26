---
title: 生命周期--Activity
date: 2019-07-29 15:42:45
edited:
tags: Android入门
categories: Android
---
## 回调方法介绍
**说起Activity的生命周期想必离不开一幅图**<!--more-->
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xMjIzOTgxNy01N2JiMzRiYmYyMDE4NTNkLnBuZw)
方法|描述|常用情景
:---:|:---:|:---:
onCreate()|当活动第一次创建时调用|初始化数据，加载布局，绑定事件等
onStart()|活动被启动时调用|略
onRestart()|活动停止后被重启时调用|略
onResume()|活动用户可见时，即开始与用户交互时调用，此时活动必位于栈顶|略
onPause()|启动或者恢复另外一个活动时调用|信息持久化存储操作，停止消耗CPU资源等执行熟读要快，否则影响下一活动使用
onStop()|活动**完全**不可见时调用|因为活动界面会被覆盖或销毁，所有要储存重要信息
onDestory()|活动被销毁即出栈时被调用|数据回收，资源释放等
onSaveInstanceState()|活动被异常停止，可以用Bundle保存数据，可以在活动再次创建时的onCreate()方法中使用保存下来的数据|防止活动异常停止导致的数据丢失

* 与用户交互（前台）时期：onResume() -> onPause()
* 可见到不可见时期（对应用而言）：onStart() -> onStop()
* 启动到销毁时期：onCreat() -> onDestory()
## Activity的启动方式
    首先要知道的是活动的管理用的是Task，Task是返回栈。
    当活动创建时压栈，活动被销毁时出栈。活动的启动方式一共有四种。
  
* **standard**
这种模式是活动默认的启动方式。特点：每创建一个活动，则一个活动压栈，不管栈中是否已经存在该活动的实例。
* **singleTop**
这种模式呢不会重复创建栈顶的活动，就是说当新创建的活动与栈顶的活动一样时不会重复创建，而是直接调用栈顶的活动。
* **singleTask**
这种模式可以让整个程序上下文只存在一个实例，就是说，创建新的实例前会判断栈中是否已经存在相同的实例，倘若已经存在，则不会新创实例。
* **singlelnstance**
指定这种模式的活动，会启动一个新的返回栈来管理该活动。无论哪个程序访问该活动都共用同一个返回栈

**那个怎么指定一个活动的启动方式呢**
在AndroidManifest.xml中的activity标签中指定，例如：
```xml
<activity android:name=".MainActivity"
          android:launchMode="singleTask">
```
## 举例
* 启动活动1
```t
活动1执行了: onCreate()
活动1执行了: onStart()
活动1执行了: onResume()
```

* 活动1跳转活动2
```t
活动1执行了: onPause()
活动2执行了: onCreate()
活动2执行了: onStart()
活动2执行了: onResume()
活动1执行了: onStop()
活动1执行了: onSaveInstanceState(Bundle outState)
```

* 活动2点击back返回活动1
```t
活动2执行了: onPause()
活动1执行了: onRestart()
活动1执行了: onStart()
活动1执行了: onResume()
活动2执行了: onStop()
活动2执行了: onDestroy()
```
1、可见活动不可见了暂时不会被销毁，但是点击back会。
2、从活动1启动活动2，活动1的  **onSaveInstanceState()**  会被执行；但是按back键返回活动1，活动2的  **onSaveInstanceState()**  并不会执行，即使活动2已经被销毁。
3、从活动1启动活动2，先执行活动1的  **onPause()**  然后创建活动2，再执行活动1的 **onStop()**，因此  **onPause()**  的操作不宜过于耗时，否则影响活动2的启动。
* 熄屏
```t
活动1执行了: onPause()
活动1执行了: onStop()
活动1执行了: onSaveInstanceState(Bundle outState)
```
* 再亮屏
```t
活动1执行了: onRestart()
活动1执行了: onStart()
活动1执行了: onResume()
```
* 点击home键
```t
活动1执行了: onPause()
活动1执行了: onStop()
活动1执行了: onSaveInstanceState(Bundle outState)
```
* 再打开app
```t
活动1执行了: onRestart()
活动1执行了: onStart()
活动1执行了: onResume()
```
熄屏和返回home，都不会销毁该活动，再次打开app会被重启，执行 **onRestart()** 方法。
* 竖屏旋转为横屏 或者 横屏转竖屏
```t
活动1执行了: onPause()
活动1执行了: onStop()
活动1执行了: onSaveInstanceState(Bundle outState)
活动1执行了: onDestroy()
活动1执行了: onCreate()
活动1执行了: onStart()
活动1执行了: onResume()
```
屏幕方向的改变，会**销毁**当前活动，并且**新创建**一个新的活动。
* 横屏状态下熄屏再亮屏 或者 横屏状态点击home键再点开app
```t
活动1执行了: onPause()
活动1执行了: onStop()
活动1执行了: onSaveInstanceState(Bundle outState)
活动1执行了: onDestroy()
活动1执行了: onCreate()
活动1执行了: onStart()
活动1执行了: onResume()
```
横屏状态下熄屏或者横屏状态点击home键，都会销毁当前活动，再次打开时新创建一个活动，此时屏幕也变成了竖屏状态。

补充：默认情况下，**系统配置发生改变时**，如屏幕旋转，Activity是会被重新创建的，当然你想要不重建Activity也行，AndroidManifest的Activity标签下有一个属性**android：configChanges**，该属性指定的系统配置即使改变了，Activity也不会被销毁重建。如，我不希望屏幕方向改变而重新创建活动则：
```xml
<activity android:name=".MainActivity"
    android:configChanges="orientation">
```
该属性常用的值有：
1、orientation：屏幕方向发生改变
2、locale：设备的本地位置发生了改变，一般指切换了系统语言；
3、keyboardHidden：键盘的可访问性发生了改变，例如用户调出了键盘。

    最后说一下onSaveInstanceState()这个正常finifh一个活动方法不会被执行的,其它情况
    停止一个活动则会执行。我的理解，onSaveInstanceState()是防止某些数据在Activity停
    止时丢失的（用Bundle存起来），如果该活动还有可能被重启或重新创建则会执行onSaveIn
    stanceState()，以便恢复上回的数据，但是正常finish一个活动，说明该活动已经正
    常结束了，我不会再用它了，则不会执行onSaveInstanceState()。
其实有兴趣还可以对Activity整个周期的的实现源码看一遍，这里不多说了，从[https://www.jianshu.com/p/ee6a0e45bbec](https://www.jianshu.com/p/ee6a0e45bbec)可知一下几点：
* activity的生命周期是通过handler消息来控制的；
* activity的实例创建是通过反射来实现的；
* 在activity onResume生命周期后，才将布局view绘制添加到系统布局中并显示给用户；
* 在activity onDestory生命周期后，只是将window中的view，DecorView移除并置为null，并没有将activity实例置为null，activity实例仍然在内存中，如果在gc时还有持有该activity的引用就会造成内存泄露，也就是说activity onDestory后只是页面的销毁，并不代表当前activity实例的销毁。
## 参考资料
* 《第一行代码》
* 博客[https://www.jianshu.com/p/b5a72a741025](https://www.jianshu.com/p/b5a72a741025)
* 博客[https://www.jianshu.com/p/ec50675ed116](https://www.jianshu.com/p/ec50675ed116)
如有错误，恳请指正。